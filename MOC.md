---
title: Map of Content
description: Главная карта vault'а — список ключевых заметок, обновляется руками.
tags: [meta, moc]
---

# MOC — Map of Content

Главная карта vault'а. В Obsidian работает как «домашняя страница».
Здесь — только ссылки на важное, без содержания.

---

## 🚀 Жизненный цикл проекта (главное)

**Мастер-плейбук:** [docs/playbooks/00-product-lifecycle.md](docs/playbooks/00-product-lifecycle.md)

9 стадий + 2 мета-плейбука:

1. [01 — Формализация идеи (IDEA.md, 6 вопросов)](docs/playbooks/01-idea-formalization.md)
2. [02 — Поиск ниши и позиционирование](docs/playbooks/02-niche-positioning.md)
3. [03 — ТЭО / НИР / экономика-маркетинг](docs/playbooks/03-nir-economics.md)
4. [04 — Техническое задание (ГОСТ 19.201)](docs/playbooks/04-tz-and-gost-19.md)
5. [05 — Архитектура (ARC.md, 18 секций)](docs/playbooks/05-architecture.md)
6. [06 — Нарезка реализации (SKILL-NN-*.md)](docs/playbooks/06-skill-series.md)
7. [07 — Комплект документации ГОСТ-19](docs/playbooks/07-gost-19-doc-set.md)
8. [08 — README + лендинг](docs/playbooks/08-readme-landing.md)
9. [09 — Устав / отчёт / IQ-roadmap / рост](docs/playbooks/09-charter-reports-growth.md)
10. **[10 — Mapping Product Focus ↔ 9 стадий](docs/playbooks/10-product-focus-mapping.md)** — какие инструменты PM-методологии (JTBD, Lean Canvas, OKR…) применять на каждой стадии
11. **[11 — Эксплуатация и техподдержка (постфаза)](docs/playbooks/11-support-operations.md)** — после релиза: роли ТП, SLA, статусы запросов, service desk
12. **[12 — 5 букв + 11 спринтов](docs/playbooks/12-product-strategy-curriculum.md)** — современная PM-канва (Business / UX / Tech / Marketing / Management) и детальная последовательность работы с продуктом по 11 спринтам
13. **[13 — управление качеством + ski-coding](docs/playbooks/13-quality-management.md)** — классический QA-цикл (Test Analysis / Design / Execution / Automation / Monitoring) × 9 стадий ЖЦ × адаптация под соло-режим
14. **[14 — Курс «От идеи к прототипу с ИИ-агентом»](docs/playbooks/14-pair-programming-course-for-analysts.md)** — 10-урочный видеокурс для начинающих продукт-аналитиков: установка VS Code + ИИ-агент (GigaCode / Gemini / Copilot / Claude Code) → discovery → HTML/React/Python-прототип → Vercel-деплой → передача команде разработки

---

## 📦 Проекты

### Активные

| Проект | Категория | Стек | Эталон в |
|--------|-----------|------|----------|
| [LEYKA](projects/active/leyka/README.md) | A — заказной | FastAPI + React | Стек по умолчанию, полный комплект ГОСТ-19, КОСГУ/Устав |
| [RAGRAF](projects/active/ragraf/README.md) | A — заказной | FastAPI + React + DuckDB | Задачный подход, SKILL-GOST-19, позиционирование |
| [AI-SKLAD](projects/active/ai-sklad/README.md) | D — демо | Python static | IDEA.md / 6 вопросов |
| [SOFIA — Арома София](projects/active/sofia/README.md) | D — продакшен-демо | React+Vite SVG | Ароматерапевт «Цветок Мудрости» (Юнг+нумерология), Vercel |
| [STROY](projects/active/stroy/README.md) | D — демо | Python Streamlit | Text-to-SQL по охране труда, RDF-онтология |
| [demo-sigma](projects/active/demo-sigma/README.md) | C — научно-инструментальный | React 19 + Vite 8 | Sigma-оператор ЕДДС, видеостена, тренажёр |
| [GONKA](projects/active/gonka/README.md) | B — pet | PWA Vite + React 19 | Серия SKILL-01…09 |
| [KNIFFEL](projects/active/kniffel/README.md) | B — pet | PWA | SKILL-структура |
| [POLINOM](projects/active/polinom/README.md) | C — научно-инструментальный | Python CLI | Билингвальный README, sigma-методология |
| [POLINOM-JAVA](projects/active/polinom-java/README.md) | C — научно-инструментальный | Java + JMH | Перенос правил Сигма на JVM |
| [NSK_OpenData_Bot](projects/active/nsk-opendata-bot/README.md) | C — научно-инструментальный | Python bot | IQ-roadmap паттерн |
| [CWS-IDEA-SCOUTING](projects/active/cws-idea-scouting/README.md) | B — внутренний инструмент | Claude Code Skill + Python | 9-гейтовая SEO-воронка отбора идеи CWS, scoring-таблица |
| [PRECONSTRUCTION](projects/active/preconstruction/README.md) | B — внутренний инструмент | Claude Code Skill (RU/EN) | Пресейл-исследование строй/ИТ-идей: 5 этапов, потоки-агенты + аудит, вердикт «нужен ли ИИ ВНУТРИ» |

### Архив
- _проекты, которые завершены или приостановлены_

---

## 🧭 Ключевые решения (ADR)

- [ADR-001 — Дефолтный стек для веб-проектов](docs/decisions/adr-001-default-stack.md) (FastAPI + React + Vite + Tailwind + TanStack Query + Zustand + SQLite)
- [ADR-002 — Дефолтный комплект документации на проект](docs/decisions/adr-002-doc-set-per-project.md) (категории A/B/C/D)
- [ADR-003 — Билингвальный README](docs/decisions/adr-003-bilingual-readme.md) (рус + англ)

