---
title: templates/
description: Шаблоны заметок. Не редактируй вручную — копируй и наполняй.
tags: [meta, templates]
---

# templates/

Заготовки для разных типов заметок. **Не вписывай в шаблоны контент** —
копируй, переименовывай, заполняй.

## Что есть

| Файл | Когда копировать |
|------|-----------------|
| [project.md](project.md) | Создаёшь карточку нового проекта в `projects/active/<slug>/README.md` |
| [skill.md](skill.md) | Достаёшь рецепт из проекта в `knowledge/skills/` |
| [lesson.md](lesson.md) | Сохраняешь урок (часто — bug + fix) в `knowledge/lessons/` |
| [decision.md](decision.md) | Пишешь ADR в `docs/decisions/` |
| [concept.md](concept.md) | Заводишь термин в `concepts/` |
| [meeting.md](meeting.md) | Новая встреча в `meetings/YYYY-MM/` |
| [person.md](person.md) | Карточка человека в `people/` |

## Как использовать в Obsidian

`Settings → Templates → Template folder: templates/`. Потом
`Cmd+P → Insert template` — выбрать нужный.

## Как использовать вручную (или через Claude Code)

```bash
cp templates/skill.md knowledge/skills/fastapi-async-db.md
```
И заполнить.

## Соглашения по frontmatter

Минимальный набор полей:

```yaml
---
title: Человеко-читаемое название
description: Одна строка, для чего эта заметка
tags: [категория/значение]
created: 2026-05-26
updated: 2026-05-26
---
```

Дополнительные поля специфичны типу — смотри в каждом шаблоне.
