---
title: Приложения к отчёту об опыте AirLLM против Ollama (2026-11-02)
description: "Протокол замеров, тексты прогонщиков, исходные данные и команды опыта AirLLM против Ollama на MacBook Air M2 16 ГБ. Вынесено из отчёта по правилу vault «не длиннее 300 строк». Временная папка опыта удаляется при перезагрузке, поэтому прогонщики и сырые результаты сохранены здесь дословно."
tags: [report, experiment, appendix, tool/airllm, tool/ollama, local-llm]
created: 2026-11-02
updated: 2026-11-02
related:
  - knowledge/tools/airllm-apple-silicon-report-2026-11-02.md
  - knowledge/tools/airllm-on-apple-silicon.md
---

# Приложения к отчёту

Отчёт: [airllm-apple-silicon-report-2026-11-02.md](airllm-apple-silicon-report-2026-11-02.md).

## Приложение А. Протокол замеров

Все прогоны — 2026-11-02, по одному на конфигурацию. Время — от начала генерации,
в секундах, как его печатал прогонщик.

**AirLLM, TinyLlama 1.1B, короткий промпт** (`results/tinyllama.log`):

```
token 1: 6.6s  token 2: 11.0s  token 3: 16.2s  token 4: 21.1s
token 5: 24.2s token 6: 27.6s  token 7: 30.5s  token 8: 33.3s
/usr/bin/time -l: 270.38 real, maximum resident set size 1558904832, peak memory footprint 1625884736
```

**AirLLM, Llama 3.1 8B, длинный промпт** (`results/llama8b.log`):

```
token 1: 25.3s   token 2: 64.0s   token 3: 84.7s   token 4: 106.0s  token 5: 130.2s
token 6: 149.1s  token 7: 173.6s  token 8: 195.3s  token 9: 218.0s  token 10: 241.7s
token 11: 260.1s token 12: 284.0s token 13: 302.0s token 14: 325.5s token 15: 346.9s
token 16: 365.3s token 17: 382.9s
интервалы: 38.7 20.7 21.3 24.2 18.9 24.5 21.7 22.7 23.7 18.4 23.9 18.0 23.5 21.4 18.4 17.6
/usr/bin/time -l: 2501.65 real, maximum resident set size 5805522944, peak memory footprint 14522799336
```

Самый длинный интервал (38,7 с) — первый шаг после чтения промпта. Медиана и разброс
посчитаны по округлённым до десятых отметкам журнала, среднее — прогонщиком по точным.

**Итоговые файлы прогонщиков** (дословно):

```json
{"repo_id": "TinyLlama/TinyLlama-1.1B-Chat-v1.0", "load_and_split_s": 212.8, "prompt_tokens": 27,
 "new_tokens": 8, "first_token_s": 6.6, "next_token_avg_s": 3.82, "total_gen_s": 33.3,
 "answer": "The capital of France is Paris.</s>"}
{"repo_id": "unsloth/Meta-Llama-3.1-8B-Instruct", "load_and_split_s": 2112.4, "prompt_tokens": 1572,
 "new_tokens": 17, "first_token_s": 25.3, "next_token_avg_s": 22.35, "total_gen_s": 382.9,
 "answer": "Двойное описание ошибок интеграции в этой заметке.<|eot_id|>"}
{"model": "llama3.1:8b", "load_s": 5.4, "prompt_tokens": 1547, "prompt_eval_s": 12.4, "new_tokens": 50,
 "tok_per_s": 16.4, "next_token_avg_s": 0.061, "total_s": 20.9,
 "answer": "В этой заметке описаны две ошибки интеграции: первая - двойной префикс в пути к API LEYKA, что приводит к 404-ошибке, а вторая - игнорирование неиз"}
{"model": "qwen2.5:7b-instruct-q4_K_M", "load_s": 4.9, "prompt_tokens": 1681, "prompt_eval_s": 12.5,
 "new_tokens": 50, "tok_per_s": 16.9, "next_token_avg_s": 0.059, "total_s": 20.4,
 "answer": "В этой заметке описаны две ошибки интеграции:\n1. Неправильная склейка пути к API, что приводило к 404 ошибке.\n2. Игнорирование"}
```

