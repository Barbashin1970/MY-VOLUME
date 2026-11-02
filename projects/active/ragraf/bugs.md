---
title: Bugs & Fixes — RAGRAF
description: Локальный журнал багов и фиксов RAGRAF. То, что применимо везде — промоутится в knowledge/lessons/.
tags: [project, ragraf, bugs]
updated: 2026-10-30
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

**31.10.2026.** След отвергнутого `@dnd-kit` снят в коммите `3e24386` (30.10.2026): пакеты пять месяцев стояли в зависимостях без единого импорта. Урок о самом следе — [changed-decision-leaves-trace-in-dependencies.md](../../../knowledge/lessons/changed-decision-leaves-trace-in-dependencies.md).

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

---

## 2026-10-30 — Заведено в knowledge/lessons/ после сверки книги с изделием

Книгу «Системный аналитик в эпоху гибридного ИИ» (`docs/COURSE-SYSTEM-ANALYST-V1.md`)
сверяли с кодом РАГРАФ по частям; попутно собраны случаи, применимые вне проекта.
По решению Олега (30.10.2026) заведены все:

- [x] [react-flow-split-change-callbacks-overwrite-each-other.md](../../../knowledge/lessons/react-flow-split-change-callbacks-overwrite-each-other.md) — висячие связи после Backspace (`f3adc42`, 04.09.2026)
- [x] [zod-strips-unknown-keys-and-crashes-on-new-enum.md](../../../knowledge/lessons/zod-strips-unknown-keys-and-crashes-on-new-enum.md) — три тихие потери полей и два пустых экрана
- [x] [defensive-overflow-hidden-and-guard-that-locked-the-defect.md](../../../knowledge/lessons/defensive-overflow-hidden-and-guard-that-locked-the-defect.md) — меню роли обрезано (`7fc5d64`, 26.09.2026)
- [x] [draft-autosave-written-but-never-read.md](../../../knowledge/lessons/draft-autosave-written-but-never-read.md) — черновик потока без читателя (не починено)
- [x] [artifact-excluded-from-image-looks-unwritten.md](../../../knowledge/lessons/artifact-excluded-from-image-looks-unwritten.md) — «0 из 11» на проде (`5f8c0ff`, 26.09.2026)
- [x] [two-adr-registries-make-references-ambiguous.md](../../../knowledge/lessons/two-adr-registries-make-references-ambiguous.md) — два реестра решений (не починено)
- [x] [global-unit-table-diverges-from-source-data.md](../../../knowledge/lessons/global-unit-table-diverges-from-source-data.md) — снег в метрах против сантиметров (`fa61b77`, `5188893`)
- [x] [foreign-api-contract-drifts-silently.md](../../../knowledge/lessons/foreign-api-contract-drifts-silently.md) — `/api/v1/api/tasks` и `status` вместо `status_filter`
- [x] [create-table-if-not-exists-skips-new-columns.md](../../../knowledge/lessons/create-table-if-not-exists-skips-new-columns.md) — 18 дней без записи опросов (`5188893`, 27.10.2026)
- [x] второй случай в [document-promise-needs-a-guard.md](../../../knowledge/lessons/document-promise-needs-a-guard.md) — 460 `bg-stone-*` при запрете дизайн-системы

---

## 2026-10-31 — Заведено в knowledge/lessons/ при выкладке книг на страницу документации

Курс РАГРАФ-V1, пособие системного аналитика и учебник Flow DSL выложены
на страницу `/documentation` с печатью в PDF. Коммиты: `6939e9c`, `417592a`, `e520f68`.
Разбор — `docs/KNIGI-NA-STRANICE-DOKUMENTACII-RAZBOR.md` в RAGRAF. Заведено по просьбе Олега:

- [x] [heading-anchor-rule-must-match-github.md](../../../knowledge/lessons/heading-anchor-rule-must-match-github.md) — своё правило якорей против GitHub: оглавление на 243 ссылки молчало бы на странице
- [x] [registry-copy-of-document-edition-drifts.md](../../../knowledge/lessons/registry-copy-of-document-edition-drifts.md) — ФРЕЙМ-35: редакция 6 в шапке, 5 в перечне страницы
- [x] [changed-decision-leaves-trace-in-dependencies.md](../../../knowledge/lessons/changed-decision-leaves-trace-in-dependencies.md) — `@dnd-kit` из образца LEYKA пережил смену решения
- [x] [published-page-ignored-document-distribution-clause.md](../../../knowledge/lessons/published-page-ignored-document-distribution-clause.md) — оговорку «не публикуется» нашли после пуша
- [x] второй случай в [artifact-excluded-from-image-looks-unwritten.md](../../../knowledge/lessons/artifact-excluded-from-image-looks-unwritten.md) — книги в `docs/`; сторож `COPY` заведён

---

## 2026-10-31 — Заведено при выкладке «Справочника строителя умного города»

Словарь сверен с изделием тремя проверками по частям (166 находок) и выложен книгой на страницу
`/documentation`; послойная модель — в РАГРАФ-ФРЕЙМ-13. Коммит `47f0419`, разбор —
`docs/ARHITEKTURA-I-SPRAVOCHNIK-RAZBOR.md` в RAGRAF. Заведено по просьбе Олега:

- [x] [unanchored-grep-counts-docstring-example.md](../../../knowledge/lessons/unanchored-grep-counts-docstring-example.md) — 252 против 251 обработчика: пример в строке документации `kappa.py`
- [x] [guard-scope-list-does-not-grow-with-delivery.md](../../../knowledge/lessons/guard-scope-list-does-not-grow-with-delivery.md) — сторож терминов не видит книги поставки (не починено, в бэклоге)
- [x] паттерн [verify-by-parts-then-splice.md](../../../knowledge/patterns/verify-by-parts-then-splice.md) — сверка по частям и склейка

