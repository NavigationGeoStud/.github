# GeoStud API

[![.NET](https://img.shields.io/badge/.NET-9.0-blue.svg)](https://dotnet.microsoft.com/download)
[![Entity Framework](https://img.shields.io/badge/Entity%20Framework-9.0-green.svg)](https://docs.microsoft.com/en-us/ef/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

**GeoStud API** — это современное решение, анализа и обработки данных студентов с поддержкой продвинутой аналитики.

## 🎯 Основные возможности

### 📊 Сбор и анализ данных студентов
- **Опросы студентов**: Сбор демографических данных, интересов и предпочтений
- **Аналитика в реальном времени**: Комплексная аналитика по демографии, интересам и поведению
- **Кэширование данных**: Оптимизированное хранение аналитических данных

### 🔐 Безопасность и аутентификация
- **JWT аутентификация**: Безопасная аутентификация с использованием JWT токенов
- **Авторизация на уровне API**: Защищенные эндпоинты с контролем доступа

### 🏗️ Архитектура и производительность
- **Версионирование API**: Поддержка версионирования API (v1)
- **Гибкая конфигурация БД**: Поддержка SQLite (разработка) и SQL Server (production)
- **Middleware для логирования**: Детальное логирование HTTP запросов
- **CORS настройки**: Настроенные политики CORS для разных сред

## 🚀 Быстрый старт

### Предварительные требования
- [.NET 9.0 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)
- SQL Server LocalDB (для production режима)
- Git

### Установка и запуск

1. **Клонирование репозитория**
```bash
git clone https://github.com/your-org/GeoStudApi.git
cd GeoStudApi
```

2. **Восстановление зависимостей**
```bash
dotnet restore
```

3. **Запуск в режиме разработки (SQLite)**
```bash
dotnet run --project GeoStud.Api.csproj -- --sqlite
```

4. **Запуск в production режиме (SQL Server)**
```bash
dotnet run --project GeoStud.Api.csproj --environment Production
```

### Доступ к API
- **Swagger UI**: `https://localhost:5001`
- **API Base URL**: `https://localhost:5001/api/v1/`
- **Health Check**: `https://localhost:5001/api/v1/health`

## 📋 Основные API Endpoints

### 🔐 Аутентификация
| Метод | Endpoint | Описание |
|-------|----------|----------|
| `POST` | `/api/v1/auth/service-login` | Аутентификация сервиса |

### 📝 Опросы студентов
| Метод | Endpoint | Описание |
|-------|----------|----------|
| `POST` | `/api/v1/survey/submit` | Отправить данные опроса |
| `GET` | `/api/v1/survey/{id}` | Получить данные опроса |
| `GET` | `/api/v1/survey` | Получить все опросы |

### 📊 Аналитика
| Метод | Endpoint | Описание |
|-------|----------|----------|
| `GET` | `/api/v1/analytics/comprehensive` | Полная аналитика |
| `GET` | `/api/v1/analytics/demographics` | Демографическая аналитика |
| `GET` | `/api/v1/analytics/interests` | Аналитика интересов |
| `GET` | `/api/v1/analytics/behavior` | Аналитика поведения |
| `GET` | `/api/v1/analytics/cached/{category}` | Кэшированные данные |

### ❤️ Мониторинг
| Метод | Endpoint | Описание |
|-------|----------|----------|
| `GET` | `/api/v1/health` | Проверка состояния API |

## 🔧 Конфигурация

### Переменные окружения
```bash
# Режим работы с базой данных
FORCE_SQLITE=true                    # Принудительное использование SQLite
ASPNETCORE_ENVIRONMENT=Development   # Среда выполнения

# JWT настройки
JWT_SECRET=YourSuperSecretKeyThatIsAtLeast32CharactersLong!
JWT_ISSUER=GeoStud.Api
JWT_AUDIENCE=GeoStud.Clients
```

### Сервисные клиенты
Система поддерживает предустановленных сервисных клиентов:

| Client ID | Secret | Описание |
|-----------|--------|----------|
| `mobile-app` | `MobileAppSecret123!` | Мобильное приложение |
| `web-app` | `WebAppSecret123!` | Веб-приложение |
| `analytics-service` | `AnalyticsServiceSecret123!` | Сервис аналитики |

## 💡 Примеры использования

### 1. Аутентификация сервиса
```bash
curl -X POST https://localhost:5001/api/v1/auth/service-login \
  -H "Content-Type: application/json" \
  -d '{
    "clientId": "mobile-app",
    "clientSecret": "MobileAppSecret123!"
  }'
```

### 2. Отправка опроса студента
```bash
curl -X POST https://localhost:5001/api/v1/survey/submit \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{
    "ageRange": "17-22",
    "isStudent": true,
    "gender": "Male",
    "isLocal": true,
    "interests": ["Учеба", "Спорт", "Музыка"],
    "budget": "500",
    "activityTime": "Evening",
    "socialPreference": "Group"
  }'
```

### 3. Получение аналитики
```bash
curl -X GET https://localhost:5001/api/v1/analytics/comprehensive \
  -H "Authorization: Bearer YOUR_TOKEN"
```

