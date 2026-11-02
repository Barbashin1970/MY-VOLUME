---
title: portfolio — сайт-портфолио Олега Барбашина
description: Визитка-витрина: первый экран говорит заказчику, что ему дадут — прототип, который можно потрогать до вложений, и документацию по ГОСТ 19, привязанную к коду, — а ниже идут проекты с живыми демо.
tags: [project, stack/vite-react, domain/portfolio, status/active, category/D]
status: active
category: D
repo: ~/portfolio
deployed: https://barbashin1970.github.io/portfolio/
created: 2026-10-26
updated: 2026-10-31
related:
  - knowledge/skills/publish-github-pages.md
  - knowledge/lessons/vscode-publish-to-github-without-commit.md
  - knowledge/lessons/guessed-vercel-subdomain-belongs-to-someone-else.md
  - knowledge/lessons/nbsp-and-overflow-hidden-hide-heading-overflow.md
  - knowledge/tools/headless-chrome-layout-checks-via-devtools.md
  - knowledge/lessons/sr-only-escapes-scroller-without-positioned-ancestor.md
  - knowledge/lessons/lenis-scrollto-double-offset-and-stale-limit.md
  - knowledge/lessons/screen-recording-of-live-system-leaks-third-party-data.md
  - knowledge/lessons/phone-photo-carries-exif-into-public-site.md
  - knowledge/patterns/skeleton-ui-placeholders.md
  - knowledge/tools/ffmpeg-review-and-redact-screen-recording.md
  - knowledge/skills/process-ai-audit-and-staff-training.md
---

# portfolio — сайт-портфолио

> **Где код:** `~/portfolio/` (до 2026-10-26 — `~/BOV`)
> **Сайт:** <https://barbashin1970.github.io/portfolio/> · **Репозиторий:** <https://github.com/Barbashin1970/portfolio> (открытый, MIT на код)
> **Статус:** active. Фазы 1 и 2 закрыты; 2026-10-27 — витрина, чат-конструктор в контакте, аудит эффектов. Следующая — фаза 3 (EN, SEO, Lighthouse), после «ок» Олега.

## Зачем

Посетитель за десять секунд понимает, кто перед ним, и хочет заказать. Аудитория
описана по работам, а не по отраслям (JTBD, `docs/AUDITORIYA.md`). 27.10.2026 работ три,
первая — та, что «будет важна в ближайшем будущем» (Олег):

1. **Разбор процессов заказчика и обучение персонала:** где ИИ снимет рутину с сотрудников,
   проверка на данных заказчика, обучение команды.
2. **Прототип, который можно потрогать** до вложений в команду и бюджет.
3. **Документация по ГОСТ 19 и отчёты о НИОКР, привязанные к коду продукта.**

Первый экран: «ИИ, который работает **на вашу команду**»; подзаголовок — текст Олега
«Мне нравится создавать: игры, платформы для бизнеса, обучающие программы. Откройте
любое из 20+ демо и решите сами…». Надпись над заголовком — «Системный аналитик ·
разработчик ИИ-систем · технический писатель».

## Что внутри

| Часть | Состояние на 2026-10-27 |
|---|---|
| Первый экран | на десктопе — буквы заголовка собираются таймлайном GSAP, справа живой граф знаний (WebGL2); на телефоне — CSS-появление по словам и статичный граф |
| Предсборка | H1, подзаголовок, title и description приходят в HTML, LCP не ждёт JavaScript |
| Витрина | 32 проекта: 6 избранных на пине со счётчиком, 23 рабочих в каталоге по шести отраслям, 9 игр своей секцией; живое демо у 27 (`src/data/projects.ts`); с 2026-10-31 — Polinom-Python (бывший «Сигма-аудит») и Polinom-Java (Р-53); скриншоты демо и видео по запросу; под каталогом — справка об объёме: за 2025–2026 27 репозиториев, ≈364 тыс. строк кода (`node scripts/count-loc.mjs ~`) |
| Контакт | чат-конструктор: вопросы по шагам, ответы кнопками пишут строки в поле, текст правится на сайте и уходит в Telegram с текстом в поле ввода или письмом; «Порекомендовать» — поделиться сайтом; с 2026-10-28 — фото Олега слева, его лицо в шапке чата (Р-52) |
| Языки | RU выкатан, словарь EN готов, страница `/en/` — фаза 3 |
| Публикация | GitHub Pages через Actions на каждый пуш в `main` |

## Стек

| Слой | Технология |
|---|---|
| Сборка и интерфейс | Vite 8, React 19, TypeScript 6.0 strict, Tailwind 4, shadcn/ui |
| Движение | GSAP 3.15 + `@gsap/react`, Lenis 1.3 — единственное сглаживание |
| Эффекты | React Bits — SplitText, Magnet, Particles, TiltedCard, адаптированные копии со своей лицензией; свои: раздача карт в играх, «печатает…» в чате, прогресс страницы |
| Шрифты | Geologica, Onest, JetBrains Mono — Fontsource, со своего домена |
| Проверки | Vitest 5, ESLint 10, сторожа на Node 22 |

**Отклонения от [ADR-001](../../../docs/decisions/adr-001-default-stack.md):** бэкенда,
TanStack Query и Zustand нет — у сайта нет серверного состояния; хостинг — GitHub Pages,
а не Vercel (решение Олега). Нужен ли ADR-004 «стек для лендинга» — вопрос открыт.

## Сторожа

