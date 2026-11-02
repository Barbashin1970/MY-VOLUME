---
title: ffmpeg — киноленты для проверки записи экрана и вырезка с размытием по секундам
description: "Как просмотреть запись экрана по кадрам и собрать из неё чистый ролик одной командой ffmpeg. Киноленты: fps + crop + drawtext с меткой времени + tile в один PNG; при -ss перед -i метка считается от точки перемотки, нужен -copyts. Чистка: split → crop → boxblur → overlay с enable='between(t,…)' по секундам исходника, затем trim + setpts + concat, затем обрезка кадра, масштаб и fps. Без setpts=PTS-STARTPTS склейка даёт паузы; радиус boxblur — не больше половины меньшей стороны, у цвета вдвое меньше. ffmpeg-static из npm ставится вне проекта."
tags: [tool, ffmpeg, video, privacy, media]
created: 2026-10-27
updated: 2026-10-27
related:
  - knowledge/lessons/screen-recording-of-live-system-leaks-third-party-data.md
---

# ffmpeg: киноленты для проверки записи экрана и вырезка с размытием

## Когда нужно

Запись экрана идёт в публикацию — витрину, документацию, презентацию. Её надо:

1. просмотреть по кадрам на чужие данные;
2. собрать чистый короткий ролик: обрезать кадр, оставить нужные отрезки, размыть
   поле в заданные секунды, сжать для сайта.

Зачем и что искать в кадрах — урок
[screen-recording-of-live-system-leaks-third-party-data](../lessons/screen-recording-of-live-system-leaks-third-party-data.md).

## ffmpeg без установки в проект

```bash
mkdir media-tools && cd media-tools && npm i ffmpeg-static
FF=$PWD/node_modules/ffmpeg-static/ffmpeg
```

Бинарник — 45 568 216 байт (`ls -l "$FF"`, 27.10.2026). В зависимости проекта его
не ставят, иначе CI качал бы его на каждой сборке. Путь передаётся переменной
`FFMPEG_PATH`. В сборке есть всё нужное ниже — проверено `"$FF" -filters`
и `"$FF" -encoders`: `drawtext`, `boxblur`, `drawbox`, `overlay`, `trim`, `concat`,
`tile`, `showinfo`, кодеки `libx264` и `libwebp`.

## Проверка: киноленты с меткой времени

Киноленты — один PNG с сеткой кадров, на каждом кадре метка времени. Смотреть
сразу всё и сравнивать соседние кадры удобно.

```bash
# вся запись: кадр раз в 4 с, сетка 3×5
"$FF" -i rec.mov -vf "fps=1/4,scale=700:-1,drawtext=text='%{pts\:hms}':x=8:y=8:fontsize=28:fontcolor=white:box=1:boxcolor=black@0.6,tile=3x5:padding=6" -frames:v 1 sheet.png

# подозрительный отрезок 25–41 с: кадр в секунду
"$FF" -ss 25 -t 16 -i rec.mov -copyts -vf "fps=1,scale=700:-1,drawtext=text='%{pts\:flt}':x=4:y=4:fontsize=20:fontcolor=white:box=1:boxcolor=red@0.8,tile=4x4:padding=6" -frames:v 1 sheet-25.png

# поле с данными крупно: 4 кадра в секунду, вырезан только его прямоугольник
"$FF" -ss 29 -t 15 -i rec.mov -copyts -vf "fps=4,crop=700:620:1400:120,scale=350:-1,drawtext=text='%{pts\:flt}':x=4:y=4:fontsize=18:fontcolor=white:box=1:boxcolor=red@0.8,tile=10x6:padding=4" -frames:v 1 strip.png

# переход покадрово: без fps берутся все кадры отрезка
"$FF" -ss 36 -t 2.6 -i rec.mov -copyts -vf "crop=2240:300:700:330,scale=1120:-1,drawtext=text='%{pts\:flt}':x=4:y=4:fontsize=20:fontcolor=white:box=1:boxcolor=red@0.8,tile=2x13:padding=4" -frames:v 1 trans.png
```

Тонкости:

- **`-ss` перед `-i` обнуляет время.** Метка `%{pts}` тогда считается от точки
  перемотки, а не от начала записи. Проверка через `showinfo`: при `-ss 5` первый
  кадр — `pts_time:0`, с `-copyts` — `pts_time:5`. Без `-copyts` секунды
  с кинолент придётся пересчитывать в уме — и ошибиться ровно в той таблице,
  по которой потом режут.
- **Сетка вмещает все кадры отрезка:** кадров = длительность × fps. 15 с × 4 =
  60 = 10×6. С `-frames:v 1` всё, что не влезло, молча пропадает.
