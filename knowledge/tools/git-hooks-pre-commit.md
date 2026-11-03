---
title: "git pre-commit-хуки vault — сторож frontmatter"
description: "Локальный pre-commit-хук MY-VOLUME: разбирает YAML-frontmatter каждой коммитируемой .md тем же парсером, что и GitHub (Ruby Psych), и останавливает коммит с адресом «файл:строка:позиция», если GitHub покажет «Error in user YAML». Здесь — полный текст хука, установка одной командой, проверка всего vault, ограничения. Хук живёт в .git/hooks/ и в репозиторий не попадает, поэтому его источник — эта заметка."
tags: [tool/git, hooks, pre-commit, yaml, frontmatter, vault]
created: 2026-11-03
updated: 2026-11-03
related:
  - knowledge/lessons/frontmatter-unquoted-colon-breaks-github-render.md
  - docs/playbooks/13-quality-management.md
---

# git pre-commit-хуки vault

## Когда открывать

- Коммит остановился с сообщением «pre-commit: frontmatter не разбирается как YAML».
- Новая машина или свежий клон vault: `.git/hooks/` не клонируется, хук ставится заново.
- GitHub показывает над заметкой «Error in user YAML».

## Сторож frontmatter

Для каждой `.md`, которая попадает в коммит (добавлена, изменена, переименована), хук берёт версию
из индекса (`git show :путь`), вырезает блок между первыми строками `---` и разбирает его
`Psych.parse`. Это тот же парсер, что у GitHub: на ошибке из
[урока](../lessons/frontmatter-unquoted-colon-breaks-github-render.md) локальный Psych выдал строку
GitHub дословно. Если блок не разбирается — коммит остановлен, в выводе `файл:строка:позиция: причина`.

Проверяется только то, что коммитится: старые заметки с ошибкой коммиту не мешают, пока их не трогают.

### Текст хука

```sh
#!/bin/sh
# Сторож frontmatter vault MY-VOLUME: YAML в начале каждой коммитируемой .md должен разбираться
# тем же парсером, что и GitHub (Ruby Psych). Иначе GitHub покажет «Error in user YAML».
# Источник и установка: knowledge/tools/git-hooks-pre-commit.md

if ! command -v ruby >/dev/null 2>&1; then
  echo "pre-commit: ruby не найден, проверка frontmatter пропущена" >&2
  exit 0
fi

exec ruby - <<'RUBY'
require 'psych'

paths = IO.popen(%w[git diff --cached --name-only --diff-filter=ACMR -z -- *.md], &:read).split("\0")
errors = []

paths.each do |path|
  text = IO.popen(['git', 'show', ":#{path}"], &:read).force_encoding('UTF-8')
  next unless text.start_with?("---\n")
  lines = text.lines
  close = lines[1..-1].index { |l| l.chomp == '---' }
  next unless close
  begin
    Psych.parse(lines[1, close].join)
  rescue Psych::SyntaxError => e
    # e.line считается от первой строки после «---», поэтому в файле это строка e.line + 1
    errors << "  #{path}:#{e.line + 1}:#{e.column}: #{[e.problem, e.context].compact.join(' ')}"
  end
end

exit 0 if errors.empty?

warn 'pre-commit: frontmatter не разбирается как YAML, GitHub покажет «Error in user YAML»:'
warn errors
warn 'Значение с «: » внутри или со служебным символом YAML в начале возьмите в двойные кавычки.'
warn 'Урок: knowledge/lessons/frontmatter-unquoted-colon-breaks-github-render.md'
exit 1
RUBY
```

### Установка

Из корня vault — вырезать хук из этой заметки и сделать исполняемым:

```sh
sed -n '/^#!\/bin\/sh$/,/^RUBY$/p' knowledge/tools/git-hooks-pre-commit.md > .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

Проверка, что стоит: `.git/hooks/pre-commit; echo $?` — при пустом индексе `0`. Снять: удалить
`.git/hooks/pre-commit`.

VS Code запускает хук при коммите из панели Source Control так же, как терминал; при отказе коммит не
создаётся, текст ошибки — в журнале Git (Output → Git).

### Проверить весь vault

Хук смотрит только коммит. Все заметки разом — из корня vault:

```sh
ruby -rpsych -e 'n = 0; bad = 0; Dir.glob("**/*.md").sort.each { |f| l = File.read(f).lines; next unless l[0] == "---\n"; c = l[1..-1].index { |x| x.chomp == "---" } or next; n += 1; begin; Psych.parse(l[1, c].join); rescue Psych::SyntaxError => e; bad += 1; puts "#{f}:#{e.line + 1}:#{e.column}: #{e.problem}"; end }; puts "frontmatter: #{n}, не разбирается: #{bad}"'
```

`Dir.glob` не заходит в скрытые каталоги (`.git`, `.obsidian`).

### Ограничения

- Нужен Ruby. На этом Mac — `/usr/bin/ruby` 2.6.10 (`ruby -v`, 2026-11-03). Без Ruby хук пишет
  предупреждение и коммит пропускает — сторожа нет.
- Проверяет синтаксис YAML, а не состав полей: пропущенный `title` или неверная дата пройдут.
- Файлы с переводами строк `\r\n` и блоком не с первой строки пропускаются без проверки.
- Обход `git commit --no-verify` не использовать: чинить причину
  ([плейбук качества](../../docs/playbooks/13-quality-management.md), правило про pre-commit).

## Связанное

- Урок: [frontmatter-unquoted-colon-breaks-github-render](../lessons/frontmatter-unquoted-colon-breaks-github-render.md).
