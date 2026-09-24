---
title: Публикация HTML5-игры на Яндекс Играх
description: Право на публикацию, обязательный SDK, правки Vite-сборки и чек-лист модерации для статичной PWA.
tags: [skill, stack/pwa, stack/vite-react]
origin: "projects/active/gonka"
created: 2026-08-03
updated: 2026-08-03
---

# Skill: Публикация HTML5-игры на Яндекс Играх

## Когда применять

Есть готовая статичная браузерная игра (Vite + React, всё в localStorage, деплой
на Vercel) и нужен **живой трафик и обратная связь** без своего маркетинга.
Подходит для pet-проектов категории B (GONKA, KNIFFEL, SMART/5-букв).

## Как применять

**Право на публикацию.** Любой Yandex ID (доапгрейдить простую регистрацию до
полноценного `@yandex.ru`). Физлицу — можно, юрлицо/ИП/самозанятость не нужны.
**Бесплатно без рекламы — разрешено** (в «комментарии разработчику» модератору
написать «рекламы нет by design»). Самозанятость нужна только для выплат при рекламе.

**Обязательный SDK** (без него не примут). В `index.html`:

```html
<script src="https://yandex.com/games/sdk/v2" async></script>
```

```ts
// src/yandex/ysdk.ts — синглтон, с фолбэком для локальной разработки
type YSDK = any;
let ysdk: YSDK | null = null;

export async function initYandex(): Promise<YSDK | null> {
  const YaGames = (window as any).YaGames;
  if (!YaGames) return null;                 // локально/вне iframe — молча пропускаем
  ysdk = await YaGames.init();
  return ysdk;
}
export function yaReady(): void {            // вызвать по РЕАЛЬНОЙ готовности UI
  ysdk?.features?.LoadingAPI?.ready?.();     // не по таймеру — иначе отказ модерации
}
export function yaLang(): 'ru' | 'en' | null {
  const l = ysdk?.environment?.i18n?.lang;   // ставить i18next + язык слов/раскладку
  return l ? (String(l).startsWith('en') ? 'en' : 'ru') : null;
}
export function onYaPause(mute: () => void, resume: () => void): void {
  ysdk?.on?.('game_api_pause', mute);        // глушить звук/таймеры (иначе звук в рекламе → отказ)
  ysdk?.on?.('game_api_resume', resume);
}
```

**Правки сборки Vite (критично):**
- `base: './'` — иначе абсолютные `/assets/...` ломаются в архиве (игру рехостят на
  многих доменах, абсолютные пути мертвы).
- **Отключить service worker** для Яндекс-сборки (PWA-кэш в их iframe конфликтует):
  флаг `VITE_TARGET=yandex` → пропустить `vite-plugin-pwa`. Полный PWA — только Vercel.

**Архив:** ZIP, `index.html` в **корне**, **≤ 100 МБ**, имена файлов **без пробелов
и кириллицы** (хешированные имена Vite безопасны; проверь руками добавленные ассеты).

**Чек-лист перед модерацией:** SDK + `ready()` по факту · авто-язык из `i18n.lang` ·
звук глушится на паузе/скрытой вкладке · нет остатков «test/debug/coming soon» ·
жанр в черновике = реальный · возрастной рейтинг · оба языка переведены полностью ·
контекстное меню погашено (`onContextMenu preventDefault`) · UI не обрезан по краям.
Модерация — **3–5 рабочих дней**.

**Разведка ниши:** каталог — SPA (счётчики не парсятся); ищи по
`games.yandex.ru` запросами вида «5 букв», «Wordle», «Виселица», «на двоих» —
оценивай насыщенность и чего НЕТ (уникальный угол важнее массового жанра).

## Почему именно так

- SDK и `base:'./'` — жёсткие требования площадки, всё остальное вторично.
- Публиковать **уникальный режим, а не массовый**: соло-Wordle/виселиц там сотни,
  а адверсариальная дуэль-хот-сит и двуязычие RU+EN — пустые ниши.
- Дробить мульти-режимную игру на **отдельные листинги** — три поверхности поиска
  и чистый матч «жанр ↔ описание» (иначе частый отказ «не соответствует жанру»).

## Известные подводные камни

- **Звук во время рекламы/паузы** — топ-причина отказа: вешать mute на
  `game_api_pause` и `visibilitychange`. НО `AudioContext.suspend()` сам по себе
  не спасает: `playSound` с авто-`resume()` тут же разбудит контекст. Нужен
  явный флаг-«замок», который `playSound` уважает —
  см. [lesson: sound-autoresume-defeats-pause-mute](../lessons/sound-autoresume-defeats-pause-mute.md).
- **`LoadingAPI.ready()` по таймеру** вместо реальной готовности — отказ.
- **Кириллица/пробелы в именах файлов** в архиве — отказ.
- Право **не-резидента РФ/Беларуси** под бесплатную игру — договоры про это молчат
  (не подтверждено).
- Отключение SW — best practice из iframe-поломок, не дословный запрет; проверять.

## Откуда взято

- Проект: `~/FIVELETTERS/docs/yandex-games-research.md` (полный отчёт + источники).
- Офиц. доки: `yandex.com/dev/games/doc/en/{concepts/requirements,concepts/moderation,sdk/sdk-about}`.
- Связанные: звук — [sound-manager-web-audio](../skills/) (паттерн GONKA/KNIFFEL),
  стек — [knowledge/stacks/](../stacks/).