**Базовый замер до опыта:** qwen2.5:7b-instruct Q4_K_M, `POST /api/generate`, промпт
«Объясни в пяти предложениях, что такое послойная загрузка языковой модели с диска
и почему она медленная.», `num_predict` 200, temperature 0: загрузка 27,31 с, промпт
63 токена за 0,99 с, 161 токен за 11,73 с — 13,7 ток/с, всего 40,18 с.

## Приложение Б. Прогонщики

`bench_airllm.py` — запуск: `python -I bench_airllm.py <repo_id> <prompt_file> <max_new_tokens> <out_json>`.

```python
import json
import sys
import time

import mlx.core as mx
from airllm import AutoModel

repo_id, prompt_file, max_new, out_json = sys.argv[1], sys.argv[2], int(sys.argv[3]), sys.argv[4]

t0 = time.time()
model = AutoModel.from_pretrained(repo_id)
t_load = time.time() - t0

messages = json.load(open(prompt_file, encoding="utf-8"))
text = model.tokenizer.apply_chat_template(messages, add_generation_prompt=True, tokenize=False)
ids = model.tokenizer(text, return_tensors="np", add_special_tokens=False)["input_ids"]
n_prompt = ids.shape[1]
print(f"prompt tokens: {n_prompt}", flush=True)

tokens, stamps = [], []
t1 = time.time()
for y in model.model_generate(mx.array(ids)):
    tokens.append(y.item())
    stamps.append(time.time() - t1)
    print(f"token {len(tokens)}: {stamps[-1]:.1f}s", flush=True)
    if len(tokens) >= max_new or tokens[-1] in (model.tokenizer.eos_token_id, 128009):
        break

answer = model.tokenizer.decode(tokens)
per_token = [b - a for a, b in zip(stamps, stamps[1:])]
res = {
    "repo_id": repo_id,
    "load_and_split_s": round(t_load, 1),
    "prompt_tokens": n_prompt,
    "new_tokens": len(tokens),
    "first_token_s": round(stamps[0], 1),
    "next_token_avg_s": round(sum(per_token) / len(per_token), 2) if per_token else None,
    "total_gen_s": round(stamps[-1], 1),
    "answer": answer,
}
json.dump(res, open(out_json, "w", encoding="utf-8"), ensure_ascii=False, indent=2)
print(json.dumps(res, ensure_ascii=False, indent=2))
```

`model_generate` вызывается напрямую, а не `generate`: так видно время каждого токена.
`128009` — токен `<|eot_id|>` у Llama 3.

`bench_ollama.py` — запуск: `python3 -I bench_ollama.py <model> <prompt_file> <max_new_tokens> <out_json>`.
Только стандартная библиотека; адрес `http://localhost`, поэтому вопрос сертификатов
python.org-сборки не возникает.

```python
import json
import sys
import urllib.request

model, prompt_file, max_new, out_json = sys.argv[1], sys.argv[2], int(sys.argv[3]), sys.argv[4]
body = {
    "model": model,
    "messages": json.load(open(prompt_file, encoding="utf-8")),
    "stream": False,
    "keep_alive": 0,
    "options": {"num_predict": max_new, "temperature": 0, "num_ctx": 8192},
}
req = urllib.request.Request("http://localhost:11434/api/chat", data=json.dumps(body).encode(),
                             headers={"Content-Type": "application/json"})
d = json.load(urllib.request.urlopen(req, timeout=600))
ns = 1e9
res = {
    "model": model,
    "load_s": round(d["load_duration"] / ns, 1),
    "prompt_tokens": d["prompt_eval_count"],
    "prompt_eval_s": round(d["prompt_eval_duration"] / ns, 1),
    "new_tokens": d["eval_count"],
    "tok_per_s": round(d["eval_count"] / (d["eval_duration"] / ns), 1),
    "next_token_avg_s": round(d["eval_duration"] / ns / d["eval_count"], 3),
    "total_s": round(d["total_duration"] / ns, 1),
    "answer": d["message"]["content"],
}
json.dump(res, open(out_json, "w", encoding="utf-8"), ensure_ascii=False, indent=2)
print(json.dumps(res, ensure_ascii=False, indent=2))
```

