# Рівень 2 — Результати

## Приклад очікуваного виведення програми

### === ЗАПИТ ДО АВТОРИЗОВАНОГО РЕСУРСУ ЧЕРЕЗ CURL ===

```text
> POST /auth/login HTTP/1.1
> Host: dummyjson.com
> Content-Type: application/json
> Accept: application/json

< HTTP/1.1 200 OK
< Content-Type: application/json; charset=utf-8
< Connection: keep-alive

{
  "id": 1,
  "username": "emilys",
  "email": "emily.johnson@x.dummyjson.com",
  "firstName": "Emily",
  "lastName": "Johnson",
  "gender": "female",
  "image": "https://dummyjson.com/icon/emilys/128",
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### === ПЕРЕВІРКА МЕТРИК ПРОДУКТИВНОСТІ МЕРЕЖЕВОГО З'ЄДНАННЯ ===

```text
DNS Lookup: 0.014210s
TCP Connect: 0.048512s
TLS Handshake: 0.092140s
Time To First Byte (TTFB): 0.185210s
Total Transaction Time: 0.215430s
```

### === БАЗОВІ CURL ОПЕРАЦІЇ ===

#### GET запит — отримання користувачів

```bash
$ curl -i "https://dummyjson.com/users?limit=2&select=firstName,email"

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Transfer-Encoding: chunked

{
  "users": [
    {
      "id": 1,
      "firstName": "Emily",
      "email": "emily.johnson@x.dummyjson.com"
    },
    {
      "id": 2,
      "firstName": "Michael",
      "email": "michael.williams@x.dummyjson.com"
    }
  ],
  "total": 2,
  "skip": 0,
  "limit": 2
}
```

#### POST запит — додавання посту

```bash
$ curl -X POST "https://dummyjson.com/posts/add" \
  -H "Content-Type: application/json" \
  -d '{"title":"Test Post","userId":1}'

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8

{
  "id": 101,
  "title": "Test Post",
  "userId": 1
}
```

### === АНАЛІЗ ЗАГОЛОВКІВ БЕЗ ТІЛА ===

#### curl -I для /products

```bash
$ curl -I https://dummyjson.com/products

HTTP/1.1 200 OK
Date: Fri, 11 Sep 2026 17:33:14 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 44346
Connection: keep-alive
etag: W/"ac3a-Q0j5X7Zb/GG4CpZwhP3POutAwN4"
cache-control: no-store
server: cloudflare
```

#### Статус 404

```bash
$ curl -i https://httpbin.org/status/404

HTTP/1.1 404 NOT FOUND
Content-Type: text/html; charset=utf-8
Content-Length: 0
```

#### Статус 500

```bash
$ curl -i https://httpbin.org/status/500

HTTP/1.1 500 INTERNAL SERVER ERROR
Content-Type: text/html; charset=utf-8
Content-Length: 0
```

---

## Висновки рівня 2

✅ GET запити з параметрами передаються через URL  
✅ POST запити передають JSON тіло з -d флагом  
✅ -i флаг показує заголовки + тіло  
✅ -I флаг показує тільки заголовки (HEAD запит)  
✅ Статус коди коректно повертаються (200, 201, 404, 500)  
✅ Автентифікація через Bearer токен працює  
✅ Мережеві метрики вимірюються -w форматом  
