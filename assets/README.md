---
title: assets/
description: Бинарные вложения — картинки, PDF, схемы.
tags: [meta]
---

# assets/

Сюда — только **бинарные файлы**: PNG, JPG, SVG, PDF, GIF, схемы из Excalidraw.

## Правила

1. **Никаких md-файлов здесь.** Md живёт в смысловых папках, а на картинки
   из неё ссылается.
2. Структурируй по префиксу или подпапке, если их станет много:
   ```
   assets/
   ├── leyka-kanban-screenshot-2026-05.png
   ├── ragraf-arch-diagram.svg
   └── meetings/2026-05-26-stroy-room.png
   ```
3. Имена файлов — kebab-case, с **датой ISO** для скриншотов
   (`leyka-bug-403-2026-05-12.png`).
4. Размер скриншота — оптимизируй (`pngquant` / `optipng`), иначе vault
   распухнет. Не клади 5MB-png, если хватает 200KB.

## Как ссылаться из md

```markdown
![Архитектура RAGRAF](../assets/ragraf-arch-diagram.svg)
```

В Obsidian работает и короткий синтаксис `![[ragraf-arch-diagram.svg]]`,
но в `docs/` и в GH Pages используй полную relative-ссылку.

## Что **не** хранить здесь

- Скачанные книги/курсы — это материал, не vault.
- Чужой код — для этого `git submodule` или внешние папки.
- Видео — раздуют git. Если очень нужно — внешний storage и ссылка.
