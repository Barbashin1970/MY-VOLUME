# Привет 👋 Меня зовут Олег Барбашин

Я строю **сложные системы вместе с ИИ** — параллельно растёт и код, и документация.
ИИ для меня не вспомогательный инструмент, а **соисполнитель**, который пишет наравне со мной.
Меня интересует **исследование границ** того, что эта связка (человек + большая модель) может сделать за итерацию.

---

## 🎿 SKICODING — рабочий артефакт, не маркетинг

В IT я с 2022 года — пришёл из тестирования и системного анализа. Несколько лет до того, как сесть писать код вместе с большой моделью, я занимался двумя вещами: гонял тест-кейсы (классы эквивалентности, граничные значения, негативные сценарии) и писал спеки по ГОСТ 19.201. Когда взял LLM в соавторы, обнаружил очевидное: модель просит **ровно то же** — чёткую задачу на входе и набор проверок на выходе. Удивления нет, но это сильно сократило путь.

Только знания QA не хватило. Модель умеет уходить в две крайности: либо **code-rush** — пишет код, разработчик мерджит, через неделю пять несовместимых архитектур и никто не помнит почему; либо **doc-paralysis** — четыре тысячи строк md без единой правки в коде. Несколько проектов я искал, как держать темп без потери прозрачности. Получилось вот что: чередовать текстовые шаги (`.md`) и кодовые (`commit`), как лыжные следы — левая, правая, левая, правая. Каждый шаг оставляет проверяемый артефакт, который сверяется с предыдущим. Назвал это **SKICODING**.

Цикл из шести шагов: аудит-md (что нашли и что закрываем) → реализация фаз → актуализация документации → новые UI-фичи → backlog отложенного с обязательным «Почему» → тесты → возврат к первому шагу. Текст и код чередуются. Ничего не пропускается. Через 8+ итераций на одном продакшен-проекте (онтологии регламентов в RAGRAF) подход устоялся, и я записал его как отдельный документ.

Побочный эффект, который я не закладывал, но честно замерил: на последних проектах одна фича закрывается за 2-4 часа против 1-2 недель в моей первой IT-команде 2022 года. Грубо ×50 по темпу. Скорее всего, это не про «модель быстро пишет код». Это про то, что между PM, dev и QA нет очередей — я один играю все три роли на каждой итерации и ни одну не пропускаю. Если у тебя сложилось иначе — буду рад услышать.

Документ открытый. Забирай, адаптируй под свою связку «человек + модель», и если станет полезно — расскажи, что у тебя получилось.

