---
title: «Publish to GitHub» без единого коммита — публичный репозиторий создан, пуш упал
description: "В папке, где уже сделан git init, но нет ни одного коммита, кнопка VS Code «Publish to GitHub» создала на GitHub пустой публичный репозиторий, добавила origin и упала на git push -u origin main с «src refspec main does not match any». Владелец видит «не смог опубликовать», а репозиторий уже существует и уже открыт. Лечится первым коммитом поимённо после сторожа и git push из терминала; повторное нажатие кнопки упрётся в занятое имя."
tags: [lesson, gotcha, git, github, tool/vscode]
severity: medium
project_origin: portfolio
created: 2026-10-27
updated: 2026-10-27
---

# Урок: «Publish to GitHub» без единого коммита

## Что произошло

26.10.2026, проект `~/portfolio`. Агент сделал в папке `git init -b main`: сторожу
`check-private` нужно знать, какие файлы уйдут в git (`git ls-files --cached --others
--exclude-standard`). Коммит агент отложил до слова владельца — так велит дисциплина
«коммит только по просьбе».

Владелец нажал в VS Code **Publish to GitHub** и написал: «не смог опубликовать репо».

Лог расширения git (`~/Library/Application Support/Code/logs/…/exthost/vscode.git/Git.log`):

```
22:19:40.743  > git remote add origin https://github.com/Barbashin1970/portfolio.git
22:19:40.923  > git push -u origin main
              error: src refspec main does not match any
              error: failed to push some refs to 'https://github.com/Barbashin1970/portfolio.git'
```

При этом репозиторий на GitHub **уже был создан** — API в ту же минуту:
`visibility: public`, `size: 0`, `created_at: 2026-10-26T15:19:36Z` (это 22:19 по Новосибирску).

## Почему это случилось

Ветка `main` после `git init` существует только как имя: коммитов в ней нет, пушить
нечего. Кнопка в такой папке своего коммита не делает — по логу видно, что она
сразу создаёт репозиторий на GitHub, добавляет `origin` и пушит текущую ветку.
Шаги идут в порядке «сначала снаружи, потом проверка», поэтому неудача оставляет
след: **пустой репозиторий, публичный с первой секунды**.

Две стороны были правы порознь: агент — не коммитить без слова владельца, кнопка —
пушить то, что есть. Вместе они дали состояние, которое ни одна сторона не планировала.

## Как починили

1. Сторож до коммита: `npm run check:private` — 0 совпадений.
2. Первый коммит **поимённо**, без `git add .` (39 файлов, `f857e89`).
3. `git push -u origin main` из терминала — учётка git лежала в osxkeychain, пуш прошёл.
4. Проверка снаружи: открытые файлы по `raw.githubusercontent.com` отдают 200, файлы
   `.private/` — 404; `git log --all --oneline -- .private` пуст.

Кнопку второй раз не нажимали: она снова попыталась бы создать репозиторий с тем же
именем, а оно уже занято.

## Чтобы не повторилось

- **Первый коммит — до публикации.** Перед кнопкой «Publish» или `gh repo create`
  выполнить `git log --oneline -1`. Ответ «does not have any commits yet» значит: пуш
  упадёт, а репозиторий успеет появиться.
- **Сделал `git init` для своих нужд — скажи владельцу, что коммита нет.** Иначе
  кнопка в редакторе выглядит готовой к работе.
- **Если репозиторий уже создан пустым — не жми «Publish» снова.** Коммит и
  `git push -u origin main`; `origin` уже прописан.
- **Пустой публичный репозиторий — повод проверить опись до первого пуша.**
  Всё, что уйдёт следом, публично навсегда вместе с историей
  ([private-repo-made-public-leaks-history](private-repo-made-public-leaks-history.md)).

## Связанное

- Проект: [portfolio](../../projects/active/portfolio/README.md).
- Навык: [publish-github-pages](../skills/publish-github-pages.md) — раздел про публикацию
  кнопкой VS Code и опись перед открытием.
