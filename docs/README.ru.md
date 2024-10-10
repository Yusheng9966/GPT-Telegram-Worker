# 🤖💬 Telegram GPT Worker - многофункциональный ИИ-ассистент

[English](../README.md) | [简体中文](./README.zh-cn.md) | [繁體中文](./README.zh-hant.md) | [日本語](./README.ja.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Русский](./README.ru.md) | [Deutsch](./README.de.md)

## 📖 О проекте

Добро пожаловать в Telegram GPT Worker! 👋 Это высокоэффективный бот для Telegram, разработанный на TypeScript. Он поддерживает множество языков и моделей ИИ, развернут на Cloudflare Workers, обеспечивая пользователям быстрый и масштабируемый сервис.

## 🌟 Основные функции

1. 🧠 **Поддержка нескольких моделей**: Интеграция с OpenAI, Google Gemini, Anthropic Claude, Groq и Azure OpenAI.
2. 💬 **Умные диалоги**: Способность запоминать контекст для естественного общения.
3. 🎨 **Генерация изображений**: Создание изображений по текстовому описанию с использованием DALL·E и Cloudflare Flux.
4. 🌍 **Многоязычная поддержка**: Встроенная функция i18n с поддержкой 8 языков.
5. 🔒 **Управление доступом**: Контроль доступа через систему белых списков.
6. ☁️ **Высокопроизводительное развертывание**: Использование возможностей граничных вычислений Cloudflare Workers.
7. 🗄️ **Эффективное управление данными**: Использование Redis для кэширования и управления данными.
8. 🔧 **Оптимизация промптов для Flux**: Опциональная функция для улучшения промптов через внешний API.

## 📋 Системные требования

Перед началом убедитесь, что у вас есть:

