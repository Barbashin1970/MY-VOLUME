---
title: Проверка вёрстки в headless Chrome — эмуляция экрана по DevTools вместо окна и виртуального времени
description: "Headless Chrome на macOS не сужает окно меньше 500 px, а --virtual-time-budget замораживает CSS-анимации и requestAnimationFrame: интро «не кончалось», анимированные блоки «не видны». Рабочий путь — протокол DevTools через WebSocket, встроенный в Node 22: Emulation.setDeviceMetricsOverride, Emulation.setEmulatedMedia для reduced-motion, ожидание в реальном времени, Page.captureScreenshot. Твины GSAP не видны в document.getAnimations() — ждать по признаку. На программном рендере (SwiftShader) кадры замирают на 1–4 с при свободном главном потоке: конец прокрутки определять по состоянию прокрутки (класс lenis-scrolling), а не по тишине, видимость — сценарием, а не по скриншоту."
tags: [tool, chrome, testing, layout, devtools-protocol]
created: 2026-10-27
updated: 2026-10-27
related:
  - knowledge/lessons/nbsp-and-overflow-hidden-hide-heading-overflow.md
  - knowledge/lessons/lenis-scrollto-double-offset-and-stale-limit.md
  - knowledge/lessons/sr-only-escapes-scroller-without-positioned-ancestor.md
---

# Проверка вёрстки в headless Chrome: эмуляция экрана по DevTools

## Когда нужно

Проверить вёрстку без человека и без Playwright: ширина телефона 320–375 px,
первый экран, режим reduced-motion, анимации, которые должны доиграть, и
скриншоты для отчёта.

## Две ловушки простого пути

**1. Окно не уже 500 px.** `--window-size=375,667` на macOS даёт окно шириной
500. Обход через iframe нужной ширины работает, но с условием: страница пробы
и сайт должны отдаваться **одним сервером**. Iframe с другого порта — чужое
происхождение, мерить его нельзя.

**2. `--virtual-time-budget` замораживает анимации.** С `--dump-dom` и бюджетом
20 000 мс статичная вёрстка мерилась верно, а анимированная — нет
(portfolio, 27.10.2026):

- на 320 и 375 px блоки с CSS-анимацией появления — «не видны»: прозрачность
  осталась на начальном кадре;
- на 1440 px таймлайн GSAP не дошёл до конца — класс `intro` остался, в H1 —
  42 разрезанные буквы, живой фон не поднялся.

Это ложные красные — и такие же ложные зелёные, если проверять только статичное.

## Рабочий путь: протокол DevTools из Node 22

WebSocket в Node 22 встроен — ни Puppeteer, ни Playwright не нужны.

1. **Запуск:** `--headless=new --remote-debugging-port=0 --user-data-dir=<временная>`;
   адрес `ws://…` читается из stderr — строка `DevTools listening on`.
2. **Вкладка:** `Target.createTarget` → `Target.attachToTarget` с `flatten: true`.
3. **Экран:** `Emulation.setDeviceMetricsOverride` с `width`, `height`,
   `deviceScaleFactor`, `mobile`. 320×568 и 375×667 — честные, без iframe.
4. **Reduced-motion:** `Emulation.setEmulatedMedia` с
   `{ name: 'prefers-reduced-motion', value: 'reduce' }`.
5. **Ожидание в настоящем времени:** опрос `Runtime.evaluate` раз в 200 мс до признака
   «страница успокоилась» с потолком 15 с.
6. **Замеры** — тем же `Runtime.evaluate`; **скриншот** — `Page.captureScreenshot`.
7. **WebGL в headless:** `--use-angle=swiftshader --enable-unsafe-swiftshader` —
   WebGL2 поднимается, живой фон рисуется.

Пять экранов (320, 375, 1440 и два с reduced-motion) с ожиданием интро — около
18 секунд.

## Как понять, что страница успокоилась

- `document.readyState === 'complete'` и `document.fonts.status === 'loaded'`;
- конечных анимаций в `document.getAnimations()` не осталось (бесконечные —
  вроде подсказки прокрутки — не ждать);
- **твины GSAP в `getAnimations()` не видны:** они пишут инлайн-стили каждый кадр.
  Ждать их по признаку результата — например, `visibility: hidden` у слоя после
  `autoAlpha: 0`. Без этого первый скриншот поймал статичный SVG посреди затухания;
