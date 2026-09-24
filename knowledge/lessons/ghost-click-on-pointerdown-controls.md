---
title: «Призрачный» клик на Android после управления по pointerdown
description: Действие на pointerdown + смена экрана/оверлея → Android досылает click по новому элементу и закрывает/пролистывает его. iOS Safari — нет.
tags: [lesson, bug, stack/react]
severity: high
project_origin: "fiveletters"
created: 2026-09-08
updated: 2026-09-08
---

# Урок: «призрачный» клик на Android после управления по pointerdown

## Что произошло

В дуэли «5 букв» на Android Chrome (на iPhone Safari — всё нормально):
при отгадывании модалка «нет такого слова» мгновенно закрывалась (мигала), а
после угаданного слова экран результата с попытками проскакивал сразу к экрану
загадывания. У загадывающего то же окно работало нормально.

## Почему это случилось

Экранная клавиатура срабатывает по `pointerdown` (ради отзывчивости). Android
после `pointerdown` **досылает синтетический `click`** — но целью выбирается
элемент, оказавшийся под пальцем уже ПОСЛЕ ре-рендера: фон открывшейся модалки
(→ dismiss) или кнопка на новом экране (→ переход). iOS Safari этот click гасит
`preventDefault` на `pointerdown`, Android — нет. Разгадка «у одного игрока
работает, у другого нет»: у загадывающего submit висел на кнопке-`onClick`
(призрака нет), у отгадывающего — только на клавише ✓ (`pointerdown` → призрак).

## Как починили

`utils/ghost-click.ts`: `markInputTap()` пишет время тапа по клавише (зовётся из
`Keyboard` на `pointerdown`); `installGhostClickGuard()` вешает **capture**-пере\-
хватчик `document` `click` и глотает click в окне 500 мс после тапа. Клавиатура
на `click` не завязана, поэтому её «родные» клики глотать безопасно. Гард ставится
в `App` (переживает смену экрана). FIVELETTERS, коммит `6bf7ca0`.

## Чтобы не повторилось

- **Вешаешь действие на `pointerdown`/`touchstart`, и оно меняет экран или
  открывает оверлей → жди призрачный `click` на Android.** Либо гаси его (guard),
  либо вешай действие на `click`.
- Закрытие модалок и переходы тестировать **именно на Android Chrome**, не только
  на iOS Safari — поведение расходится.
- Тот же `pointerdown`-паттерн клавиатуры есть в GONKA/KNIFFEL/SMART-DUEL — при
  мобильном тесте проверить там же. Для площадок (Яндекс Игры) это блокер: почти
  вся аудитория на мобилках.

## Связанное

- Skill: [publish-yandex-games](../skills/publish-yandex-games.md) — мобильная площадка.
- Похожий урок: [sound-autoresume-defeats-pause-mute](sound-autoresume-defeats-pause-mute.md) — тоже общий код `sound.ts`/UI в FIVELETTERS→SMART-DUEL.
- Проект: `~/FIVELETTERS` (фикс), `~/SMART-DUEL` (перенести, см. BACKLOG M0).
