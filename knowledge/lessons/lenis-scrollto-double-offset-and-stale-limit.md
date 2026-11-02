---
title: Lenis 1.3 — scrollTo сам вычитает scroll-margin-top, а предел прокрутки устаревает после смены вёрстки
description: "В portfolio переход к «Контакту» промахивался по двум независимым причинам. Первая — двойной отступ: Lenis 1.3 в scrollTo(элемент) сам вычитает scroll-margin-top цели и scroll-padding-top контейнера, а по плану добавлялся ещё offset под шапку — 128 px вместо 64. Вторая — устаревший предел: Lenis пересчитывает размеры по ResizeObserver с задержкой 250 мс и обрезает цель по старому limit (19 558 px при настоящих 21 355 после смены фильтра). Лечение — без offset, lenis.resize() перед переходом и на каждый ScrollTrigger.refresh(). Проверять переход к нижней секции."
tags: [lesson, gotcha, scroll, lenis, gsap, stack/react]
severity: medium
project_origin: portfolio
created: 2026-10-27
updated: 2026-10-27
---

# Урок: Lenis 1.3 — двойной отступ и устаревший предел прокрутки

## Что произошло

27.10.2026, фаза 2 сайта-портфолио: Lenis 1.3.27 — единственное сглаживание,
GSAP ScrollTrigger, закреплённая шапка, пин ленты избранного, фильтр каталога.
Ссылка «Контакт» в шапке должна ставить секцию ровно под шапку. Сторож вёрстки
мерит это в Chrome: верх секции минус высота шапки, допуск ±8 px.

Переход промахивался, причём по-разному: то секция вставала ниже шапки на её
высоту, то прокрутка останавливалась, не доехав до «Контакта».

## Почему это случилось

Две независимые причины. Правка одной не закрывает другую.

**1. Двойной отступ под шапку.** У секций стоит `scroll-margin-top: var(--header-h)` —
та же переменная, которой задана высота шапки, так что числа «на глаз» нет
(урок [accordion-scroll-lands-on-wrong-section](accordion-scroll-lands-on-wrong-section.md)).
План по брифу добавлял ещё `lenis.scrollTo(target, { offset: -headerHeight })`.
А Lenis 1.3 в `scrollTo(элемент)` **сам** вычитает отступы — так в исходнике
(`node_modules/lenis/dist/lenis.mjs`):

```js
target = rect.top + this.animatedScroll
       - scrollMargin    // getComputedStyle(цель).scrollMarginTop
       - scrollPadding   // getComputedStyle(контейнер).scrollPaddingTop
target += adjustedOffset // offset из параметров — сверху
target = clamp(0, target, this.limit)
```

Замер на 375×667: секция вставала на 128 px от верха вместо 64.

**2. Устаревший предел прокрутки.** Размеры Lenis пересчитывает сам:
`ResizeObserver` на контент и окно, но через `debounce` 250 мс, и каждое новое
изменение откладывает пересчёт. А `ScrollTrigger.refresh()` и смена фильтра
меняют длину страницы сразу. Переход, начатый в этом окне, обрезается по старому
`limit` — последняя строка выше. Замер на 320×568 после смены фильтра каталога:
предел 19 558 px при настоящих 21 355. «Контакт» внизу страницы — цель за старым
пределом, и прокрутка упиралась в него.

На верхних секциях не видно ни той, ни другой ошибки. Вторая проявляется только
у цели за старым пределом, то есть внизу страницы. Первая видна везде, но
на 64 px её легко принять за «так задумано».

## Как починили

`~/portfolio/src/motion/anchors.ts` и `~/portfolio/src/motion/SmoothScroll.tsx`:

```ts
// Переход: сначала позиции, потом размеры Lenis, потом прокрутка. Без offset.
ScrollTrigger.refresh();
lenis.resize();
lenis.scrollTo(target);

// Любой пересчёт ScrollTrigger (шрифты, фильтр, пин) — и Lenis пересчитывает длину.
ScrollTrigger.addEventListener('refresh', () => lenis.resize());
```

Без Lenis (reduced-motion) тот же переход делает
`target.scrollIntoView({ block: 'start' })` — он тоже учитывает `scroll-margin-top`.
Поэтому отступ задан один раз, в CSS, и верен в обоих режимах.

## Чтобы не повторилось

- **Отступ под шапку — один раз, в CSS** (`scroll-margin-top` у цели или
  `scroll-padding-top` у корня). Библиотеку прокрутки сначала проверить: вычитает ли
  она его сама. Lenis 1.3 вычитает — `offset` поверх даёт двойной.
- **Перед программной прокруткой — свежие размеры.** Всё, что меняет длину
  страницы (фильтр, пин, раскрытие, шрифты), — затем `ScrollTrigger.refresh()`
  и `lenis.resize()`, и только потом `scrollTo`. Подписать `lenis.resize()`
  на событие `refresh` ScrollTrigger, чтобы не помнить об этом в каждом месте.
- **Проба — переход к нижней секции и после смены вёрстки:** нажать фильтр,
  затем «Контакт». Верхняя секция ошибку предела не покажет никогда.
- **Конец прокрутки определять по состоянию прокрутки, а не по тишине.** Пока
  Lenis анимирует, на `<html>` висит класс `lenis-scrolling`. В headless Chrome
  кадры ещё и замирают — см.
  [headless-chrome-layout-checks-via-devtools](../tools/headless-chrome-layout-checks-via-devtools.md).

## Связанное

- Проект: [portfolio](../../projects/active/portfolio/README.md); разбор —
  `~/portfolio/docs/FAZA-2-RAZBOR.md`, «Найдено по ходу».
- Родственный урок без библиотеки, три причины промаха:
  [accordion-scroll-lands-on-wrong-section](accordion-scroll-lands-on-wrong-section.md).
