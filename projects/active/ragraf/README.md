---
title: RAGRAF
description: Платформа замкнутого контура «датчик ↔ регламент ↔ задача ↔ человек». Практическая реализация задачного подхода Сибирской школы.
tags: [project, stack/fastapi-react, status/active, category/A]
status: active
category: A
repo: ~/RAGRAF
deployed: https://ragraf.up.railway.app
created: 2026-05-01
updated: 2026-05-26
related:
  - concepts/zadachny-podhod.md
  - concepts/tfs-anokhina.md
  - concepts/sigma-methodology.md
---

# RAGRAF

> **Где код:** `~/RAGRAF/`
> **Прод:** https://ragraf.up.railway.app
> **Что это:** платформа, замыкающая контур между датчиком (IoT/таск-трекер),
> формальным регламентом и исполнителем-человеком (через мессенджер).
> **Расшифровка:** Regulation Authoring with Graph-RAG, Author Framework.

## Зачем

На рынке (РФ, 2026) **отсутствует целостный инструмент**, который
одновременно работает над:
- любыми датчиками (IoT / видеоаналитика / DAS / извещатели),
- любыми таск-трекерами (LEYKA, Jira, YouTrack),
- любыми мессенджерами (Telegram, ВК, корп. каналы),
с **формально-проверяемой логикой** регламента.

См. [TZ_RAGRAF §2](`~/RAGRAF/docs/TZ_RAGRAF.md`) — детальное ТЭО ниши.

## Главная идея — задачный подход

RAGRAF — практическая реализация **задачного подхода**
С.С. Гончарова / Д.И. Свириденко (Сибирская математическая школа,
ИМ СО РАН). Каждый цифровой регламент = формальная задача из 4 компонентов
+ 6 стадий ТФС Анохина.

См. [concepts/zadachny-podhod](../../../concepts/zadachny-podhod.md),
[concepts/tfs-anokhina](../../../concepts/tfs-anokhina.md).

## Стек

| Слой | Технология |
|------|-----------|
| Backend | Python 3.12, FastAPI, rdflib, pyshacl, DuckDB (local store) |
| Storage | DuckDB authoritative + Turtle/OWL fixtures + опционально upstream Sigma |
| Frontend | React 18, Vite, TanStack Query, Zustand, React Flow (DSL), Cytoscape (graph view) |
| AI | RAGU GraphRAG (опционально); OpenAI-compatible chat |
| Deploy | Railway (single-server full-stack) |

## Документация (в `~/RAGRAF/docs/`)

| Документ | Стадия ЖЦ |
|----------|-----------|
| `TZ_RAGRAF.md` (v2.0) | 4 — ТЗ по ГОСТ 19.201, версия после Phase 1+2 |
| `STRATEGY-POSITIONING.md` | 2 — стратегический отчёт (10 секций) |
| `ragraf_economics_marketing_report.md` | 3 — ТЭО, TCO, ROI, 5-летняя траектория |
| `REPORT-PROJECT.md` | 9 — отчёт о проекте |
| `ARC.md` + `ARCHITECTURE.md` + `ARC-SIGMA.md` + `ARCH-CRITIQUE-SIGMA.md` | 5 — архитектура (несколько версий) |
| `regulation-viz-skill.md` | 5/6 — спека визуализации |
| `RAGRAF_UserManual_GOST19.md` | 7 — Руководство оператора |
| `SKILL-GOST-19.md` | мета — конвенции ГОСТ-19 для всех документов проекта |
| `SKILL-D0SL.md`, `SKILL-SIGMA-AUDIT.md`, `SKILL-SKICODING.md` | 6 — навыки |
| `BACKLOG.md` | 9 — живой бэклог |
| `GLOSSARY.md` | — словарь (включает Часть XVIII «Задачный подход») |
| `RULES-MANAGEMENT.md` | — описание домена |
| `SMART_CITY_ONTOLOGIES.md` | — внешние онтологии |
| `DATA-FLOW-AUDIT.md` | — аудит потока данных |
| `CAMPUS-NSU-REGULATIONS.md` | — кейс с НГУ кампусом |
| `E2E-TRAFFIC-SPEED-FINE.md` | — end-to-end сценарий |
| `FORMULA-SPEC.md` | — спецификация формул |
| `RAGU_SURFACE.md` | — поверхность GraphRAG |
| `STRATEGY-POSITIONING.md` | 2 — позиционирование (~200 строк) |
| `sigma-audit-RAGRAF-2026-05-13*.md` | — аудиты sigma-методологией |
| `PLAN-R-MOCK.md`, `CONCEPT-ARHI-CONSTRUCTION.md`, `REQ-*.md` | 2-3 — концепты и требования |
| `DEPLOY*.md` | 7 — инструкции по установке |
| `draft_arc.md`, `draft_readme.md` | — черновики |

> **Кросс-документная таблица** — в `~/RAGRAF/docs/SKILL-GOST-19.md` §7.

## Skills RAGRAF (специфические рецепты)

См. локальный [skills.md](skills.md) (перенесённый из MY-VOLUME).
Покрывает: React Flow DnD, Cytoscape, SHACL bridge, RAGU integration,
FastAPI layout, Turtle serializer, DuckDB threading, optimistic
mutations, three-view editor, versioning.

## Положение в экосистеме

```
РАГРАФ → Σ Сигма → КАППА
```
- **РАГРАФ** — точка входа для пилотов (10-10 000 регламентов).
- **Σ Сигма** — городская платформа масштабирования (Ростелеком партнёр).
- **КАППА** — образовательный слой обучения методологов.

## Связанное

- Концепт: [zadachny-podhod](../../../concepts/zadachny-podhod.md)
- Концепт: [tfs-anokhina](../../../concepts/tfs-anokhina.md)
- Концепт: [sigma-methodology](../../../concepts/sigma-methodology.md)
- ADR: [adr-002-doc-set-per-project](../../../docs/decisions/adr-002-doc-set-per-project.md) — RAGRAF демонстрирует категорию A (полный комплект)
- Skills: [skills.md](skills.md)
