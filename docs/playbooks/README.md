---
title: docs/playbooks/
description: Step-by-step руководства — как пройти конкретный путь от начала до конца.
tags: [meta, docs, playbooks]
---

# docs/playbooks/

**Playbook** = последовательность шагов, после которых ты гарантированно
в нужной точке.

- Туториал = «как работает X»
- Playbook = «как **сделать** X, шаг за шагом»

Здесь — playbook'и. Туториалы (если будут) — в `knowledge/<подкатегория>/`.

---

## Какие playbook'и стоит написать первыми

По твоим проектам видно повторяющиеся пути:

| Файл | Зачем |
|------|-------|
| `start-fastapi-react-project.md` | Поднять backend+frontend проект LEYKA-style за 30 минут |
| `start-pwa-vite-project.md` | Поднять PWA GONKA/SOFIA-style |
| `deploy-static-demo-vercel.md` | Раскатать демо (AI-SKLAD style) |
| `deploy-fastapi-railway.md` | Раскатать backend на Railway |
| `migrate-sqlite-to-postgres.md` | Когда вырос за лимит SQLite |
| `setup-claude-code-for-project.md` | Создать `.claude/`, skills, hooks |
| `add-i18n-to-react-app.md` | Подключить i18next по образцу GONKA |

## Формат

```markdown
---
title: Поднять FastAPI + React проект за 30 минут
description: От пустой папки до работающего hello-world с auth.
tags: [doc, playbook, stack/fastapi-react]
estimated_minutes: 30
prerequisites:
  - Python 3.12
  - Node 22
  - uv установлен
---

# Поднять FastAPI + React проект

## Что в результате

Конкретное наблюдаемое состояние. «Откроется браузер на localhost:5173 с
работающим логином».

## Шаги

### 1. Создать структуру

​\`\`\`bash
…
​\`\`\`

### 2. Backend

…

### 3. Frontend

…

### 4. Проверка

Как убедиться, что всё работает.

## Что часто ломается

Известные проблемы и обходы. Каждое — со ссылкой на
[../../knowledge/lessons/](../../knowledge/lessons/).
```

## Принципы

1. **Императивный язык:** «Сделай это», не «можно сделать это».
2. **Проверка после каждого шага:** «Если работает — будет вот так».
3. **Идемпотентность, где возможно:** повторный запуск не ломает.
4. **Точные версии:** «Python 3.12», не «новый Python».
5. **Линки на уроки:** в каждом сложном месте — ссылка на
   `knowledge/lessons/`, где описаны типовые проблемы.