- **прокрутка кончилась** — по признаку самой прокрутки, а не по тишине: следующий раздел.

## Третья ловушка: кадры замирают на программном рендере

С `--use-angle=swiftshader` кадры **замирают на 1–4 с посреди прокрутки**, хотя
главный поток свободен: длинных задач нет (portfolio, 27.10.2026). Картина плавает
от прогона к прогону. Четыре прогона подряд — обычный, без картинок, без размытия
и снова обычный — дали четыре разные картины, а второй обычный прошёл без единой
паузы. Значит, дело не в странице.

Что это ломает:

- **Условие «ничего не двигается 600 мс — прокрутка кончилась»** срабатывает
  на паузе посреди анимации. Замер берётся с полпути, и переход по якорю выглядит
  промахом, которого на сайте нет.
- **Скриншот ловит секции до появления.** На телефонных снимках секции игр
  и контакта выходили пустыми, хотя на сайте всё было на месте.

Как ждать: по признаку самого приложения. Lenis, пока анимирует, держит
на `<html>` класс `lenis-scrolling`:

```js
// Конец прокрутки: класса lenis-scrolling нет и scrollY стоит 6 опросов подряд.
let last = -1, still = 0;
for (let i = 0; i < 150 && still < 6; i++) {          // потолок — 15 с
  await sleep(100);
  const moving = document.documentElement.classList.contains('lenis-scrolling');
  still = !moving && Math.abs(scrollY - last) < 0.5 ? still + 1 : 0;
  last = scrollY;
}
```

После этой правки прогон прошёл трижды подряд без ошибок.

Правила:

- **Ждать признак приложения, а не тишину и не таймер:** класс сглаживания,
  `aria-busy`, атрибут состояния. Тишина на программном рендере врёт.
- **Видимость проверять сценарием, а не глазами по скриншоту.** В portfolio так:
  каждый `[data-reveal]` выше низа экрана должен получить `visibility: visible`
  и `opacity > 0.9`, ожидание до 5 с. Скриншот — иллюстрация для человека.
  Пустой кадр сначала проверяют сценарием.
- **Перенос в CI — отдельная проверка.** На раннере ubuntu тот же рендер
  программный, и как он себя ведёт, ещё не испытано (portfolio, бэклог Б-22).

Почему у сайта промах при переходе по якорю бывал и настоящим — две причины
в самой библиотеке прокрутки:
[lenis-scrollto-double-offset-and-stale-limit](../lessons/lenis-scrollto-double-offset-and-stale-limit.md).

## Что ещё умеет тот же канал

- **Настоящая прокрутка:** `Input.synthesizeScrollGesture` с `gestureSourceType:
  'mouse'` или `'touch'`. Так проверили жалобу «сайт не листается»: колесо и палец
  листали ровно на высоту содержимого — 49 px на 1440×900, 86 px на 375×667. Поломки
  не было, под первым экраном просто не было содержимого.
- **Живой адрес вместо локальной сборки** — тот же набор проверок по URL сайта.

## Чем подтвердили, что проверка не врёт

Мутациями в portfolio: вернули старый размер H1 — 6 расхождений; вернули кнопку
на несуществующий якорь `#featured` — сторож назвал его на всех экранах.

## Где реализация

- `~/portfolio/scripts/lib/chrome.mjs` — запуск Chrome, вкладка с эмуляцией,
  `until`, `screenshot`, сырой `call` для любого метода протокола.
- `~/portfolio/scripts/check-layout.mjs` — экраны, признак «успокоилась», замеры
  и ожидания; ключи `--shots DIR`, `--url`, `--only`.

## Связанное

- Урок, ради которого меряется сам заголовок:
  [nbsp-and-overflow-hidden-hide-heading-overflow](../lessons/nbsp-and-overflow-hidden-hide-heading-overflow.md).
- Почему ширину страницы меряют в каждом режиме, а не только на десктопе:
  [sr-only-escapes-scroller-without-positioned-ancestor](../lessons/sr-only-escapes-scroller-without-positioned-ancestor.md).
- Проект: [portfolio](../../projects/active/portfolio/README.md).
