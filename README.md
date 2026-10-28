# 🚛 ТандемСтрой — Корпоративный сайт с бэкендом

Полноценный веб-проект для транспортной компании, специализирующейся на перевозке негабаритных грузов и спецтехники по Центральной России.

---

## 🗂 Стек

| Слой | Технологии |
|---|---|
| Фронтенд | Vanilla HTML/CSS/JS, адаптивная вёрстка |
| Бэкенд | Python 3.11, FastAPI, asyncpg |
| База данных | PostgreSQL (Supabase) |
| Email | Resend API |
| AI чат-бот | OpenRouter API (nvidia/nemotron) |
| Telegram бот | Bot API (webhook) |
| Хостинг фронта | GitHub Pages |
| Хостинг бэка | Amvera Cloud |
| Мониторинг | UptimeRobot |

---

## ✨ Функциональность

### Сайт
- Лендинг с разделами: услуги, география (25+ регионов), как работаем, ЭДО/ЭТрН, контакты
- Форма заявки с валидацией и согласием на обработку персональных данных (152-ФЗ)
- Встроенный AI чат-бот (OpenRouter) с fallback-ответами
- Политика конфиденциальности и Пользовательское соглашение (модальные окна)
- Полный SEO-пакет: мета-теги, Open Graph, Twitter Card, Schema.org JSON-LD
- PWA-манифест, favicon, apple-touch-icon

### Бэкенд (FastAPI)
- `POST /leads` — приём заявок, сохранение в БД
- `POST /chat` — AI чат-бот
- `GET /health` — health-check для UptimeRobot (GET + HEAD)
- `GET /business-card` — HTML рекламная визитка
- `GET /leads` — список заявок (защищён ADMIN_SECRET)
- `POST /bot/webhook` — Telegram webhook
- `GET /photos/*` — отдача статики (фото для рассылки)

### Уведомления
- При новой заявке — письмо на два email-адреса + Telegram уведомление всем администраторам
- Telegram уведомление содержит все данные заявки с кликабельным телефоном

### Email-рассылка
- Автоматическая — каждый понедельник в 08:00 МСК
- Ручной запуск через Telegram бот командой `/newsletter`
- Батчи по 15 писем с паузой 90 секунд (защита от попадания в спам)
- HTML-письмо с фото-коллажем из реальных рейсов
- Умное обращение по имени/ФИО из базы
- Заголовки `List-Unsubscribe`, `Precedence: bulk`
- Отчёт о прогрессе в Telegram после каждого батча

### Telegram бот
- `/help` — список команд
- `/stats` — статистика базы контактов
- `/newsletter` — запустить рассылку вручную
- `/cancel` — остановить рассылку

### ЭДО / ЭТрН
- Информационный раздел на сайте об обязательном переходе на ЭТрН с 01.09.2026

---

## 🗄 Структура проекта

```
/
├── index.html          # Фронтенд (GitHub Pages)
├── site.webmanifest    # PWA манифест
├── favicon-16.png
├── favicon-32.png
├── apple-touch-icon.png
├── og-image.jpg        # Превью для соцсетей 1200×630
├── CNAME               # Кастомный домен GitHub Pages
└── backend/
    ├── main.py         # FastAPI приложение
    ├── requirements.txt
    ├── amvera.yml      # Конфиг деплоя Amvera
    └── photos/         # Фото для email-рассылки
        ├── photo1.png
        ├── photo2.png
        ├── photo3.png
        ├── photo4.png
        └── photo5.png
```

---

## ⚙️ Переменные окружения

```env
DATABASE_URL=postgresql://...      # Supabase Session Pooler
OPENROUTER_API_KEY=sk-or-...
RESEND_API_KEY=re_...
RESEND_FROM_EMAIL=info@tandemstroy-vrn.ru
BOT_TOKEN=...                      # Telegram Bot Token
ADMIN_IDS=123456789,987654321      # Telegram ID администраторов
ADMIN_SECRET=...                   # Пароль для /leads и /admin/*
WEBHOOK_URL=https://....amvera.tech
AI_MODEL=nvidia/nemotron-3.5-super-vl:free
```

---

## 🚀 Деплой

**Фронтенд → GitHub Pages**
1. Push в `main` ветку
2. Settings → Pages → Deploy from branch

**Бэкенд → Amvera**
1. Загрузить `main.py`, `requirements.txt`, `amvera.yml`, папку `photos/`
2. Добавить переменные окружения в панели Amvera
3. `git push` — пересборка автоматическая

**Мониторинг → UptimeRobot**
- URL: `https://....amvera.tech/health`
- Метод: HEAD
- Интервал: 5 минут

---

## 📬 Как работает рассылка

```
/newsletter в TG боте
        ↓
send_weekly_newsletter()
        ↓
Батч 1: 15 писем (пауза 2.5с между каждым)
        ↓ пауза 90с
Батч 2: 15 писем
        ↓ ...
Отчёт в Telegram: "✅ Отправлено X/Y"
```

---

## 📋 Зависимости

```
fastapi
uvicorn[standard]
asyncpg
aiohttp
pydantic[email]
python-dotenv
```

---

*Проект разработан в октябре 2026 года.*
