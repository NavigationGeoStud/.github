# GeoStud API

**GeoStud API** — это REST API для сбора и анализа данных студентов с поддержкой продвинутой аналитики.

## 📋 API Спецификация

### Базовый URL
```
/api/v1/
```

### Аутентификация
API использует JWT токены для аутентификации. Все защищенные endpoints требуют заголовок:
```
Authorization: Bearer YOUR_JWT_TOKEN
```

## 🔐 Аутентификация

### POST /auth/service-login
Аутентификация сервисных клиентов для получения JWT токена.

**Запрос:**
```json
{
  "clientId": "string",
  "clientSecret": "string"
}
```

**Ответ:**
```json
{
  "token": "string",
  "expiresIn": 3600,
  "tokenType": "Bearer"
}
```

**Коды ответа:**
- `200` - Успешная аутентификация
- `401` - Неверные учетные данные
- `400` - Некорректный запрос

## 📝 Опросы студентов

### POST /survey/submit
Отправка данных опроса студента.

**Запрос:**
```json
{
  "ageRange": "string",           // "17-22", "23-30", "31-40", "40+"
  "isStudent": boolean,           // true/false
  "gender": "string",             // "Male", "Female", "Other"
  "isLocal": boolean,             // true/false
  "interests": ["string"],        // Массив интересов
  "budget": "string",             // "0-500", "500-1000", "1000-2000", "2000+"
  "activityTime": "string",       // "Morning", "Afternoon", "Evening", "Night"
  "socialPreference": "string"    // "Solo", "Group", "Mixed"
}
```

**Ответ:**
```json
{
  "id": "string",
  "submittedAt": "2024-01-01T00:00:00Z",
  "status": "success"
}
```

### GET /survey/{id}
Получение данных конкретного опроса.

**Ответ:**
```json
{
  "id": "string",
  "ageRange": "string",
  "isStudent": boolean,
  "gender": "string",
  "isLocal": boolean,
  "interests": ["string"],
  "budget": "string",
  "activityTime": "string",
  "socialPreference": "string",
  "submittedAt": "2024-01-01T00:00:00Z"
}
```

### GET /survey
Получение списка всех опросов с пагинацией.

**Параметры запроса:**
- `page` (int, optional) - Номер страницы (по умолчанию 1)
- `pageSize` (int, optional) - Размер страницы (по умолчанию 10)

**Ответ:**
```json
{
  "surveys": [
    {
      "id": "string",
      "ageRange": "string",
      "isStudent": boolean,
      "gender": "string",
      "submittedAt": "2024-01-01T00:00:00Z"
    }
  ],
  "totalCount": 100,
  "page": 1,
  "pageSize": 10,
  "totalPages": 10
}
```

## 📊 Аналитика

### GET /analytics/comprehensive
Получение комплексной аналитики по всем данным. Включает демографические данные, интересы и поведенческие паттерны.

**Описание категорий:**
- **Demographics** - Возрастное и гендерное распределение, соотношение студентов
- **Interests** - Топ интересов с количественными показателями
- **Behavior** - Паттерны поведения по бюджету, времени активности и социальным предпочтениям

**Ответ:**
```json
{
  "demographics": {
    "ageDistribution": {
      "17-22": 45,
      "23-30": 35,
      "31-40": 15,
      "40+": 5
    },
    "genderDistribution": {
      "Male": 60,
      "Female": 35,
      "Other": 5
    },
    "studentRatio": 0.75
  },
  "interests": {
    "topInterests": [
      {"name": "Учеба", "count": 80},
      {"name": "Спорт", "count": 65},
      {"name": "Музыка", "count": 50}
    ]
  },
  "behavior": {
    "budgetDistribution": {
      "0-500": 30,
      "500-1000": 40,
      "1000-2000": 25,
      "2000+": 5
    },
    "activityTimeDistribution": {
      "Morning": 20,
      "Afternoon": 35,
      "Evening": 40,
      "Night": 5
    }
  },
  "generatedAt": "2024-01-01T00:00:00Z"
}
```

### GET /analytics/demographics
Получение детальной демографической аналитики.

**Категории данных:**
- **Возрастное распределение** - Процентное соотношение по возрастным группам
- **Гендерное распределение** - Распределение по полу
- **Статус студента** - Соотношение студентов и не-студентов
- **Локальность** - Соотношение местных и приезжих

**Ответ:**
```json
{
  "ageDistribution": {
    "17-22": 45,
    "23-30": 35,
    "31-40": 15,
    "40+": 5
  },
  "genderDistribution": {
    "Male": 60,
    "Female": 35,
    "Other": 5
  },
  "studentRatio": 0.75,
  "localRatio": 0.80,
  "totalResponses": 200
}
```

