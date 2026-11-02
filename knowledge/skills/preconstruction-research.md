---
title: Preconstruction research — пресейл-исследование идеи многопоточными агентами
description: Skill-указатель на навыки preconstruction-ru/-en в ~/PRECONSTRUCTION — диалог-скоупинг → потоки-исследователи → скептический аудит → единый пресейл-отчёт с питчем и вердиктом по ИИ. Ставятся плагином Claude Code, архивами для claude.ai и Perplexity, установщиком. Код живёт в проекте; здесь — когда/зачем/как.
tags: [skill, preconstruction, presale, research, multi-agent, ai-necessity, ru, en]
origin: projects/active/preconstruction (навыки preconstruction-ru / preconstruction-en)
adapted_for: пресейл строительных и ИТ-для-стройки идей (методология «5 этапов пресейла с ИИ», мини-хакатон НГАУДИ)
created: 2026-06-10
updated: 2026-11-02
related:
  - knowledge/skills/discovery-interview.md
  - knowledge/skills/ski-coding.md
  - knowledge/lessons/when-to-use-breaks-claude-ai-upload.md
  - concepts/preconstruction.md
  - concepts/ai-necessity-criteria.md
  - concepts/anti-scope.md
  - docs/playbooks/02-niche-positioning.md
---

# Skill: Preconstruction research

> **Это указатель, а не копия.** Реальные навыки живут в проекте `~/PRECONSTRUCTION`; vault не
> дублирует код — описывает когда/зачем/как и куда смотреть. Правишь поведение — правь в проекте.

## Где живёт сам skill

- `~/PRECONSTRUCTION/skills/preconstruction-ru/SKILL.md` — русская версия;
  `~/PRECONSTRUCTION/skills/preconstruction-en/SKILL.md` — английское зеркало.
  `.claude/skills/` проекта — симлинки на них.
- `.../references/` у каждого — 5 файлов: `interview-protocol`, `research-roles`,
  `ai-necessity-criteria`, `audit-checklist`, `report-template`; `.../examples/` — 3 эталона.
- `~/PRECONSTRUCTION/knowledge-base/` (RU) и `knowledge-base-en/` (EN) — методология как учебник;
  навыку она не нужна, навык самодостаточен.

## Когда применять

**Триггер** — нужно проработать/исследовать/упаковать проектную идею (строительную или
ИТ-для-стройки), оценить рынок/конкурентов/реализуемость или решить, **нужен ли ИИ ВНУТРИ
продукта**.

**Не применять**, когда:
- идея уже исследована и упакована;
- это фаза кодинга (тогда [ski-coding](ski-coding.md));
- это формализация сырой идеи Олега под его vault (тогда [discovery-interview](discovery-interview.md)).

## Что делает (4 фазы)

- **Фаза 0 — диалог/скоупинг:** Restate + 2–3 волны вопросов, выбор глубины
  «быстрая»/«полная», антископ, нормативный контекст РФ.
- **Фаза 1 — потоки-исследователи:** A Бизнес-аналитик (этапы 1–2), B Маркетолог (этап 2),
  C Системный аналитик · техэкспертиза (этап 3), D Системный аналитик · применимость ИИ (этап 4).
  Ограничения работы (без подагентов, бюджет поиска) и пометки провенанса у каждого утверждения.
- **Фаза 2 — скептический аудит:** числа/источники, провенанс главного факта, противоречия
  между потоками, пробелы, перепроверка вердикта по ИИ.
- **Фаза 3 — сведение** в `research/<слаг>-YYYY-MM-DD.md` (в чатах — файлом или ответом).

Методология = **5 этапов пресейла**: идентификация → бизнес-анализ → техэкспертиза →
[применимость ИИ](../../concepts/ai-necessity-criteria.md) → упаковка.

## Центральный принцип

> ИИ — инструмент **исследования**, а не обязательный компонент продукта. Разделяй «создано
> С ПОМОЩЬЮ ИИ» (вайб-кодинг) и «содержит ИИ ВНУТРИ» (ML в проде). Честность важнее «крутости ИИ».

## Как поставить и запустить

- **Claude Code, любая ОС:** `/plugin install preconstruction --marketplace Barbashin1970/PRECONSTRUCTION`
  → навыки `/preconstruction:preconstruction-ru` и `/preconstruction:preconstruction-en`.
- **claude.ai / приложение Claude:** архив `zip/claude/…`; **Perplexity:** архив `zip/perplexity/…`.
- **Codex и другие агенты:** `install.sh` / `install.ps1` → `~/.claude/skills`, `~/.agents/skills`.
- В самом `~/PRECONSTRUCTION` навыки видны и без установки: `/preconstruction-ru`.

Дальше — «проработай идею: <описание>». Навык сам ведёт диалог и запускает потоки.

## Чем отличается от discovery-interview

| | discovery-interview | preconstruction-research |
|---|---|---|
| Цель | Формализовать идею Олега в IDEA.md (внутреннее) | Пресейл-исследование внешней/клиентской идеи (deliverable) |
| Выход | Черновик в `inbox/` его vault | `research/<слаг>-<дата>.md` + питч в проекте |
| Движок | Интервью в одном агенте | Интервью + многопоточный ресёрч + аудит |

Оба начинаются с диалога и pushback, но дают разные артефакты. Если вердикт «делаем С ПОМОЩЬЮ
ИИ» → следующий шаг прототип, где применяется [ski-coding](ski-coding.md).

## Известные подводные камни

- Параллельные `Agent`-вызовы должны идти в **одном** сообщении, иначе нет реальной параллельности;
  где `Agent` нет (claude.ai, Perplexity), потоки идут по очереди — фазы не пропускаются.
- Не путай глубину: «быстрая» (≈4 потока) vs «полная» (+ глубокий поиск + аудит в 2 голоса).
- Дату в имени файла отчёта не выдумывай — бери из среды или спроси.
- Шапка `SKILL.md` — только шесть полей спецификации, описание до 1024 байт: иначе отказ
  claude.ai или Perplexity ([урок](../lessons/when-to-use-breaks-claude-ai-upload.md)).
  После правки навыка — `python3 tools/build_zips.py` и `python3 tools/check.py`.
- Это исследовательская фаза — **не писать код продукта**.

## Связанное

- Skill: [discovery-interview](discovery-interview.md) — родственный интервью-паттерн (сиблинг)
- Skill: [ski-coding](ski-coding.md) — следующий шаг, если решено делать прототип
- Урок: [when-to-use-breaks-claude-ai-upload](../lessons/when-to-use-breaks-claude-ai-upload.md)
- Концепты: [preconstruction](../../concepts/preconstruction.md), [ai-necessity-criteria](../../concepts/ai-necessity-criteria.md), [anti-scope](../../concepts/anti-scope.md)
- Проект: [projects/active/preconstruction](../../projects/active/preconstruction/README.md)
