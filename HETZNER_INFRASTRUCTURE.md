# 🖥️ Инфраструктура Hetzner

**Сервер:** `ssh root@65.21.49.91`  
**Ресурсы:** 173GB/226GB диск (80%), 8.5GB/15GB RAM  
**Контейнеры:** 51 (39 активных) | **БД:** 6 PostgreSQL | **Redis:** 4 | **Домены:** 50+

---

## 🗄️ PostgreSQL: `10.0.0.3:5432`

**User:** `teleeng` | **Pass:** `S65EcfVRXYs`

| База | Проект | Описание | Записей |
|------|--------|----------|---------|
| `billing` | Billing | Платежи, подписки | - |
| `support` | Support | Тикеты, саппорт | - |
| `teleengprod` | ManyClubs | Недвижка, рыбалка, распаковщик | - |
| `school` | School | Обучающая платформа | - |
| `sudnik` | Sudnik Admin | Личная поддержка + AI автоответы | 954 юзеров, 32K сообщений |
| `alex1` | Legacy | Старые проекты | - |

**Подключение:**
```bash
ssh -L 5433:10.0.0.3:5432 root@65.21.49.91
psql -h localhost -p 5433 -U teleeng -d billing
```

---

## 🐳 Основные сервисы

| Проект | Контейнер | Порт | БД | Домен |
|--------|-----------|------|----|----|
| **ManyClubs** | manyclubs + celery + redis + flower | 9100, 8888, 5550 | teleengprod | manyclubs.teleeng.co |
| **Billing** | billing | 9160 | billing | billing.teleeng.co |
| **Support** | support + celery + redis | 9150 | support | support.teleeng.co |
| **School** | school-app-run-* | - | school | school.teleeng.co |
| **Sudnik Admin** | sudnik-admin + celery + redis | 9170 | sudnik | sudnik-admin.teleeng.co |

**Стек:** Django 4.2.6, Celery, Redis, PostgreSQL  
**Репозитории:** `/root/repos/{billing,manyclubs,support}`

### 🤖 Юзерботы

**11 активных:** teleengdev (0-4), sudnik_new, support, teleengnews, teleengnews_telethon, userbot-dolise, userbot-sudnik  
**Порты:** 11000-11014, 9778-9779

### 💳 Payments v2

**6 инстансов:** v2payments (9005), v2payments-beta (9007), assistant-payments (9009/9010), mercury-payments (9012), school-payments (9008)

### 🔧 Другие сервисы

| Сервис | Порт | Описание |
|--------|------|----------|
| office-frontend | 3010 | app.aioffice.me |
| inn-org-lookup | 9155 | Поиск по ИНН |
| mailer-email-service | 9880 | mailer.teleeng.co |
| squid-proxy | 7777 | proxy.teleeng.co |
| mercury-celery + redis | - | mercury.teleeng.co |
| webapp-web | 9784 | webapp.teleeng.ai |
| profiler-web | 9667 | Profiler |

---

## 🌐 Все домены и сервисы

### Основные продукты:
- ✅ **teleeng.co** — главный сайт
- ✅ **teleeng.me** — альтернативный домен
- ✅ **billing.teleeng.co** — биллинг система
- ✅ **support.teleeng.co** — техподдержка
- ✅ **school.teleeng.co** — школа

### ManyClubs:
- ✅ **manyclubs.teleeng.co** — prod
- ✅ **manyclubsbeta.teleeng.co** — beta
- ✅ **manyclubsstage.teleeng.co** — stage
- ✅ **manyclubs-django.teleeng.co** — Django admin

### Assistants (ассистенты):
- ✅ **assistant.teleeng.co** — prod
- ✅ **assistantbeta.teleeng.co** — beta
- ✅ **mercury.teleeng.co** — старый ассистент

