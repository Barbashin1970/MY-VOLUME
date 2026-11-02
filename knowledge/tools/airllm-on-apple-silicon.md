---
title: AirLLM на Apple Silicon — запускается, но в сотни раз медленнее Ollama и на длинном промпте отвечает неверно
description: "Совет из коротких видео «pip install airllm — мощные модели на слабом ноутбуке» проверен замером 2026-11-02 на MacBook Air M2 16 ГБ. Llama 3.1 8B, промпт на 1,5 тыс. токенов: AirLLM — 22,35 с на токен, пик памяти 14,5 ГБ, ответ неверный; Ollama на тех же весах (Q4_K_M) — 0,061 с на токен, ответ верный. На Mac только Llama и Mistral, релиз на PyPI без исправлений для Mac, rope_scaling Llama 3.1 не учитывается, trust_remote_code включён по умолчанию. Вывод: на Mac не использовать, локальные модели — через Ollama."
tags: [tool, airllm, ollama, local-llm, apple-silicon, mlx, macos]
created: 2026-11-02
updated: 2026-11-02
related:
  - knowledge/tools/airllm-apple-silicon-report-2026-11-02.md
  - knowledge/tools/airllm-apple-silicon-report-2026-11-02-appendix.md
  - knowledge/lessons/python-org-macos-urllib-no-root-certificates.md
  - knowledge/skills/otbor-proverok.md
  - projects/active/ragraf/README.md
---

# AirLLM на Apple Silicon

## Когда открывать

Попался совет «поставь AirLLM — запустишь 70B на слабом ноутбуке», или нужно запустить
на Mac модель больше его памяти.

## Коротко — замер

MacBook Air M2 16 ГБ, 2026-11-02, Llama 3.1 8B, промпт на 1,5 тыс. токенов с известным
эталоном ответа, по одному прогону. Подробно — [отчёт](airllm-apple-silicon-report-2026-11-02.md),
протокол и прогонщики — [приложение](airllm-apple-silicon-report-2026-11-02-appendix.md).

| | AirLLM (main `c6f3ec0`), bf16 16,1 ГБ | Ollama 0.40.2, Q4_K_M 4,9 ГБ |
|---|---|---|
| Время на токен | **22,35 с** | **0,061 с** (16,4 ток/с) |
| Подготовка | 35 мин: скачать и нарезать | 5,4 с загрузки |
| Пиковая память | 14,5 ГБ из 16 (`/usr/bin/time -l`) | не мерилась |
| Ответ | неверный | верный |

Разница — примерно в 366 раз. **На Mac AirLLM не использовать.**

## Как устроено и почему так медленно

AirLLM держит в памяти один слой: поднял, посчитал, выгрузил. Но на **каждый новый токен**
он заново читает с диска все слои (`airllm_llama_mlx.py:414–433`) — для 8B это 16,1 ГБ на
токен. Даже TinyLlama 1.1B, целиком лежащая в кэше, дала 3,82 с на токен: время уходит
на пересборку слоёв, а не только на диск. Метод экономит видеопамять ценой скорости;
на Mac, где память общая, не экономится и она.

## Ловушки (на 2026-11-02)

1. **«Установка в одну команду» — неправда для Mac.** `mlx` в зависимостях нет, ставится
   отдельно. Релиз 4.0.0 на PyPI (2026-09-05) не содержит исправлений #368 (`rope_theta`
   портил вывод Llama 3 после ~800–1000 токенов) и #371 (на Mac любая модель шла через
   путь Llama). Оба влиты 2026-10-19 только в main — ставить так:
   `pip install mlx "airllm @ git+https://github.com/lyogavin/airllm.git#subdirectory=air_llm"`.
2. **На macOS только `LlamaForCausalLM` и `MistralForCausalLM`.** DeepSeek, Qwen, Kimi —
   нет; Kimi K3 вообще требует CUDA 12 и flash-attn. Список «Llama 3 • DeepSeek • Mistral •
   Kimi» из видео — про видеокарты NVIDIA.
3. **`rope_scaling` не учитывается.** У Llama 3.1 он типа `llama3`, а в RoPE передаётся
   только `base=rope_theta` (`airllm_llama_mlx.py:107–108`). Вероятная причина неверного
   ответа на длинном промпте; не доказано.
4. **Память не экономится.** Пик 14,5 ГБ, своп вырос с 9,0 до 13,6 ГБ. Вызовов очистки
   кэша MLX в коде нет — предположительно, буферы копятся там.
5. **`trust_remote_code=True` по умолчанию** (#293, открыт): загрузка чужого репозитория
   выполняет его Python-код. Брать модели без файлов `.py` — это видно в API Hugging Face.
6. **Диск: оригинал плюс нарезка по слоям.** Для 70B это 141,1 ГБ и ещё столько же;
   на ноутбуке с 256-гигабайтным SSD такая модель не помещается вовсе.
7. **Смоук на малой модели ничего не доказывает.** У TinyLlama `rope_theta` = 10000 и нет
   `rope_scaling` — ровно тот случай, где оба дефекта пути MLX не проявляются. Проверять
   на целевой модели и длинном промпте со сверкой с эталоном
   ([отбор проверок](../skills/otbor-proverok.md): зелёный набор ничего не доказывает).
8. **Python с python.org.** В опыте не помешал: pip и huggingface_hub берут certifi сами.
   Но `urllib` в той же сборке падает на HTTPS —
   [урок](../lessons/python-org-macos-urllib-no-root-certificates.md).

## Если всё же повторять

1. Отдельный venv, установка из main (ловушка 1), модели скачивать в свой каталог
   через `HF_HOME` — потом удалить одним `rm -rf`.
2. Целевая модель без `.py`, промпт длиннее 1000 токенов, эталон ответа известен заранее.
3. Запуск через `/usr/bin/time -l` — время и `peak memory footprint`.
4. Та же модель в Ollama на том же промпте — без базы число ничего не значит.
5. Стоп: проход дольше 10 минут или падение Metal (#367) — записать «не работает».

Прогонщики и команды — в [приложении](airllm-apple-silicon-report-2026-11-02-appendix.md).

## Что использовать вместо

Ollama с моделями 7–8B в 4 битах (около 5 ГБ). На этом ноутбуке qwen2.5:7b-instruct Q4_K_M
дала 16,9 ток/с на промпте в 1,7 тыс. токенов и ответила верно. Именно она и bge-m3 стоят
в настройках [РАГРАФ](../../projects/active/ragraf/README.md) для локального режима. Модели
12–14B (8–9 ГБ) на 16 ГБ — только при закрытых тяжёлых приложениях.

## Источники

- [AirLLM — репозиторий](https://github.com/lyogavin/airllm), [пакет на PyPI](https://pypi.org/project/airllm/)
- Issues: [#368](https://github.com/lyogavin/airllm/issues/368), [#371](https://github.com/lyogavin/airllm/issues/371),
  [#367](https://github.com/lyogavin/airllm/issues/367), [#364](https://github.com/lyogavin/airllm/issues/364),
  [#295](https://github.com/lyogavin/airllm/issues/295), [#293](https://github.com/lyogavin/airllm/issues/293)
- [Исходный текст пути MLX, коммит c6f3ec0](https://github.com/lyogavin/airllm/blob/c6f3ec04ae69dea6fc8ad5886cc1d017eda291fe/air_llm/airllm/airllm_llama_mlx.py)
