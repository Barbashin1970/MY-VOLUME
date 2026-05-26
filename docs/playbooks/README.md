---
title: docs/playbooks/
description: 13 плейбуков жизненного цикла проекта — от идеи до роста + мета-плейбуки.
tags: [meta, docs, playbooks]
---

# docs/playbooks/

Сюда сложены плейбуки **жизненного цикла продукта** — извлечённый
опыт из LEYKA, RAGRAF, AI-SKLAD, GONKA, POLINOM, NSK_OpenData_Bot.

> **Главная точка входа:** [00-product-lifecycle.md](00-product-lifecycle.md)
> — мастер-карта 9 стадий с переходами и сигналами «готово».

---

## Список плейбуков

| Стадия | Файл | Эталон |
|--------|------|--------|
| 0 — мастер | [00-product-lifecycle](00-product-lifecycle.md) | — |
| 1 — IDEA.md | [01-idea-formalization](01-idea-formalization.md) | AI-SKLAD/IDEA.md |
| 2 — позиционирование | [02-niche-positioning](02-niche-positioning.md) | RAGRAF/docs/STRATEGY-POSITIONING.md |
| 3 — ТЭО/НИР | [03-nir-economics](03-nir-economics.md) | RAGRAF/docs/ragraf_economics_marketing_report.md |
| 4 — ТЗ (ГОСТ 19.201) | [04-tz-and-gost-19](04-tz-and-gost-19.md) | RAGRAF/docs/TZ_RAGRAF.md, LEYKA/docs/LEYKA_TZ_GOST19.md |
| 5 — архитектура | [05-architecture](05-architecture.md) | RAGRAF/docs/ARC.md (18 секций) |
| 6 — SKILL-NN серия | [06-skill-series](06-skill-series.md) | GONKA/SKILL-01…09 |
| 7 — комплект ГОСТ-19 | [07-gost-19-doc-set](07-gost-19-doc-set.md) | LEYKA/docs/LEYKA_*_GOST19.md (5 файлов) |
| 8 — README + лендинг | [08-readme-landing](08-readme-landing.md) | RAGRAF/README.md, POLINOM/README.md (билингвально) |
| 9 — устав / отчёт / рост | [09-charter-reports-growth](09-charter-reports-growth.md) | LEYKA/SKILL-PROJECT-CHARTER-AND-REPORTS, NSK/IQ-roadmap |
| **мета — Product Focus** | [10-product-focus-mapping](10-product-focus-mapping.md) | Концепт: [product-focus](../../concepts/product-focus.md) |
| **постфаза — эксплуатация и ТП** | [11-support-operations](11-support-operations.md) | Обезличенный регламент SaaS-компании v2.04 + [sla](../../concepts/sla.md) |
| **мета — 5 букв + 11 спринтов** | [12-product-strategy-curriculum](12-product-strategy-curriculum.md) | Курс «Стратегия развития продукта» v3.0 (обезличенный) + 10 концептов спринтов |

---

## Как пользоваться

1. **Стартуешь новый проект?** Открой [00-product-lifecycle](00-product-lifecycle.md)
   и [ADR-002](../decisions/adr-002-doc-set-per-project.md) — определись с категорией (A/B/C/D).
2. **На какой стадии застрял?** Открой соответствующий плейбук стадии —
   там структура документа, эталон, чек-лист.
3. **Не уверен, нужна ли стадия?** В плейбуке есть таблица «когда
   пропускать» и сигнал перехода в следующую.

## Связанное

- ADR: [adr-002-doc-set-per-project](../decisions/adr-002-doc-set-per-project.md)
- Шаблоны: [templates/](../../templates/README.md)
