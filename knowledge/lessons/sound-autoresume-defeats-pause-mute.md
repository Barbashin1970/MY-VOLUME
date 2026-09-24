---
title: Авто-resume в playSound сводит на нет глушение звука на паузе
description: Web Audio разблокировка (ctx.resume) и намеренное ctx.suspend борются — нужен явный флаг-замок, который playSound уважает.
tags: [lesson, bug, stack/react, stack/pwa]
severity: medium
project_origin: "smart-duel"
created: 2026-08-03
updated: 2026-08-03
---

# Урок: авто-resume в playSound сводит на нет глушение звука на паузе

## Что произошло

В сборке под Яндекс Игры звук обязан молчать во время рекламы (событие
`game_api_pause`) — иначе отказ модерации. Повесили `suspendAudio()`
(`AudioContext.suspend()`) на паузу, ревью-агент отчитался «звук глушится».
Но звук всё равно прорывался: первый же игровой `playSound()` снова включал
контекст, и реклама шла со звуком.

## Почему это случилось

`playSound()` в начале делает `if (ctx.state === 'suspended') ctx.resume()` —
это нужно для разблокировки аудио после первого жеста пользователя (браузеры
не дают звучать до жеста). Но ровно это же **будит контекст, приглушённый
платформенной паузой**. Игровые события во время рекламы не останавливаются,
поэтому любой их звук мгновенно снимал `suspend`. `suspend()` без «замка»
бесполезен: `ctx.state` — не защёлка, а гонка между resume и suspend.

## Как починили

Явный флаг состояния «глушить», который `playSound` уважает:

```ts
let platformPaused = false;
export function suspendAudio() { platformPaused = true;  ctx?.suspend?.(); /* + pause() всех HTMLAudio */ }
export function resumeAudio()  { platformPaused = false; ctx?.resume?.();  }   // только жест / game_api_resume
export function playSound(n) { if (muted || platformPaused) return; /* … resume-разблокировка … */ }
```

Плюс глушение на `visibilitychange` (скрытая вкладка — не только реклама).
Файл: `~/SMART-DUEL/src/utils/sound.ts`.

## Чтобы не повторилось

- **Разблокировка (`ctx.resume` в playSound) и намеренное глушение (`ctx.suspend`)
  — противоборствующие силы.** Нужен явный флаг-«замок» состояния «пауза»,
  который проверяет `playSound`; нельзя полагаться только на `ctx.state`.
- Тот же паттерн `sound.ts` живёт в **GONKA и KNIFFEL** — при портировании любой
  на Яндекс/рекламу починить это ПЕРВЫМ.
- Код-ревью чеклист: «звук глушится на паузе» проверять не по факту вызова
  `suspend()`, а сценарием **пауза → вызвать playSound → должна быть тишина**.
  Авто-проверка, смотрящая только «вызывается ли suspend», даёт ложное «ОК».

## Связанное

- Skill: [publish-yandex-games](../skills/publish-yandex-games.md) — там же остальные грабли модерации.
- Проект: `~/SMART-DUEL` (форк `~/FIVELETTERS`), общий `utils/sound.ts` c GONKA/KNIFFEL.