### GET /analytics/interests
Получение аналитики по интересам и предпочтениям.

**Категории интересов:**
- **Образование** - Учеба, наука, исследования
- **Спорт и здоровье** - Фитнес, командные виды спорта, активный отдых
- **Творчество** - Музыка, искусство, дизайн
- **Технологии** - IT, программирование, гаджеты
- **Социальная активность** - Волонтерство, общественная деятельность

**Ответ:**
```json
{
  "topInterests": [
    {"name": "Учеба", "count": 80, "percentage": 40.0},
    {"name": "Спорт", "count": 65, "percentage": 32.5},
    {"name": "Музыка", "count": 50, "percentage": 25.0},
    {"name": "IT", "count": 45, "percentage": 22.5},
    {"name": "Искусство", "count": 30, "percentage": 15.0}
  ],
  "totalResponses": 200,
  "uniqueInterests": 25
}
```

### GET /analytics/behavior
Получение аналитики поведенческих паттернов.

**Категории поведения:**
- **Финансовое поведение** - Распределение по бюджетным категориям
- **Временные предпочтения** - Активность в разное время суток
- **Социальные предпочтения** - Индивидуальная или групповая активность
- **Локальные паттерны** - Поведение местных vs приезжих

**Ответ:**
```json
{
  "budgetDistribution": {
    "0-500": 30,
    "500-1000": 40,
    "1000-2000": 25,
    "2000+": 5
  },
  "activityTimeDistribution": {
    "Morning": 20,
    "Afternoon": 35,
    "Evening": 40,
    "Night": 5
  },
  "socialPreferenceDistribution": {
    "Solo": 25,
    "Group": 50,
    "Mixed": 25
  },
  "localVsNonLocal": {
    "local": {
      "avgBudget": "750",
      "preferredTime": "Evening",
      "socialPreference": "Group"
    },
    "nonLocal": {
      "avgBudget": "1200",
      "preferredTime": "Afternoon",
      "socialPreference": "Mixed"
    }
  }
}
```

### GET /analytics/cached/{category}
Получение кэшированных аналитических данных для оптимизации производительности.

**Доступные категории:**
- `demographics` - Демографические данные
- `interests` - Данные по интересам
- `behavior` - Поведенческие данные
- `comprehensive` - Комплексная аналитика

**Параметры:**
- `category` (string, required) - Категория кэшированных данных

**Ответ:** Зависит от категории (см. соответствующие endpoints выше)

## ❤️ Мониторинг

### GET /health
Проверка состояния API.

**Ответ:**
```json
{
  "status": "healthy",
  "timestamp": "2024-01-01T00:00:00Z",
  "version": "1.0.0",
  "database": "connected"
}
```

## 📊 Сущности и структуры данных

### Survey (Опрос студента)
Основная сущность для хранения данных опроса студентов.

**Поля:**
- `id` (string) - Уникальный UUID идентификатор
  - Пример: `"550e8400-e29b-41d4-a716-446655440000"`
- `ageRange` (string) - Возрастная группа
  - Возможные значения: `"17-22"`, `"23-30"`, `"31-40"`, `"40+"`
- `isStudent` (boolean) - Статус студента
  - `true` - Студент, `false` - Не студент
- `gender` (string) - Половая принадлежность
  - Возможные значения: `"Male"`, `"Female"`, `"Other"`
- `isLocal` (boolean) - Локальность проживания
  - `true` - Местный житель, `false` - Приезжий
- `interests` (string[]) - Массив интересов и увлечений
  - Примеры: `["Учеба", "Спорт", "Музыка", "IT", "Искусство"]`
  - Максимум 10 элементов
- `budget` (string) - Бюджетная категория
  - Возможные значения: `"0-500"`, `"500-1000"`, `"1000-2000"`, `"2000+"`
- `activityTime` (string) - Предпочтительное время активности
  - Возможные значения: `"Morning"`, `"Afternoon"`, `"Evening"`, `"Night"`
- `socialPreference` (string) - Социальные предпочтения
  - Возможные значения: `"Solo"`, `"Group"`, `"Mixed"`
- `submittedAt` (datetime) - Время отправки опроса (ISO 8601)
  - Пример: `"2024-01-15T14:30:00Z"`

