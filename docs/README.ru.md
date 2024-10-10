# 🤖💬 Telegram GPT Worker - многофункциональный ИИ-ассистент

[English](../README.md) | [简体中文](./README.zh-cn.md) | [繁體中文](./README.zh-hant.md) | [日本語](./README.ja.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Русский](./README.ru.md) | [Deutsch](./README.de.md)

## 📖 О проекте

Добро пожаловать в Telegram GPT Worker! 👋 Это эффективный бот для Telegram, разработанный на TypeScript. Он поддерживает множество языков и ИИ-моделей, развернут на Cloudflare Workers, обеспечивая пользователям быстрый и масштабируемый сервис.

## 🌟 Ключевые функции

1. 🧠 **Поддержка нескольких моделей**: Интеграция с OpenAI, Google Gemini, Anthropic Claude, Groq и Azure OpenAI.
2. 💬 **Умные диалоги**: Контекстная память для обеспечения плавности разговора.
3. 🎨 **Генерация изображений**: Создание изображений по текстовому описанию с использованием DALL·E и Cloudflare Flux.
4. 🖼️ **Анализ изображений**: Загрузка и интеллектуальный анализ изображений с использованием OpenAI или Google Gemini.
5. 🌍 **Многоязычность**: Встроенная поддержка i18n для 8 языков.
6. 🔒 **Управление доступом**: Контроль доступа через белый список для повышения безопасности.
7. ☁️ **Высокопроизводительное развертывание**: Использование edge computing Cloudflare Workers для быстрого отклика.
8. 🗄️ **Эффективное управление данными**: Использование Redis для кэширования и управления данными.
9. 🔧 **Оптимизация промптов Flux**: Опциональная функция для улучшения промптов Flux через внешнее API.

## 📋 Системные требования

Перед началом работы убедитесь, что у вас есть:

