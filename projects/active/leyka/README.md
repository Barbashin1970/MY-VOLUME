---
title: LEYKA
description: Корпоративный таск-трекер ЦИИ НГУ. FastAPI + React + SQLite. Эталон стека и документации.
tags: [project, stack/fastapi-react, status/active, category/A]
status: active
category: A
repo: ~/LEYKA
deployed: production (Vercel + Railway)
created: 2026-04-15
updated: 2026-05-26
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

| Документ | Стадия ЖЦ |
|----------|-----------|
| `STARTING-A-PROJECT.md` | 0 — эталонный документ для нового LEYKA-like проекта (250+ строк) |
| `SKILL-Project.md` | 4 — ТЗ для backend (исторический) |
| `SKILL-Start-plan.md` | 4 — стартовая структура и псевдокод (исторический) |
| `ARC.md` | 5 — архитектура |
| `ARCHITECTURE-101.md` | 5 — упрощённое описание |
| `SKILL-*.md` (10+ файлов) | 6 — навыки по фичам (Auth, DevSecOps, INFRA, TESTPLAN, MERMAID, PWA, KANBAN, RELEASE, WIKI, UI-Master) |
| `LEYKA_TZ_GOST19.md` | 7 — ТЗ по ГОСТ 19.201-78 |
| `LEYKA_UserManual_GOST19.md` | 7 — Руководство оператора (19.505-79) |
| `LEYKA_AdminGuide_GOST19.md` | 7 — Руководство админа (19.503-79) |
| `LEYKA_InstallManual_GOST19.md` | 7 — Инструкция по установке (19.502-78) |
| `LEYKA_PMI_GOST19.md` | 7 — ПМИ (19.301-79) |
| `README.md` | 8 — лендинг + onboarding |
| `SKILL-PROJECT-CHARTER-AND-REPORTS.md` | 9 — анализ Устав/Отчёт + gap-анализ модели |
| `USER-STORIES.md` | — user stories каталог |
| `AUDIT-REPORT.md` + `AUDIT-REPORT-FRONTEND.md` | — внутренний аудит |

## Главные ценности LEYKA для других проектов

1. **`STARTING-A-PROJECT.md`** — это **эталон**, к которому можно
   возвращаться при старте любого нового веб-проекта. См. [adr-001-default-stack](../../../docs/decisions/adr-001-default-stack.md).

2. **`SKILL-PROJECT-CHARTER-AND-REPORTS.md`** — модель данных для
   автоматической генерации Устава и годового Отчёта (КОСГУ, KPI,
   ProjectMilestone). Это **gold standard** для академических проектов.
   См. [09-charter-reports-growth](../../../docs/playbooks/09-charter-reports-growth.md).

3. **Полный комплект ГОСТ-19** — 5 файлов. Эталон, на который опирается
   `~/RAGRAF/docs/SKILL-GOST-19.md`.

4. **Доменная модель из 5 сущностей** (Partner / Project / User / Task /
   Comment + TaskStatusHistory через SQLAlchemy event) — переносимый
   паттерн для CRM/PM-систем.

## Связанное

- ADR: [adr-001-default-stack](../../../docs/decisions/adr-001-default-stack.md)
- Концепт: [kosgu](../../../concepts/kosgu.md)
- Концепт: [gost-19](../../../concepts/gost-19.md)
- Плейбук: [09-charter-reports-growth](../../../docs/playbooks/09-charter-reports-growth.md)
