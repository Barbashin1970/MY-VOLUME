---
title: knowledge/patterns/
description: Архитектурные паттерны, переносимые между стеками и проектами.
tags: [meta, knowledge, patterns]
---

# knowledge/patterns/

«Паттерн» = архитектурный приём **выше уровня фреймворка**. Он переносим
между TypeScript/Python/Java, между FastAPI/Django, между React/Vue.

## Примеры — то, что уже видно по твоим проектам

- `optimistic-mutations.md` — оптимистичные мутации с откатом (LEYKA → RAGRAF).
- `three-view-editor.md` — редактор в трёх представлениях (форма / слайдеры / source) — NSK → RAGRAF.
- `kanban-board-dnd.md` — Kanban с drag-and-drop (LEYKA).
- `repository-with-fallback.md` — слой данных с приоритетами источников (RAGRAF: DuckDB → fixtures → upstream).
- `immutable-snapshots.md` — версионирование через снапшоты (RAGRAF).

## Чем паттерн отличается от skill

| Skill (`../skills/`) | Pattern (этот раздел) |
|----------------------|----------------------|
| Конкретный рецепт с кодом | Идея на уровне диаграммы |
| Привязан к стеку (React Flow DnD) | Не привязан к стеку |
| 50-150 строк кода | Может вообще не содержать кода |
| Цель: «скопировал — работает» | Цель: «понял идею — применю где угодно» |

## Шаблон заметки

```markdown
---
title: Optimistic Mutations with Rollback
tags: [pattern, knowledge]
applicable_to:
  - stack/fastapi-react
  - stack/pwa-vite-zustand
---

# Pattern: <название>

## Проблема

Что трудно делать без паттерна.

## Решение (идея)

Описание паттерна — нейтрально к технологии.

## Диаграмма

​\`\`\`
[ASCII / Mermaid / ссылка на assets/]
​\`\`\`

## Реализации в твоих проектах

- LEYKA — `frontend/src/components/Kanban/Board.tsx:moveMutation`
- RAGRAF — `frontend/src/lib/saveFlowMutation.ts`

## Когда **не** применять

Границы. В каких условиях паттерн вреден.
```

## Правило: паттерн появляется на **третьем** применении

Первый раз — это решение. Второй — совпадение. **Третий — паттерн.**
Не пытайся сразу обобщать после первого раза — выйдет преждевременная
абстракция.
