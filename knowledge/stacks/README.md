---
title: knowledge/stacks/
description: Связки технологий — твои рабочие стеки, повторяющиеся между проектами.
tags: [meta, knowledge, stacks]
---

# knowledge/stacks/

«Стек» = связка `язык + фреймворк + библиотеки + конвенции`, которую ты
повторно собираешь. Здесь — твои **рабочие стеки**, обкатанные на проектах.

## Какие стеки уже видно по твоим проектам

Заметки, которые имеет смысл завести первыми:

| Файл | Описание | Источник |
|------|----------|----------|
| `fastapi-react-vite.md` | FastAPI 0.115 + SQLAlchemy 2 + React 18 + Vite 5 + TanStack Query | LEYKA, RAGRAF |
| `pwa-vite-zustand.md` | Vite 6 + React 19 + Zustand + i18next + Tailwind | GONKA, SOFIA, KNIFFEL |
| `python-static-demo.md` | Python без зависимостей + статический HTML + Vercel | AI-SKLAD |
| `java-claude-sandbox.md` | Java + Claude-driven dev — playground | JAVA-CLAUDE-TEST, POLINOM-JAVA |
| `graphrag-rdflib.md` | RAGU + rdflib + pyshacl + DuckDB | RAGRAF |

## Что в заметке про стек

```markdown
---
title: FastAPI + React + Vite
tags: [stack/fastapi-react, knowledge]
projects:
  - leyka
  - ragraf
---

# Stack: FastAPI + React + Vite

## Версии (на момент написания)

| Компонент | Версия |
| ... | ... |

## Структура проекта

​\`\`\`
backend/app/{api, services, schemas, domain}
frontend/src/{components, pages, lib, api}
​\`\`\`

## Конвенции

- Async везде на бэке.
- TanStack Query как источник server-state.
- ...

## Что часто ломалось (ссылки на lessons/)

- [duckdb-rlock-deadlock](../lessons/duckdb-rlock-deadlock.md)
- ...

## Стартовый чек-лист

- [ ] uv init / npm create vite
- [ ] ...
```

## Когда писать новый файл

Если ты собираешь те же 5+ библиотек уже **во второй раз** — заводи
заметку. На третий раз — она уже должна работать как «болванка нового
проекта».

## Что не сюда

- Описание архитектуры конкретного проекта → `projects/<x>/arc.md`.
- Рецепт «как добавить drag-and-drop» → `../skills/`.
- Один баг → `../lessons/`.
