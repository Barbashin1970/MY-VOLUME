---
title: Публикация статического сайта на GitHub Pages
description: Как выложить HTML/CSS/JS без сервера по ссылке <ник>.github.io/<Repo>/ — подготовка путей, .nojekyll, публичный репозиторий, включение Pages, проверка командой и таблица причин сбоев (регистр имени, Jekyll в режиме ветки, старые версии действий). Сайт со ссылками от корня — копия с приставкой через Actions. Опись перед открытием репозитория, тариф и лимиты.
tags: [skill, stack/static, deploy, github-pages]
origin: "~/ENGINEER"
created: 2026-10-06
updated: 2026-10-06
---

# Skill: Публикация статического сайта на GitHub Pages

## Когда применять

Готовый **статический** сайт — HTML, CSS, JavaScript, всё считается в браузере
(калькулятор, прототип, демо для курса) — нужно открыть по ссылке бесплатно, прямо
из репозитория с кодом.

Не подходит: серверная логика (Python, Node на Pages не исполняются — только раздача
файлов), секреты в коде, приватный репозиторий на бесплатном тарифе. Продуктовым
фронтендам по [ADR-001](../../docs/decisions/adr-001-default-stack.md) — Vercel.
Markdown-сайт (разделы vault) — другой путь, через Jekyll и `_config.yml`:
README vault, раздел «Если хочется опубликовать через GitHub Pages».

## Как применять

### 1. Подготовить проект

- `index.html` — в корне репозитория (или в `docs/`), имя строчными.
- **Пути относительные:** `style.css`, `./script.js`, `img/logo.png`. Абсолютный
  `/style.css` уйдёт в корень домена `<ник>.github.io/`, а сайт лежит в `/<Repo>/`.
- **Пустой `.nojekyll`** в корне публикуемой папки: Pages отдаёт файлы как есть.
  Без него сайт собирается Jekyll, а тот выбрасывает файлы и папки на `_`
  и разбирает markdown своим конвертером.

  ```bash
  : > .nojekyll
  ```

- Проверить локально **по http, а не `file://`** — с теми же относительными путями,
  что будут на Pages:

  ```bash
  python3 -m http.server 8000      # http://localhost:8000
  ```

- **Регистр имён файлов совпадает со ссылками.** macOS не различает `Style.css`
  и `style.css`, GitHub Pages различает: локально работает, на сайте 404
  [замер 2026-10-06: `…/ENGINEER/Index.html` → 404].

### 2. Репозиторий — публичный, имя строчными

Адрес сайта повторяет имя репозитория **с учётом регистра**, поэтому имя сразу
строчными, через дефис: `engineer`, `kniffel-game`.

**Через VS Code** («Publish to GitHub» в панели Source Control): диалог предлагает
имя папки — из папки `ENGINEER` получился репозиторий `ENGINEER` и адрес
`/ENGINEER/`. Исправить имя в диалоге до публикации; выбрать **public**.

