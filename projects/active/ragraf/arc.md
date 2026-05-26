---
title: Архитектура RAGRAF (краткая карта)
description: Карта-указатель на полную архитектуру в ~/RAGRAF/docs/. Здесь — только ключевые отметки.
tags: [project, ragraf, arc]
updated: 2026-05-26
---

# Архитектура RAGRAF

> **Полная архитектура** живёт в коде: `~/RAGRAF/docs/ARC.md` +
> `~/RAGRAF/docs/ARCHITECTURE.md` + `ARC-SIGMA.md` + `ARCH-CRITIQUE-SIGMA.md`.
> Здесь — карта-указатель для быстрого доступа.

## Слои

```
┌─────────────────────────────────────────────────────┐
│ Frontend (React + Vite + React Flow + Cytoscape)    │
├─────────────────────────────────────────────────────┤
│ Backend (FastAPI + rdflib + pyshacl + RAGU)         │
├─────────────────────────────────────────────────────┤
│ Storage (DuckDB authoritative + Turtle fixtures)    │
├─────────────────────────────────────────────────────┤
│ External (LEYKA tasks, IoT sensors, Sigma upstream) │
└─────────────────────────────────────────────────────┘
```

## Ключевые архитектурные решения

| Решение | Где описано | Обоснование |
|---------|-------------|-------------|
| DuckDB как локальный store (а не Postgres) | ARC §10 | Single-user prototype, миграция через `DATABASE_URL` |
| `threading.RLock()` (а не `Lock`) | RAGRAF/skills.md «DuckDB как локальный store» | `init_db → seed → save()` дедлочит на обычном Lock |
| React Flow с **нативным HTML5 DnD** | skills.md «Drag-and-Drop» | Не `@dnd-kit` — это собственный паттерн React Flow |
| Turtle text/plain в upstream | skills.md «Regulation→Turtle» | Бит-в-бит совместимо с Sigma и Apache Jena |
| `AcceptanceCriterion` first-class | TZ §1.2 + STRATEGY-POSITIONING §2 | Реализация задачного подхода |
| 5 типов критерия | TZ §3.3 | no_violation / specific_output / custom / sensor_returned_normal / task_closed_within_sla |

## Замкнутый контур данных (главный flow)

```
IoT/Task → ETL puller → etl_snapshots → flow_executor
                                            │
                            ┌───────────────┼───────────────┐
                       leyka_task        webhook         direct
                            │
                       regulation_metrics ← acceptance_resolver
                            │
                       UI бадж + 30-day sparkline → методолог
```

## Связь со стадиями ЖЦ

| Стадия документ | RAGRAF файл |
|-----------------|-------------|
| 1 IDEA | (исторически не сохранён, см. v1.0 TZ) |
| 2 POSITIONING | docs/STRATEGY-POSITIONING.md |
| 3 ECONOMICS | docs/ragraf_economics_marketing_report.md + REPORT-PROJECT.md |
| 4 TZ | docs/TZ_RAGRAF.md (v2.0) |
| 5 ARC | docs/ARC.md + ARCHITECTURE.md |
| 6 SKILL | docs/SKILL-D0SL.md + SKILL-SIGMA-AUDIT.md + SKILL-SKICODING.md + skills.md (этой папки) |
| 7 GOST-19 | docs/RAGRAF_UserManual_GOST19.md + SKILL-GOST-19.md |
| 8 README | README.md |
| 9 ROADMAP | docs/BACKLOG.md |

## Связанное

- Skills проекта: [skills.md](skills.md)
- Концепты: [zadachny-podhod](../../../concepts/zadachny-podhod.md), [tfs-anokhina](../../../concepts/tfs-anokhina.md)
- Плейбук: [05-architecture](../../../docs/playbooks/05-architecture.md)