### Платежи (payments):
- ✅ **pay.teleeng.co** — основной
- ✅ **payv2.teleeng.co** — v2 prod
- ✅ **payv2beta.teleeng.co** — v2 beta
- ✅ **payv2stage.teleeng.co** — v2 stage
- ✅ **payments.teleeng.co** — старый
- ✅ **paymentsbeta.teleeng.co** — старый beta
- ✅ **assistantpay.teleeng.co** — для assistant
- ✅ **assistantbetapay.teleeng.co** — для assistant beta
- ✅ **mercurypay.teleeng.co** — для mercury
- ✅ **schoolpay.teleeng.co** — для школы

### Юзерботы:
- ✅ **userbot.teleeng.co**
- ✅ **userbot-dolise.teleeng.co**
- ✅ **userbot-sudnik.teleeng.co**
- ✅ **userbot-support.teleeng.co**
- ✅ **userbot-teleengdev.teleeng.co** (+ dev1, dev2, dev3, dev4)

### Личные проекты:
- ✅ **sudnik-admin.teleeng.co** — админка Андрея
- ✅ **sudnik.pro** — личный сайт
- ✅ **ai.sudnik.pro** — AI проекты

### Другие сервисы:
- ✅ **app.aioffice.me** — Office app
- ✅ **app.contexta.cc** — Contexta
- ✅ **chat.teleeng.co** — чат
- ✅ **chat.teleeng.me** — альт. чат
- ✅ **mailer.teleeng.co** — почтовый сервис
- ✅ **proxy.teleeng.co** — прокси
- ✅ **collector.teleeng.co** — сборщик данных
- ✅ **webapp.teleeng.ai** — веб-приложение
- ✅ **appsmith.teleeng.co** — Appsmith

### SPG проекты:
- ✅ **spg.teleeng.co** — prod
- ✅ **spg-beta.teleeng.co** — beta

---

## 📁 Структура проектов на сервере

### `/root/repos/` — Активные репозитории (GitHub)
```
/root/repos/
├── billing/          # Биллинг система (prod)
├── manyclubs/        # ManyClubs (prod)
└── support/          # Support система (prod)
```

### `/root/projects/` — Старые версии
```
/root/projects/
├── manyclubs/        # Старая версия ManyClubs
├── payments/         # Старая платежная система
└── v2/
    └── payments/     # Payments v2 (старая папка)
```

### Другие важные директории:
```
/root/
├── billing/          # Билинг (активный)
├── support/          # Саппорт (активный)
├── school/           # Школа (активный)
├── sudnik-admin/     # Админка Андрея
├── office/           # Office frontend
├── inn/              # INN lookup service
├── mailer/           # Email service
├── proxy/            # Squid proxy
├── userbot/          # Юзерботы configs
├── userbot-dolise/   # Userbot Dolise
├── userbot-sudnik/   # Userbot Sudnik
├── school-webapp/    # School webapp
└── webapp/           # Assistant webapp
```

---

## 🔄 Redis инстансы

| Контейнер | Проект | Порт | Статус |
|-----------|--------|------|--------|
| `sudnik-admin-redis-1` | Sudnik Admin | 6379 | ✅ Up 6 weeks |
| `support-redis-1` | Support | 6379 | ✅ Up 6 weeks |
| `manyclubs-redis-1` | ManyClubs | 6379 | ✅ Up 6 weeks |
| `mercury-redis-1` | Mercury | 6379 | ✅ Up 7 months |

---

## 💾 Использование ресурсов

### Диск:
- **Всего:** 226GB
- **Занято:** 173GB (80%)
- **Свободно:** 44GB
- **Docker images:** 31.35GB (можно освободить 15GB)
- **Docker containers:** 50.41GB
- **Build cache:** 23.09GB (можно очистить)

### Память:
- **Всего:** 15GB
- **Используется:** 8.5GB
- **Свободно:** 554MB
- **Buffer/Cache:** 6.2GB
- **Доступно:** 6.4GB

