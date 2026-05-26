---
title: NSK_OpenData_Bot
description: Бот по открытым данным Новосибирска. Эталон IQ-roadmap как модели роста системы.
tags: [project, stack/python-bot, status/active, category/C]
status: active
category: C
repo: ~/NSK_OpenData_Bot
created: 2026-02-15
updated: 2026-05-26
related:
  - concepts/iq-roadmap.md
---

# NSK_OpenData_Bot

> **Где код:** `~/NSK_OpenData_Bot/`
> **Что это:** агрегатор открытых данных Новосибирска. Бот + Studio UI.

## Зачем

Демонстрация работы с **открытыми данными мегаполиса** — opendata.novo-sibirsk.ru,
data.gov.ru, openbudget.mfnso.ru, Яндекс.Расписания, 2GIS, ГИБДД.
Цель — пройти по **IQ-индексу городов Минстроя** от ~35 до ~70 баллов.

## Стек

| Слой | Технология |
|------|-----------|
| Backend | Python (без явного фреймворка — модульный роутер/планировщик/исполнитель) |
| Storage | YAML datasets + статические Python-data модули |
| Frontend | Static HTML (`src/static/studio.html`) — 3-вкладочный редактор YAML-регламентов |
| Внешние API | Яндекс.Расписания, 2GIS Traffic, opendata.novo-sibirsk.ru, openbudget |

## Документация (в `~/NSK_OpenData_Bot/`)

| Документ | Стадия ЖЦ |
|----------|-----------|
| `README.md` | 8 |
| `ARC.md` | 5 — архитектура (роутер + планировщик + исполнитель) |
| `BACKLOG.md` | 9 |
| `SIGMA_BACKLOG.md` | 9 — отдельный бэклог по Sigma-методологии (аудит) |
| **`IQ-roadmap.md`** | 9 — **эталон IQ-roadmap паттерна** |

## Главные ценности NSK для других проектов

1. **`IQ-roadmap.md` — эталон модели роста**. См. [iq-roadmap](../../../concepts/iq-roadmap.md)
   и [09-charter-reports-growth §C](../../../docs/playbooks/09-charter-reports-growth.md).
   Внешняя метрика (IQ-индекс Минстроя), 14 направлений, 5 фаз, прогноз
   прироста по фазам.

2. **`Studio editor`** (`src/static/studio.html` + `src/routes/admin.py`)
   — 3-вкладочный редактор YAML-регламентов (YAML source / Tree / Quick
   sliders). RAGRAF позаимствовал паттерн для редактора регламентов в
   Turtle. См. RAGRAF `skills.md` § «Three-view regulation editor».

3. **Шаблон расширения системы** — 6-шаговый quick reference «как
   добавить новый топик» (datasets → router → planner → executor → API
   → render). Паттерн для расширяемых агрегаторов.

4. **`SIGMA_BACKLOG.md` параллельно с `BACKLOG.md`** — паттерн «два
   измерения работы»: продуктовое (BACKLOG) и методологическое (SIGMA).

## Связанное

- Концепт: [iq-roadmap](../../../concepts/iq-roadmap.md)
- Плейбук: [09-charter-reports-growth](../../../docs/playbooks/09-charter-reports-growth.md) §C
- Использован как референс в: [ragraf](../ragraf/) (Studio editor)