Все ADR: [docs/decisions/](docs/decisions/README.md)

---

## 🧠 Концепты (глоссарий, разрастающийся в граф)

### Методологическая база

- [Задачный подход (Гончаров-Свириденко)](concepts/zadachny-podhod.md) — 4 компонента задачи. Базис RAGRAF.
- [ТФС Анохина](concepts/tfs-anokhina.md) — 6 стадий деятельности.
- [Сигма-методология](concepts/sigma-methodology.md) — полиномиальная сложность. Базис POLINOM.
- **[Product Focus](concepts/product-focus.md)** — гибкая методология продуктового менеджмента (4 модели). Дополняет 9 стадий ЖЦ.
- [Preconstruction = Pre-Sales](concepts/preconstruction.md) — стройка-preconstruction и ИТ-pre-sales: одна фаза ЖЦ ДО реализации под двумя именами.

### Продуктовая стратегия и исследования

- [JTBD — Jobs To Be Done](concepts/jtbd.md) — главный инструмент фокуса «Клиент».
- [VPC — Value Proposition Canvas](concepts/vpc.md) — карта ценности + профиль потребителя (Остервальдер).
- [Lean Canvas](concepts/lean-canvas.md) — одностраничная бизнес-модель.
- [Mom Test](concepts/mom-test.md) — правила интервью Роба Фитцпатрика.
- [Bartle-психотипы](concepts/bartle-psychotypes.md) — сегментация по мотивам.
- [Пропасть Мура](concepts/moores-chasm.md) — кривая принятия инноваций.
- [Критерии необходимости ИИ](concepts/ai-necessity-criteria.md) — нужен ли ИИ ВНУТРИ продукта: «создано С ПОМОЩЬЮ ИИ» vs «содержит ИИ ВНУТРИ».

### Метрики, экономика, рост

- [AARRR — пиратская воронка](concepts/aarrr-funnel.md) — Acquisition / Activation / Retention / Revenue / Referral.
- [Юнит-экономика](concepts/unit-economics.md) — TAM/SAM/SOM, ARPU, ARPPU, CAC, LTV.
- [PLG — Product-Led Growth](concepts/plg.md) — продукт сам себя продаёт.

### Управление, приоритизация, фасилитация

- [Фреймворки приоритизации](concepts/prioritization-frameworks.md) — MoSCoW, RICE, ICE, Kano, WSJF, ABC-XYZ.
- [Модели команд](concepts/team-models.md) — Tuckman / DISC / PAEI / Belbin / Servant / McKinsey 7S.
- [6 шляп Эдварда де Боно](concepts/six-thinking-hats.md) — facilitation-метод.
- [SLA — Service Level Agreement](concepts/sla.md) — соглашение об уровне обслуживания.

### Качество, тестирование, DevOps

- [Пирамида тестирования](concepts/test-pyramid.md) — юнит → интеграционные → E2E → ручные.
- [Техники тест-дизайна](concepts/test-design-techniques.md) — черновик-чистовик, классы эквивалентности, туры.
- [Баг-трекинг](concepts/bug-tracking.md) — серьёзность vs приоритет, статусы.
- [CI/CD](concepts/ci-cd.md) — непрерывная интеграция и доставка.

### Документация

- [ГОСТ-19 (ЕСПД)](concepts/gost-19.md) — российский стандарт документации ПО.
- [Антископ](concepts/anti-scope.md) — список «что НЕ делаем».

### Финансы и рост

- [КОСГУ](concepts/kosgu.md) — статьи бюджета для гос/корп проектов.
- [IQ-roadmap](concepts/iq-roadmap.md) — модель роста системы (NSK-стиль).

Все концепты: [concepts/](concepts/README.md)

---

## 📚 Knowledge — переиспользуемое знание

Структура: [knowledge/README.md](knowledge/README.md)

- [knowledge/languages/](knowledge/languages/) — идиомы языков
- [knowledge/stacks/](knowledge/stacks/) — рабочие стеки (FastAPI+React, PWA, Java)
- [knowledge/patterns/](knowledge/patterns/) — переносимые архитектурные приёмы + **[gated-funnel-scoring](knowledge/patterns/gated-funnel-scoring.md)** (воронка гейтов + scoring-таблица для отбора кандидатов)
- [knowledge/skills/](knowledge/skills/) — атомарные рецепты + **[discovery-interview](knowledge/skills/discovery-interview.md)** (опрос Олега перед новым проектом) + **[ski-coding](knowledge/skills/ski-coding.md)** (методология соло-разработки с LLM, ноу-хау Олега) + **[preconstruction-research](knowledge/skills/preconstruction-research.md)** (указатель на пресейл-скиллы `preconstruction-ru`/`-en`: диалог → параллельные агенты → аудит → пресейл-отчёт) + **[adversarial-citation-factcheck](knowledge/skills/adversarial-citation-factcheck.md)** (проверка списка научных ссылок per-source агентами со скептическим дефолтом и сырым arXiv API)
- [knowledge/tools/](knowledge/tools/) — внешние инструменты (Claude Code, Obsidian, Vercel)
- [knowledge/lessons/](knowledge/lessons/) — **уроки, выученные на боль** (читай первым на старте нового проекта)

---

## 👥 Люди и встречи

- [people/](people/README.md) — карточки коллег и заказчиков
- [meetings/](meetings/README.md) — журнал встреч

---

## 🗂 Утилитарные папки

- [templates/](templates/README.md) — шаблоны новых заметок
- [inbox/](inbox/README.md) — быстрый сброс мыслей (разгребается раз в неделю)
- [assets/](assets/README.md) — картинки, PDF
