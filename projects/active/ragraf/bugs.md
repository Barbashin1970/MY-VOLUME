---
title: Bugs & Fixes — RAGRAF
description: Локальный журнал багов и фиксов RAGRAF. То, что применимо везде — промоутится в knowledge/lessons/.
tags: [project, ragraf, bugs]
updated: 2026-05-26
---

# RAGRAF — баги и фиксы

> Извлечено из [skills.md](skills.md) и STRATEGY-POSITIONING §2.3.

---

## 2026-05-23 — Cross-connection DuckDB JOIN в etl_control_store

**Симптом.** В пульте ETL отображалось «7 ч назад» вместо реального времени.

**Причина.** Cross-connection JOIN в DuckDB через
`etl_control_store.list_sources()`.

**Фикс.** Переписали запрос внутри одного соединения.

**Применимо вне проекта?** Да — паттерн «DuckDB JOIN только внутри одного коннекта». Кандидат на [knowledge/lessons/duckdb-single-connection-joins.md].

---

## 2026-05-23 — Timezone-баг в record_metric

**Симптом.** `acceptance_resolver` видел `age = -7h` (отрицательное!) у только что записанных метрик.

**Причина.** DuckDB Python-коннектор конвертировал **aware UTC datetime
в local time** при записи. При чтении уже считал как UTC → -7 часов
разницы.

**Фикс.** `datetime.now(timezone.utc).replace(tzinfo=None)` — записываем
**naive UTC instant**, а не aware-UTC.

**Применимо вне проекта?** ✅ ДА — критическое правило для DuckDB.
Кандидат на [knowledge/lessons/duckdb-tz-naive-utc.md].

---

## Известный с самого старта — DuckDB threading.Lock дедлочит

**Симптом.** На старте при `init_db → seed → save()` приложение виснет.

**Причина.** Обычный `threading.Lock()` не reentrant. Внутри `init_db`
вызывается `save()`, который пытается захватить тот же лок → deadlock.

**Фикс.** Использовать `threading.RLock()` (reentrant lock).

**Урок:** NSK для своего YAML-store избегал этого через простой file
write — у RAGRAF DuckDB и нужен явный лок.

**Применимо вне проекта?** ✅ ДА — для любого single-file-store на
DuckDB. Кандидат на [knowledge/lessons/duckdb-rlock-vs-lock.md].

---

## React Flow использует нативный HTML5 DnD, не @dnd-kit

**Симптом** (потенциальный): попытка прикрутить `@dnd-kit/core` для
перетаскивания нод палитры на canvas → не работает.

**Причина.** React Flow имеет **собственный паттерн** на нативном
HTML5 DnD (`onDragOver`, `onDrop` на `<ReactFlow>`). `@dnd-kit` тут
не нужен.

**Фикс.** Использовать `dataTransfer.setData('application/reactflow-type', ...)`
+ `screenToFlowPosition(...)`. См. [skills.md](skills.md) § «Drag-and-Drop палитры».

**Применимо?** ✅ ДА. [knowledge/lessons/react-flow-native-dnd.md] кандидат.

---

## Промоутирование в knowledge/lessons/

Все 4 урока — **кандидаты в `knowledge/lessons/`**, потому что
повторятся в следующих проектах с DuckDB / React Flow:

- [ ] `duckdb-rlock-vs-lock.md`
- [ ] `duckdb-tz-naive-utc.md`
- [ ] `duckdb-single-connection-joins.md`
- [ ] `react-flow-native-dnd.md`

Заведи их, когда будешь стартовать следующий проект с DuckDB или
React Flow.
