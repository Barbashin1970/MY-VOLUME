---
title: Preconstruction research — пресейл-исследование идеи многопоточными агентами
description: Skill-указатель на Claude Code скиллы preconstruction-ru/-en в ~/PRECONSTRUCTION — диалог-скоупинг → параллельные потоки-агенты → адверсариальный аудит → единый пресейл-отчёт с питчем и вердиктом по ИИ. Код живёт в проекте; здесь — когда/зачем/как.
tags: [skill, preconstruction, presale, research, multi-agent, ai-necessity, ru, en]
origin: projects/active/preconstruction (Claude Code скиллы preconstruction-ru / preconstruction-en)
adapted_for: пресейл строительных и ИТ-для-стройки идей (методология «5 этапов пресейла с ИИ», мини-хакатон НГАУДИ)
created: 2026-06-10
updated: 2026-06-10
related:
  - knowledge/skills/discovery-interview.md
  - knowledge/skills/ski-coding.md
  - concepts/preconstruction.md
  - concepts/ai-necessity-criteria.md
  - concepts/anti-scope.md
  - docs/playbooks/02-niche-positioning.md
---

# Skill: Preconstruction research

> **Это указатель, а не копия.** Реальные скиллы живут в проекте `~/PRECONSTRUCTION`; vault не
> дублирует код — описывает когда/зачем/как и куда смотреть. Правишь поведение — правь в проекте.

## Где живёт сам skill

- `~/PRECONSTRUCTION/.claude/skills/preconstruction-ru/SKILL.md` — русская версия (полнее).
- `~/PRECONSTRUCTION/.claude/skills/preconstruction-en/SKILL.md` — английское зеркало.
- `.../references/` у каждого — 4 файла: `interview-protocol`, `research-roles`,
  `audit-checklist`, `report-template`.
- `~/PRECONSTRUCTION/knowledge-base/` (RU) и `knowledge-base-en/` (EN) — методология как учебник.

## Когда применять

**Триггер** — нужно проработать/исследовать/упаковать проектную идею (строительную или
ИТ-для-стройки), оценить рынок/конкурентов/реализуемость или решить, **нужен ли ИИ ВНУТРИ
продукта**.

**Не применять**, когда:
- идея уже исследована и упакована;
- это фаза кодинга (тогда [ski-coding](ski-coding.md));
- это формализация сырой идеи Олега под его vault (тогда [discovery-interview](discovery-interview.md)).

## Что делает (4 фазы)

- **Фаза 0 — диалог/скоупинг:** Restate + 2–3 волны `AskUserQuestion`, выбор глубины
  «быстрая»/«полная», антископ, нормативный контекст РФ. См. `references/interview-protocol.md`.
- **Фаза 1 — параллельные потоки-агенты** (в одном сообщении): A Бизнес-аналитик (этапы 1–2),
  B Маркетолог (этап 2), C Системный аналитик ИИ · техэкспертиза (этап 3), D Системный аналитик
  ИИ · применимость ИИ (этап 4). См. `references/research-roles.md`.
- **Фаза 2 — адверсариальный аудит:** числа/источники, противоречия между потоками, пробелы,
  перепроверка вердикта по ИИ. См. `references/audit-checklist.md`.
- **Фаза 3 — сведение** в `research/<слаг>-YYYY-MM-DD.md`: executive summary, разбор 5 этапов,
  раздел «Применимость ИИ», питч, риски, журнал аудита. См. `references/report-template.md`.

Методология = **5 этапов пресейла**: идентификация → бизнес-анализ → техэкспертиза →
[применимость ИИ](../../concepts/ai-necessity-criteria.md) → упаковка.

## Центральный принцип

> ИИ — инструмент **исследования**, а не обязательный компонент продукта. Разделяй «создано
> С ПОМОЩЬЮ ИИ» (вайб-кодинг) и «содержит ИИ ВНУТРИ» (ML в проде). Честность важнее «крутости ИИ».

## Как запустить

Открой `~/PRECONSTRUCTION` в Claude Code → вызови `/preconstruction-ru` (или `/preconstruction-en`
по языку отчёта) → «проработай идею: <описание>». Скилл сам ведёт диалог и запускает агентов.
Никакого ручного кода здесь.

## Чем отличается от discovery-interview

| | discovery-interview | preconstruction-research |
|---|---|---|
| Цель | Формализовать идею Олега в IDEA.md (внутреннее) | Пресейл-исследование внешней/клиентской идеи (deliverable) |
| Выход | Черновик в `inbox/` его vault | `research/<слаг>-<дата>.md` + питч в проекте |
| Движок | Интервью в одном агенте | Интервью + многопоточный ресёрч + аудит |

Оба начинаются с диалога и pushback, но дают разные артефакты. Если вердикт «делаем С ПОМОЩЬЮ
ИИ» → следующий шаг прототип, где применяется [ski-coding](ski-coding.md).

## Известные подводные камни

- Параллельные `Agent`-вызовы должны идти в **одном** сообщении, иначе нет реальной параллельности.
- Не путай глубину: «быстрая» (≈4 потока) vs «полная» (+ deep-research + аудит в 2 голоса).
- Дату в имени файла отчёта не выдумывай — спроси у пользователя или оставь плейсхолдер.
- `knowledge-base/` русскоязычная, `knowledge-base-en/` — для иностранных студентов.
- Это исследовательская фаза — **не писать код продукта**.

## Связанное

- Skill: [discovery-interview](discovery-interview.md) — родственный интервью-паттерн (сиблинг)
- Skill: [ski-coding](ski-coding.md) — следующий шаг, если решено делать прототип
- Концепты: [preconstruction](../../concepts/preconstruction.md), [ai-necessity-criteria](../../concepts/ai-necessity-criteria.md), [anti-scope](../../concepts/anti-scope.md)
- Проект: [projects/active/preconstruction](../../projects/active/preconstruction/README.md)
