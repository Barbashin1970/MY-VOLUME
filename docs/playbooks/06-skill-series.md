---
title: Стадия 6 — Нарезка реализации серией SKILL-NN-*.md
description: Главный паттерн нарезки работы Олега. Эталон — GONKA/SKILL-01 … SKILL-09.
tags: [doc, playbook, lifecycle, stage-6, skills]
status: living
updated: 2026-05-26
---

# Стадия 6 — Нарезка реализации (SKILL-NN-*.md)

**Вход:** ARC.md закрыт.
**Выход:** серия `SKILL-01-idea.md … SKILL-NN-<последняя-фича>.md`.
**Время:** скилл пишется в начале выполнения соответствующего блока.
**Эталон:** `~/GONKA/SKILL-01-idea.md` … `SKILL-09-demo-mode.md`,
`~/SOFIA/SKILL-01..06`, `~/KNIFFEL/SKILL-01..06`.

> Это **главный паттерн** Олега: вместо одного огромного TODO-листа —
> упорядоченная серия SKILL-файлов, каждый закрывает одну фичу /
> вертикаль / этап.

---

## Зачем серия SKILL-NN-*.md

| Аспект | Один TODO/BACKLOG | Серия SKILL-NN |
|--------|-------------------|----------------|
| Видимость прогресса | Один файл растёт и тонет в правках | Каждый SKILL = веха, видна стадия |
| Контекст для AI | Размытый, всё в одной куче | Узкий, точечный — «возьми SKILL-04 и сделай» |
| Возврат через год | Не вспомнишь логику | Откроешь SKILL-06 — увидишь как было |
| Возврат к фиче | Сложно найти, что обсуждалось | SKILL-NN — единая точка по фиче |
| Промоутирование | Не вытащить | Вытаскивается в `knowledge/skills/` целиком |

---

## Канонический порядок (по GONKA)

```
SKILL-01-idea.md          ← формулировка продукта, user stories, доменная модель
SKILL-02-setup.md         ← инфраструктура: Vite, CI, базовые папки, scripts
SKILL-03-core.md          ← основная логика (для игры — game-loop, state machine)
SKILL-04-ui.md            ← UI-каркас (меню, экраны, маршрутизация)
SKILL-05-polish.md        ← полировка: анимации, звуки, i18n, dark theme
SKILL-06-<advanced>.md    ← главная advanced-фича (для GONKA: AI player)
SKILL-07-<feature>.md     ← следующая фича (simulator)
SKILL-08-<feature>.md     ← (multiplayer)
SKILL-09-demo-mode.md     ← demo-режим / витрина продукта
```

Для не-игр (LEYKA / RAGRAF) серия именуется иначе:

```
LEYKA/docs/SKILL-AUTH.md
LEYKA/docs/SKILL-DevSecOps.md
LEYKA/docs/SKILL-INFRA.md
LEYKA/docs/SKILL-TESTPLAN.md
LEYKA/docs/SKILL-MERMAID.md
LEYKA/docs/SKILL-PROJECT-CHARTER-AND-REPORTS.md
LEYKA/docs/SKILL-PWA.md
LEYKA/docs/SKILL-RELEASE.md
LEYKA/docs/SKILL-UI-Master.md
LEYKA/docs/SKILL-WIKI.md
LEYKA/docs/SKILL-KANBAN-COMPACT-VIEW.md
```

То есть скилл = **вертикаль / фича / слой**, а не порядковый шаг.

---

## Структура внутри одного SKILL-файла

Стандартный шаблон (по GONKA/SKILL-01-idea.md):

```markdown
# SKILL-NN: <Название>

> Руководство для разработчика проекта по <чему>.

## Фаза 1: Описание

### Продукт в одном абзаце
<абзац>

### User Stories
- Как X, я хочу Y, чтобы Z.
- …

### Доменная модель
\`\`\`
ClassName
├── field1: type
└── field2: type
\`\`\`

## Фаза 2: Стек технологий
| Задача | Технология | Почему |
| … | … | … |

## Фаза 3: <Платформы / Реализация / Алгоритмы>
<содержание>

## Контрольный список фазы 1
- [x] CLAUDE.md создан
- [ ] Доменная модель в types/
- [ ] …
```

Каждый SKILL заканчивается **контрольным списком**, который превращается
в реальный progress-tracker.

---

## Принципы

### 1. Один SKILL — одно осмысленное завершённое состояние

После закрытия SKILL-04-ui должна работать вся базовая навигация.
Не «начали 4 UI-задачи в разных направлениях».

### 2. SKILL пишется в момент старта работы над фичей

Не на старте проекта (не угадаешь все 9 SKILL'ов). Пишешь когда
открываешь следующий блок.

### 3. SKILL не редактируется после закрытия фазы

Превращается в «исторический документ». Если фича развивается —
**новый** SKILL с номером N+M, **не** правка старого.

### 4. SKILL-09-demo-mode — обязательно

Это финальная стадия любого проекта: **демо-режим / витрина**.
Без него продукт не показать. У GONKA это headless-game за 50мс,
бот против бота с лог-летой и виртуальной валютой ставок.

### 5. Тяжёлые SKILL разносятся на «-foundation», «-advanced»

Если SKILL > 500 строк, разбивай: `SKILL-06-ai-player-foundation.md` +
`SKILL-06-ai-player-evaluator.md`.

---

## Связь с другими стадиями

| SKILL-NN | Закрывает требования из |
|----------|-------------------------|
| SKILL-01-idea | IDEA.md + ТЗ §1-3 |
| SKILL-02-setup | ARC.md §1-2 |
| SKILL-03-core | ARC.md §3-6 (домен, DSL, ЖЦ) |
| SKILL-04-ui | ARC.md §17 (UI-фичи) |
| SKILL-NN-feature | ТЗ §3.N (требование к функциональности) |

---

## Промоутирование в knowledge/

Когда закрыл SKILL и понял «это применимо везде»:

```
projects/active/gonka/skills/06-ai-bot-evaluator.md  ← проектная версия с GONKA
                  │
              извлеки общее
                  ▼
knowledge/skills/ai-bot-evaluator-pattern.md  ← без GONKA, для SOFIA/KNIFFEL
```

Источник в проекте остаётся, абстрагированная версия живёт в
`knowledge/skills/` и переиспользуется.

---

## Связанное

- Предыдущая: [05-architecture](05-architecture.md)
- Следующая: [07-gost-19-doc-set](07-gost-19-doc-set.md)
- Шаблон: [templates/skill.md](../../templates/skill.md)
- knowledge: [knowledge/skills/](../../knowledge/skills/)
- Эталоны: `~/GONKA/SKILL-01…09`, `~/LEYKA/docs/SKILL-*.md`
