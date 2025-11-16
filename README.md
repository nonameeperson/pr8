# JwtWorkshop

Простий .NET 9 Minimal API, що демонструє автентифікацію за допомогою JWT (JSON Web Tokens) та авторизацію на основі ролей (RBAC).

## 🚀 Локальний запуск

### 1. Встановіть залежності

Якщо ви цього ще не зробили, додайте необхідний пакет для JWT:

```bash
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer --version 9.0.0
2. Встановіть секретний ключ
Для шифрування токенів потрібен секретний ключ. Ключ повинен мати довжину щонайменше 32 символи (256 біт).

Для Windows (PowerShell):

PowerShell

$env:JWT_SECRET="this_is_my_super_secure_key_that_is_32_bytes_long"
Для macOS/Linux (Bash):

Bash

export JWT_SECRET="this_is_my_super_secure_key_that_is_32_bytes_long"
3. Запустіть сервер
Bash

dotnet run
Сервер запуститься на http://localhost:XXXX (наприклад, http://localhost:5261).

⚙️ Тестування API
Використовуйте curl або Postman для тестування ендпоінтів.

1. Вхід (Login)
Отримує JWT-токен.

Метод: POST

Ендпоінт: /login

Приклад (curl):

Bash

curl -X POST http://localhost:5261/login \
     -H "Content-Type: application/json" \
     -d '{"Email":"user@example.com","Password":"user123"}'
Postman:

Тип запиту: POST

URL: http://localhost:5261/login

Body -> raw -> JSON

Тіло (Body):

JSON

{
  "Email": "user@example.com",
  "Password": "user123"
}
Відповідь (200 OK):

JSON

{
  "access_token": "eyJhbGciOiJIUz...",
  "token_type": "Bearer",
  "expires_in": 900
}
Скопіюйте access_token для наступних запитів.

2. Отримання профілю (Захищено)
Повертає ID та роль користувача з токена.

Метод: GET

ЕндпоіNT: /profile

Авторизація: Bearer Token

Приклад (curl): (Замініть $TOKEN на ваш access_token)

Bash

curl http://localhost:5261/profile \
     -H "Authorization: Bearer $TOKEN"
Postman:

Тип запиту: GET

URL: http://localhost:5261/profile

Authorization -> Type: Bearer Token

Вставте ваш access_token у поле Token.

Відповідь (200 OK):

JSON

{
  "user_id": "2",
  "role": "user"
}
3. Видалення користувача (Тільки Admin)
Демонструє авторизацію за роллю "admin".

Метод: DELETE

Ендпоінт: /users/5 (або будь-яке число)

Авторизація: Bearer Token (потрібен токен адміна)

Тест 1: З токеном user (Очікуємо 403)
Отримайте токен для user@example.com.

Спробуйте виконати запит.

Приклад (curl):

Bash

curl -i -X DELETE http://localhost:5261/users/5 \
     -H "Authorization: Bearer $USER_TOKEN"
Відповідь: HTTP/1.1 403 Forbidden

Тест 2: З токеном admin (Очікуємо 200)
Отримайте токен для admin@example.com.

Виконайте запит з токеном адміна.

Приклад (curl):

Bash

curl -X DELETE http://localhost:5261/users/5 \
     -H "Authorization: Bearer $ADMIN_TOKEN"
Відповідь (200 OK):

JSON

{
  "message": "User 5 deleted (demo)"
}
```