### Рекомендации по очистке:
```bash
# Очистить неиспользуемые образы
docker image prune -a

# Очистить build cache
docker builder prune

# Очистить остановленные контейнеры
docker container prune

# Полная очистка (осторожно!)
docker system prune -a --volumes
```

---

## 🔧 Технологический стек

### Backend:
- **Python/Django:** 4.2.6 (все основные проекты)
- **Celery:** Для асинхронных задач (ManyClubs, Support, School)
- **Redis:** Кэширование и брокер сообщений

### Database:
- **PostgreSQL:** 10.0.0.3 (все проекты используют один сервер БД)

### Frontend:
- **Node.js:** Office, webapp, profiler-web

### Infrastructure:
- **Nginx:** Reverse proxy для всех сервисов
- **Docker:** Контейнеризация всех сервисов
- **Docker Compose:** Оркестрация

### Integrations:
- **Stripe:** Платежи (Billing)
- **CloudPayments:** Платежи (Billing)
- **Telegram Bot API:** Все боты и юзерботы
- **Email:** Mailer service

---

## 🚨 Остановленные сервисы (можно удалить)

Эти контейнеры остановлены и могут быть удалены для освобождения места:

| Контейнер | Проект | Статус | Действие |
|-----------|--------|--------|----------|
| `school` | School (old) | Exited 5 weeks ago | ⚠️ Можно удалить |
| `school-celery-beat-1` | School (old) | Exited 6 weeks ago | ⚠️ Можно удалить |
| `school-exporter-1` | School (old) | Exited 6 weeks ago | ⚠️ Можно удалить |
| `school-celery-1` | School (old) | Exited 5 weeks ago | ⚠️ Можно удалить |
| `school-flower-1` | School (old) | Exited 6 weeks ago | ⚠️ Можно удалить |
| `school-redis-1` | School (old) | Exited 5 weeks ago | ⚠️ Можно удалить |
| `school-web-1` | School webapp (old) | Exited 6 weeks ago | ⚠️ Можно удалить |
| `school-webapp-web-1` | School webapp (old) | Exited 4 months ago | ⚠️ Можно удалить |
| `v2payments-stage` | Payments v2 stage | Exited 4 months ago | ⚠️ Можно удалить |
| `llmproxy-proxy-llm-service-1` | LLM Proxy | Exited 3 months ago | ⚠️ Можно удалить |
| `userbot` | Old userbot | Exited 3 months ago | ⚠️ Можно удалить |
| `pcard-app-1` | School card | Exited 4 months ago | ⚠️ Можно удалить |

**Команда для удаления:**
```bash
docker rm school school-celery-beat-1 school-exporter-1 school-celery-1 school-flower-1 school-redis-1 school-web-1 school-webapp-web-1 v2payments-stage llmproxy-proxy-llm-service-1 userbot pcard-app-1
```

---

## 📋 Чек-лист для работы с сервером

### Подключение:
```bash
ssh root@65.21.49.91
```

### Мониторинг:
```bash
# Все контейнеры
docker ps -a

# Логи конкретного сервиса
docker logs -f <container_name>

# Использование ресурсов
docker stats

# Диск
df -h

# Память
free -h
```

### Работа с базой данных:
```bash
# SSH туннель для локального подключения
ssh -L 5433:10.0.0.3:5432 root@65.21.49.91

# Подключение с локальной машины
psql -h localhost -p 5433 -U teleeng -d billing
```

### Деплой:
```bash
# Billing
cd /root/repos/billing && make deploy-prod

# ManyClubs
cd /root/repos/manyclubs && make deploy-prod

# Support
cd /root/repos/support && make deploy-prod
```

### Nginx:
```bash
# Проверить конфигурацию
nginx -t

# Перезагрузить
nginx -s reload

# Логи
tail -f /var/log/nginx/error.log
tail -f /var/log/nginx/access.log
```

---

## 🔍 База данных `sudnik` — Детали

**Назначение:** Личная система поддержки с AI-автоответами  
**Контейнер:** `sudnik-admin` (использует образ Support)  
**Статистика:** 954 пользователей, 32,294 сообщений, 10 автоответов

