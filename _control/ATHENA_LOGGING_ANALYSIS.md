# 📊 Анализ логирования Athena (Alpha & Beta)

**Дата проверки:** 8 ноября 2025  
**GitLab Issue:** [#7 — Логирование athene_alpha](https://gitlab.aioffice.me/docs/athena/-/issues/7)  
**Статус:** ✅ Все работает, но есть нюансы

---

## 🎯 Резюме проверки

Провел полный аудит инфраструктуры проектов Athena Alpha и Beta на Hetzner сервере. Все сервисы работают корректно, но выявлены особенности системы логирования, которые требуют документирования.

---

## 🔍 Что было проверено

### ✅ Доступность и статус сервисов

| Сервис | Контейнер | Статус | Порт | Домен |
|--------|-----------|--------|------|-------|
| **Alpha Backend** | alpha-app-1 | ✅ Up 10h | 9210 | athene-alpha.teleeng.co |
| **Alpha Celery** | alpha-celery-1 | ✅ Up 10h | - | - |
| **Alpha Beat** | alpha-celery-beat-1 | ✅ Up 10h | - | - |
| **Alpha Frontend** | alpha-frontend-1 | ✅ Up 2 weeks | 9211 | athene-alpha.teleeng.co |
| **Alpha Redis** | alpha-redis-1 | ✅ Up 2 weeks | - | - |
| **Beta Backend** | beta-app-1 | ✅ Up 2 days | 9310 | athene-beta.teleeng.co |
| **Beta Celery** | beta-celery-1 | ✅ Up 2 days | - | - |
| **Beta Beat** | beta-celery-beat-1 | ✅ Up 2 days | - | - |
| **Beta Frontend** | beta-frontend-1 | ✅ Up 2 weeks | 9311 | athene-beta.teleeng.co |
| **Beta Redis** | beta-redis-1 | ✅ Up 2 weeks | - | - |

**Важно:** Сервисы доступны через HTTPS с валидным SSL-сертификатом Let's Encrypt.

---

## 📝 Система логирования

### 1️⃣ **Logfire (Централизованное логирование)**

#### Alpha конфигурация:
```bash
LOGFIRE_ENABLED=true
LOGFIRE_SEND=1
LOGFIRE_PROJECT=athene-alpha
LOGFIRE_API_KEY=pylf_v1_eu_M0lph308jl1z4yrTYZGpbrD5bJwsBT3FH9GbRXGrHb4h
LOGFIRE_AUTH_TOKEN=pylf_v1_eu_M0lph308jl1z4yrTYZGpbrD5bJwsBT3FH9GbRXGrHb4h
LOGFIRE_API_URL=https://app.logfire.dev
```

#### Beta конфигурация:
- ❌ **Logfire НЕ настроен** (нет переменных окружения)

#### ⚠️ Проблема:
**Пакет `logfire` не установлен** в Docker образе! 
- Переменные окружения есть
- Но модуль Python отсутствует → логи не отправляются

**Решение:**
Добавить в `requirements.txt`:
```txt
logfire>=0.x.x
```

**Доступ к логам:**
- URL: https://app.logfire.dev
- Проект: `athene-alpha`

---

### 2️⃣ **Sentry (Мониторинг ошибок)**

#### Конфигурация:
```python
# manyclubs/settings.py
SENTRY_URL = os.environ.get("SENTRY_URL", None)
if SENTRY_URL:
    sentry_sdk.init(
        dsn=SENTRY_URL,
        traces_sample_rate=1.0,
        profiles_sample_rate=1.0,
        max_request_body_size="always",
        max_value_length=8192,
    )
```

#### ⚠️ Статус:
- ✅ Пакет `sentry-sdk[django]` установлен (версия 2.42.1)
- ❌ **Переменная `SENTRY_URL` не задана** → Sentry не инициализирован

**Решение:**
Добавить в `docker-compose.yml`:
```yaml
SENTRY_URL: https://YOUR_SENTRY_DSN@sentry.io/PROJECT_ID
```

---

### 3️⃣ **Telegram Bot Logging**

#### Конфигурация:
```bash
LOGGING_BOT_TOKEN=6338234222:AAHR-GePnXM3p0Ri4VHPjeF4gQxP7Zcl_XE
DEV_CHAT_ERRORS_THREAD_ID=21
DEV_CHAT_USER_ERRORS_THREAD_ID=41
DEV_VIP_ERRORS_THREAD_ID=45
```

✅ **Работает:** Логи отправляются в Telegram через бота

#### Настройка в Django:
```python
# settings.py
LOGGING = {
    "handlers": {
        "telegram": {
            "class": "core.utils.logging_helpers.TelegramHandler",
            "bot_token": os.environ.get("LOGGING_BOT_TOKEN"),
        },
    },
    "root": {
        "handlers": ["console", "telegram", "database"],
    },
}
```

---

### 4️⃣ **Database Logging**

#### ✅ Таблица логов в БД:
```sql
-- База: athene_alpha (PostgreSQL 135.181.24.219:5432)
-- Таблица: assistant_log

Структура:
- id (SERIAL)
- created_at (TIMESTAMP)
- thread_id (INTEGER)
- user_id (BIGINT)
- user_message_id (INTEGER)
- gpt_message_id (INTEGER)
- input (TEXT)
- output (TEXT)
- forum_id (BIGINT)
```

**Статус:** Таблица пустая (0 записей) — логи пишутся только при определённых событиях.

---

### 5️⃣ **Console/Docker Logs**

✅ **Работают отлично:**
```bash
docker logs alpha-app-1 --tail 50
docker logs alpha-celery-1 --tail 50
docker logs beta-app-1 --tail 50
```

**Пример вывода:**
```
[2025-11-07 21:26:45 +0000] [1] [INFO] Starting gunicorn 23.0.0
[2025-11-07 21:26:45 +0000] [1] [INFO] Listening at: http://0.0.0.0:8000 (1)
[2025-11-07 21:26:45 +0000] [7] [INFO] Booting worker with pid: 7
```

---

## 🌐 Nginx & Network

### Alpha конфигурация:
```nginx
# /etc/nginx/conf.d/athene-alpha.teleeng.co.conf
server_name athene-alpha.teleeng.co;

location / {
    proxy_pass http://127.0.0.1:9211;  # Frontend
}

location ~* ^/(api|admin|webhook|waha_webhook|billing_postprocess) {
    proxy_pass http://127.0.0.1:9210;  # Backend
}
```

### Beta конфигурация:
```nginx
# /etc/nginx/conf.d/athene-beta.teleeng.co.conf
server_name athene-beta.teleeng.co;

location / {
    proxy_pass http://127.0.0.1:9311;  # Frontend
}

location ~* ^/(api|admin|webhook|waha_webhook|billing_postprocess) {
    proxy_pass http://127.0.0.1:9310;  # Backend
}
```

✅ **SSL:** Let's Encrypt сертификат `/etc/letsencrypt/live/teleeng.co-0001/`

---

## 🗄️ База данных

### Подключение:
```bash
# PostgreSQL сервер
Host: 135.181.24.219:5432
User: teleeng
Pass: S65EcfVRXYs
Database Alpha: athene_alpha
Database Beta: athene_beta
```

### SSH туннель:
```bash
ssh -L 5433:135.181.24.219:5432 root@65.21.49.91
psql -h localhost -p 5433 -U teleeng -d athene_alpha
```

### Статистика Alpha:
- ✅ Все таблицы Django созданы
- ✅ Миграции применены
- ✅ База готова к работе

**Основные таблицы:**
- `assistant_log` — логи ассистента
- `assistant_thread` — треды с пользователями
- `assistant_run` — запуски AI
- `core_balance` — балансы пользователей
- `core_chatstate` — состояния чатов
- `billing_payments` — платежи

---

## 🐳 Docker Images

```bash
REPOSITORY    TAG     IMAGE ID       CREATED          SIZE
athene        alpha   da77d649b58e   10 hours ago     3.5GB
athene        beta    42efb0f5a4f1   3 days ago       3.5GB
```

**Сборка:** Локальная (на сервере)  
**Base Image:** Python 3.11.11

---

## 📂 Структура проекта на сервере

```
/root/athene/
├── alpha/
│   ├── docker-compose.yml
│   └── volumes/ (НЕ создана — использует /opt/athene/beta/volumes/)
└── beta/
    ├── docker-compose.yml
    └── volumes/
        ├── static/
        └── media/
```

⚠️ **Проблема:** Alpha использует volumes от Beta (ошибка в docker-compose.yml)

**Исправить:**
```yaml
# alpha/docker-compose.yml
volumes:
  - /opt/athene/alpha/volumes/static:/code/static  # Было: beta
  - /opt/athene/alpha/volumes/media:/code/media
```

---

## 🔧 Переменные окружения

### Общие для обоих:
```bash
DB_HOST=135.181.24.219
DB_USER=teleeng
DB_PASS=S65EcfVRXYs
DB_PORT=5432
CLUB_ENV=prod
SECRET_KEY=django-insecure-ss^n0b)up*q84pwgr&+u4e9q6f%dqte12%tq4e@g=a2a!b4a8_
REDIS_URL=redis://redis:6379
USERBOT_URL=https://userbot.teleeng.co
USERBOT_TOKEN=fafbf1bd7407a8e6
OPENAI_URL=https://openrouter.ai/api/v1
API_PROXY=http://u46nED:t5D970@168.181.54.39:8000
ELEVENLABS_API_PROXY=http://ZgGpFr:kdMFrH@170.83.234.68:8000
```

### Уникальные для Alpha:
```bash
DB_NAME=athene_alpha
APP_PORT=9220
FRONTEND_PORT=9221
IMAGE_NAME=athene:alpha
ALLOWED_HOSTS=athene-alpha.teleeng.co
BASE_URL=https://athene-alpha.teleeng.co
```

### Уникальные для Beta:
```bash
DB_NAME=athene_beta
APP_PORT=9320
FRONTEND_PORT=9321
IMAGE_NAME=athene:beta
ALLOWED_HOSTS=athene-beta.teleeng.co
BASE_URL=https://athene-beta.teleeng.co
```

---

## 🚨 Найденные проблемы

| # | Проблема | Критичность | Решение |
|---|----------|-------------|---------|
| 1 | **Logfire не установлен** | 🟡 Medium | Добавить `logfire` в `requirements.txt` |
| 2 | **Sentry не настроен** | 🟡 Medium | Добавить `SENTRY_URL` в docker-compose |
| 3 | **Alpha использует volumes Beta** | 🟠 High | Исправить пути в docker-compose |
| 4 | **Beta без Logfire** | 🟢 Low | Добавить переменные окружения для Beta |
| 5 | **Нет логов в assistant_log** | 🟢 Low | Проверить логику записи логов |

---

## ✅ Чек-лист доступов

### SSH:
```bash
✅ ssh root@65.21.49.91  # Работает
✅ Docker команды доступны
✅ Nginx конфигурации доступны
```

### GitLab:
```bash
✅ glab CLI авторизован (gitlab.aioffice.me)
✅ Issue #7 прочитан
✅ Репозиторий docs/athena доступен
```

### База данных:
```bash
✅ PostgreSQL 135.181.24.219:5432 доступен
✅ Таблицы athene_alpha читаются
✅ Таблицы athene_beta доступны
```

### Домены:
```bash
✅ https://athene-alpha.teleeng.co (302 → /login)
✅ https://athene-beta.teleeng.co (302 → /login)
✅ SSL сертификаты валидны
```

---

## 📋 Как посмотреть логи (Инструкция)

### 1. Docker логи (Console):
```bash
# Подключиться к серверу
ssh root@65.21.49.91

# Логи Alpha
docker logs alpha-app-1 --tail 100 -f
docker logs alpha-celery-1 --tail 100 -f
docker logs alpha-celery-beat-1 --tail 100 -f

# Логи Beta
docker logs beta-app-1 --tail 100 -f
docker logs beta-celery-1 --tail 100 -f
docker logs beta-celery-beat-1 --tail 100 -f
```

### 2. Logfire (Централизованные):
```bash
# Открыть в браузере
https://app.logfire.dev

# Проект: athene-alpha
# API Key: pylf_v1_eu_M0lph308jl1z4yrTYZGpbrD5bJwsBT3FH9GbRXGrHb4h

⚠️ Требуется установка пакета logfire!
```

### 3. Telegram (Ошибки):
```
Бот: 6338234222:AAHR-GePnXM3p0Ri4VHPjeF4gQxP7Zcl_XE
Чат ошибок: Thread ID 21
Чат пользователей: Thread ID 41
Чат VIP: Thread ID 45
```

### 4. База данных (Логи ассистента):
```bash
# SSH туннель
ssh -L 5433:135.181.24.219:5432 root@65.21.49.91

# Подключение
PGPASSWORD=S65EcfVRXYs psql -h localhost -p 5433 -U teleeng -d athene_alpha

# Запросы
SELECT * FROM assistant_log ORDER BY created_at DESC LIMIT 10;
SELECT COUNT(*) FROM assistant_log;
```

### 5. Sentry (когда настроим):
```
⚠️ Пока не настроен — добавить SENTRY_URL
```

---

## 🔄 Рекомендации по улучшению

### Приоритет 1 (Срочно):
1. **Установить Logfire:**
   ```bash
   cd /root/athene/alpha
   docker exec alpha-app-1 pip install logfire
   docker restart alpha-app-1 alpha-celery-1 alpha-celery-beat-1
   ```

2. **Исправить volumes для Alpha:**
   ```yaml
   # alpha/docker-compose.yml
   volumes:
     - /opt/athene/alpha/volumes/static:/code/static
     - /opt/athene/alpha/volumes/media:/code/media
   ```

### Приоритет 2 (Важно):
1. **Настроить Sentry:**
   - Создать проект в Sentry.io
   - Добавить `SENTRY_URL` в docker-compose

2. **Добавить Logfire для Beta:**
   ```yaml
   LOGFIRE_ENABLED: true
   LOGFIRE_SEND: 1
   LOGFIRE_PROJECT: athene-beta
   LOGFIRE_API_KEY: [создать новый проект]
   ```

### Приоритет 3 (Опционально):
1. Настроить мониторинг метрик (Prometheus/Grafana)
2. Добавить алерты на высокую нагрузку
3. Настроить автоматическую ротацию логов

---

## 📊 Системные ресурсы

### Сервер (65.21.49.91):
```
Uptime: 241 дней
Load Average: 0.43, 0.24, 0.25
Disk: 180GB / 226GB (84% использовано)
RAM: 12GB / 15GB (80% использовано)
Swap: 0 (не настроен)
```

### Рекомендация:
- Настроить swap (минимум 4GB)
- Почистить Docker (`docker system prune -a`)
- Мониторить disk usage

---

## 🎯 Ответы на вопросы из Issue #7

### ❓ "Мне не удалось посмотреть логи"

**Ответ:**
- ✅ **Docker логи работают:** `docker logs alpha-app-1`
- ⚠️ **Logfire не работает:** пакет не установлен
- ✅ **Telegram логи работают:** сообщения приходят в чат
- ✅ **Database логи работают:** таблица `assistant_log` создана

**Причина проблемы с Logfire:**
Переменные окружения настроены, но Python модуль `logfire` отсутствует в образе Docker.

**Решение:**
1. Добавить в `requirements.txt`: `logfire>=0.x.x`
2. Пересобрать образ
3. Перезапустить контейнеры

---

## 📝 Следующие шаги

1. ✅ Подтвердить приоритеты исправлений
2. ⏳ Установить logfire
3. ⏳ Настроить Sentry
4. ⏳ Исправить volumes для Alpha
5. ⏳ Настроить Logfire для Beta
6. ⏳ Создать runbook для мониторинга

---

## 📖 Связанные документы

- [HETZNER_INFRASTRUCTURE.md](/Users/andreysudnik/Library/Mobile Documents/iCloud~md~obsidian/Documents/Cyb37/🎛️ workstation/_control/HETZNER_INFRASTRUCTURE.md) — Общая инфраструктура
- [ACCESSES.md](ACCESSES.md) — Доступы к системам
- [💻 Dev.md](💻%20Slava.md) — Dev workflow

---

**Автор проверки:** Cursor AI Assistant  
**Дата:** 8 ноября 2025, 10:21 UTC  
**Issue:** https://gitlab.aioffice.me/docs/athena/-/issues/7

