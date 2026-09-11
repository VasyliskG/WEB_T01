# Структура проекту — Практична робота №1

## Потрібні інструменти

### Обов'язкові
- ✅ `curl` — консольний клієнт HTTP (перевірка: `curl --version`)
- ✅ `git` — контроль версій (перевірка: `git --version`)
- ✅ Веббраузер з DevTools (Chrome, Firefox, Safari)

### Рекомендовані
- Postman (вебверсія або десктоп) — графічний REST клієнт
- VS Code + REST Client розширення
- Текстовий редактор для документації

### API-сервіси (вже готові, безплатні)
- `https://dummyjson.com/` — REST API з CRUD операціями
- `https://httpbin.org/` — сервіс-ехо для тестування заголовків/cookies

---

## Рекомендована структура каталогів

```
T01/
├── README.md                 # Основна документація проекту
├── PR1.md                    # (існує) Завдання
├── PROJECT_STRUCTURE.md      # (цей файл) Орієнтир
│
├── level_1/                  # Рівень 1 — базовий
│   ├── postman_requests.json # Експорт колекції Postman
│   ├── devtools_analysis.md  # Результати аналізу DevTools
│   └── screenshots/          # Скриншоти Postman/DevTools
│       ├── get_request.png
│       ├── post_request.png
│       ├── put_request.png
│       ├── delete_request.png
│       └── devtools_network.png
│
├── level_2/                  # Рівень 2 — curl діагностика
│   ├── curl_commands.sh      # Сценарій з усіма curl-запитами
│   ├── curl_results.txt      # Необроблені вихідні дані
│   ├── curl_analysis.md      # Розбір та аналіз результатів
│   ├── cookies.txt           # Cookie jar (згенерований)
│   └── performance_metrics.md # Таблиця з таймінгами
│
├── level_3/                  # Рівень 3 — HTTPS/TLS/CORS
│   ├── tls_handshake.md      # Аналіз TLS рукостискання
│   ├── cookie_lifecycle.md   # Дослідження cookies
│   ├── cookie_security.md    # HttpOnly/Secure/SameSite атрибути
│   ├── cors_preflight.md     # Аналіз CORS OPTIONS запитів
│   └── security_headers.txt  # Збережені заголовки безпеки
│
└── ANSWERS.md                # Відповіді на контрольні питання
```

---

## Оформлення документів

### README.md (обов'язково)
```markdown
# Практична робота №1: Дослідження мережевих протоколів

## Проект

[Короткий опис, що було зроблено]

## Структура проекту

[Описати каталоги та файли]

## Як запустити

[Інструкції для репродукції]

### Рівень 1
[Результати + скриншоти]

### Рівень 2
[Результати + output]

### Рівень 3
[Результати + аналіз]

## Висновки

[Ключові вивчені концепції]

## Відповіді на контрольні питання

[1. ... 2. ... тощо]
```

### Аналіз DevTools (level_1/devtools_analysis.md)

Таблиця структури запиту:

| Категорія | Параметр | Значення |
|-----------|----------|----------|
| **General** | Request URL | `https://dummyjson.com/products` |
| | Request Method | `GET` |
| | Status Code | `200 OK` |
| | Remote Address | `IP:PORT` |
| **Response Headers** | `content-type` | `application/json; charset=utf-8` |
| | `date` | `[server date]` |
| | `server` | `[server info]` |
| | `etag` | `[hash]` |
| **Request Headers** | `accept` | `*/*` |
| | `user-agent` | `curl/7.x.x` |
| | `accept-encoding` | `gzip, deflate` |
| **Timing** | DNS Lookup | `[ms]` |
| | Initial Connection | `[ms]` |
| | TTFB | `[ms]` |
| | Content Download | `[ms]` |

### curl_results.txt — формат збереження

```bash
=== TEST 1: GET /products/1 ===
curl -s https://dummyjson.com/products/1 | jq .

{результат}

=== TEST 2: POST /products/add ===
curl -X POST https://dummyjson.com/products/add \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","price":99.99}'

{результат}

# ... і так далі
```

