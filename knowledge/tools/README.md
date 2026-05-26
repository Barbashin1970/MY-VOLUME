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