[![SKICODING — полная версия с code-patterns, security-checklist, test-discipline](https://img.shields.io/badge/📖_Забрать_SKICODING_(675_строк)-FF6B35?style=for-the-badge)](https://github.com/Barbashin1970/RAGRAF/blob/main/docs/SKILL-SKICODING.md)

---

## 🧰 Стек, в котором я работаю

**Backend**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-D71F00?style=for-the-badge&logo=sqlalchemy&logoColor=white)
![DuckDB](https://img.shields.io/badge/DuckDB-FFF000?style=for-the-badge&logo=duckdb&logoColor=black)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)

**Frontend**

![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![TanStack Query](https://img.shields.io/badge/TanStack_Query-FF4154?style=for-the-badge&logo=react-query&logoColor=white)

**Семантика и онтологии**

![RDF](https://img.shields.io/badge/RDF/Turtle-005A9C?style=for-the-badge)
![SHACL](https://img.shields.io/badge/SHACL-2D8B57?style=for-the-badge)
![OWL](https://img.shields.io/badge/OWL-7B3F00?style=for-the-badge)

**ИИ и инструменты**

![Claude](https://img.shields.io/badge/Claude_API-D97757?style=for-the-badge&logo=anthropic&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)
![Ollama](https://img.shields.io/badge/Ollama-000000?style=for-the-badge&logo=ollama&logoColor=white)

**DevOps**

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=for-the-badge&logo=vercel&logoColor=white)

---

## 🚀 Проекты — открой и потрогай

### 🏢 Прод-системы для бизнеса и нормативки

| Проект | Особенность | Открыть |
|--------|-------------|---------|
| **LEYKA** | **Прод, не демо.** Таск-трекер для команд **до 100 человек**, КОСГУ-категоризация. Внедрён в НГУ, **могу развернуть в любой команде**. Заходи гостем. | [![Open](https://img.shields.io/badge/▶_Открыть_(гость)-0F766E?style=for-the-badge)](https://leyka-nsu.vercel.app) |
| **RAGRAF** | Контроль регламентов через онтологии — SHACL + DuckDB + React Flow. Цикл «датчик → правило → задача → акцептор». | [![Open](https://img.shields.io/badge/▶_Открыть-1E40AF?style=for-the-badge)](https://ragraf.up.railway.app/) |

> Полный цикл по обоим: ТЭО → ТЗ ГОСТ 19.201 → архитектура → реализация → ГОСТ-19 комплект документации.

### 🔬 Научная разработка — верификаторы кода на алгоритмические риски

Опираются на **Сигма-методологию** (полиномиальная сложность). Выявляют потенциально опасные паттерны **до runtime** — то, что классические линтеры не ловят.

| Проект | Особенность | Открыть |
|--------|-------------|---------|
| **POLINOM** (Python) | Верификатор алгоритмических рисков для Python-кода | [![Open](https://img.shields.io/badge/▶_Открыть-6366F1?style=for-the-badge)](https://polinom-sigma-audit.up.railway.app/ru) |
| **POLINOM-JAVA** | Перенос правил Сигмы на JVM + JMH-бенчмарки производительности | [![Open](https://img.shields.io/badge/▶_Открыть-B45309?style=for-the-badge)](https://polinom-java.up.railway.app/) |

### 🛠 Демо и инструменты

| Проект | Особенность | Открыть |
|--------|-------------|---------|
| **AI-SKLAD** | ИИ-анализ остатков склада за 15 минут вместо 2 часов в Excel | [![Open](https://img.shields.io/badge/▶_Открыть-DC2626?style=for-the-badge)](https://ai-sklad.vercel.app/) |
| **STROY** (Safety Builder) | Text-to-SQL по охране труда: чек-листы СИЗ, наряды-допуски, без галлюцинаций ИИ | [![Open](https://img.shields.io/badge/▶_Открыть-EA580C?style=for-the-badge)](https://safety-builder.streamlit.app/) |
| **NSK OpenData Bot** | Бот по открытым данным Новосибирска, паттерн IQ-roadmap | [![Open](https://img.shields.io/badge/▶_Открыть-475569?style=for-the-badge)](https://nsk-opendata-bot.up.railway.app/) |
| **SmartCity Prompt** | Конструктор промптов для городских и нормативных задач | [![Open](https://img.shields.io/badge/▶_Открыть-0E7490?style=for-the-badge)](https://smartcityprompt.vercel.app/) |

### 🎮 Мультиплеер-игры — мой эксперимент с фронт-бэк системами

Игры для **2-4 игроков** с **удалённой игрой по сети**. Учусь строить полные realtime-стэки: лобби, синхронизация состояния, боты, WebSocket клиент↔сервер.

| Проект | Особенность | Открыть |
|--------|-------------|---------|
| **Orbital Race** (GONKA) | PWA-гонки на React 19, до 4 игроков по сети | [![Open](https://img.shields.io/badge/▶_Открыть-059669?style=for-the-badge)](https://gonka-two.vercel.app/) |
| **Tigra Dice** (KNIFFEL) | PWA-Яцзы, до 4 игроков по сети | [![Open](https://img.shields.io/badge/▶_Открыть-0891B2?style=for-the-badge)](https://tigra-dice.vercel.app/) |

### ✨ Личные и обучающие

| Проект | Особенность | Открыть |
|--------|-------------|---------|
| **SOFIA** | Персональная ароматерапия по Юнгу и нумерологии | [![Open](https://img.shields.io/badge/▶_Открыть-7C3AED?style=for-the-badge)](https://sofia-europe.vercel.app/) |
| **Harry Potter Quiz** | Квиз по вселенной HP | [![Open](https://img.shields.io/badge/▶_Открыть-92400E?style=for-the-badge)](https://harrypotterquiz-tau.vercel.app/home) |

---

## 🤝 Давай работать вместе

**У тебя есть идея сложной системы — приходи.**

Я ищу задачи, в которых:

- ✅ Нужно **спроектировать с нуля** и не повторять чужие решения.
- ✅ Есть **домен с правилами и онтологиями** (регламенты, нормативка, чек-листы, графы знаний).
- ✅ Заказчик готов **двигаться итерациями** с живой документацией — а не «дайте спек на 200 страниц, через год вернёмся».
- ✅ Можно **подружить ИИ с детерминированной логикой** — не «свободный LLM-агент», а гибрид «LLM для парсинга → SQL/SPARQL для генерации».

**Что ты получишь:**

- Систему **+ полный комплект документации** (ТЗ ГОСТ 19.201, архитектура, тест-стратегия, README билингвально, отчёты).
- **Прозрачный процесс** — каждый коммит виден, каждое решение зафиксировано как ADR.
- **Никаких чёрных ящиков** — даже ИИ-логика детерминирована и проверяема.

📩 **Напиши**, опиши задачу — и я скажу, через сколько итераций мы её закроем.

---

## 📬 Контакты

- ✉️ **Email:** banksnab@gmail.com
- 💼 **GitHub:** [@Barbashin1970](https://github.com/Barbashin1970)
- 📍 **Локация:** Новосибирск, Россия

---

<div align="center">

**«Сложные системы не строятся в одиночку. Я приглашаю в соавторы и людей, и ИИ — и слежу, чтобы все следы оставались проверяемыми.»**

</div>
