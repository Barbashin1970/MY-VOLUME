---
title: concepts/
description: Атомарные понятия — глоссарий, который превращается в граф знаний.
tags: [meta, concepts]
---

# concepts/

Это твой **глоссарий** + сеть атомарных понятий. Один файл = один термин.

Идея — из метода Zettelkasten / «evergreen notes»: маленькие связанные
заметки лучше длинных документов, потому что граф находит неожиданные
аналогии.

> Эталон стиля, на который опереться — твой [`~/AI-SKLAD/glossary.md`](../../AI-SKLAD/glossary.md).
> Здесь — то же самое, но разбито по одному термину на файл, со ссылками.

---

## Что сюда писать

- Доменные термины: «SKU», «партия», «WMS», «KPI» (склад / LEYKA).
- Технические термины, которые часто объясняешь: «idempotency key», «ADR».
- Чужие термины с твоей трактовкой: «оптимистичная мутация по-нашему — это …».

## Что **не** писать

- Очевидные универсальные термины (`function`, `class`) — на это есть Wikipedia.
- Длинные статьи. Если файл > 1 экрана — разрежь на 2 термина и свяжи.

---

## Атомарность

**Один концепт — один файл.** Если в заметке два независимых термина:

```
❌ erp-wms-crm.md   (три термина в одном файле)

✅ erp.md           (ERP — что такое)
✅ wms.md           (WMS — что такое, ссылка «не путать с erp.md»)
✅ crm.md           (CRM — что такое, ссылка «не путать с erp.md»)
```

Граф между ними появляется через раздел `## Не путать с`.

## Имя файла

Краткое, kebab-case, без префиксов: `sku.md`, `optimistic-mutation.md`,
`adr.md`, `kanban.md`.

В frontmatter — `aliases:`, чтобы Obsidian находил термин и по
синонимам/русскому варианту.

## Шаблон

См. [../templates/concept.md](../templates/concept.md).

## Как этим пользуется Claude

Когда ты пишешь «сделай SHACL-bridge», Claude читает
`concepts/shacl.md` → понимает контекст → не путает с похожим. Поэтому
качественные `concepts/` ускоряют ответы AI.

## Как сюда попадает термин

| Источник | Триггер |
|----------|---------|
| `projects/<x>/...` | Объяснял термин впервые — вынеси сюда |
| `meetings/...` | Прозвучал новый термин — заведи карточку |
| `~/<PROJECT>/...` (код) | В коде встретил незнакомое имя из домена — кратко зафиксируй |
| Книга / статья | Если применимо к твоей работе |

---

## Тематические группы

### Методологическая база
- [zadachny-podhod](zadachny-podhod.md) — задачный подход (Гончаров-Свириденко)
- [tfs-anokhina](tfs-anokhina.md) — ТФС Анохина
- [sigma-methodology](sigma-methodology.md) — Сигма-методология
- [product-focus](product-focus.md) — Product Focus (российская гибкая методология PM)

### Продуктовая стратегия и исследования
- [jtbd](jtbd.md) — Jobs To Be Done
- [vpc](vpc.md) — Value Proposition Canvas (Остервальдер)
- [lean-canvas](lean-canvas.md) — 9-блочный канвас Эша Маурьи
- [mom-test](mom-test.md) — правила интервью Роба Фитцпатрика
- [bartle-psychotypes](bartle-psychotypes.md) — сегментация по мотивам, не по демографии
- [moores-chasm](moores-chasm.md) — пропасть Мура между ранним и массовым рынком

### Метрики и экономика
- [aarrr-funnel](aarrr-funnel.md) — пиратская воронка метрик
- [unit-economics](unit-economics.md) — TAM/SAM/SOM, ARPU, ARPPU, CAC, LTV

### Бэклог и приоритизация
- [prioritization-frameworks](prioritization-frameworks.md) — MoSCoW, RICE, ICE, Kano, WSJF, ABC-XYZ
- [anti-scope](anti-scope.md) — список «что НЕ делаем»

### Рост и фасилитация
- [plg](plg.md) — Product-Led Growth
- [six-thinking-hats](six-thinking-hats.md) — 6 шляп Эдварда де Боно

### Управление и команды
- [team-models](team-models.md) — Tuckman / DISC / PAEI / Belbin / Servant / McKinsey 7S
- [sla](sla.md) — Service Level Agreement

### Документация и финансы (российская специфика)
- [gost-19](gost-19.md) — ЕСПД, стандарт документации ПО
- [kosgu](kosgu.md) — статьи бюджета для гос/корп
- [iq-roadmap](iq-roadmap.md) — NSK-модель роста системы

### Качество, тестирование, DevOps
- [test-pyramid](test-pyramid.md) — юнит / интеграционные / E2E / ручные
- [test-design-techniques](test-design-techniques.md) — черновик-чистовик, классы эквивалентности, туры
- [bug-tracking](bug-tracking.md) — severity vs priority, статусы
- [ci-cd](ci-cd.md) — непрерывная интеграция и доставка