### curl_analysis.md — структура аналізу

```markdown
## Запит: GET /products/1

**Команда:**
\`\`\`bash
curl -s https://dummyjson.com/products/1
\`\`\`

**Статус:** 200 OK

**Аналіз:**
- Успішне отримання даних
- JSON формат дозволяє обробку на клієнті
- ...

---
```

### performance_metrics.md

| Запит | DNS (ms) | Connect (ms) | TLS (ms) | TTFB (ms) | Total (ms) |
|-------|----------|-------------|---------|-----------|-----------|
| `GET /products/1` | 15 | 45 | 90 | 185 | 215 |
| `GET /users?limit=2` | 12 | 42 | 88 | 180 | 210 |
| ... | ... | ... | ... | ... | ... |

### tls_handshake.md

```markdown
## TLS Handshake для dummyjson.com

**Команда:**
\`\`\`bash
curl -v https://dummyjson.com/products/1
\`\`\`

**Етапи:**

1. **TCP Connection**
   - Адреса: [IP]
   - Порт: 443
   - Status: Connected

2. **ClientHello**
   - TLS версії: 1.2, 1.3
   - Список Cipher Suites: ...

3. **ServerHello**
   - Обрана версія: TLSv1.3
   - Узгоджений шифр: ...

4. **Certificate Chain**
   - Видавець: [CA]
   - Subject: dummyjson.com
   - Дійсний до: [дата]
   - Alt Names: *.dummyjson.com, ...

5. **Key Exchange**
   - Алгоритм: ECDHE
   - Симетричне шифрування: AES-256-GCM

---
```

### ANSWERS.md — контрольні питання

```markdown
# Відповіді на контрольні питання

## 1. Чим відрізняється стартовий рядок від статусного?

**Стартовий рядок (запит):**
\`GET /resource HTTP/1.1\`
- Метод HTTP
- URI ресурсу
- Версія протоколу

**Статусний рядок (відповідь):**
\`HTTP/1.1 200 OK\`
- Версія протоколу
- Код статусу (3 цифри)
- Текстове пояснення

...

## 2. ...
```

---

## Workflow (запропонований порядок робіт)

### День 1: Рівень 1
1. ✅ Встановити Postman
2. ✅ Виконати 4 запити (GET, POST, PUT, DELETE)
3. ✅ Зберегти запити в JSON
4. ✅ Відкрити DevTools, повторити запити, записати таблицю
5. ✅ Зробити скриншоти

### День 2: Рівень 2
1. ✅ Підготувати `curl_commands.sh` зі всіма запитами
2. ✅ Запустити скрипт, збережіти вихід в `curl_results.txt`
3. ✅ Розібрати кожен результат в `curl_analysis.md`
4. ✅ Вимірити таймінги (-w формат)

### День 3: Рівень 3
1. ✅ Запустити `curl -v` для TLS аналізу
2. ✅ Дослідити cookies (-c/-b прапорці)
3. ✅ Симулювати CORS preflight (OPTIONS)
4. ✅ Написати обґрунтування атрибутів безпеки

### День 4: Завершення
1. ✅ Написати README.md
2. ✅ Надати відповіді на контрольні питання
3. ✅ Відкомітити все в Git
4. ✅ Підготуватися до захисту

---

## Контрольний список перед здачею

- [ ] Git репозиторій з усіма файлами
- [ ] README.md з описом + скриншотами
- [ ] Рівень 1: Postman JSON + DevTools таблиця + скриншоти
- [ ] Рівень 2: `curl_commands.sh` + результати + аналіз
- [ ] Рівень 3: TLS, cookies, CORS аналізи
- [ ] ANSWERS.md з 8 відповідями на питання
- [ ] Жодних помилок, чистий код
- [ ] Готовність пояснити кожен пункт викладачеві
