---
title: LEYKA
description: Корпоративный таск-трекер ЦИИ НГУ. FastAPI + React + SQLite. Эталон стека и документации.
tags: [project, stack/fastapi-react, status/active, category/A]
status: active
category: A
repo: ~/LEYKA
deployed: production (Vercel + Railway)
created: 2026-04-15
updated: 2026-10-26
related:
  - docs/decisions/adr-001-default-stack.md
  - docs/decisions/adr-002-doc-set-per-project.md
---

# LEYKA

> **Где код:** `~/LEYKA/`
> **Что это:** корпоративный таск-трекер для ЦИИ НГУ (Центр искусственного
> интеллекта НГУ). 10-100 пользователей. Канбан + чат внутри задачи + KPI.
> **Статус:** в эксплуатации.

## Зачем

Менеджеры ведут проекты заказчиков, разработчики/аналитики выполняют
задачи, директор видит KPI. Замена связки «1С + Excel + WhatsApp +
устные договорённости».

## Что отличает от Jira / Trello

1. **Чат внутри задачи** — обсуждение, упоминания, файлы и история
   статусов в одной ленте.
2. **Канбан с optimistic-update** — карточка перемещается мгновенно.
3. **Двухуровневая RBAC** (Redmine-style) — глобальный `is_superuser`
   + проектная роль с 12 per-permission флагами.
4. **KPI как живой агрегат** — не ручной отчёт, а подсчёт фактов,
   подтверждённых менеджерами.

## Стек

| Слой | Технология |
|------|-----------|
| Backend | Python 3.12, FastAPI 0.115, SQLAlchemy 2.0 (async), aiosqlite |
| Storage | SQLite WAL (до 100 пользователей) |
| Frontend | React 18.3, TS 5.6 strict, Vite 5.4, Tailwind 3.4 |
| State | TanStack Query (server) + Zustand (UI) |
| Auth | python-jose JWT HS256 + bcrypt + Mail.ru/Google SSO |
| Deploy | Vercel (фронт) + Railway (бэк) |

## Документация (в `~/LEYKA/docs/`)

С 2026-10-26 комплект по ГОСТ 19 собран в `docs/GOST-19/` с обозначениями `LEYKA-NN`
вместо прежних `ЦИИ НГУ.LEYKA.*-2026`; таблица соответствия и ведомость — в `docs/GOST-19/README.md`.

| Документ | Стадия ЖЦ |
|----------|-----------|
| `STARTING-A-PROJECT.md` | 0 — эталонный документ для нового LEYKA-like проекта (250+ строк) |
| `SKILL-Project.md` | 4 — ТЗ для backend (исторический) |
| `SKILL-Start-plan.md` | 4 — стартовая структура и псевдокод (исторический) |
| `GOST-19/LEYKA-13 Описание программы.md` | 5 — архитектура; черновик описания программы, до 2026-10-26 — `ARC.md` |
| `GOST-19/VNE-KOMPLEKTA/ARCHITECTURE-101.md` | 5 — упрощённое описание |
| `SKILL-*.md` (10+ файлов) | 6 — навыки по фичам (Auth, DevSecOps, MERMAID, PWA, KANBAN, RELEASE, WIKI, UI-Master); INFRA и TESTPLAN — в `GOST-19/VNE-KOMPLEKTA/` |
| `GOST-19/` | 7 — комплект ЕСПД: LEYKA-1 ТЗ (19.201-78), LEYKA-1/2 ТЗ на подсистему отчётности, LEYKA-13 (черновик), LEYKA-32 руководство системного программиста (19.503-79), LEYKA-34 руководство оператора (19.505-79), LEYKA-51 ПМИ (19.301-79); написано шесть из одиннадцати, порядок дописывания — `PLAN-KOMPLEKTA.md` |
| `GOST-19/VNE-KOMPLEKTA/` | 7 — сопутствующие, вне комплекта: инструкция по установке LEYKA-ИУ, DEPLOY*, GOOGLE_DRIVE_SETUP, SKILL-INFRA, SKILL-TESTPLAN |
| `README.md` (корень репозитория) | 8 — лендинг + onboarding |
| `SKILL-PROJECT-CHARTER-AND-REPORTS.md` | 9 — анализ Устав/Отчёт + gap-анализ модели |
| `BEKLOG.md`, `USE-CASES.md`, `*-RAZBOR.md`, `UGT/` | — рабочие документы skicoding: бэклог отложенного, сценарии, разборы с решениями, видение конвейера по УГТ |
| `GOST-19/VNE-KOMPLEKTA/USER-STORIES.md` | — user stories каталог |
| `AUDIT-REPORT.md` + `AUDIT-REPORT-FRONTEND.md` (корень репозитория) | — внутренний аудит |

## Главные ценности LEYKA для других проектов

1. **`STARTING-A-PROJECT.md`** — это **эталон**, к которому можно
   возвращаться при старте любого нового веб-проекта. См. [adr-001-default-stack](../../../docs/decisions/adr-001-default-stack.md).

2. **`SKILL-PROJECT-CHARTER-AND-REPORTS.md`** — модель данных для
   автоматической генерации Устава и годового Отчёта (КОСГУ, KPI,
   ProjectMilestone). Это **gold standard** для академических проектов.
   См. [09-charter-reports-growth](../../../docs/playbooks/09-charter-reports-growth.md).

3. **Комплект ГОСТ 19** — `docs/GOST-19`, обозначения `LEYKA-NN`: шесть документов
   из одиннадцати написаны, ведомость честно называет ненаписанное. Прежняя версия
   комплекта была опорой для `~/RAGRAF/docs/SKILL-GOST-19.md`; образец в навыке
   `gost-19-primery/komplekt-leyka` обновлён 2026-10-26 (обезличен).

4. **Доменная модель из 5 сущностей** (Partner / Project / User / Task /
   Comment + TaskStatusHistory через SQLAlchemy event) — переносимый
   паттерн для CRM/PM-систем.

5. **Подсистема отчётности по Программе Центра** (ТЗ LEYKA-1/2): контрольные точки Плана,
   комплекты отчётных документов по типам результата, приёмка не автором, отчётный период,
   выгрузка и риски; рамка Правил включается настройкой — подсистема работает и без гранта.
   Модель — 53 контрольные точки ОПД 2026 на проде с 2026-10-26.

## Связанное

- ADR: [adr-001-default-stack](../../../docs/decisions/adr-001-default-stack.md)
- Концепт: [kosgu](../../../concepts/kosgu.md)
- Концепт: [gost-19](../../../concepts/gost-19.md)
- Плейбук: [09-charter-reports-growth](../../../docs/playbooks/09-charter-reports-growth.md)