- **Пиксели — исходника.** На Retina запись вдвое больше CSS-пикселей (у записи
  LEYKA — 2940×1844). Координаты поля снимают с кинолент без `scale` или
  пересчитывают на коэффициент масштаба.

## Чистка: граф фильтров

Порядок: **сначала размытие, потом отрезки, потом обрезка кадра**. Тогда и секунды,
и пиксели в параметрах — исходника: их берут прямо с кинолент.

```
[0:v]split[base][patch];
[patch]crop=W:H:X:Y,boxblur=R:3:R/2:3[blurred];
[base][blurred]overlay=X:Y:enable='between(t,A,B)+between(t,C,D)'[masked];
[masked]split=2[k0][k1];
[k0]trim=start=S0:end=E0,setpts=PTS-STARTPTS[p0];
[k1]trim=start=S1:end=E1,setpts=PTS-STARTPTS[p1];
[p0][p1]concat=n=2:v=1:a=0[joined];
[joined]crop=CW:CH:CX:CY,scale=960:-2,fps=30[out]
```

Кодирование для сайта:

```bash
"$FF" -i rec.mov -filter_complex "<граф>" -map "[out]" -c:v libx264 -preset slow -crf 30 -profile:v main -pix_fmt yuv420p -an -movflags +faststart out.mp4
```

Тонкости:

- **Окна размытия складываются через `+`:** `enable='between(t,30.3,36.45)+between(t,38,43.6)'`.
- **Радиус `boxblur` — не больше половины меньшей стороны прямоугольника, у цвета
  в yuv420 — вдвое меньше.** Иначе ffmpeg не запускается: на кадре 64×32 —
  «Invalid luma_param radius value 20, must be >= 0 and <= 16», для цвета предел 8.
  В portfolio: `R = min(16, floor(min(w,h)/2) − 1)`, у цвета `R/2`, три прохода.
- **`setpts=PTS-STARTPTS` после каждого `trim` обязателен.** Проверка на тестовом
  ролике: отрезки 2–4 и 6–8 с без него встали на 2,0–3,9 и 10,1–12,0 с — в начале
  и между кусками паузы. С ним — сплошные 0–3,9 с.
- **`-t` при `trim` не нужен:** длительность задают отрезки.
- **`scale=960:-2`** держит высоту чётной, как требует yuv420p; **`fps=30`** после
  склейки выравнивает частоту.
- **`-an` — без звука.** В записи экрана бывает микрофон и звуки уведомлений;
  для витрины звук не нужен и тоже несёт чужое.
- **Размытие — не стирание.** Для пароля, ключа, номера документа —
  `drawbox=x=…:y=…:w=…:h=…:color=black:t=fill` или вырезка отрезка.

Готовая обёртка — `scripts/encode-video.mjs` в открытом репозитории
https://github.com/Barbashin1970/portfolio. Флаги `--crop W:H:X:Y`, `--crop-top N`,
`--keep A-B,C-D`, `--blur X:Y:W:H@A-B,C-D`, `--portrait` собирают этот граф
и пишут `public/media/<slug>.mp4`.

## Проверка результата

Те же киноленты, но по готовому файлу и **в его координатах**: после обрезки
и масштаба поле стоит в другом месте.

```bash
# зона размытия по всему ролику, 4 кадра в секунду
"$FF" -i out.mp4 -vf "fps=4,crop=330:90:520:120,scale=495:-1,drawtext=text='%{pts\:flt}':x=3:y=3:fontsize=16:fontcolor=white:box=1:boxcolor=red@0.8,tile=4x12:padding=4" -frames:v 1 check-blur.png

# стык покадрово: 0,45 с вокруг склейки, все кадры
"$FF" -ss 3.1 -t 0.45 -i out.mp4 -copyts -vf "crop=330:90:520:120,drawtext=text='%{pts\:flt}':x=3:y=3:fontsize=14:fontcolor=white:box=1:boxcolor=red@0.8,tile=4x4:padding=3" -frames:v 1 edge-cut.png

# несколько стыков — одной картинкой
"$FF" -i edge-cut.png -i edge-open.png -i edge-close.png -filter_complex "[0][1][2]vstack=inputs=3" edges.png
```

## Скриншот того же экрана

```bash
"$FF" -i shot.png -vf "crop=2560:1600:0:74,scale=960:600:flags=lanczos" -c:v libwebp -quality 72 shot.webp
```

`crop` оставляет окно приложения без полосы окна браузера: снимок с Retina
2560×1600 — это кадр 1280×800 CSS px.

## Связанное

- Урок, ради которого всё это: [screen-recording-of-live-system-leaks-third-party-data](../lessons/screen-recording-of-live-system-leaks-third-party-data.md).
- Проект: [portfolio](../../projects/active/portfolio/README.md); команды
  с аргументами — `~/portfolio/docs/INVENTORY.md`, «Медиа карточек».