## Приложение В. Исходные данные

- `prompt_short.json`: `[{"role": "user", "content": "What is the capital of France? Answer in one sentence."}]`
- `prompt_long.json` собран так (SHA-256 файла `731a6b87…2fd3ec3`):

```python
note = Path("knowledge/lessons/foreign-api-contract-drifts-silently.md").read_text(encoding="utf-8")
json.dump([{"role": "user", "content": note + "\n\n---\nОтветь по-русски в двух-трёх предложениях: "
            "какие две ошибки интеграции описаны в этой заметке?"}], open("prompt_long.json", "w"), ensure_ascii=False)
```

Заметка на момент опыта — 4524 знака, SHA-256 `8c84b11f…5a56cc055`. Если она с тех пор
правилась, для точного повтора нужна её редакция от 2026-11-02 из git.

## Приложение Г. Команды

Окружение стенда:

```sh
system_profiler SPHardwareDataType | grep -E "Model Name|Model Identifier|Chip|Total Number of Cores|Memory:"
sw_vers; df -h /System/Volumes/Data; python3 --version; ollama --version
sysctl -n kern.memorystatus_vm_pressure_level; sysctl -n vm.swapusage   # до, во время и после прогонов
top -l 1 -o mem -n 20 -stats pid,command,mem,cmprs                      # кто держит память
```

Установка AirLLM — отдельный venv, ветка main:

```sh
python3 -m venv venv
./venv/bin/pip install mlx "airllm @ git+https://github.com/lyogavin/airllm.git#subdirectory=air_llm"
cat venv/lib/python3.12/site-packages/airllm-*.dist-info/direct_url.json   # commit_id c6f3ec04…
```

Прогоны (модели AirLLM скачивались в отдельный каталог через `HF_HOME`):

```sh
HF_HOME=$PWD/hf /usr/bin/time -l ./venv/bin/python -I bench_airllm.py TinyLlama/TinyLlama-1.1B-Chat-v1.0 prompt_short.json 20 tinyllama.json
HF_HOME=$PWD/hf /usr/bin/time -l ./venv/bin/python -I bench_airllm.py unsloth/Meta-Llama-3.1-8B-Instruct prompt_long.json 50 llama8b.json
ollama pull llama3.1:8b
python3 -I bench_ollama.py llama3.1:8b prompt_long.json 50 ollama_llama8b.json
python3 -I bench_ollama.py qwen2.5:7b-instruct-q4_K_M prompt_long.json 50 ollama_qwen7b.json
```

Первоисточники:

```sh
curl -s https://pypi.org/pypi/airllm/json            # версия, даты релизов, requires_dist
curl -s https://api.github.com/repos/lyogavin/airllm/issues/368   # и 371, 367, 364, 295, 293
curl -s https://api.github.com/repos/lyogavin/airllm/tags         # последний тег v4.0.0
curl -s "https://huggingface.co/api/models/<repo>?blobs=true"     # gated, архитектура, размер safetensors
curl -sL https://huggingface.co/<repo>/resolve/main/config.json   # rope_theta, rope_scaling, слои
curl -s https://raw.githubusercontent.com/lyogavin/airllm/c6f3ec04ae69dea6fc8ad5886cc1d017eda291fe/air_llm/airllm/airllm_llama_mlx.py \
  | grep -nE "rope|scaling|clear_cache|set_cache_limit"
```

Уборка после опыта: удалены venv (1,3 ГБ) и каталог `hf` (34 ГБ); из Ollama удалена
`llama3.1:8b` (`ollama rm llama3.1:8b`); оставлены qwen2.5:7b-instruct Q4_K_M и bge-m3 — на
них настроен РАГРАФ.