- Аккаунт [Cloudflare](https://dash.cloudflare.com/)
- Аккаунт Telegram и токен бота
- База данных [Upstash](https://upstash.com/) Redis (с включенной функцией [Eviction](https://upstash.com/docs/redis/features/eviction))
- API-ключ хотя бы для одного ИИ-сервиса

## 🚀 Быстрый старт

1. Клонируйте репозиторий
2. Настройте необходимые переменные окружения
3. Разверните на Cloudflare Workers
4. Настройте вебхук Telegram

Подробные инструкции по развертыванию смотрите ниже.

## 📝 Доступные команды

- `/start` - Запуск бота
- `/language` - Смена языка
- `/switchmodel` - Переключение модели ИИ
- `/new` - Начать новый диалог
- `/history` - Получить резюме истории диалога
- `/help` - Получить справку
- `/img` - Сгенерировать изображение (DALL-E)
- `/flux` - Сгенерировать изображение (Cloudflare Flux)

## 📁 Структура проекта

```plaintext
/GPT-Telegram-Worker
│
├── /src
│   ├── /api
│   │   ├── azure.ts               # Взаимодействие с Azure API
│   │   ├── claude.ts              # Взаимодействие с Claude API
│   │   ├── flux-cf.ts             # Интерфейс генерации изображений Cloudflare AI
│   │   ├── gemini.ts              # Взаимодействие с Google Gemini API
│   │   ├── groq.ts                # Взаимодействие с Groq API
│   │   ├── image_generation.ts    # Интерфейс генерации изображений DALL·E
│   │   ├── model_api_interface.ts # Стандартный интерфейс API моделей
│   │   ├── openai_api.ts          # Взаимодействие с OpenAI API
│   │   └── telegram.ts            # Логика Telegram бота
│   ├── /config                    # Конфигурационные файлы
│   │   └── commands.ts            # Конфигурация команд бота
│   ├── /utils
│   │   ├── helpers.ts             # Вспомогательные функции
│   │   ├── i18n.ts                # Многоязычная поддержка
│   │   └── redis.ts               # Операции с Redis
│   ├── index.ts                   # Точка входа
│   └── env.ts                     # Конфигурация переменных окружения
├── /types                         # Файлы определения типов
│   └── telegram.d.ts              # Определения типов Telegram API
├── wrangler.toml                  # Конфигурация Cloudflare Worker
├── tsconfig.json                  # Конфигурация TypeScript
├── package.json                   # Зависимости проекта
└── README.md                      # Описание проекта
```

## 🚀 Подробное руководство

### Развертывание на Cloudflare Workers

#### Использование Wrangler CLI

1. Установите Wrangler CLI:

   ```bash
   npm install -g @cloudflare/wrangler
   ```

2. Войдите в аккаунт Cloudflare:

   ```bash
   wrangler login
   ```

3. Создайте новый проект Workers:

   ```bash
   wrangler init telegram-bot
   ```

4. Скопируйте файл `dist/index.js` в проект.

5. Отредактируйте файл `wrangler.toml`, настройте проект:

   ```toml
   name = "telegram-bot"
   type = "javascript"
   account_id = "ваш_id_аккаунта"
   workers_dev = true
   ```

6. Разверните на Cloudflare Workers:

   ```bash
   wrangler publish
   ```

#### Использование панели управления Cloudflare

1. Войдите в [панель управления Cloudflare](https://dash.cloudflare.com/).
2. Выберите "Workers & Pages".
3. Нажмите "Create application" и выберите "Create Worker".
4. Назовите Worker и нажмите "Deploy".
5. Скопируйте и вставьте содержимое `dist/index.js` в редактор, сохраните файл.
6. Добавьте необходимые переменные окружения в "Settings".

### Настройка вебхука Telegram

Используйте Telegram Bot API для настройки вебхука, пример URL:

```bash
https://api.telegram.org/bot<ВАШ_ТОКЕН_БОТА>/setWebhook?url=https://ваш-воркер.ваш-поддомен.workers.dev
```

### Локальная разработка

1. Клонируйте проект:

   ```bash
   git clone https://github.com/ваше-имя-пользователя/telegram-bot.git
   ```

2. Установите зависимости:

   ```bash
   npm install
   ```

3. Настройте переменные окружения.

4. Скомпилируйте TypeScript:

   ```bash
   npm run build
   ```

5. Запустите бота:

   ```bash
   npm start
   ```

## 🔧 Переменные окружения

| Название переменной | Описание | Значение по умолчанию | Пример |
|---------------------|----------|------------------------|--------|
| OPENAI_API_KEY | Ключ API OpenAI | - | sk-abcdefghijklmnopqrstuvwxyz123456 |
| OPENAI_BASE_URL | Базовый URL API OpenAI | https://api.openai.com/v1 | https://ваша-конечная-точка.com/v1 |
| OPENAI_MODELS | Список доступных моделей OpenAI | - | gpt-3.5-turbo,gpt-4 |
| TELEGRAM_BOT_TOKEN | Токен бота Telegram | - | 123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11 |
| WHITELISTED_USERS | Список ID пользователей, которым разрешено использовать бота | - | 12345678,87654321 |
| SYSTEM_INIT_MESSAGE | Начальное системное сообщение | You are a helpful assistant. | Вы - полезный ассистент. |
| SYSTEM_INIT_MESSAGE_ROLE | Роль начального системного сообщения | system | system |
| DEFAULT_MODEL | Модель ИИ по умолчанию | - | gpt-3.5-turbo |
| UPSTASH_REDIS_REST_URL | URL REST Upstash Redis | - | https://ваш-redis-url.upstash.io |
| UPSTASH_REDIS_REST_TOKEN | Токен REST Upstash Redis | - | ваш-токен-redis |
| DALL_E_MODEL | Версия модели DALL-E | dall-e-3 | dall-e-3 |
| CLOUDFLARE_API_TOKEN | Токен API Cloudflare | - | ваш-токен-api-cloudflare |
| CLOUDFLARE_ACCOUNT_ID | ID аккаунта Cloudflare | - | ваш-id-аккаунта-cloudflare |
| FLUX_STEPS | Количество шагов генерации Flux | 4 | 4-8, максимум 8 шагов |
| PROMPT_OPTIMIZATION | Включить оптимизацию промптов | false | true |
| EXTERNAL_API_BASE | Базовый URL внешнего API | - | https://внешний-api.com |
| EXTERNAL_MODEL | Название внешней модели | - | название-внешней-модели |
| EXTERNAL_API_KEY | Ключ внешнего API | - | ключ-внешнего-api |
| GOOGLE_MODEL_KEY | Ключ API модели Google AI | - | ваш-ключ-google-api |
| GOOGLE_MODEL_BASEURL | Базовый URL API модели Google AI | https://generativelanguage.googleapis.com/v1beta | https://ваша-конечная-точка-google.com |
| GOOGLE_MODELS | Список доступных моделей Google AI | - | gemini-pro,gemini-pro-vision |
| GROQ_API_KEY | Ключ API Groq | - | ваш-ключ-api-groq |
| ANTHROPIC_API_KEY | Ключ API Anthropic | - | ваш-ключ-api-anthropic |
| ANTHROPIC_BASE_URL | Базовый URL API Anthropic | https://api.anthropic.com | https://ваша-конечная-точка-anthropic.com |

Примечание: Некоторые переменные требуют ручной настройки и не имеют значений по умолчанию.

## ⚠️ Важные замечания

1. 🚦 **Разумно используйте квоты API**: Особенно для сервисов генерации изображений, следите за ограничениями использования.
2. 🔐 **Защищайте конфиденциальную информацию**: Надежно храните переменные окружения и ключи API.
3. 🧠 **Изучите особенности моделей**: Выбирайте модель ИИ, наиболее подходящую для вашего сценария.
4. 🔄 **Поддерживайте актуальность**: Регулярно обновляйте код и функции для оптимальной производительности.
5. 🛡️ **Безопасность прежде всего**: Регулярно обновляйте ключи API, следуйте принципу наименьших привилегий.
6. 🎨 **Оптимизация промптов Flux**: При включении PROMPT_OPTIMIZATION убедитесь, что правильно настроены EXTERNAL_API_BASE, EXTERNAL_MODEL и EXTERNAL_API_KEY.

## 🚀 Оптимизация промптов Flux

Когда переменная окружения PROMPT_OPTIMIZATION установлена в true, функция генерации изображений Flux использует внешний API для оптимизации промптов. Эта функция работает следующим образом:

1. Пользователь предоставляет исходный промпт.
2. Система использует внешний API, настроенный с помощью EXTERNAL_API_BASE, EXTERNAL_MODEL и EXTERNAL_API_KEY, для оптимизации промпта.
3. Оптимизированный промпт используется моделью Flux для генерации изображения.

Эта функция помогает создавать более точные изображения, соответствующие особенностям модели Flux. Для использования этой функции убедитесь, что все соответствующие переменные окружения настроены правильно.

## 🔧 Устранение неполадок

- Бот не отвечает? Проверьте настройки вебхука и конфигурацию переменных окружения.
- Столкнулись с ограничениями API? Проверьте использование вашей квоты API.

## 📄 Лицензия

Этот проект распространяется под [лицензией MIT](LICENSE).

Copyright (c) 2024 [snakeying]
