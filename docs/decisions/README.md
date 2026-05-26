---
title: docs/decisions/
description: ADR — Architecture Decision Records. Системные решения, общие для всех твоих проектов.
tags: [meta, docs, adr]
---

# docs/decisions/

**ADR** = Architecture Decision Record. Короткий неизменяемый документ
формата «что решили, в каком контексте, какие последствия».

Сюда попадают **системные решения**, которые ты применяешь от проекта к
проекту. Не локальные «в LEYKA взяли SQLite» — а «по умолчанию беру
SQLite, пока пользователей < 100».

---

## Формат именования

`adr-NNN-краткое-решение.md`, где `NNN` — порядковый номер (000, 001, …).
Номера **не переиспользуются**.

Примеры:

- `adr-001-default-stack-fastapi-react.md`
- `adr-002-sqlite-first-postgres-later.md`
- `adr-003-no-wikilinks-in-docs.md`
- `adr-004-tanstack-query-as-server-state.md`

## Lifecycle (поле `status` в frontmatter)

```
proposed  → accepted  →  superseded
                     ↘  deprecated
```

- `proposed` — обсуждается, ещё не правило.
- `accepted` — действует. **Не редактировать.** Только добавлять `superseded_by`.
- `superseded` — заменено новым ADR (поле `superseded_by` указывает на него).
- `deprecated` — больше не применяется, замены нет.

## Шаблон

См. [../../templates/decision.md](../../templates/decision.md).

## Почему ADR не редактируется

Решение принято в **определённом контексте**. Через год контекст может
поменяться, и решение поменяется тоже. Но история того, почему было так —
ценнее, чем подчищенная актуальная версия.

Поэтому: **новое решение = новый ADR со ссылкой `supersedes` на старый**.
Старый помечается `superseded`, но не удаляется.

## Источники ADR

| Откуда приходит | Как |
|-----------------|-----|
| `projects/<x>/decisions.md` | Когда локальное решение применил третий раз — это уже правило, ADR |
| Авторский — после ретро/анализа | Сел и сформулировал |
| После боли (`knowledge/lessons/`) | «Чтобы не повторилось, теперь делаю так» |

## Кандидаты на первые ADR (по твоим проектам)

- Дефолтный стек для веб-приложений (FastAPI + React + Vite + Tailwind + TanStack Query).
- Когда переходить с SQLite на PostgreSQL.
- Где хранить файлы (URL, а не байты, как в LEYKA).
- Когда использовать Zustand vs Context vs TanStack Query.
- Полиси по AI-ассистентам: какой Claude/Copilot когда применяется.
