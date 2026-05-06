# CityPulse API Documentation

## Базовый URL
http://localhost:8080/api/v1

text

---

## Статус сервера

### Проверка статуса
GET /status

text

**Ответ:**
```json
{
  "status": "online",
  "version": "1.0.0",
  "features": {
    "auth": true,
    "places": true,
    "generation": true,
    "audio": true,
    "localization": true
  }
}
Аутентификация
Регистрация
text
POST /auth/register
Content-Type: application/json

{
  "email": "user@mail.ru",
  "password": "123456",
  "name": "Имя пользователя"
}
Ответ (успех):

json
{
  "userId": "d3ae9e91-7a64-41e5-a107-89a9ff9964ac",
  "email": "user@mail.ru",
  "name": "Имя пользователя",
  "message": "Регистрация успешна"
}
Ответ (ошибка):

json
{
  "error": "The email address is already in use by another account."
}
Вход
text
POST /auth/login
Content-Type: application/json

{
  "email": "user@mail.ru",
  "password": "123456"
}
Ответ (успех):

json
{
  "userId": "d3ae9e91-7a64-41e5-a107-89a9ff9964ac",
  "email": "user@mail.ru",
  "name": "Имя пользователя",
  "token": "eyJhbGciOiJSUzI1NiIs..."
}
Ответ (ошибка):

json
{
  "error": "Неверный email или пароль"
}
Получить профиль
text
GET /auth/profile/{userId}
Ответ:

json
{
  "uid": "d3ae9e91-7a64-41e5-a107-89a9ff9964ac",
  "name": "Имя пользователя",
  "language": "ru",
  "geolocationEnabled": true,
  "createdAt": "2026-05-01T07:27:07.013Z",
  "lastActiveAt": "2026-05-01T07:27:07.013Z"
}
Обновить профиль
text
PUT /users/{userId}
Content-Type: application/json

{
  "name": "Новое имя",
  "language": "en",
  "geolocationEnabled": true
}
Ответ:

json
{
  "message": "Обновлено",
  "user": {
    "uid": "d3ae9e91-7a64-41e5-a107-89a9ff9964ac",
    "name": "Новое имя",
    "language": "en",
    "geolocationEnabled": true
  }
}
Достопримечательности
Получить все места
text
GET /places
Ответ: массив из 20 объектов

json
[
  {
    "id": "place_001",
    "name": {
      "ru": "Эрмитаж",
      "en": "Hermitage Museum"
    },
    "category": "MUSEUM",
    "latitude": 59.9398,
    "longitude": 30.3146,
    "address": {
      "ru": "Дворцовая пл., 2, Санкт-Петербург",
      "en": "Palace Square, 2, Saint Petersburg"
    },
    "images": ["https://example.com/hermitage1.jpg"],
    "descriptionShort": {
      "ru": "Крупнейший музей",
      "en": "Largest museum"
    },
    "rating": 4.9
  }
]
Получить места по категории
text
GET /places?category=MUSEUM
Категории:

MUSEUM — Музеи

TEMPLE — Храмы

PARK — Парки

MONUMENT — Памятники

HISTORICAL_BUILDING — Исторические здания

ARCHITECTURE — Архитектура

Ответ: отфильтрованный массив мест

Поиск по названию
text
GET /places/search?query=собор
Ответ: массив подходящих мест

Места по геолокации
text
GET /places?lat=59.9398&lng=30.3146&radius=5.0
Параметры:

lat — широта

lng — долгота

radius — радиус в км (по умолчанию 5.0)

Ответ: массив мест в радиусе

Получить одно место
text
GET /places/{placeId}
Ответ: один объект места

Генерация рассказа
Сгенерировать рассказ
text
POST /generations
Content-Type: application/json

{
  "placeId": "place_001",
  "style": "HISTORIAN",
  "language": "ru"
}
Стили:

STANDARD — Стандартный

HISTORIAN — Историк

FOR_KIDS — Для детей

LEGEND — Легенда

ROMANTIC — Романтик

Языки: ru, en

Ответ:

json
{
  "generationId": "f9bfd57d-0929-4cf4-8c3d-de945c5ab48a",
  "placeName": "Эрмитаж",
  "text": "Государственный Эрмитаж — один из крупнейших...",
  "hasAudio": true,
  "audioBase64": "SUQzBAAAAAAAI1RTU0UAAAAPAAADTGF2ZjU3..."
}
Получить сгенерированный рассказ
text
GET /generations/{generationId}
Ответ:

json
{
  "id": "f9bfd57d-0929-4cf4-8c3d-de945c5ab48a",
  "placeId": "place_001",
  "style": "HISTORIAN",
  "text": {
    "ru": "Государственный Эрмитаж — один из крупнейших..."
  },
  "language": "ru",
  "hasAudio": true,
  "createdAt": "2026-05-01T10:00:00Z"
}
Скачать аудио
text
GET /generations/{generationId}/audio
Ответ: бинарный файл audio/mpeg (MP3)

Локализация
Получить все переводы для языка
text
GET /localization?lang=ru
Языки:

ru — Русский

en — English

zh — 中文

es — Español

de — Deutsch

fr — Français

Ответ:

json
{
  "welcome_title": "Добро пожаловать в CityPulse!",
  "welcome_subtitle": "Ваш персональный гид с искусственным интеллектом",
  "enter_name": "Как вас зовут?",
  "search_placeholder": "Найти достопримечательность",
  "my_location": "Моё местоположение",
  "categories_all": "Все",
  "category_historical": "Исторические здания",
  "category_park": "Парки",
  "category_museum": "Музеи",
  "category_temple": "Храмы",
  "category_monument": "Памятники",
  "category_architecture": "Архитектура",
  "generate_story": "Сгенерировать рассказ",
  "play_audio": "Воспроизвести аудио",
  "menu_profile": "Профиль",
  "menu_language": "Язык интерфейса",
  "menu_geolocation": "Доступ к геопозиции",
  "style_standard": "Стандартный",
  "style_historian": "Историк",
  "style_kids": "Для детей",
  "style_legend": "Легенда",
  "style_romantic": "Романтик"
}
Получить список доступных языков
text
GET /localization/languages
Ответ:

json
{
  "languages": [
    ["ru", "Русский"],
    ["en", "English"],
    ["zh", "中文"],
    ["es", "Español"],
    ["de", "Deutsch"],
    ["fr", "Français"]
  ]
}
Коды ошибок
Код	Описание
200	Успех
400	Неверный запрос
403	Доступ запрещён
404	Не найдено
500	Внутренняя ошибка сервера
Формат ошибки:

json
{
  "error": "Описание ошибки"
}
Данные для тестирования
Места: place_001 — place_020 (20 достопримечательностей Санкт-Петербурга)

Пользователи (после регистрации):

test@mail.ru / 123456 — Александр