| Сторож | Что ловит |
|---|---|
| `check-private` | строки внутренней описи из `.private/` в файлах, которые уйдут в git, и в `dist/` |
| `check-dist` | H1, подзаголовок, title и description в HTML сходятся со словарём; заглушки брифа и служебные пометки нигде в `dist/` |
| `check-layout` | вёрстка в headless Chrome: 320, 375 и 1440 px, reduced-motion, первый экран, ошибки гидратации, якоря; действия человека — пин и счётчик избранного, фильтр, видео, переход к «Контакту», чат с правкой руками, «Порекомендовать» |
| `tests/numbers.test.ts` | числа в README и описи сходятся с `projects.ts`, со склонением |
| `tests/tokens.test.ts` | контраст пар «текст — фон» по WCAG в обеих темах |
| `tests/media.test.ts` | в медиа сайта нет EXIF, XMP и места съёмки; размеры фото — как в данных |

## Документы (в `~/portfolio/docs/`)

| Документ | О чём |
|---|---|
| `PLAN.md` | архитектура, секции, токены, фазы, тексты и их история |
| `FAZA-1-RAZBOR.md`, `FAZA-2-RAZBOR.md` | разрывы фаз и что закрыто: было — стало — чем подтверждается |
| `VITRINA-RAZBOR.md`, `CHAT-RAZBOR.md`, `EFFEKTY-RAZBOR.md` | разборы итераций 27.10.2026: витрина и LEYKA, позиционирование и чат, аудит эффектов |
| `AUDITORIYA.md` | аудитория по работам, кейсы, доказательства, чего не выносим вперёд |
| `USE-CASES.md` | сценарии использования → испытания |
| `BEKLOG.md` | отложенное с причиной и признаком «когда пора» |
| `INVENTORY.md` | опись проектов и демо, каждое число — с командой |

Решения Р-01…Р-53 и договорённости — в `~/portfolio/CLAUDE.md`. Работа идёт
по SKICODING: разбор до кода, документы обновляются в том же шаге.

## Применённые навыки и уроки

- Навыки: `skicoding`, [publish-github-pages](../../../knowledge/skills/publish-github-pages.md).
- Уроки, которые здесь действуют:
  [private-repo-made-public-leaks-history](../../../knowledge/lessons/private-repo-made-public-leaks-history.md),
  [browser-translator-breaks-react-dom](../../../knowledge/lessons/browser-translator-breaks-react-dom.md),
  [forma-vs-otkrytaya-pochta](../../../knowledge/lessons/forma-vs-otkrytaya-pochta.md),
  [email-obfuscation-that-isnt](../../../knowledge/lessons/email-obfuscation-that-isnt.md),
  [accordion-scroll-lands-on-wrong-section](../../../knowledge/lessons/accordion-scroll-lands-on-wrong-section.md),
  [spa-catchall-rewrite-is-a-soft-404](../../../knowledge/lessons/spa-catchall-rewrite-is-a-soft-404.md),
  [html-guard-misses-placeholders-in-downloadable-files](../../../knowledge/lessons/html-guard-misses-placeholders-in-downloadable-files.md).
- Родились здесь:
  [vscode-publish-to-github-without-commit](../../../knowledge/lessons/vscode-publish-to-github-without-commit.md),
  [guessed-vercel-subdomain-belongs-to-someone-else](../../../knowledge/lessons/guessed-vercel-subdomain-belongs-to-someone-else.md),
  [nbsp-and-overflow-hidden-hide-heading-overflow](../../../knowledge/lessons/nbsp-and-overflow-hidden-hide-heading-overflow.md),
  [headless-chrome-layout-checks-via-devtools](../../../knowledge/tools/headless-chrome-layout-checks-via-devtools.md),
  [sr-only-escapes-scroller-without-positioned-ancestor](../../../knowledge/lessons/sr-only-escapes-scroller-without-positioned-ancestor.md),
  [lenis-scrollto-double-offset-and-stale-limit](../../../knowledge/lessons/lenis-scrollto-double-offset-and-stale-limit.md),
  [screen-recording-of-live-system-leaks-third-party-data](../../../knowledge/lessons/screen-recording-of-live-system-leaks-third-party-data.md),
  [phone-photo-carries-exif-into-public-site](../../../knowledge/lessons/phone-photo-carries-exif-into-public-site.md),
  [ffmpeg-review-and-redact-screen-recording](../../../knowledge/tools/ffmpeg-review-and-redact-screen-recording.md);
  паттерн [skeleton-ui-placeholders](../../../knowledge/patterns/skeleton-ui-placeholders.md)
  родился в NSK_OpenData_Bot, здесь — третье применение.

## Ограничения, которые нельзя нарушать

- **Ничего не выдумываем:** цифры — из GitHub и слов Олега, отзывы и логотипы —
  только подтверждённые. Цифры раздела «Доказательства» из первой попытки
  портфолио (`ai-point`) не используются.
- **Ссылок на код приватных репозиториев нет** (`repoUrl?: never` в типах).
- **Внутреннее — только в `.private/`**, вне git; сторож — до каждого коммита.
- **Не рекламируем n8n, Node-RED и OWL** — доказательств в проектах нет.
- **Формы заявки нет:** Telegram и `mailto:`; чат собирает текст запроса в браузере,
  а отправляет его сам посетитель — сайт данных не собирает.

## Что дальше

Фаза 3: страница EN, SEO, 404, Lighthouse на мобильном профиле — после «ок» Олега.
Отложенное — `~/portfolio/docs/BEKLOG.md`.
