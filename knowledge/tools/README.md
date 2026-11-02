---
title: knowledge/tools/
description: Внешние инструменты — IDE, CLI, AI-ассистенты, devops-утилиты.
tags: [meta, knowledge, tools]
---

# knowledge/tools/

Заметки про **инструменты вокруг кода**: Claude Code, Obsidian, git,
docker, vercel, postgres CLI, IDE-настройки, и т.д.

## Что писать сюда

- Конфиги и хуки (`claude-code-hooks.md`, `git-hooks-pre-commit.md`).
- Workflow-приёмы (`obsidian-dataview-tables.md`).
- Команды, которые ты используешь часто и каждый раз гуглишь
  (`docker-cleanup-commands.md`).
- Настройки IDE, которые повторяешь между машинами (`vscode-mcp-setup.md`).

## Заметки

- [slides-txt-export.md](slides-txt-export.md) — выгрузка слайдов в .txt из Google Slides:
  что в ней теряется и переставляется, как её нормализовать и проверять правки до записи.
- [headless-chrome-layout-checks-via-devtools.md](headless-chrome-layout-checks-via-devtools.md) —
  проверка вёрстки без Playwright: эмуляция экрана и reduced-motion по протоколу DevTools
  из Node 22, ожидание в реальном времени, скриншоты; почему не окно и не виртуальное время.

## Чем отличается от stacks/

- `tools/` — про **процесс разработки**: что я запускаю в терминале / IDE.
- `stacks/` — про **то, что компилируется в продукт**: библиотеки, версии.

## Идеи стартовых заметок (по твоей системе)

- `claude-code-setup.md` — твой `.claude/` со скилами и хуками.
- `obsidian-vault-config.md` — плагины и шорткаты, которыми ты пользуешься в этом vault'е.
- `vercel-deploy-static.md` — как ты раскатываешь AI-SKLAD/демо.
- `railway-fastapi-deploy.md` — как поднимаешь backend (LEYKA, RAGRAF).
- `git-commit-conventions.md` — твой стиль коммитов.
- `pip-audit-npm-audit.md` — что прогоняешь перед стартом проекта.

## Шаблон

```markdown
---
title: Vercel: статический деплой Python-демо
tags: [tool/vercel, knowledge]
related: [stacks/python-static-demo.md]
---

# Vercel: статический деплой Python-демо

## Когда нужно

Раздать клиентам ссылку на демо, которое не хранит состояние сервера.

## Минимальная настройка

​\`\`\`json
// vercel.json
{ ... }
​\`\`\`

## Что часто ломается

…
```
