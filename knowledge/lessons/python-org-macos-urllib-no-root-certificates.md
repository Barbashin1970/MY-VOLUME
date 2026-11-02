---
title: Python с python.org на macOS не доверяет ни одному сертификату — curl отвечает, urllib падает
description: Сборка Python с python.org на macOS приходит со своим OpenSSL и без файла корневых сертификатов. Пока не запущен «Install Certificates.command», urllib падает на любом HTTPS с CERTIFICATE_VERIFY_FAILED, хотя curl по тому же адресу отвечает, а certifi в venv установлен.
tags: [lesson, gotcha, stack/python, macos, tls]
severity: medium
project_origin: "leyka"
created: 2026-10-10
updated: 2026-10-10
---

# Урок: Python с python.org на macOS не видит корневых сертификатов

## Что произошло

Скрипт на стандартной библиотеке (`urllib`) должен был войти в боевую LEYKA
и завести 53 задачи. Разведка перед этим шла через `curl` по тому же адресу,
и сервер отвечал. Сухой прогон скрипта прошёл: в сеть он не ходит. Боевой
прогон упал на первом же запросе:

```
ssl.SSLCertVerificationError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed:
unable to get local issuer certificate (_ssl.c:1000)
```

Первым запросом был вход, поэтому в базу не записалось ничего.

## Почему это случилось

`curl` и Python на Mac проверяют сертификаты по разным спискам корней:

| Кто | Чем проверяет | Откуда корни |
|---|---|---|
| `/usr/bin/curl` 8.7.1 | SecureTransport, LibreSSL 3.3.6 | хранилище macOS |
| Python 3.12.0 с python.org | собственный OpenSSL 3.0.11 | `/Library/Frameworks/Python.framework/Versions/3.12/etc/openssl/cert.pem` — **файла нет** |

Установщик python.org кладёт рядом `/Applications/Python 3.12/Install Certificates.command`.
Этот скрипт делает `pip install --upgrade certifi` и ставит на место `cert.pem`
ссылку на набор certifi. Пока его не запускали, у этого Python нет ни одного
корневого сертификата.

С толку сбивает venv. В venv бэкенда LEYKA certifi установлен (его тянут httpx
и requests), но `urllib` о нём не знает: certifi используют только те
библиотеки, которые обращаются к нему сами. В одном и том же окружении httpx
работает, а `urllib` падает. Сам venv смотрит на файл базового интерпретатора,
поэтому ему не помогает и собственный certifi.

Проверка одной командой:

```sh
python3 -c "import ssl, os; p = ssl.get_default_verify_paths(); print(p.openssl_cafile, os.path.exists(p.openssl_cafile))"
# /Library/Frameworks/Python.framework/Versions/3.12/etc/openssl/cert.pem False
```

## Как починили

Проверку сертификатов не отключали, а явно указали Python системный набор корней:

```python
ctx = ssl.create_default_context(cafile="/etc/ssl/cert.pem")
opener = urllib.request.build_opener(urllib.request.HTTPSHandler(context=ctx), ...)
```

`/etc/ssl/cert.pem` входит в macOS и обновляется вместе с системой.

## Чтобы не повторилось

- **На новом Mac с Python от python.org один раз запустить
  `Install Certificates.command`.** Это чинит HTTPS для самого Python и для всех
  venv, собранных на нём: они смотрят в тот же `cert.pem`.
- **Скрипту, который будут запускать на чужой машине, путь к корням указывать
  явно:** `cafile=certifi.where()`, если certifi стоит, иначе `/etc/ssl/cert.pem`
  на macOS.
- **Не отключать проверку: никаких `verify=False` и `ssl._create_unverified_context()`.**
  Особенно когда скрипт входит на боевой сервер под root.
- **То, что отвечает `curl`, ничего не говорит о Python.** Связь проверять
  тем же интерпретатором и той же библиотекой, которыми пойдёт работа.
- **Сухой прогон должен ходить в сеть хотя бы на чтение.** Здесь он только
  собирал задачи, и TLS всплыл уже в боевом прогоне. Спас порядок: вход стоял
  раньше первой записи. Правило для скриптов «сухой прогон → запись»: в сухом
  прогоне сделать один читающий запрос (`/health`, вход, список), а боевой
  прогон начинать со входа, а не с записи.

## Связанное

- Проект: [LEYKA](../../projects/active/leyka/README.md) — заведение КТ плана на боевую доску.
- Соседний урок того же дня: [splitlines-drops-rows-on-soft-line-break](splitlines-drops-rows-on-soft-line-break.md).
  Там ошибку поймал сухой прогон, здесь — нет.
