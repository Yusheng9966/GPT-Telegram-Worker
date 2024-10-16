# 🤖💬 Telegram GPT Worker - многофункциональный ИИ-ассистент

[English](../README.md) | [简体中文](./README.zh-cn.md) | [繁體中文](./README.zh-hant.md) | [日本語](./README.ja.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Русский](./README.ru.md) | [Deutsch](./README.de.md)

## 📖 О проекте

Добро пожаловать в Telegram GPT Worker! 👋 Это эффективный Telegram-бот, разработанный на TypeScript. Он поддерживает множество языков и моделей ИИ, развернут на Cloudflare Workers, обеспечивая пользователям быстрый и масштабируемый сервис.

## 🌟 Ключевые функции

1. 🧠 **Поддержка нескольких моделей**: Интеграция с OpenAI, Google Gemini, Anthropic Claude, Groq и Azure OpenAI.
2. 🔗 **Поддержка моделей, совместимых с OpenAI**: Разработано специально для систем управления и распространения интерфейсов моделей ИИ, таких как One API и New API, с поддержкой автоматического получения списка моделей.
3. 💬 **Умный диалог**: Способность запоминать контекст для естественного общения.
4. 🎨 **Генерация изображений**: Создание изображений по текстовому описанию с использованием DALL·E и Cloudflare Flux.
5. 🖼️ **Анализ изображений**: Пользователи могут загружать изображения для интеллектуального анализа с помощью моделей OpenAI или Google Gemini.
6. 🌍 **Многоязычность**: Встроенная поддержка i18n для 8 языков.
7. 🔒 **Управление доступом**: Контроль доступа через белый список для повышения безопасности.
8. ☁️ **Высокопроизводительное развертывание**: Использование возможностей краевых вычислений Cloudflare Workers для быстрого отклика.
9. 🗄️ **Эффективное управление данными**: Использование Redis для кэширования и управления данными.
10. 🔧 **Оптимизация подсказок Flux**: Опциональная функция для оптимизации подсказок генерации изображений модели Flux через внешний API.

## 📋 Системные требования

Перед началом использования убедитесь, что у вас есть:

