---
title: GONKA
description: PWA-гонка на фишках (Parchisi/Ludo-like). Эталон серии SKILL-01 … SKILL-09 для игровых проектов.
tags: [project, stack/pwa-vite-react19, status/active, category/B]
status: active
category: B
repo: ~/GONKA
created: 2026-04-01
updated: 2026-05-26
---

# GONKA

> **Где код:** `~/GONKA/`
> **Что это:** PWA-гонка на фишках (Parchisi/Ludo-like) для 2-4 игроков.
> Вариативные правила через AdminPanel (3 пресета: GONKA, PARCHEESI, LUDO).
> v1.0 — локально + AI-боты. v2.0 — мультиплеер через Railway + Socket.io.

## Зачем

Pet-проект. Образец нарезки игрового продукта через SKILL-NN серию.

## Стек

| Слой | Технология |
|------|-----------|
| Сборка | Vite 6 |
| Язык | TypeScript strict |
| UI | React 19 |
| Рендер игры | SVG inline |
| State | Zustand (4 store) |
| Персистентность | localStorage |
| i18n | i18next (RU + EN) |
| PWA | vite-plugin-pwa |
| Тесты | Vitest (55 тестов) |
| Звук | Web Audio API (13 процедурных звуков) |
| Жесты | MediaPipe Vision |
| Сеть | Socket.io-client + Node.js Socket.io сервер |

## Документация (в `~/GONKA/`)

| Документ | Стадия ЖЦ |
|----------|-----------|
| `CLAUDE.md` | мета — контекст для AI-ассистента (tech stack, файловая структура) |
| `ARC.md` | 5 — архитектура (game loop, state machine, AI, network) |
| `README.md` | 8 — обзор |
| `SKILL-01-idea.md` | 6 — описание продукта, user stories, доменная модель |
| `SKILL-02-setup.md` | 6 — Vite, CI, базовые папки |
| `SKILL-03-core.md` | 6 — движок игры (movement, dice, capture, blockade) |
| `SKILL-04-ui.md` | 6 — UI-каркас, экраны, маршрутизация |
| `SKILL-05-polish.md` | 6 — анимации, звуки, i18n |
| `SKILL-06-ai-player.md` | 6 — AI-боты, 3 уровня сложности, evaluator |
| `SKILL-07-simulator.md` | 6 — headless-симулятор партий (~50мс) |
| `SKILL-08-multiplayer.md` | 6 — Socket.io, лобби, reconnect |
| `SKILL-09-demo-mode.md` | 6 — демо-режим / витрина продукта |

## Главные ценности GONKA для других проектов

1. **Эталон серии SKILL-01 … SKILL-09**. 9 файлов, каждый = один шаг.
   Идеальная карта реализации игры. См. [06-skill-series](../../../docs/playbooks/06-skill-series.md).

2. **Стек PWA-Vite-React19** — переиспользуется в `SOFIA` и `KNIFFEL`.
   Стек для категории B (pet/learning).

3. **`SKILL-06-ai-player.md`** — паттерн AI-бота через скоринг ходов
   (10 факторов в bot-evaluator) и difficulty-веса. Переносимый
   паттерн для игр.

4. **`SKILL-07-simulator.md`** — **headless-game** за ~50мс. Bot vs Bot
   гонки тысячами за минуту, для отладки AI и тестов.

5. **`SKILL-09-demo-mode.md`** — финальная стадия любого продукта:
   витрина. У GONKA — bot vs bot race, виртуальная валюта G, ставки.

## Связанное

- Плейбук: [06-skill-series](../../../docs/playbooks/06-skill-series.md) — GONKA как главный эталон
- Близкие проекты: [sofia](../sofia/), [kniffel](../kniffel/) — переиспользуют SKILL-структуру