**Пример полной записи:**
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "ageRange": "17-22",
  "isStudent": true,
  "gender": "Male",
  "isLocal": true,
  "interests": ["Учеба", "Спорт", "IT"],
  "budget": "500-1000",
  "activityTime": "Evening",
  "socialPreference": "Group",
  "submittedAt": "2024-01-15T14:30:00Z"
}
```

### AnalyticsData (Аналитические данные)
Структура для хранения кэшированных аналитических данных.

**Поля:**
- `category` (string) - Категория аналитических данных
  - Возможные значения: `"demographics"`, `"interests"`, `"behavior"`, `"comprehensive"`
- `data` (object) - JSON объект с аналитическими данными
  - Структура зависит от категории
- `generatedAt` (datetime) - Время генерации данных (ISO 8601)
  - Пример: `"2024-01-15T15:00:00Z"`
- `expiresAt` (datetime) - Время истечения кэша (ISO 8601)
  - Пример: `"2024-01-15T18:00:00Z"`
- `version` (string) - Версия структуры данных
  - Пример: `"1.0"`

**Пример записи:**
```json
{
  "category": "demographics",
  "data": {
    "ageDistribution": {"17-22": 45, "23-30": 35},
    "genderDistribution": {"Male": 60, "Female": 35}
  },
  "generatedAt": "2024-01-15T15:00:00Z",
  "expiresAt": "2024-01-15T18:00:00Z",
  "version": "1.0"
}
```

### ServiceClient (Сервисный клиент)
Конфигурация для аутентификации сервисов.

**Поля:**
- `clientId` (string) - Уникальный идентификатор клиента
  - Примеры: `"mobile-app"`, `"web-app"`, `"analytics-service"`
- `clientSecret` (string) - Секретный ключ для аутентификации
  - Длина: минимум 16 символов
  - Пример: `"MobileAppSecret123!"`
- `name` (string) - Человекочитаемое название сервиса
  - Примеры: `"Мобильное приложение"`, `"Веб-приложение"`
- `isActive` (boolean) - Статус активности клиента
  - `true` - Активен, `false` - Заблокирован
- `createdAt` (datetime) - Время создания клиента
  - Пример: `"2024-01-01T00:00:00Z"`
- `lastUsed` (datetime) - Время последнего использования
  - Пример: `"2024-01-15T14:30:00Z"`

**Пример записи:**
```json
{
  "clientId": "mobile-app",
  "clientSecret": "MobileAppSecret123!",
  "name": "Мобильное приложение",
  "isActive": true,
  "createdAt": "2024-01-01T00:00:00Z",
  "lastUsed": "2024-01-15T14:30:00Z"
}
```

### ErrorResponse (Структура ошибок)
Стандартная структура для возврата ошибок API.

**Поля:**
- `error` (object) - Объект с информацией об ошибке
  - `code` (string) - Код ошибки
  - `message` (string) - Описание ошибки
  - `details` (object, optional) - Дополнительные детали
- `timestamp` (datetime) - Время возникновения ошибки
- `path` (string) - Путь к endpoint, где произошла ошибка

**Примеры ошибок:**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Некорректное значение поля ageRange",
    "details": {
      "field": "ageRange",
      "provided": "15-20",
      "allowed": ["17-22", "23-30", "31-40", "40+"]
    }
  },
  "timestamp": "2024-01-15T14:30:00Z",
  "path": "/api/v1/survey/submit"
}
```

## 🔧 Коды ошибок

### HTTP статус коды
- `200` - Успешный запрос
- `201` - Ресурс создан
- `400` - Некорректный запрос
- `401` - Не авторизован
- `403` - Доступ запрещен
- `404` - Ресурс не найден
- `500` - Внутренняя ошибка сервера

### Формат ошибок
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Описание ошибки",
    "details": {
      "field": "ageRange",
      "reason": "Недопустимое значение"
    }
  }
}
```

## 🔐 Сервисные клиенты

Предустановленные клиенты для аутентификации:

| Client ID | Secret | Описание |
|-----------|--------|----------|
| `mobile-app` | `MobileAppSecret123!` | Мобильное приложение |
| `web-app` | `WebAppSecret123!` | Веб-приложение |
| `analytics-service` | `AnalyticsServiceSecret123!` | Сервис аналитики |

## 📝 Примеры запросов

### Аутентификация
```bash
curl -X POST /api/v1/auth/service-login \
  -H "Content-Type: application/json" \
  -d '{
    "clientId": "mobile-app",
    "clientSecret": "MobileAppSecret123!"
  }'
```

### Отправка опроса
```bash
curl -X POST /api/v1/survey/submit \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "ageRange": "17-22",
    "isStudent": true,
    "gender": "Male",
    "isLocal": true,
    "interests": ["Учеба", "Спорт", "Музыка"],
    "budget": "500-1000",
    "activityTime": "Evening",
    "socialPreference": "Group"
  }'
```

### Получение аналитики
```bash
curl -X GET /api/v1/analytics/comprehensive \
  -H "Authorization: Bearer YOUR_TOKEN"
```