**Вручную:** [github.com/new](https://github.com/new) → имя → **Public** → README,
`.gitignore` и лицензию не добавлять, если они уже есть в проекте → **Create repository**.

```bash
git init -b main
git add index.html style.css script.js .nojekyll README.md   # поимённо, не git add .
git commit -m "Первая версия"
git remote add origin https://github.com/<ник>/<repo>.git
git push -u origin main
```

### 3. Включить Pages

`https://github.com/<ник>/<repo>/settings/pages` → **Build and deployment** →
**Source**: *Deploy from a branch* → **Branch**: `main`, папка `/ (root)` (или `/docs`)
→ **Save**.

Первая сборка — 1–2 минуты, видна на вкладке **Actions** («pages build and
deployment»). Готово, когда на странице Pages написано «Your site is live at …».
Дальше каждый `push` в эту ветку публикуется сам.

Из терминала, если стоит `gh` и выполнен `gh auth login` [не проверено в наших проектах]:

```bash
gh repo create <repo> --public --source=. --remote=origin --push
gh api -X POST repos/<ник>/<repo>/pages -f "source[branch]=main" -f "source[path]=/"
```

### 4. Адрес

`https://<ник строчными>.github.io/<Repo — точно как имя репозитория>/`

- домен регистр не различает, путь различает [замер 2026-10-06: `/ENGINEER/` → 200,
  `/engineer/` → 404];
- репозиторий с именем `<ник>.github.io` публикуется в корень: `https://<ник>.github.io/`.

### 5. Проверить командой, а не «должно работать»

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://<ник>.github.io/<Repo>/      # 200
curl -s https://api.github.com/repos/<ник>/<Repo> | python3 -c \
  "import sys,json; d=json.load(sys.stdin); print(d['visibility'], 'pages:', d['has_pages'])"
# public pages: True
```

Ответ 200 доказывает, что файл отдаётся, но не что страница работает. Работу проверяет
прогон в настоящем браузере. В ENGINEER это `tests/scenarios.html`: она нажимает
кнопки в iframe и сверяет табло. Без человека:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new \
  --user-data-dir=/tmp/pages-check --virtual-time-budget=10000 \
  --dump-dom "https://<ник>.github.io/<Repo>/tests/scenarios.html" | grep -o 'Пройдено [^<]*'
```

На macOS Chrome после `--dump-dom` может не завершиться сам: вывод уже получен,
процесс закрыть (Ctrl+C или `kill`).

## Почему 404: таблица разбора

| Симптом | Причина | Как проверить | Что делать |
|---|---|---|---|
| 404 на любом адресе сайта | Pages не включён | API: `has_pages: false` | шаг 3 |
| 404 сразу после включения | идёт первая сборка | вкладка **Actions** | подождать 1–2 минуты |
| 404 на вашем адресе, тот же адрес в другом регистре открывается | путь различает регистр имени репозитория | `full_name` в ответе API | адрес как в имени репозитория или переименовать репозиторий: **Settings** → **General** |
| страница без стилей, скрипты не грузятся | абсолютные пути `/style.css` | DevTools → Network: 404 на `/style.css` | относительные пути; у Vite — `base` |
| 404 на файлах и папках, начинающихся с `_` | их выбросил Jekyll | есть ли `.nojekyll` | добавить `.nojekyll` |
| локально файл есть, на сайте 404 | регистр имени файла | `git ls-files` против ссылок в коде | `git mv Style.css style.css` |
| в настройках нет Pages или предлагают тариф | приватный репозиторий на бесплатном тарифе | API: `visibility` | сделать репозиторий публичным |
| после `push` видна старая версия | кэш: Pages отдаёт `cache-control: max-age=600` [замер] | `curl -sI <адрес>` | подождать до 10 минут или Cmd+Shift+R |
| запуск «pages build and deployment» падает на «Build with Jekyll» | выбран *Deploy from a branch*, а в репозитории markdown с `{{…}}` | вкладка **Actions** | для сайта из своей папки — Source: *GitHub Actions* и свой workflow |
| «No artifacts named "github-pages" were found», хотя артефакт есть | старый `deploy-pages@v4` при новом `upload-artifact` [замер 2026-10-06] | вкладка **Actions** → Artifacts | текущие версии: `deploy-pages@v5`, `upload-pages-artifact@v5` |
| `configure-pages` падает «Get Pages site failed» | Pages ещё не переключён на *GitHub Actions* | **Settings → Pages** | Source: *GitHub Actions*, затем **Re-run failed jobs** |

## Сайт со сборкой (Vite, React) [не проверено в наших проектах]

Pages раздаёт только готовые файлы — собрать их нужно до публикации.

- `base: './'` в `vite.config` (или `'/<Repo>/'` в точном регистре имени), иначе
  ассеты ищутся в корне домена — та же правка, что в
  [publish-yandex-games](publish-yandex-games.md).
- Без Actions: `build.outDir: 'docs'`, собрать, закоммитить `docs/`, в шаге 3 выбрать
  папку `/docs`; `.nojekyll` положить в `public/`, чтобы Vite копировал его в сборку.
  **Не годится, если в `docs/` лежит документация** — сборка очищает папку.
- С Actions: **Source**: *GitHub Actions* и стартовый workflow, который GitHub
  предлагает на той же странице.

## Сайт со ссылками от корня — копия с приставкой через Actions

Многостраничный сайт, который собирает свой сборщик и ссылается от корня (`/lekciya-1/`,
`/assets/sajt.css`) и уже живёт на Vercel в корне домена. Переписывать сборщик ради Pages
незачем: Pages получает **копию** с приставкой `/<Repo>` ко всем адресам.

- Скрипт копии (в PROMPT-Professor — `tools/dlya-pages.py`, только stdlib) приписывает
  основу к адресам в атрибутах страниц (`href`, `src`, `srcset`, у `srcset` — к каждому
  адресу), к строкам-адресам в скриптах, к `manifest` (`id`, `start_url`, `scope`, иконки)
  и к списку сохранения service worker.
- **Ловушка — одиночная `'/'` в скриптах.** Правило «дополнить каждую строку `'/'`»
  ломает чужой смысл: в конструкторе это символ base64 (`.replace(/_/g, '/')`). Дополнять
  только строки вида `'/x…'`, одиночные — точечно и с проверкой «найдено ровно одно».
- **PWA в подпапке:** `register('<Repo>/sw.js')` — область становится `/<Repo>/`; главная
  в списке сохранения — `"/<Repo>/"`, а не `"/"`, иначе установка worker падает.
- **Самопроверка в конце:** адрес от корня без основы — остановка сборки.
- Публикация — **Source: GitHub Actions**, workflow: `checkout` → скрипт копии в `_pages`
  → `configure-pages` → `upload-pages-artifact` (path: `_pages`) → `deploy-pages`;
  основа — `/${{ github.event.repository.name }}` (точный регистр). Deploy from a branch
  не подходит: он умеет только корень или `/docs`, а сайт — в `site/`; из корня он прогнал
  бы через Jekyll весь репозиторий вместе с `docs/` (в PROMPT-Professor эта сборка упала
  на подстановках `{{…}}` — к счастью).
- **Версии действий — текущие, не по памяти:** на 2026-10-06 `actions/checkout@v7`,
  `configure-pages@v6`, `upload-pages-artifact@v5`, `deploy-pages@v5`. С `deploy-pages@v4`
  (2024) при новом `upload-artifact` запуск падает: «No artifacts named "github-pages" were
  found» — хотя артефакт выгружен [замер 2026-10-06]. Проверять:
  `curl -s https://api.github.com/repos/actions/deploy-pages/releases/latest`. С v4
  `upload-pages-artifact` не берёт файлы на точку (`.nojekyll`) — при публикации через
  Actions Jekyll и не запускается.
- Проверять копию локально **в подпапке**, как на Pages:
  `python3 -m http.server 8093 --directory /tmp/pages`, копия в `/tmp/pages/<Repo>/`;
  в браузере — ни одного запроса мимо `/<Repo>/` и ни одного 4xx
  [замер 2026-10-06, PROMPT-Professor: 49 из 49 в Chromium и WebKit — локально в подпапке
  и затем на живом `barbashin1970.github.io/PROMPT-Professor/`; на живом: адрес без слэша —
  301 на адрес со слэшем, несуществующий адрес — 404 со своей страницей `404.html` проекта,
  `.webmanifest` — `application/manifest+json`, PWA сохраняет весь список и открывает
  страницы без сети].
- Если запуск падает с «Branch "main" is not allowed to deploy to github-pages due to
  environment protection rules» — **Settings → Environments → github-pages → Deployment
  branches** → добавить `main`.

## Перед тем как открыть приватный репозиторий

Pages на бесплатном тарифе требует публичный репозиторий — а открывается **всё**: дерево,
`docs/`, история. В PROMPT-Professor при открытии по прямой ссылке читались папка
«не публиковать», ключ ответов зачёта и старые коммиты с теми же файлами (урок
[private-repo-made-public-leaks-history](../lessons/private-repo-made-public-leaks-history.md)).

- **Опись до смены видимости:** `git ls-files` по папкам; `git log --all --oneline -- <путь>`
  по всему, что называлось «не публиковать», черновиком, ответами, договорами.
- **`.gitignore` не снимает с учёта** — нужен `git rm -r --cached <папка>`; и не чистит
  историю.
- **Порядок:** закрыть → снять с учёта и закоммитить → `git filter-repo --invert-paths
  --path <…>` **в свежей копии** (в рабочей бывают незакоммиченные правки) →
  `git push --force` → в рабочей копии `git fetch && git reset --hard origin/main` → открыть.
- `forks_count` в API — форк чужой копии не вычистить.
- **Лицензия** для образца, который будут переиспользовать: код — `LICENSE` (MIT, чистый
  текст — так GitHub его распознаёт), тексты — отдельный файл с CC BY 4.0 и списком того,
  что под чужими условиями (CC BY-SA-производные, чужие промпты, логотипы, картинки
  нейросетей по условиям сервисов).

## Тариф и лимиты

**Бесплатно и без срока** — публичный сайт из публичного репозитория на GitHub Free
[замер 2026-10-06: `ENGINEER` и `PROMPT-Professor` опубликованы без платного тарифа].

Плашка «You can try GitHub Enterprise risk-free for 30 days» в настройках Pages — не срок
сайта, а пробный период **GitHub Enterprise Cloud**. Он нужен только для *приватной*
публикации — сайта, который видят лишь члены организации: «To publish a GitHub Pages site
privately, your organization must use GitHub Enterprise Cloud» (docs.github.com, Changing
the visibility of your GitHub Pages site). Для открытого сайта не нужен.

Лимиты (docs.github.com, GitHub Pages limits, 2026-10-06):

| Что | Лимит |
|---|---|
| опубликованный сайт | не больше 1 ГБ |
| репозиторий | рекомендовано до 1 ГБ |
| трафик | мягкий лимит 100 ГБ в месяц |
| сборки | мягкий лимит 10 в час — **не для своего workflow на Actions** |
| публикация | тайм-аут 10 минут |
| запрещено | бизнес и интернет-магазин на бесплатном хостинге, коммерческие транзакции, SaaS |

Прикидка для курса: первый визит с PWA сохраняет ~3,5 МБ — 100 ГБ хватает примерно
на 28 тысяч первых визитов в месяц; повторные идут из сохранённого.

## Почему именно так

- *Deploy from a branch* без Actions — ноль конфигурации; сайту без сборки Actions
  ничего не добавляют.
- `.nojekyll` вместо `_config.yml`: HTML-приложению Jekyll не нужен, а файлы на `_`
  и время сборки отнимает.
- Публичный репозиторий — на бесплатном тарифе Pages публикуется только из публичных.
- Vercel и Netlify отвергнуты для демо: отдельный аккаунт и привязка, а Pages живёт
  в том же репозитории, что и код.

## Известные подводные камни

- **Публично всё:** `docs/`, тесты, история коммитов. Секрет, попавший в историю,
  из неё не исчезает — менять сам секрет.
- **Почта автора коммитов видна в истории.** Скрыть — настройка GitHub «Keep my email
  addresses private» и noreply-адрес в `git config user.email`.
- Публикуется только ветка и папка, выбранные в шаге 3; `push` в другую ветку
  сайт не меняет.
- Публичный репозиторий без файла `LICENSE` — код нельзя законно переиспользовать;
  лицензию выбрать до публикации.

## Откуда взято

- `~/ENGINEER`, 2026-10-06: калькулятор после проверки GigaCode. Репозиторий
  `Barbashin1970/ENGINEER` опубликован кнопкой VS Code; сначала 404, потому что Pages
  не включён, затем 404 на строчном адресе из-за регистра. Pages из `main` / root,
  `.nojekyll`; на живом сайте сценарии проходят 48 из 48.
- `~/PROMPT-Professor`, 2026-10-06: многостраничный сайт курса со ссылками от корня;
  основной сайт — Vercel, дубль — Pages через копию с приставкой `/PROMPT-Professor`
  и workflow; при открытии репозитория — опись, `vhod/` в `.gitignore`, чистка истории,
  лицензия MIT + CC BY 4.0.
- Связанное: [publish-yandex-games](publish-yandex-games.md),
  [ski-coding](ski-coding.md), урок
  [agent-reports-done-without-running-code](../lessons/agent-reports-done-without-running-code.md).