### Таблицы:

| Таблица | Описание | Записей |
|---------|----------|---------|
| `core_user` | Пользователи из Telegram | 954 |
| `core_message` | История сообщений | 32,294 |
| `core_approvedreply` | Шаблоны автоответов | 10 |
| `core_config` | Конфигурация AI промптов | 8 |
| `core_club` | Клубы/группы | 0 |

### Функции:
- **AI генерация ответов** через OpenAI (GPT-4o)
- **Автоответы** по regex паттернам
- **Связь с офисным чатом** (OFFICE_CHAT_ID: -1002608176335)
- **Userbot интеграция** (userbot-sudnik.teleeng.co)
- **Vector база** для семантического поиска (vecv2)
- **ElevenLabs** для голосовых сообщений
- **Celery** для фоновой обработки

### Ключевые конфиги:
```
GEN_MODEL: gpt-4o
AUTO_REPLIES: 0 (выключены)
AUTOREPLY_TEXT: "Привет! 🙂 Обычно отвечаю в течение 24 часов..."
```

---

## 🔍 База данных `alex1` — Детали

**Назначение:** Legacy база для обучающих программ с подписками  
**Используется:** ManyClubs, School (через доп. подключение)  
**Статистика:** 442 пользователей, 1,277 чатов, 11K контента, 52K транзакций

### Таблицы:

| Таблица | Описание | Записей |
|---------|----------|---------|
| `costs` | Финансовые транзакции/затраты | 52,453 |
| `logs` | Логи действий | 12,281 |
| `content` | Учебный контент программ | 11,359 |
| `chats` | Telegram чаты/подписки | 1,277 |
| `users` | Пользователи программ | 442 |
| `talkLog` | История диалогов | 274 |
| `questions` | Вопросы/тесты | 77 |
| `programs` | Программы обучения | 56 |
| `soficontent` | Контент Софи | 50 |
| `payments` | Платежи | 2 |
| `checkout_widget_data` | Данные виджетов | - |
| `subscription_management` | Управление подписками | - |
| `program_types` | Типы программ | - |

### Структура основных таблиц:

**chats** (1,277 записей):
- `chatId`, `programCode`, `class` — привязка к программам
- `subscription_enabled`, `price_rub`, `price_eur` — подписки
- `processName`, `processArg` — текущий процесс
- `checker_now` — кто проверяет
- `21ready` — готовность к 21-дневной программе
- `feedback_start_time`, `last_lesson_time` — таймлайны

**users** (442 записи):
- `user_id`, `chat_id`, `username`, `name`
- `status`, `comment`, `link`
- `teachers_class` — преподавательский класс
- `rate`, `fee` — оплата

**content** (11,359 записей):
- `programCode`, `class` — привязка к уроку
- `fromChatId`, `fromMessageId` — Telegram ссылки
- `command` — команда/действие

**programs** (56 программ):
- `programCode`, `ownerChatId` — код и владелец программы

**Назначение:** Устаревшая система управления обучающими курсами/программами через Telegram с системой подписок, проверки домашних заданий и учёта затрат.

---

## 🔗 Связанные документы

- [ACCESSES.md](../Billing/docs/ACCESSES.md) — Детальная документация по Billing системе
- [💻 Dev.md](./💻%20Dev.md) — Dev заметки и правила работы

---

## 📝 Примечания

1. **Все проекты используют единый PostgreSQL сервер** на `10.0.0.3`
2. **Пароль от БД** во всех проектах одинаковый: `S65EcfVRXYs`
3. **Django 4.2.6** — версия во всех основных проектах
4. **Redis** используется для кэширования и Celery
5. **Nginx** проксирует все запросы на соответствующие контейнеры
6. **SSH доступ** работает только по ключу (пароль не настроен)

---

**Последнее обновление:** 23 октября 2025  
**Аудит провёл:** Cursor AI Assistant

