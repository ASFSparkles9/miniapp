# VPN VLESS MiniApp

Telegram Mini App для продажи VPN VLESS подписок с интеграцией X-UI.

## Возможности

- 🛡️ Продажа VPN подписок через Telegram
- 📱 Адаптивный интерфейс для Telegram Mini App
- 🔐 Авторизация через Telegram (без пароля)
- 💳 Поддержка нескольких платёжных систем
- 📊 Админ-панель для управления тарифами
- 🔗 Генерация VLESS ссылок и QR-кодов
- 📡 Интеграция с X-UI панелью

## Стек технологий

### Backend
- **FastAPI** — высокопроизводительный веб-фреймворк
- **SQLAlchemy + SQLite** — ORM и база данных
- **JWT** — аутентификация
- **X-UI API** — управление VPN сервером

### Frontend
- **React** — библиотека для UI
- **@twa-dev/sdk** — SDK для Telegram Mini App
- **Axios** — HTTP клиент
- **CSS3** — современные стили

## Быстрый старт

### 1. Клонирование и установка

```bash
# Клонировать репозиторий
cd vpn-miniapp

# Установить зависимости backend
cd backend
pip install -r requirements.txt

# Установить зависимости frontend
cd ../frontend
npm install
```

### 2. Настройка окружения

Создайте файл `.env` в папке `backend/`:

```env
# X-UI Configuration
XUI_API_URL=http://localhost:54321
XUI_API_KEY=your_api_key_here

# Database
DATABASE_URL=sqlite+aiosqlite:///./vpn.db

# JWT Secret (измените на случайную строку)
SECRET_KEY=your_super_secret_key_here_change_me

# Admin credentials
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin123
```

### 3. Запуск Backend

```bash
cd backend
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

API будет доступен по адресу: `http://localhost:8000`

### 4. Запуск Frontend

```bash
cd frontend
npm start
```

Frontend будет доступен по адресу: `http://localhost:3000`

### 5. Настройка X-UI

1. Установите X-UI панель на вашем сервере
2. Включите API в настройках X-UI
3. Скопируйте API ключ в файл `.env`

## API Endpoints

### Аутентификация
- `POST /api/auth/register` — регистрация
- `POST /api/auth/login` — вход по email/username
- `POST /api/auth/telegram` — авторизация через Telegram

### Тарифы
- `GET /api/tariffs` — список тарифов
- `POST /api/tariffs` — создание тарифа (админ)
- `PUT /api/tariffs/{uuid}` — обновление тарифа (админ)

### Подписки
- `POST /api/subscriptions` — создание подписки
- `GET /api/subscriptions` — список подписок пользователя
- `GET /api/subscriptions/{uuid}` — детали подписки с конфигом

### Платежи
- `GET /api/payments/{uuid}` — статус платежа

## Структура проекта

```
vpn-miniapp/
├── backend/
│   ├── main.py           # Основное приложение FastAPI
│   ├── models.py         # Модели SQLAlchemy
│   ├── schemas.py        # Pydantic схемы
│   ├── database.py       # Настройки БД
│   ├── security.py       # JWT аутентификация
│   ├── xui_client.py     # Клиент X-UI API
│   ├── config.py         # Конфигурация
│   └── requirements.txt  # Зависимости Python
│
└── frontend/
    ├── public/
    │   └── index.html
    ├── src/
    │   ├── App.js        # Основной компонент
    │   ├── index.js      # Точка входа
    │   └── index.css     # Стили
    └── package.json      # Зависимости npm
```

## Добавление платёжных системы

### YooKassa (пример)

```python
# В файле payment_service.py
import yookassa

yookassa.Configuration.account_id = 'your_shop_id'
yookassa.Configuration.secret_key = 'your_secret_key'

async def create_yookassa_payment(amount: float, description: str):
    payment = yookassa.Payment.create({
        "amount": {
            "value": amount,
            "currency": "RUB"
        },
        "payment_method_data": {
            "type": "bank_card"
        },
        "confirmation": {
            "type": "redirect",
            "return_url": "https://t.me/yourbot/app"
        },
        "description": description
    })
    return payment.confirmation.confirmation_url
```

### CryptoBot

```python
# Пример для CryptoBot API
async def create_cryptobot_invoice(amount: float, currency: str = "USDT"):
    async with httpx.AsyncClient() as client:
        response = await client.post(
            "https://pay.crypt.bot/api/createInvoice",
            headers={"Crypto-Pay-API-Token": "your_token"},
            json={
                "asset": currency,
                "amount": amount,
                "description": "VPN Subscription",
                "expires_in": 3600
            }
        )
        return response.json()
```

## Деплой

### Docker (рекомендуется)

```dockerfile
# Dockerfile.backend
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Docker Compose

```yaml
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      - XUI_API_URL=http://xui:54321
    depends_on:
      - xui
  
  frontend:
    build: ./frontend
    ports:
      - "3000:80"
  
  xui:
    image: ghcr.io/alireza0/x-ui:latest
    ports:
      - "54321:54321"
```

## Разработка

### Создание миграций БД

```bash
cd backend
alembic init migrations
alembic revision --autogenerate -m "Initial migration"
alembic upgrade head
```

### Тестирование

```bash
# Backend tests
cd backend
pytest

# Frontend tests
cd frontend
npm test
```

## Лицензия

MIT License

## Поддержка

По вопросам обращайтесь: @your_telegram