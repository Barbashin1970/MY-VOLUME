---
title: "ADR-001: Дефолтный стек для веб-проектов"
description: По умолчанию веб-проект делается на FastAPI + React + Vite + Tailwind + TanStack Query + Zustand + SQLite (до 100 пользователей).
tags: [adr, decision]
status: accepted
date: 2026-05-26
supersedes: null
superseded_by: null
---

# ADR-001: Дефолтный стек для веб-проектов

## Контекст

После 8 проектов (LEYKA, RAGRAF, AI-SKLAD, GONKA, SOFIA, KNIFFEL, NSK,
POLINOM) сложилась повторяющаяся связка технологий. Каждый раз
изобретать выбор стека — потеря 1-3 дней и риск выбрать невалидированную
библиотеку.

LEYKA `STARTING-A-PROJECT.md` уже зафиксировал стек как «эталон для
аналогичного корпоративного таск-трекера 10-100 пользователей».

## Решение

**По умолчанию веб-проект делается на следующем стеке.**

### Backend

| Компонент | Версия | Обоснование |
|-----------|--------|-------------|
| Python | 3.12 | Asyncio TaskGroup, structural pattern matching |
| FastAPI | 0.115 | Строго асинхронный, OpenAPI из коробки |
| Uvicorn | 0.32 | ASGI-сервер |
| SQLAlchemy | 2.0 (async) | Только новый `select(...)` стиль |
| DB driver | aiosqlite 0.20 | До 100 пользователей; для PG — asyncpg |
| Alembic | 1.14 | Миграции с поддержкой async |
| Pydantic | v2.10 | Валидация request/response |
| Auth | python-jose 3.5 + passlib[bcrypt] 1.7 | JWT HS256 + bcrypt |
| Rate-limit | slowapi 0.1.9 | На login-эндпоинт |
| Тесты | pytest 9 + pytest-asyncio 1.3 + httpx ASGITransport | HTTP-контракт > unit |

### Frontend

| Компонент | Версия | Обоснование |
|-----------|--------|-------------|
| React | 18.3 (или 19 для игр) | Без RSC; client-components |
| TypeScript | 5.6 strict | `tsc --noEmit` в CI |
| Vite | 5.4 (или 6) | HMR, нативный ESM |
| Tailwind CSS | 3.4 | + `clsx` + `tailwind-merge` через `cn` |
| TanStack Query | 5.59 | Server state, кэш, optimistic |
| Zustand | 5.0 | Local UI state (sidebar, wallpaper) |
| Иконки | lucide-react | Однотонные SVG, tree-shake |
| Codegen | openapi-typescript 7.4 | Из OpenAPI → `src/api/types.gen.ts` |
| Тесты | Vitest 2.1 | Pure-функции и stores |

### Хостинг (по умолчанию)

| Слой | Платформа | Стоимость |
|------|-----------|-----------|
| Frontend | Vercel | бесплатно (hobby) или $20/мес (pro) |
| Backend | Railway | $5-20/мес |
| Or both | Fly.io | $5-15/мес |

### БД

| Условие | Решение |
|---------|---------|
| ≤ 100 пользователей и < 100 MB данных | SQLite (WAL mode) |
| > 100 пользователей или > 1GB | PostgreSQL 15+ (asyncpg) |
| Локальный store (single-user / desktop-like) | DuckDB (как RAGRAF) |

## Когда отклоняться

| Контекст | Стек |
|----------|------|
| **Игра / PWA** | React 19 + Vite 6 + Zustand + i18next + PixiJS/SVG (GONKA-стиль) |
| **Демо без сервера** | Чистый Python + статический HTML + Vercel (AI-SKLAD-стиль) |
| **Telegram-бот** | aiogram + SQLite (NSK-стиль) |
| **Java sandbox** | Spring Boot + JPA (POLINOM-JAVA) |
| **CLI-утилита** | Python + click/typer, без веб-слоя |

В этих случаях — **не** применяй default. Создай новый ADR с
обоснованием.

## Альтернативы, которые рассмотрели

| Вариант | Почему не выбрали |
|---------|-------------------|
| Django + Django Rest Framework | Тяжелее; нет нативного async; ORM не SQLAlchemy 2 |
| Next.js (full-stack) | RSC сложнее в дебаге; vendor-lock на Vercel |
| Nest.js (TS backend) | Не сидит так же глубоко с Python ML/data-stack |
| Postgres с первого дня | Overhead для < 100 user; SQLite + WAL хватает |

## Последствия

### Позитивные
- Новый проект стартует за **30 минут** (см. будущий [playbook](../playbooks/)).
- AI-ассистент знает конвенции и не предлагает альтернативы каждый раз.
- Skills из `~/LEYKA/` переносятся в новые проекты с минимальной адаптацией.

### Негативные
- Привязка к Python для backend (нет JVM-проектов, кроме POLINOM-JAVA).
- SQLite — потолок ~100 пользователей; миграция на PG плановая, но
  это работа на 1-2 недели.

### Нейтральные
- Зависимость от Vercel/Railway: можно мигрировать на self-hosted
  Ubuntu + systemd + nginx — есть playbook в LEYKA.

## Ссылки

- LEYKA: `~/LEYKA/docs/STARTING-A-PROJECT.md` §2 — полная таблица стека
- LEYKA: `~/LEYKA/docs/ARC.md` §10 — обоснование SQLite WAL
- RAGRAF: `~/RAGRAF/docs/ARC.md` — вариация с DuckDB
- Плейбук: [docs/playbooks/05-architecture](../playbooks/05-architecture.md)
