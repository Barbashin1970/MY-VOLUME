---
title: CI/CD — непрерывная интеграция и доставка
description: "Continuous Integration + Continuous Delivery. Автоматизированный pipeline от пуша до прода. Стандарт для категории A. Стек: Git + GitHub Actions / Jenkins + Docker + Kubernetes."
tags: [concept, domain/qa, en, devops, automation]
aliases:
  - CI/CD
  - Continuous Integration
  - Непрерывная интеграция
created: 2026-05-26
updated: 2026-05-26
---

# CI/CD — непрерывная интеграция и доставка

**Подход, в котором каждое изменение кода автоматически проверяется и доставляется на следующий шаг pipeline'а — без ручных команд.** В пределе превращается в Continuous Testing (CT) — каждую секунду состояние системы известно.

## CI vs CD vs CT

| Аббревиатура | Что |
|--------------|-----|
| **CI — Continuous Integration** | Каждый commit/PR автоматически проверяется (юнит-тесты, линт, build) |
| **CD — Continuous Delivery** | После успешного CI код автоматически готов к деплою на любой стенд |
| **CD — Continuous Deployment** | После CI код **сам** деплоится на прод (без кнопки) |
| **CT — Continuous Testing** | На каждом стенде непрерывно бегут регресс-тесты (E2E, нагрузочные) |

> **Continuous Delivery ≠ Continuous Deployment.** Delivery значит «готов к деплою»; Deployment значит «уже задеплоено».

## Pipeline-стадии

```
Push → CI ────────────────────────────────────── CD ───────────────►
       │                                          │
       ├ Сборка                                   ├ Релиз
       ├ Юнит-тесты                               ├ Развёртывание
       ├ Линт / type-check                        ├ Эксплуатация
       ├ Интеграционные тесты                     ├ Мониторинг
       └ E2E-тесты (на staging)
```

Каждый шаг — это **автоматическая команда**. Ручные операции заменены скриптами и манифестами.

## Инструменты (стандартный стек)

| Инструмент | Зачем | Альтернативы |
|-----------|-------|--------------|
| **Git** | Объединить код разработчиков | (нет — Git стандарт) |
| **GitHub Actions** | Запускать pipeline по событиям git | Jenkins, GitLab CI, CircleCI |
| **Docker** | Контейнеризация окружения | Podman, Buildah |
| **Kubernetes** | Управление контейнерами в проде | Docker Swarm, Nomad |
| **Allure / pytest-html** | Отчёты о тестах | JUnit XML + grep |
| **Codecov / Coveralls** | Метрики покрытия | self-hosted (Sonarqube) |

**Дефолт для категории A** ([ADR-001](../docs/decisions/adr-001-default-stack.md)): GitHub Actions + Docker. Kubernetes — только при реальном multi-instance scale.

## Минимальный pipeline (для категории A)

### CI (на каждый PR)

```yaml
on: [pull_request]
jobs:
  test:
    - checkout
    - setup python / node
    - pip install -r requirements.txt / npm ci
    - ruff check .  # линт Python
    - mypy .        # type-check
    - pytest backend/tests/  # юнит + интеграционные
    - cd frontend && npx tsc --noEmit && npm test
```

### CD (на merge в main)

```yaml
on:
  push:
    branches: [main]
jobs:
  deploy-staging:
    - docker build
    - docker push
    - kubectl apply (staging namespace)
    - smoke-test через curl/playwright

  deploy-prod:
    needs: deploy-staging
    if: github.event_name == 'release'
    - kubectl apply (prod namespace)
```

## Чем CI/CD полезен для QA

| Эффект | Как работает |
|--------|--------------|
| **Раннее обнаружение багов** | Тесты бегут на каждый PR — баг ловится за минуты, не за дни |
| **Маленькие инкременты** | Сложно сломать много за один commit |
| **Воспроизводимая среда** | Docker гарантирует «работает на проде = работает у меня» |
| **Скорость релиза** | От commit до прода — часы, не недели |
| **Откат за минуту** | `kubectl rollout undo` — мгновенный |

## Когда CI/CD **не** окупается

- **Pet-проект** (категория B) — overhead настройки превышает пользу.
- **Демо** (категория D) — деплой раз в месяц вручную.
- **Научный инструмент** (категория C) — pipeline на бенчмарки, но не на «прод».
- **Однопользовательское локальное приложение** — деплоя нет вообще.

## Применение в твоих проектах

| Проект | CI/CD-зрелость | Что внедрить |
|--------|----------------|--------------|
| **RAGRAF** | Частично — pre-commit hooks + smoke-import тесты локально | GitHub Actions: pytest на PR, build + deploy на Fly.io на merge |
| **LEYKA** | Аналогично RAGRAF | Pipeline + автодеплой на Railway |
| **AI-SKLAD** | Нет | Pipeline избыточен (D). Достаточно npm run build перед коммитом |
| **POLINOM-JAVA** | Нет | JMH-бенчмарки в GitHub Actions на каждый PR |
| **SOFIA** | Vercel автодеплой | Уже есть auto-deploy от Vercel при push в main. CI с тестами — нет, добавить |
| **GONKA / KNIFFEL** | Нет | GitHub Pages + автодеплой через actions |

## Связь с ski-coding

В [ski-coding §7](../knowledge/skills/ski-coding.md) есть **локальный аналог CI** — pre-commit гарантирует:
- `python -c "import app.main"` (backend smoke).
- `npx tsc --noEmit` (frontend type-check).
- `pytest backend/tests/test_<последняя_фаза>.py`.

Это «CI для одного» — без сервера, но с теми же гарантиями. **Когда проект растёт → переезжаем на GitHub Actions** без изменения логики (те же команды, только бегут в облаке).

## Защита от типовых проблем

| Проблема | Решение в CI/CD |
|----------|-----------------|
| «Работает у меня, не работает у тебя» | Docker контейнеризирует окружение |
| Долгий feedback loop | Параллельные runners в pipeline |
| Секреты в коде | GitHub Secrets / Vault, не в `.env` |
| «Pipeline стал медленным» | Кэширование зависимостей, инкрементальная сборка |
| «Тесты иногда падают рандомно (flaky)» | Карантин flaky-тестов в отдельную группу, чинить отдельно |

## Антипаттерны

❌ **`--no-verify` для обхода pre-commit** — обходишь именно те проверки, которые ловят баги.

❌ **Pipeline без auto-rollback** — если деплой сломал прод, нужна команда «откатить за минуту».

❌ **Один большой job на 30 минут** — разбивай на параллельные.

❌ **CI запускает E2E на каждый PR** — медленно. E2E — только на merge в main или nightly.

❌ **Деплой ручной, потому что «страшно автоматизировать»** — если страшно, значит pipeline не покрывает рисков. Чинить pipeline, не возвращаться к ручному.

## Связанное

- [test-pyramid](test-pyramid.md) — какие тесты на каком этапе pipeline бегут
- [bug-tracking](bug-tracking.md) — пайплайн репортит баги в трекер автоматически
- Skill: [ski-coding §7](../knowledge/skills/ski-coding.md) — pre-commit как «локальный CI»
- Плейбук: [13-quality-management](../docs/playbooks/13-quality-management.md) §VIII
- ADR: [adr-001-default-stack](../docs/decisions/adr-001-default-stack.md) — стек по умолчанию
