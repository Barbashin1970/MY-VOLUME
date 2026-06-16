---
title: SOFIA — журнал багов
description: Зафиксированные баги SOFIA и ссылки на извлечённые уроки.
tags: [project, sofia, bugs]
created: 2026-05-31
updated: 2026-05-31
---

# SOFIA — журнал багов

## 2026-05-31 — Браузерный переводчик ломает UI

**Симптом:** в вебе/PWA надписи периодически обрезаны и перемешаны
(«Создать рецепт» → «рел комп»), флаг языка не совпадает с текстом. У
пользователя установлены расширение-переводчик и VPN; баг «плавал».

**Root cause:** автоперевод (Google Translate / Chrome / расширения) мутирует
DOM в обход React Native Web → реконсиляция падает. Триггер — `<html lang="en">`
при русском контенте; VPN менял целевую локаль браузера.

**Фикс:** полностью отключён браузерный автоперевод (`translate="no"` +
`notranslate` на `<html>`/`#root`, `<meta name="google" content="notranslate">`)
+ синхронизация `<html lang>` с активным языком i18n. В проде гарды инжектятся
в рантайме (`+html.tsx` игнорируется при static export). Бамп `CACHE_NAME` v30.
Коммиты в `~/SOFIA`: `expo/app/+html.tsx`, `expo/app/_layout.tsx`,
`expo/locales/index.ts`, `expo/public/service-worker.js`.

**Урок:** [knowledge/lessons/browser-translator-breaks-react-dom.md](../../../knowledge/lessons/browser-translator-breaks-react-dom.md)
