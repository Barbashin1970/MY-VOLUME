---
title: POLINOM-JAVA
description: Java-вариант POLINOM. Sigma-методология для JVM-стека. Бенчмарки через JMH.
tags: [project, stack/java, status/active, category/C]
status: active
category: C
repo: ~/POLINOM-JAVA
related_repo: https://github.com/Barbashin1970/POLINOM-JAVA
created: 2026-03-15
updated: 2026-05-26
related:
  - concepts/sigma-methodology.md
  - projects/active/polinom/
---

# POLINOM-JAVA

> **Где код:** `~/POLINOM-JAVA/`
> **Что это:** Java-вариант POLINOM. Тестирует, как правила
> Сигма-методологии переносятся на JVM-стек.

## Зачем

Доказать или опровергнуть **универсальность** Сигма-правил для разных
языков. Java выбран как самый промышленный JVM-язык, контрастный с
Python (типизация, JIT, нет GIL).

## Стек

| Слой | Технология |
|------|-----------|
| Язык | Java 17+ |
| Бенч | JMH (Java Microbenchmark Harness) |
| Build | Gradle / Maven |

## Документация (в `~/POLINOM-JAVA/`)

| Документ | Стадия ЖЦ |
|----------|-----------|
| `README.md` | 8 |
| `PLAN-JAVA-PLAYGROUND.md` | — план Java playground |
| `SIGMA_AUDIT.md` | 6 — Java-вариант аудитора |
| `SIGMA_AUDIT-03.md` | 6 — следующая итерация аудитора |

## Связь с POLINOM

13 контролируемых примеров параллельно на Python и Java. Сводный
бенч даёт ответ:

- Какие правила работают одинаково (P1, P2 — bounded iteration).
- Какие **инвертируются** (что хорошо в Python — плохо в Java, и наоборот).
- Где разница в производительности достигает ×100+ (типично — Java
  выигрывает на горячем коде, Python — на старте процесса).

## Связанное

- Главный проект: [polinom](../polinom/)
- Концепт: [sigma-methodology](../../../concepts/sigma-methodology.md)
