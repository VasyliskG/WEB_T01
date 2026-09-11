# Рівень 1 — базовий: дослідження HTTP через DevTools та Postman (HTTP файли)

## Мета

Дослідити HTTP CRUD-операції (GET, POST, PUT, DELETE) до публічного REST API та аналізувати мережеву активність через DevTools браузера.

---

## Структура завдань

| Завдання | Метод | Endpoint | Очікуваний результат | Файл |
|----------|-------|----------|---------------------|------|
| 1 | `GET` | `/products/1` | `200 OK` | [GET.http](t2/GET/GET.http) |
| 2 | `POST` | `/products/add` | `201 Created` | [POST.http](t2/POST/POST.http) |
| 3 | `PUT` | `/products/1` | `200 OK` | [PUT.http](t2/PUT/PUT.http) |
| 4 | `DELETE` | `/products/1` | `200 OK` | [DELETE.http](t2/DELETE/DELETE.http) |

---

## Завдання 1: Підготовка середовища

**Файл:** [preparation.md](t1/preparation.md)

Перевірка версії `curl` та доступних протоколів.

**Результат:** curl 8.18.0 встановлено із підтримкою HTTP/1.1, HTTP/2, HTTPS, TLS 1.2/1.3.

---

## Завдання 2–5: CRUD операції

HTTP-файли розташовані у [t2](t2) та містять REST Client запити для VS Code або подібних інструментів.

### GET /products/1

```http
GET https://dummyjson.com/products/1
```

**Результат:** `200 OK`, тіло містить JSON товару з полями `id`, `title`, `price`, `description`, `images` тощо.

### POST /products/add

```http
POST https://dummyjson.com/products/add
Content-Type: application/json

{
  "title": "Essence Mascara Lash Princess",
  "price": 9.99
}
```

**Результат:** `201 Created`, відповідь містить автоматично згенерований `id` (наприклад, `195`).

### PUT /products/1

```http
PUT https://dummyjson.com/products/1
Content-Type: application/json

{
    "title": "Updated Title"
}
```

**Результат:** `200 OK`, поле `title` оновлено, решта полів збережена.

### DELETE /products/1

```http
DELETE https://dummyjson.com/products/1
```

**Результат:** `200 OK`, відповідь містить маркер `"isDeleted": true`.

---

## Завдання 6: DevTools аналіз

**Файл:** [devtools_analysis.md](t3/devtools_analysis.md)

Аналіз мережевої активності запиту `GET /products` через браузерні DevTools.

### Зібрані дані

- **URL запиту:** https://dummyjson.com/products
- **Метод:** GET
- **Статус:** 200 OK
- **Час відповіді:** ~141 мс
- **Фази з'єднання:**
  - DNS Lookup: 1.53 мс
  - Queueing: 0.59 мс
  - Request: 46.43 мс
  - TTFB: 21.94 мс
  - Content Download: 70.64 мс

### Заголовки

**Response Headers:**
- `content-type: application/json; charset=utf-8`
- `server: cloudflare`
- `etag: W/"ac3a-Q0j5X7Zb/GG4CpZwhP3POutAwN4"`

**Request Headers:**
- `accept: text/html, application/xhtml+xml, ...`
- `user-agent: Mozilla/5.0 (X11; Linux x86_64) ...`
- `accept-encoding: gzip, deflate, br, zstd`

### Screenshot

DevTools Network панель збережена у [devtools_network.png](t3/devtools_network.png).

---

## Запуск запитів

### Варіант 1: VS Code REST Client

1. Відкрити файли з папки [t2](t2) у VS Code.
2. Клікнути на `Send Request` вище кожного запиту.
3. Відповідь з'явиться у панелі справа.

### Варіант 2: curl у терміналі

```bash
# GET
curl https://dummyjson.com/products/1

# POST
curl -X POST https://dummyjson.com/products/add \
  -H "Content-Type: application/json" \
  -d '{"title":"Test","price":9.99}'

# PUT
curl -X PUT https://dummyjson.com/products/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"Updated"}'

# DELETE
curl -X DELETE https://dummyjson.com/products/1
```

---

## Висновок

- `GET` безпечний і ідемпотентний; не змінює стан ресурсу.
- `POST` з `201 Created` означає нову залежну сутність; неідемпотентний (кожний запит створює новий ресурс).
- `PUT` ідемпотентний; повторний запит із тим самим тілом дає той же результат.
- `DELETE` ідемпотентний; видалення вже видаленого ресурсу повертає той же статус.
- TTFB домінує у вимірюванні часу; мережеві затримки більші за обробку на сервері.
- Cloudflare кешує відповіді та прискорює доставку контенту.

---

**Дата створення:** 11 вересня 2026 р.  
**API:** https://dummyjson.com/  
**Бібліографія:** [PR1.md](../PR1.md) — детальний опис завдання
