---
title: PRECONSTRUCTION
description: Методология и два зеркальных навыка агента (RU/EN) для качественного пресейл-исследования строительных и ИТ-для-стройки идей: диалог → потоки-исследователи → скептический аудит → единый отчёт с питчем. С версии 1.1.0 — плагин Claude Code, архивы для claude.ai и Perplexity, установщики для macOS/Linux/Windows.
tags: [project, status/active, category/B, skill, plugin, preconstruction, presale]
status: active
category: B
repo: ~/PRECONSTRUCTION
remote: https://github.com/Barbashin1970/PRECONSTRUCTION
deployed: GitHub (публичный) — плагин `preconstruction@preconstruction-skills`, архивы zip/claude и zip/perplexity
license: тексты и навыки CC BY 4.0, код MIT
created: 2026-06-10
updated: 2026-11-02
related:
  - knowledge/skills/preconstruction-research.md
  - knowledge/skills/discovery-interview.md
  - knowledge/lessons/when-to-use-breaks-claude-ai-upload.md
  - knowledge/tools/perplexity-skill-description-byte-limit.md
  - concepts/preconstruction.md
  - concepts/ai-necessity-criteria.md
  - concepts/anti-scope.md
  - docs/playbooks/02-niche-positioning.md
---

# PRECONSTRUCTION

> **Где код:** `~/PRECONSTRUCTION/`, git-репозиторий, публичный на GitHub
> (`Barbashin1970/PRECONSTRUCTION`).
> **Что это:** база знаний + два зеркальных навыка агента (`preconstruction-ru` /
> `preconstruction-en`) в открытом формате Agent Skills. На вход — сырая проектная идея;
> на выход — пресейл-отчёт с питчем и вердиктом «нужен ли ИИ ВНУТРИ продукта».
> **Категория B** — внутренний инструмент/методология и материал курса «ИИ в строительстве».

## Зачем

ИИ на стадии **preconstruction / presale** — ДО начала реализации, где решается 70–80%
успеха проекта и предотвращается 30–40% перерасхода. Центральный принцип: ИИ — инструмент
**исследования и упаковки идеи**, а не обязательный компонент продукта. Жёстко разделяем
«создано **С ПОМОЩЬЮ** ИИ» (вайб-кодинг) ≠ «содержит ИИ **ВНУТРИ**» (ML в проде). Собрано из
трёх материалов НГУ/НГАУДИ (мини-хакатон для архитекторов-градостроителей).

## Что внутри (What)

- **`skills/preconstruction-ru/`** и **`preconstruction-en/`** — единственный источник навыков:
  `SKILL.md` + `references/` из 5 файлов (interview-protocol, research-roles,
  ai-necessity-criteria, audit-checklist, report-template) + `examples/` (3 эталонных отчёта).
  `.claude/skills/` — симлинки на них.
- **`.claude-plugin/`** — плагин `preconstruction` и маркетплейс `preconstruction-skills`
  (`source: "./"` — плагин = весь репозиторий).
- **`zip/claude/`**, **`zip/perplexity/`** — архивы двух раскладок; собирает
  `tools/build_zips.py`, сверяет `tools/check.py`.
- **`install.sh`**, **`install.ps1`** — ссылки или копии в `~/.claude/skills` и `~/.agents/skills`.
- **`evals/`** — три проверки поведения для `claude plugin eval` (ещё не прогонялись).
- **`knowledge-base/`**, **`knowledge-base-en/`** — учебник: 8 заметок + `sources/`.
- **`docs/`** — `PAKET-RAZBOR.md` (разбор упаковки и решения Р-1…Р-7), `USE-CASES.md`, `BEKLOG.md`.

## Пять этапов пресейла (ядро)

| # | Этап | Суть |
|---|------|------|
| 1 | Идентификация проблемы | кто страдает, масштаб, потери |
| 2 | Бизнес-анализ | рынок, конкуренты, SWOT, TAM/SAM/SOM |
| 3 | Техэкспертиза | стек, нормативы (СП/ГОСТ/ФЗ), риски |
| 4 | **Применимость ИИ** (ключевой) | нужен ли ИИ ВНУТРИ продукта |
| 5 | Упаковка в питч | презентация + вердикт по ИИ |

