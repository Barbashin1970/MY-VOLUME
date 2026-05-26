---
title: POLINOM
description: Sigma-методология полиномиальной сложности + CLI-аудитор sigma-audit + agent-rules. Категория C — научно-инструментальный.
tags: [project, stack/python-cli, status/active, category/C]
status: active
category: C
repo: ~/POLINOM
deployed: https://polinom-sigma-audit.up.railway.app/
related_repo: https://github.com/Barbashin1970/POLINOM
created: 2026-03-01
updated: 2026-05-26
related:
  - concepts/sigma-methodology.md
  - projects/active/polinom-java/
---

# POLINOM

> **Где код:** `~/POLINOM/`
> **Playground:** https://polinom-sigma-audit.up.railway.app/
> **Что это:** научно-инструментальный проект вокруг **Сигма-методологии**
> полиномиальной сложности (Гончаров/Нечесов/Свириденко, ИМ СО РАН).
> Включает CLI-аудитор, agent-rules для AI-ассистентов и эмпирический бенч.

## Зачем

Превратить теоретическую серию статей сибирских математиков в
**воспроизводимый научно-практический кейс**. Дать разработчику
one-command инструмент контроля сложности кода с научным обоснованием
правил.

## Три инструмента в одном репо

1. **🛡 `sigma-audit` CLI** (v0.4) — статический анализатор Python без
   LLM на libcst. Сканирует на **32 канонических анти-паттерна**
   (M1-M4 + T + P5-P9 + J1-J5 + S1-S6 + R1-R2 + X1).
   **12 из них чинит автоматически** с доказательством эквивалентности.
   80/80 тестов. **Полный комплект документации по ГОСТ 19**.
   v0.4: режим `sandbox` — `sigma-audit sandbox <path>` делает безопасный
   клон, прогоняет audit → verify → fix → verify → audit, выдаёт
   единый HTML BEFORE/AFTER.

2. **🤖 Агентский пакет `agent-rules`** — drop-in инструкции для AI-агентов
   (Claude Code, Cursor, Copilot, Aider, Continue.dev). Один файл —
   и агент Sigma-aware: пишет код по правилам молча; по триггеру
   «sigma audit» выдаёт read-only отчёт со score 0-100.

3. **🔬 Эмпирический бенч** — 13 контролируемых примеров одновременно
   на Python и Java (JMH). Отвечает: какие правила переносятся в
   каждую среду без потерь, какие инвертируются, где разница ×100+.

## Стек

| Слой | Технология |
|------|-----------|
| Язык | Python 3.11+ |
| Парсер | libcst (CST-аудит) |
| Тесты | pytest, 80 кейсов |
| CI | GitHub Actions (sigma bench) |
| Deploy | Railway (для playground) |

## Документация (в `~/POLINOM/`)

| Документ | Стадия ЖЦ |
|----------|-----------|
| `README.md` (русский) | 8 — главный лендинг с бейджами, value-prop, концептуальной картинкой |
| `README-en.md` | 8 — английская версия |
| `ARC.md` | 5 — архитектура |
| `REVIEW.md` | — внутренний обзор |
| `backlog.md` | 9 |
| `backlog-acselerator.md` | 9 — отдельный бэклог под акселератор |
| `docs/MTS-DOGFOOD-REPORT.md` | 3 — эмпирическая валидация на 6 публичных репо МТС (1733 файла, 251 169 LOC) |
| Комплект ГОСТ-19 в `docs/` | 7 — ТЗ, ПМИ, РО, РС, ОП |

## Главные ценности POLINOM для других проектов

1. **Билингвальный README** (`README.md` + `README-en.md`) — эталон
   категории C. См. [adr-003-bilingual-readme](../../../docs/decisions/adr-003-bilingual-readme.md).

2. **«sigma-audit sandbox» как пилот за 5 минут** — паттерн
   one-command-demo для коммерческой воронки. Клиент даёт путь к
   репо → через 5 минут получает HTML-отчёт.

3. **`agent-rules`** — паттерн «правила как drop-in для AI-агентов».
   Переносимо в любой проект.

4. **CI-бейдж как social proof** — бейджи: bench results, language
   version, demo, related projects.

5. **Эмпирическая валидация теории** — паттерн «теоретическая статья
   → CLI-инструмент → бенчмарк → бумага». Воспроизводимый научный
   процесс.

## Коммерческая стратегия

Целевые клиенты: Сбер, Т-Банк, МТС, Альфа, Ростелеком. Воронка:
«дай путь к одному микросервису → 5 минут → HTML-отчёт». См. POLINOM
`README.md` §«Коммерческая ценность».

## Связанное

- Концепт: [sigma-methodology](../../../concepts/sigma-methodology.md)
- Близкий проект: [polinom-java](../polinom-java/) — Java-версия
- ADR: [adr-003-bilingual-readme](../../../docs/decisions/adr-003-bilingual-readme.md)
