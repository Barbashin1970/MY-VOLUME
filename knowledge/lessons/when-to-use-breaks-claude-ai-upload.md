---
title: Поле when_to_use, спасшее навыки в Perplexity, ломает их загрузку в claude.ai
description: "Чтобы уложить русское описание навыка в 1024 байта Perplexity, хвост ключевых слов перенесли в when_to_use — его понимает Claude Code. Но claude.ai, Skills API и package_skill.py допускают в шапке SKILL.md только шесть полей (name, description, license, compatibility, metadata, allowed-tools) и на любом другом падают: «Unexpected key(s) in SKILL.md frontmatter: when_to_use». Валидатор Anthropic отверг навыки NSU-AI-SKILLS и PROMPT-Professor. Каждая приёмная среда — свой валидатор; одна шапка, годная везде, — описание до 1024 байт без when_to_use, и сторож на допустимые поля."
tags: [lesson, gotcha, skills, claude-code, agents]
severity: medium
project_origin: preconstruction
created: 2026-11-02
updated: 2026-11-02
---

# Урок: «поле, которое понимает одна среда, роняет загрузку в другой»

## Что произошло

2026-11-02, упаковка навыков PRECONSTRUCTION под требования Anthropic. Официальный
валидатор шапки — `quick_validate.py` из навыка skill-creator — прогнан на навыках двух
соседних пакетов, которые уже раздаются слушателям:

```bash
python3 -I ~/.claude/skills/synced/*/skill-creator/scripts/quick_validate.py ~/NSU-AI-SKILLS/skills/skicoding
# Unexpected key(s) in SKILL.md frontmatter: when_to_use.
# Allowed properties are: allowed-tools, compatibility, description, license, metadata, name
```

Тот же отказ у `~/PROMPT-Professor/skills/thinking-with-six-hats`. В обоих пакетах поле
`when_to_use` стоит у каждого навыка: архивы этих навыков claude.ai не примет. Вдобавок
оба сборщика кладут `SKILL.md` в корень архива, а claude.ai ждёт там папку навыка.

## Почему это случилось

- **Обход одного предела опёрся на расширение другой среды.** Perplexity меряет описание
  в байтах ([perplexity-skill-description-byte-limit](../tools/perplexity-skill-description-byte-limit.md)),
  и хвост ключевых слов переехал в `when_to_use`. Это поле — расширение **Claude Code**:
  там оно складывается с описанием (общий предел 1536 знаков). Стандарт Agent Skills
  его не знает.
- **Сред три, проверено две.** Документация Claude Code прямо говорит: вне Claude Code —
  загрузка в claude.ai, Skills API, `package_skill.py` — разрешены только шесть полей,
  любое другое — жёсткая ошибка. Навыки проверяли в Claude Code (работает) и в Perplexity
  (лишнее поле молча игнорирует) — и вывели, что поле безвредно.
- **Отказ виден только при загрузке.** Локально навык работает, сторож пакета проверял
  байты и ссылки, но не состав полей.

## Как починили

В PRECONSTRUCTION (версия 1.1.0):

- описание уложено в 1024 байта целиком, без `when_to_use`: русское — 482 знака / 842 байта,
  ключевые слова — в самом описании;
- в шапке только `name`, `description`, `license`, `metadata` — значения одной строкой
  в двойных кавычках;
- сторож `tools/check.py` роняет прогон на любом поле вне шести и на описании длиннее
  1024 знаков **или** 1024 байт; проверено подложенной ошибкой;
- распакованные архивы для claude.ai прогнаны `quick_validate.py`: «Skill is valid!».

В NSU-AI-SKILLS и PROMPT-Professor (тот же день) — второй путь: навык живёт и в Claude Code,
поэтому `when_to_use` в исходнике **оставлен**, а сборщик кладёт рядом второй архив для
Claude — папка навыка в корне, строка `when_to_use` вырезана, дата `SKILL.md` взята
из исходника (иначе архив менялся бы при каждой сборке):

| Пакет | Архив для Perplexity | Архив для Claude | Сторож |
|---|---|---|---|
| NSU-AI-SKILLS | `zip/<навык>.zip` | `zip/claude/<навык>.zip` | `soberi-zip.sh` отказывается собирать архив для Claude при чужих полях |
| PROMPT-Professor | `site/files/navyki/<навык>.zip` | `site/files/navyki/claude/<навык>.zip`, кнопка «zip для Claude» | `proverit-sajt.py`: раскладка, имя папки, поля шапки |

Все 18 архивов для Claude прошли `quick_validate.py`. Попутно нашлось: в NSU-AI-SKILLS архивы
`gost-19-primery` и `gost-19-priyomka` отстали от правки навыков 27.10.2026 на три недели —
сторожа «архив собран из текущих исходников» там не было (в PRECONSTRUCTION он есть).

## Чтобы не повторилось

**Правило 1. Каждая приёмная среда — свой валидатор.** Claude Code, claude.ai, Skills API,
Perplexity, Codex принимают разное. «Работает у меня в Claude Code» ничего не говорит про
архив для claude.ai.

**Правило 2. Одна шапка, годная везде,** — пересечение требований: поля только из шести
(`name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`), описание
до 1024 **байт** (строже предела Anthropic в 1024 знака), без `<` и `>`, `name` равно имени
папки. Расширения Claude Code (`when_to_use`, `argument-hint`, `disable-model-invocation`
и т. п.) — либо только в навыках, которые никуда, кроме Claude Code, не уезжают, либо
вырезаются сборщиком из архива для Claude (так в NSU-AI-SKILLS и PROMPT-Professor).

**Правило 3. Сторож на допустимые поля** — рядом со сторожем на байты. И перед раздачей
архивов — `quick_validate.py` на распакованном архиве, а не на исходной папке: так
проверяется ровно то, что получит claude.ai.

**Правило 4. Архив для claude.ai устроен иначе, чем для Perplexity:** claude.ai ждёт папку
навыка в корне архива, Perplexity принимал `SKILL.md` в корне. Сборщик собирает оба вида.

## Связанное

- Проект: [projects/active/preconstruction](../../projects/active/preconstruction/README.md) —
  `docs/PAKET-RAZBOR.md`, решение Р-2.
- Предел в байтах, из-за которого появился `when_to_use`:
  [perplexity-skill-description-byte-limit](../tools/perplexity-skill-description-byte-limit.md)
- Сборка архивов только сценарием: [finder-zip-ships-ignored-files](finder-zip-ships-ignored-files.md)
- Утверждение документа о себе — предмет проверки: [document-promise-needs-a-guard](document-promise-needs-a-guard.md)
- Источник требования: https://code.claude.com/docs/en/skills (раздел о полях шапки вне Claude Code).