Центральный вердикт всей методологии — этап 4: «нужен ли ИИ ВНУТРИ: ДА/НЕТ» с обоснованием.

## Механика навыка

Диалог → потоки-исследователи → аудит → сведение. Все четыре фазы обязательны в любой среде;
меняется способ:

- **Claude Code** — потоки A–D параллельно через `Agent`, вопросы — `AskUserQuestion`, отчёт —
  `research/<слаг>-<дата>.md`.
- **claude.ai, Perplexity, другие чаты** — потоки по очереди своими разделами, аудит — отдельным
  проходом, вопросы текстом, отчёт файлом или ответом.
- Потоки работают с **ограничениями** (не порождать подагентов, бюджет поиска) и ставят
  **пометки провенанса** `[первичный]` / `[из выдачи]` / `[vendor-claim]` / `[гипотеза]` —
  по уроку [search-agent-fanout-without-provenance](../../../knowledge/lessons/search-agent-fanout-without-provenance.md).

## Как поставить и использовать

| Среда | Как |
|---|---|
| Claude Code (любая ОС, терминал, VS Code, JetBrains) | `/plugin install preconstruction --marketplace Barbashin1970/PRECONSTRUCTION` |
| claude.ai, приложение Claude | архив `zip/claude/…` → Customize → Skills → Upload a skill |
| Perplexity | архив `zip/perplexity/…` → Настройка → Навыки → Загрузить навык |
| Codex, другие агенты | `install.sh` / `install.ps1` |

Дальше — «проработай идею: …». Для слушателей — `STUDENT-SETUP.md` в репозитории.
Выпуск новой версии: поднять `version` в `plugin.json` и `metadata.version` в обоих `SKILL.md`,
`python3 tools/build_zips.py`, `python3 tools/check.py`, `CHANGELOG.md`.

## Load-bearing инсайты

- **Честность важнее «крутости ИИ»** — признать, что классика лучше, ценится выше всего.
- **«Двойной пресейл»** для ИТ-для-стройки: уровень 1 — строительная проблема, уровень 2 — ИТ-решение.
- **Пределы LLM** (кейс с чертежами фасадов): для точной техработы — специализированные
  платформы (KREO, Togal.AI, Bluebeam Revu), не LLM «в лоб».
- **Упаковка (2026-11-02):** одна шапка `SKILL.md`, годная везде, — только шесть полей
  спецификации и описание до 1024 байт; `when_to_use` ломает загрузку в claude.ai
  ([урок](../../../knowledge/lessons/when-to-use-breaks-claude-ai-upload.md)). Архив для claude.ai —
  папка навыка в корне, для Perplexity — `SKILL.md` в корне.
- Самый свежий Claude Code на машине — внутри расширения VS Code; `claude` в PATH отставал (2.1.63).

## Открыто (бэклог проекта)

Загрузка в claude.ai и Perplexity руками автора, прогон `install.ps1` на Windows, прогон
`claude plugin eval`, проверка на телефоне, иконка для каталога плагинов Anthropic.

## Связанное

- Skill: [preconstruction-research](../../../knowledge/skills/preconstruction-research.md) — указатель на эти навыки
- Skill: [discovery-interview](../../../knowledge/skills/discovery-interview.md) — родственный интервью/много-агентный паттерн
- Уроки: [when-to-use-breaks-claude-ai-upload](../../../knowledge/lessons/when-to-use-breaks-claude-ai-upload.md), [finder-zip-ships-ignored-files](../../../knowledge/lessons/finder-zip-ships-ignored-files.md)
- Инструмент: [perplexity-skill-description-byte-limit](../../../knowledge/tools/perplexity-skill-description-byte-limit.md)
- Концепты: [preconstruction](../../../concepts/preconstruction.md), [ai-necessity-criteria](../../../concepts/ai-necessity-criteria.md), [anti-scope](../../../concepts/anti-scope.md), [jtbd](../../../concepts/jtbd.md), [unit-economics](../../../concepts/unit-economics.md)
- Playbook: [02-niche-positioning](../../../docs/playbooks/02-niche-positioning.md)