- Аккаунт [Cloudflare](https://dash.cloudflare.com/)
- Аккаунт Telegram и токен бота
- База данных [Upstash](https://upstash.com/) Redis (с включенной функцией [Eviction](https://upstash.com/docs/redis/features/eviction))
- API-ключ как минимум для одного ИИ-сервиса

## 🚀 Быстрый старт

1. Клонируйте репозиторий проекта
2. Настройте необходимые переменные окружения
3. Разверните на Cloudflare Workers
4. Настройте вебхук Telegram

Подробные инструкции по развертыванию смотрите ниже.

## 📝 Доступные команды

- `/start` - Запуск бота
- `/language` - Смена языка
- `/switchmodel` - Переключение модели ИИ
- `/new` - Начало нового диалога
- `/history` - Получение краткого содержания истории диалога
- `/help` - Получение справки
- `/img` - Генерация изображения (DALL-E)
- `/flux` - Генерация изображения (Cloudflare Flux)

## 📁 Структура проекта

```
/GPT-Telegram-Worker
│
├── /src
│   ├── /api
│   │   ├── azure .ts              # Обработка взаимодействия с Azure API
│   │   ├── claude.ts              # Обработка взаимодействия с Claude API
│   │   ├── flux-cf.ts             # Обработка интерфейса ИИ-рисования Cloudflare
│   │   ├── gemini.ts              # Обработка взаимодействия с Google Gemini API
│   │   ├── groq.ts                # Обработка взаимодействия с Groq API
│   │   ├── image_generation.ts    # Обработка интерфейса рисования DALL·E
│   │   ├── model_api_interface.ts # Общий интерфейс, определяющий стандартную структуру API модели
│   │   ├── openai_api.ts          # Обработка взаимодействия с OpenAI API
│   │   ├── openai_compatible.ts   # Обрабатывает взаимодействия с OpenAI совместимым API
│   │   └── telegram.ts            # Логика Telegram бота
│   ├── /config                    # Файлы конфигурации
│   │   └── commands.ts            # Команды Telegram бота
│   ├── /utils
│   │   └── helpers.ts             # Вспомогательные функции и инструменты
│   │   └── i18n.ts                # Функции многоязычности
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

5. Отредактируйте файл `wrangler.toml`, настройте проект:

   ```toml
   name = "telegram-bot"
   type = "javascript"
   account_id = "your_account_id"
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
6. В "Settings" добавьте необходимые переменные окружения.

### Настройка вебхука Telegram

Используйте Telegram Bot API для настройки вебхука, пример URL:

```bash
https://api.telegram.org/bot<YOUR_BOT_TOKEN>/setWebhook?url=https://your-worker.your-subdomain.workers.dev/webhook
```

```bash
https://api.telegram.org/bot123456789:abcdefghijklmn/setWebhook?url=https://gpt-telegram-worker.abcdefg.workers.dev/webhook
```

### Локальная разработка

1. Клонируйте проект:

   ```bash
   git clone https://github.com/your-username/telegram-bot.git
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
| OPENAI_BASE_URL | Базовый URL API OpenAI | https://api.openai.com/v1 | https://your-custom-endpoint.com/v1 |
| OPENAI_MODELS | Список доступных моделей OpenAI | - | gpt-3.5-turbo,gpt-4 |
| TELEGRAM_BOT_TOKEN | Токен Telegram бота | - | 123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11 |
| WHITELISTED_USERS | Список ID пользователей, которым разрешено использовать бота | - | 12345678,87654321 |
| SYSTEM_INIT_MESSAGE | Начальное сообщение системы | You are a helpful assistant. | Вы - полезный ассистент. |
| SYSTEM_INIT_MESSAGE_ROLE | Роль начального сообщения системы | system | system |
| DEFAULT_MODEL | Модель ИИ, используемая по умолчанию | - | gpt-3.5-turbo |
| UPSTASH_REDIS_REST_URL | URL Upstash Redis REST | - | https://your-redis-url.upstash.io |
| UPSTASH_REDIS_REST_TOKEN | Токен Upstash Redis REST | - | your-redis-token |
| DALL_E_MODEL | Версия модели DALL-E | dall-e-3 | dall-e-3 |
| CLOUDFLARE_API_TOKEN | Токен API Cloudflare | - | your-cloudflare-api-token |
| CLOUDFLARE_ACCOUNT_ID | ID аккаунта Cloudflare | - | your-cloudflare-account-id |
| FLUX_STEPS | Количество шагов генерации Flux | 4 | 4-8, максимум 8 |
| PROMPT_OPTIMIZATION | Включение оптимизации подсказок | false | true |
| EXTERNAL_API_BASE | Базовый URL внешнего API | - | https://external-api.com |
| EXTERNAL_MODEL | Название внешней модели | - | external-model-name |
| EXTERNAL_API_KEY | Ключ внешнего API | - | external-api-key |
| GOOGLE_MODEL_KEY | Ключ API модели Google AI | - | your-google-api-key |
| GOOGLE_MODEL_BASEURL | Базовый URL API модели Google AI | https://generativelanguage.googleapis.com/v1beta | https://your-custom-google-endpoint.com |
| GOOGLE_MODELS | Список доступных моделей Google AI | - | gemini-pro,gemini-pro-vision |
| GROQ_API_KEY | Ключ API Groq | - | your-groq-api-key |
| ANTHROPIC_API_KEY | Ключ API Anthropic | - | your-anthropic-api-key |
| ANTHROPIC_BASE_URL | Базовый URL API Anthropic | https://api.anthropic.com | https://your-custom-anthropic-endpoint.com |
| OPENAI_COMPATIBLE_KEY | Ключ API, совместимый с OpenAI | - | sk-abcdefghijklmnopqrstuvwxyz123456 |
| OPENAI_COMPATIBLE_URL | Базовый URL API, совместимого с OpenAI | - | https://your-custom-endpoint.com/v1 |

Примечание: Некоторые переменные требуют ручной настройки и не имеют значений по умолчанию.

## 🚀 Функция анализа изображений

Позволяет пользователям загружать изображения и получать результаты анализа ИИ. Как использовать:

1. Пользователь отправляет боту изображение.
2. В описании изображения добавляет запрос на анализ, например, "Пожалуйста, проанализируйте это изображение".
3. Бот использует текущую выбранную модель ИИ (OpenAI или Google Gemini) для анализа изображения.
4. Результаты анализа отправляются пользователю в виде текстового сообщения.

Примечание: Убедитесь, что используемая вами модель ИИ поддерживает функцию анализа изображений. Если текущая модель не поддерживает эту функцию, бот предложит переключиться на модель с поддержкой мультимодальности.

## 🚀 Оптимизация подсказок Flux

Когда переменная окружения PROMPT_OPTIMIZATION установлена в true, функция генерации изображений Flux использует внешний API для оптимизации подсказок. Эта функция работает следующим образом:

1. Пользователь предоставляет исходную подсказку.
2. Система использует внешний API, настроенный с помощью EXTERNAL_API_BASE, EXTERNAL_MODEL и EXTERNAL_API_KEY, для оптимизации подсказки.
3. Оптимизированная подсказка используется моделью Flux для генерации изображения.

Эта функция помогает создавать более точные изображения, соответствующие особенностям модели Flux. Для использования этой функции убедитесь, что все соответствующие переменные окружения настроены правильно.

## ⚠️ Важные замечания

1. 🚦 **Разумное использование квот API**: Особенно для сервисов генерации и анализа изображений, следите за ограничениями использования.
2. 🔐 **Защита конфиденциальной информации**: Надежно храните переменные окружения и ключи API.
3. 🧠 **Понимание особенностей моделей**: Выбирайте модель ИИ, наиболее подходящую для вашего сценария использования.
4. 🔄 **Поддержание актуальности**: Регулярно обновляйте код и функционал для оптимальной производительности.
5. 🛡️ **Безопасность прежде всего**: Регулярно обновляйте ключи API, следуйте принципу наименьших привилегий.
6. 🎨 **Оптимизация подсказок Flux**: При включении PROMPT_OPTIMIZATION убедитесь, что правильно настроены EXTERNAL_API_BASE, EXTERNAL_MODEL и EXTERNAL_API_KEY.
7. ⛔ **Важное замечание**: Чтобы избежать потенциальных конфликтов, не рекомендуется добавлять модели, уже используемые другими API, в OpenAI Compatible. Например, если вы настроили Gemini API и выбрали модель gemini-1.5-flash, не следует добавлять ту же модель в OpenAI Compatible.

## 🔧 Устранение неполадок

- Бот не отвечает? Проверьте настройки вебхука и конфигурацию переменных окружения.
- Столкнулись с ограничениями API? Проверьте использование квоты вашего API.
- Не удается выполнить анализ изображения? Убедитесь, что вы используете модель с поддержкой мультимодальности, такую как GPT-4o/GPT-4o-mini или Gemini 1.5 Pro/flash и другие подобные модели.

## 📄 Лицензия

Этот проект распространяется под [лицензией MIT](LICENSE).

Copyright (c) 2024 [snakeying]