- Аккаунт [Cloudflare](https://dash.cloudflare.com/)
- Аккаунт Telegram и токен бота
- База данных [Upstash](https://upstash.com/) Redis (с включенной функцией [Eviction](https://upstash.com/docs/redis/features/eviction))
- API-ключ хотя бы для одного ИИ-сервиса

## 🚀 Быстрый старт

1. Клонируйте репозиторий проекта
2. Настройте необходимые переменные окружения
3. Разверните на Cloudflare Workers
4. Настройте вебхук Telegram

Подробные инструкции по развертыванию смотрите ниже.

## 📝 Доступные команды

- `/start` - Запуск бота
- `/language` - Смена языка
- `/switchmodel` - Переключение ИИ-модели
- `/new` - Начало нового диалога
- `/history` - Получение резюме истории диалога
- `/help` - Получение справки
- `/img` - Генерация изображения (DALL-E)
- `/flux` - Генерация изображения (Cloudflare Flux)

## 📁 Структура проекта

```
/GPT-Telegram-Worker
│
├── /src
│   ├── /api
│   │   ├── azure.ts               # Взаимодействие с Azure API
│   │   ├── claude.ts              # Взаимодействие с Claude API
│   │   ├── flux-cf.ts             # Интерфейс рисования Cloudflare AI
│   │   ├── gemini.ts              # Взаимодействие с Google Gemini API
│   │   ├── groq.ts                # Взаимодействие с Groq API
│   │   ├── image_generation.ts    # Интерфейс рисования DALL·E
│   │   ├── model_api_interface.ts # Общий интерфейс для стандартной структуры API моделей
│   │   ├── openai_api.ts          # Взаимодействие с OpenAI API
│   │   └── telegram.ts            # Логика Telegram бота
│   ├── /config                    # Файлы конфигурации
│   │   └── commands.ts            # Команды Telegram бота
│   ├── /utils
│   │   └── helpers.ts             # Вспомогательные функции и инструменты
│   │   └── i18n.ts                # Функции для многоязычности
│   │   └── redis.ts               # Функции Upstash Redis
│   │   └── image_analyze.ts       # Функции загрузки изображений
│   ├── index.ts                   # Входной файл, обработка запросов и ответов
│   └── env.ts                     # Настройка переменных окружения
├── /types                         # Файлы определения типов
│   └── telegram.d.ts              # Определения типов для Telegram API
├── wrangler.toml                  # Файл конфигурации Cloudflare Worker
├── tsconfig.json                  # Файл конфигурации TypeScript
├── package.json                   # Файл зависимостей проекта
└── README.md                      # Документация проекта
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

5. Отредактируйте файл `wrangler.toml`, настроив проект:

   ```toml
   name = "telegram-bot"
   type = "javascript"
   account_id = "ваш_account_id"
   workers_dev = true
   ```

6. Разверните на Cloudflare Workers:

   ```bash
   wrangler publish
   ```

#### Использование Cloudflare Dashboard

1. Войдите в [Cloudflare Dashboard](https://dash.cloudflare.com/).
2. Выберите "Workers & Pages".
3. Нажмите "Create application" и выберите "Create Worker".
4. Назовите Worker и нажмите "Deploy".
5. Скопируйте содержимое `dist/index.js` в редактор и сохраните файл.
6. Добавьте необходимые переменные окружения в разделе "Settings".

### Настройка вебхука Telegram

Используйте API Telegram Bot для настройки вебхука, пример URL:

```bash
https://api.telegram.org/bot<ВАШ_ТОКЕН_БОТА>/setWebhook?url=https://your-worker.your-subdomain.workers.dev
```

### Локальная разработка

1. Клонируйте проект:

   ```bash
   git clone https://github.com/ваш-пользователь/telegram-bot.git
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

| Имя переменной | Описание | Значение по умолчанию | Пример |
|----------------|----------|------------------------|--------|
| OPENAI_API_KEY | Ключ API OpenAI | - | sk-abcdefghijklmnopqrstuvwxyz123456 |
| OPENAI_BASE_URL | Базовый URL API OpenAI | https://api.openai.com/v1 | https://ваш-пользовательский-эндпоинт.com/v1 |
| OPENAI_MODELS | Список доступных моделей OpenAI | - | gpt-3.5-turbo,gpt-4 |
| TELEGRAM_BOT_TOKEN | Токен бота Telegram | - | 123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11 |
| WHITELISTED_USERS | Список ID пользователей, которым разрешено использовать бота | - | 12345678,87654321 |
| SYSTEM_INIT_MESSAGE | Начальное системное сообщение | You are a helpful assistant. | Вы — полезный помощник. |
| SYSTEM_INIT_MESSAGE_ROLE | Роль начального системного сообщения | system | system |
| DEFAULT_MODEL | ИИ-модель по умолчанию | - | gpt-3.5-turbo |
| UPSTASH_REDIS_REST_URL | URL Upstash Redis REST | - | https://ваш-redis-url.upstash.io |
| UPSTASH_REDIS_REST_TOKEN | Токен Upstash Redis REST | - | ваш-redis-токен |
| DALL_E_MODEL | Версия модели DALL-E | dall-e-3 | dall-e-3 |
| CLOUDFLARE_API_TOKEN | Токен API Cloudflare | - | ваш-cloudflare-api-токен |
| CLOUDFLARE_ACCOUNT_ID | ID аккаунта Cloudflare | - | ваш-cloudflare-account-id |
| FLUX_STEPS | Количество шагов генерации Flux | 4 | 4-8, максимум 8 |
| PROMPT_OPTIMIZATION | Включение оптимизации промптов | false | true |
| EXTERNAL_API_BASE | Базовый URL внешнего API | - | https://external-api.com |
| EXTERNAL_MODEL | Имя внешней модели | - | external-model-name |
| EXTERNAL_API_KEY | Ключ внешнего API | - | external-api-key |
| GOOGLE_MODEL_KEY | Ключ API модели Google AI | - | ваш-google-api-ключ |
| GOOGLE_MODEL_BASEURL | Базовый URL API модели Google AI | https://generativelanguage.googleapis.com/v1beta | https://ваш-пользовательский-google-эндпоинт.com |
| GOOGLE_MODELS | Список доступных моделей Google AI | - | gemini-pro,gemini-pro-vision |
| GROQ_API_KEY | Ключ API Groq | - | ваш-groq-api-ключ |
| ANTHROPIC_API_KEY | Ключ API Anthropic | - | ваш-anthropic-api-ключ |
| ANTHROPIC_BASE_URL | Базовый URL API Anthropic | https://api.anthropic.com | https://ваш-пользовательский-anthropic-эндпоинт.com |

Примечание: Некоторые переменные требуют ручной настройки и не имеют значений по умолчанию.

## 🚀 Функция анализа изображений

Позволяет пользователям загружать изображения и получать результаты анализа ИИ. Как использовать:

1. Пользователь отправляет изображение боту.
2. В описании изображения добавляет подсказку для анализа, например, "Пожалуйста, проанализируйте это изображение".
3. Бот использует выбранную ИИ-модель (OpenAI или Google Gemini) для анализа изображения.
4. Результат анализа отправляется пользователю в виде текстового сообщения.

Примечание: Убедитесь, что используемая ИИ-модель поддерживает анализ изображений. Если текущая модель не поддерживает эту функцию, бот предложит переключиться на модель с поддержкой мультимодальности.

## ⚠️ Важные замечания

1. 🚦 **Разумное использование квот API**: Особенно для сервисов генерации и анализа изображений, следите за ограничениями использования.
2. 🔐 **Защита конфиденциальной информации**: Надежно храните переменные окружения и ключи API.
3. 🧠 **Понимание особенностей моделей**: Выбирайте ИИ-модель, наиболее подходящую для вашего сценария использования.
4. 🔄 **Поддержание актуальности**: Регулярно обновляйте код и функции для оптимальной производительности.
5. 🛡️ **Безопасность прежде всего**: Регулярно обновляйте ключи API, следуйте принципу наименьших привилегий.
6. 🎨 **Оптимизация промптов Flux**: При включении PROMPT_OPTIMIZATION убедитесь, что правильно настроены EXTERNAL_API_BASE, EXTERNAL_MODEL и EXTERNAL_API_KEY.

## 🚀 Оптимизация промптов Flux

Когда переменная окружения PROMPT_OPTIMIZATION установлена в true, функция генерации изображений Flux использует внешнее API для оптимизации промптов. Эта функция работает следующим образом:

1. Пользователь предоставляет исходный промпт.
2. Система использует внешнее API, настроенное с помощью EXTERNAL_API_BASE, EXTERNAL_MODEL и EXTERNAL_API_KEY, для оптимизации промпта.
3. Оптимизированный промпт используется для генерации изображения моделью Flux.

Эта функция помогает создавать более точные изображения, соответствующие особенностям модели Flux. Для использования этой функции убедитесь, что все соответствующие переменные окружения настроены правильно.

## 🔧 Устранение неполадок

- Бот не отвечает? Проверьте настройки вебхука и конфигурацию переменных окружения.
- Столкнулись с ограничениями API? Проверьте использование квоты вашего API.
- Не удается выполнить анализ изображения? Убедитесь, что вы используете модель с поддержкой мультимодальности, такую как GPT-4o/GPT-4o-mini или Gemini 1.5 Pro/flash и другие подобные модели.

## 📄 Лицензия

Этот проект распространяется под [лицензией MIT](LICENSE).

Copyright (c) 2024 [snakeying]
