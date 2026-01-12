# T-Bank Bot Configuration Repository

Этот репозиторий содержит централизованные конфигурации для всех микросервисов проекта T-Bank Bot.

## Структура файлов

- `application.yml` - общие настройки для всех микросервисов
- `eureka-server.yml` - конфигурация Eureka Server (порт 8761)
- `api-gateway.yml` - конфигурация API Gateway (порт 8080)
- `user-service.yml` - конфигурация User Service (динамический порт)
- `tinkoff-integration-service.yml` - конфигурация Tinkoff Integration Service (динамический порт)
- `telegram-bot-service.yml` - конфигурация Telegram Bot Service (динамический порт)

## Динамическое присваивание портов

Для микросервисов используется `server.port: 0`, что позволяет Spring Boot автоматически выбирать свободный порт.
Это необходимо для балансировки нагрузки и запуска нескольких экземпляров одного сервиса.

## Переменные окружения

Конфигурации используют переменные окружения для секретных данных.

### Настройка через .env файл (рекомендуется)

В корне проекта создан файл `.env` для хранения всех секретов:

```env
# Telegram Bot Configuration
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
TELEGRAM_BOT_USERNAME=your_bot_username_here

# Tinkoff Invest API Configuration
TINKOFF_API_TOKEN=your_tinkoff_api_token_here

# Database Configuration
DB_USER=postgres
DB_PASSWORD=postgres
```

**Преимущества:**
- ✅ Все секреты в одном месте
- ✅ Автоматическая загрузка при запуске сервисов
- ✅ Файл `.env` добавлен в `.gitignore` (не попадет в Git)
- ✅ Есть шаблон `.env.example` для других разработчиков

### Альтернативный способ (системные переменные)

Если не используешь `.env` файл, установи переменные окружения:

**Windows CMD:**
```cmd
set TELEGRAM_BOT_TOKEN=your_token
set TELEGRAM_BOT_USERNAME=your_username
set TINKOFF_API_TOKEN=your_token
```

**Windows PowerShell:**
```powershell
$env:TELEGRAM_BOT_TOKEN="your_token"
$env:TELEGRAM_BOT_USERNAME="your_username"
$env:TINKOFF_API_TOKEN="your_token"
```

## Обновление конфигурации

После изменения конфигурации в этом репозитории:
1. Закоммитить и запушить изменения
2. Вызвать endpoint `/actuator/refresh` на нужном сервисе для применения изменений без перезапуска

```bash
curl -X POST http://localhost:{service_port}/actuator/refresh
```

## Получение токенов

### Telegram Bot Token
1. Найди [@BotFather](https://t.me/BotFather) в Telegram
2. Отправь `/newbot`
3. Следуй инструкциям
4. Скопируй полученный токен

### Tinkoff API Token
1. Открой приложение Tinkoff Investments
2. Настройки → Токен для API
3. Создай новый токен
4. Скопируй его (показывается только один раз!)

## Безопасность

⚠️ **ВАЖНО:**
- Никогда не коммить `.env` файл в Git
- Не публиковать токены в интернете
- Регулярно обновлять токены
- Использовать разные токены для dev/prod окружений
