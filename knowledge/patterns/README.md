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
- `live-bound-decision-regulation.md` — регламент-надстройка над живым ETL: вход → формула → switch по границам, данные кормит applier по param_id (RAGRAF: traffic-forecast, mobile-index).
- `claim-the-layer-positioning.md` — позиционирование сложного продукта: заявить СЛОЙ, категория в eyebrow, H1 — глагол-результат, lineage вместо логотипов (RAGRAF hero).
- `horizontal-engine-vertical-packs.md` — одно ядро + сменные регламент-паки; не зарываться в одну вертикаль; выбор вектора по «где необходимы × где легко бить автоматизацию» (RAGRAF: 3 вектора).
- `verify-dont-orchestrate.md` — внешний rule-движок не в путь реакции, только в путь пост-фактум проверки/аудита; оркестрацию отдай встроенной автоматизации трекера (Redmine/d0sl → Яндекс Трекер).

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
