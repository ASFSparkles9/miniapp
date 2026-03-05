# 📱 VPN VLESS MiniApp | Telegram Integration

![React](https://img.shields.io/badge/React-18.2+-61DAFB?style=for-the-badge&logo=react)
![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-009688?style=for-the-badge&logo=fastapi)
![Telegram](https://img.shields.io/badge/Telegram-Mini%20App-2CA5E0?style=for-the-badge&logo=telegram)
![Xray](https://img.shields.io/badge/Xray-VLESS%20Reality-00897B?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker)

Современное Telegram Mini App для продажи VPN-подписок VLESS с интеграцией X-UI панелью и адаптивным интерфейсом.

## ✨ Ключевые возможности

### 🎯 Пользовательский опыт
- 📱 **Native Telegram интеграция** - авторизация без паролей
- 🎨 **Адаптивный дизайн** - идеальное отображение на всех устройствах
- 💳 **Мультиплатежная система** - поддержка различных платежных шлюзов
- 🔗 **Умная генерация** - автоматическое создание VLESS конфигураций
- 📊 **Личный кабинет** - управление подписками в реальном времени

### 🛠️ Технические преимущества
- ⚡ **FastAPI backend** - высокая производительность API
- 🔐 **JWT аутентификация** - безопасная сессия пользователей
- 🔄 **Real-time синхронизация** - мгновенное обновление статусов
- 🐳 **Docker контейнеризация** - простое развертывание
- 📡 **X-UI API интеграция** - seamless управление VPN сервером

## 🏗️ Архитектура системы

```
vpn-miniapp/
├── 📁 backend/              # FastAPI сервер
│   ├── main.py             # Основное приложение
│   ├── models.py           # SQLAlchemy модели
│   ├── schemas.py          # Pydantic валидация
│   ├── security.py         # JWT аутентификация
│   ├── xui_client.py       # X-UI API клиент
│   └── payment_service.py  # Платежные шлюзы
├── 📁 frontend/             # React приложение
│   ├── src/
│   │   ├── App.js          # Главный компонент
│   │   ├── components/     # UI компоненты
│   │   └── services/       # API клиенты
│   └── public/
└── 🐳 docker-compose.yml    # Оркестрация контейнеров
```

## 🚀 Технологический стек

### Backend
- **FastAPI 0.104+** - асинхронный веб-фреймворк
- **SQLAlchemy 2.0+** - ORM с async поддержкой
- **Pydantic** - валидация и сериализация данных
- **JWT** - токеновая аутентификация
- **X-UI API** - управление VPN инфраструктурой

### Frontend
- **React 18.2+** - компонентный UI
- **@twa-dev/sdk** - Telegram Mini App SDK
- **Axios** - HTTP клиент с interceptors
- **CSS3** - современная стилизация с анимациями

### Инфраструктура
- **Docker & Docker Compose** - контейнеризация
- **SQLite (dev) / PostgreSQL (prod)** - базы данных
- **Nginx** - reverse proxy и статика

## 🎮 Функциональность

### Пользовательские сценарии
1. **Авторизация через Telegram** - без ввода паролей
2. **Выбор тарифного плана** - гибкие опции подписки
3. **Оплата через платежные шлюзы** - YooKassa, CryptoBot
4. **Получение VPN конфигурации** - VLESS ссылки и QR-коды
5. **Управление подписками** - продление и отслеживание

### Административные функции
- 📊 **Аналитическая панель** - статистика продаж и пользователей
- 🎯 **Управление тарифами** - создание и редактирование планов
- 👥 **Пользовательский менеджмент** - просмотр и управление
- 💰 **Финансовая аналитика** - детальная отчетность

## 🛠️ Быстрый старт

### Требования
- Docker & Docker Compose
- Node.js 18+ (для разработки)
- Python 3.11+ (для разработки)

### Запуск через Docker Compose

```bash
# Клонирование репозитория
git clone https://github.com/yourusername/vpn-miniapp.git
cd vpn-miniapp

# Конфигурация окружения
cp backend/.env.example backend/.env
# Отредактируйте .env с вашими настройками

# Запуск всех сервисов
docker-compose up -d

# Приложение будет доступно в Telegram по ссылке
# https://t.me/yourbot/app
```

### Ручная установка

```bash
# Backend
cd backend
pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000

# Frontend (новый терминал)
cd frontend
npm install
npm start
```

## ⚙️ Конфигурация

### Backend (.env)
```env
# X-UI Integration
XUI_API_URL=http://localhost:54321
XUI_API_KEY=your_api_key_here

# Database
DATABASE_URL=sqlite+aiosqlite:///./vpn.db

# Security
SECRET_KEY=your_super_secret_key_here
JWT_ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30

# Admin
ADMIN_USERNAME=admin
ADMIN_PASSWORD=secure_password

# Payment Gateways
YOOKASSA_SHOP_ID=your_shop_id
YOOKASSA_SECRET_KEY=your_secret_key
CRYPTOBOT_TOKEN=your_crypto_token
```

### Frontend конфигурация
```javascript
// src/config.js
export const API_BASE_URL = process.env.REACT_APP_API_URL || 'http://localhost:8000';
export const TELEGRAM_BOT_USERNAME = process.env.REACT_APP_BOT_USERNAME || 'yourbot';
```

## 📡 API Documentation

### Аутентификация
```http
POST /api/auth/telegram
Content-Type: application/json

{
  "init_data": "telegram_init_data_string",
  "user": {
    "id": 123456789,
    "first_name": "John",
    "username": "john_doe"
  }
}
```

### Управление подписками
```http
GET /api/subscriptions
Authorization: Bearer <jwt_token>

POST /api/subscriptions
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "tariff_id": "uuid",
  "payment_method": "yookassa"
}
```

## 💳 Интеграция платежных систем

### YooKassa
```python
async def create_yookassa_payment(amount: float, description: str):
    payment = yookassa.Payment.create({
        "amount": {"value": amount, "currency": "RUB"},
        "payment_method_data": {"type": "bank_card"},
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

## 🐳 Docker Deployment

```yaml
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=sqlite+aiosqlite:///./vpn.db
    volumes:
      - ./data:/app/data
      
  frontend:
    build: ./frontend
    ports:
      - "3000:80"
    depends_on:
      - backend
      
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - frontend
      - backend
```

## 📊 Мониторинг и аналитика

### Метрики приложения
- 📈 **Active Users** - количество активных пользователей
- 💰 **Revenue** - доход по периодам
- 🔄 **Conversion Rate** - конверсия в подписки
- 📱 **Device Stats** - статистика устройств

### Health checks
```python
@app.get("/health")
async def health_check():
    return {
        "status": "healthy",
        "timestamp": datetime.utcnow(),
        "version": "1.0.0"
    }
```

## 🔒 Безопасность

- ✅ **JWT токены** с коротким сроком жизни
- ✅ **CORS** настройка для Telegram доменов
- ✅ **Валидация** всех входных данных
- ✅ **Rate limiting** для API запросов
- ✅ **Environment variables** для секретов

## 🚀 Производительность

- ⚡ **Async/await** для неблокирующих операций
- 🗄️ **Connection pooling** для базы данных
- 💾 **Redis кэширование** (опционально)
- 📦 **Lazy loading** компонентов React

## 📱 Демонстрация

*[Здесь будут скриншоты интерфейса MiniApp]*

## 🎯 Бизнес-кейсы

- 🏢 **VPN провайдеры** - минимизация затрат на разработку
- 🌐 **SaaS платформы** - интеграция VPN услуг
- 💼 **Корпоративные решения** - внутренние VPN сервисы
- 🚀 **Стартапы** - быстрый запуск VPN бизнеса

## 🤝 Вклад в проект

Проект демонстрирует экспертизу в:
- Full Stack разработке (React + FastAPI)
- Telegram Bot API и Mini App
- API интеграции и автоматизации
- DevOps и контейнеризации
- Fintech интеграциях

## 📄 Лицензия

MIT License - свободное использование с указанием авторства

## 👨‍💻 Автор

**Ваше Имя** - Full Stack Developer
- GitHub: [your-username](https://github.com/your-username)
- Telegram: [@your-telegram]
- Portfolio: [your-portfolio.com](https://your-portfolio.com)

---

⭐ **Поддержите проект звездой** и следите за обновлениями!
