---
title: CWS-IDEA-SCOUTING
description: Claude Code Skill — интервью-методология отбора идеи для расширения Chrome Web Store через 9-гейтовую SEO-воронку с единой scoring-таблицей.
tags: [project, status/active, category/B, skill, seo]
status: active
category: B
repo: ~/CWS-IDEA-SCOUTING
deployed: локально (Claude Code Skill)
created: 2026-05-29
updated: 2026-05-29
related:
  - knowledge/patterns/gated-funnel-scoring.md
  - knowledge/skills/discovery-interview.md
  - docs/playbooks/02-niche-positioning.md
  - concepts/lean-canvas.md
  - concepts/unit-economics.md
  - concepts/aarrr-funnel.md
---

# CWS-IDEA-SCOUTING

> **Где код:** `~/CWS-IDEA-SCOUTING/` (не git-репозиторий — папка-скилл)
> **Что это:** Claude Code Skill, упаковывающий bootcamp-методологию выбора
> идеи для расширения Chrome Web Store. На вход — кандидаты-идеи или ключевые
> слова; на выход — scouting-таблица с вердиктом GO / NEEDS-WORK / NO-GO.
> **Категория B** — внутренний инструмент/методология (скилл), не клиентский продукт.

## Зачем

Выбор идеи для CWS-расширения упирается не в код, а в **SEO-нишу**: ключевое
слово становится именем расширения и определяет ~70% успеха в выдаче. Скилл
формализует жёсткий отбор «останавливайся рано»: каждый кандидат проходит
9 последовательных гейтов с hard-kill правилами, чтобы не тратить недели на
заведомо непроходную нишу.

## Что внутри (What)

- **`SKILL.md`** — точка входа: роль «ментора-интервьюера», карта 9 гейтов,
  load-bearing правила, дисциплина вывода (одна таблица `cws-scouting.md`).
- **`references/`** (15 файлов) — по файлу на гейт + доступ к Semrush +
  Chrome-Stats API + рубрики оценки + точные вопросы интервью.
- **`scripts/`** — 3 Python-инструмента: `calc_revenue.py`,
  `calc_world_traffic.py`, `chrome_stats.py` (Chrome-Stats API, 1 кредит/вызов).
- **`assets/`** — шаблон таблицы на 24 колонки, чек-лист, примеры GO/NO-GO.
- **`config/`** — ключ Chrome-Stats API.

## Девять гейтов (ядро)

| # | Гейт | Hard-kill |
|---|---|---|
| 1.1 | Idea sourcing | юзеров <10K → drop |
| 1.2 | Revenue potential | rev score ≤3 → drop |
| 1.3 | Simplification | не сводимо к 1 функции → **HARD STOP** |
| 1.4 | Keyword research | ниже порога объёма → drop |
| 1.5 | Name polishing | широкая <2K / узкая <500 US exact → drop |
| 1.6 | Softy SERP check | >30% несофтверных результатов → drop |
| 1.7 | Adjacent tails | хвосты шумные → root-only fallback |
| 1.8 | Competitor optimization | root занят оптимизированным конкурентом → аджасент |
| 1.9 | Keyword difficulty | scarlet KD + noisy → drop |

Итоговый score = `0.35·revenue + 0.25·impl + 0.25·KD + 0.15·traffic` минус
red-flag штрафы. Вердикт: **GO ≥7.5 / NEEDS-WORK 5.5–7.5 / NO-GO <5.5**.

## Load-bearing инсайты методологии

- Расширения **обгоняют сайты** в Google по software-intent запросам, но
  **не обгоняют оптимизированные расширения** на их корневом ключе → ниши/адженты.
- **500 US exact/мес жизнеспособно**: ×10 (англоязычные рынки) ×10 (переводы)
  ≈ 50K реального трафика — конкуренты это игнорируют.
- Порть **только веб-сервисы** (калькуляторы, конвертеры), не статьи и не маркетплейсы.
- Источники бери со **страниц 7–20** сортировки app-database.com, не с 1–3.
- **Meta-правило Зуева:** при двух кандидатах тестируй сначала нишевый
  (ниже KD/трафик); провалится нишевый — широкий тем более.

## Положение в экосистеме скиллов

```
cws-idea-scouting → cws-extension-development → cws-final-checks
   (отбор идеи)        (6 гейтов до кода)         (предзалив CWS)
```

Скилл — это операционализация **стадии 2 (поиск ниши и позиционирование)**
для конкретного канала (CWS). Стыкуется с [Lean Canvas](../../../concepts/lean-canvas.md),
[юнит-экономикой](../../../concepts/unit-economics.md) (revenue gate ≈ ARPU/LTV)
и [AARRR](../../../concepts/aarrr-funnel.md) (Acquisition через SEO-канал).

## Как использовать для исследований

1. **По назначению** — сказать «CWS idea scouting» и пройти интервью по 9 гейтам.
2. **Скрипты автономно** — `calc_world_traffic.py`, `calc_revenue.py`,
   `chrome_stats.py check` работают вне интервью как research-инструменты.
3. **Как рамка** — переносимый приём вынесен в
   [knowledge/patterns/gated-funnel-scoring.md](../../../knowledge/patterns/gated-funnel-scoring.md).

## Связанное

- Паттерн: [gated-funnel-scoring](../../../knowledge/patterns/gated-funnel-scoring.md)
- Skill: [discovery-interview](../../../knowledge/skills/discovery-interview.md) — родственный интервью-паттерн
- Playbook: [02-niche-positioning](../../../docs/playbooks/02-niche-positioning.md)
- Концепты: [lean-canvas](../../../concepts/lean-canvas.md), [unit-economics](../../../concepts/unit-economics.md), [aarrr-funnel](../../../concepts/aarrr-funnel.md)
