# Cyb37 Workspace ▸ Фидбэк & Реорганизация

TL;DR ▸ 12 проектов · логика ≠ структура · 80% без Git · ACTION_ITEMS только в 2 проектах · Doc-Framework внедрён 8% → нужна реорганизация + Git setup + единый формат задач.

---

## 🧠 Что у тебя есть

### Логическая модель (манифест)
**[WORKSPACE_MANIFEST.md](./WORKSPACE_MANIFEST.md)** ✅ отлично отражает твою ментальную карту:
- 7 бизнес-юнитов чётко структурированы
- Products · B2B Projects · Community · Internal · Research
- Приоритеты (P0-P2) · статусы · публичные ссылки

### Физическая структура
**iCloud Drive workspace** = смесь кода + docs + проектов:
- Код: `teleeng_assistant/` `Billing/` `athene/` `teleeng_school/` `vecv2/`
- Документация: `team_docs/` `AI compas/` `strategy/` `reference/`
- Meta: `AI Dev/` (фреймворки) · `notes/` (оперативка)
- Legacy: `archive/frank_rg/` `media/` `webpages/`

### Фреймворк управления
**Documentation-Driven Management** ✅ создан и работает:
- Pilot на `teleeng_assistant` завершён успешно
- Структура: `operations/` → `playbooks/` `incidents/` `support_cases/`
- Incident Reports · ACTION_ITEMS · Playbooks tested
- OnePager Style Guide → компактный формат

---

## ⚠️ Гэпы: логика vs физика

| Проблема | Факты | Влияние |
|----------|-------|---------|
| **Манифест ≠ файлы** | 7 Units в манифесте · 20+ папок на диске | Путаница · поиск сложен |
| **Git отсутствует** | 80% проектов без репозитория | Нет истории · нет backup |
| **ACTION_ITEMS хаос** | Только в 2 проектах · формат текст + таблицы | Задачи теряются · не агрегируются |
| **Doc-Framework 8%** | Применён 1 из 12 проектов | Несистематичность |
| **AGENT_INSTRUCTIONS** | Только в 2 проектах (_private/) | AI работает вслепую 83% случаев |

---

## 🎯 Структура Git repositories

### Текущее состояние

**GitHub @sudnik008 (личный):**
- ✅ `ai_compas` — есть
- ✅ `lp_sphie_ai` — есть
- ✅ `hr` — есть
- ❌ `ai-dev` — нет (указан в манифесте!)

**GitHub @teleengco (организация):**
- 🔒 Все приватные (0 public)
- ✅ `assistant` `billing` `vecv2` `team_docs` — есть (предположительно)
- ❌ Публичных репозиториев нет

### Что создать

#### Личные репозитории (@sudnik008)

| Репозиторий | Синхронизация | Тип | Публичность |
|-------------|---------------|-----|-------------|
| **ai-dev** | `AI Dev/` → full | Meta | 🌐 Public |
| **cyb37-strategy** | `strategy/` → full | Docs | 🌐 Public |
| **cyb37-reference** | `reference/` → full | Docs | 🔒 Private |
| **ai-compas** | `AI compas/` → update | Community | 🌐 Public |

#### Организация (@teleengco)

| Репозиторий | Синхронизация | Тип | Публичность |
|-------------|---------------|-----|-------------|
| **team_docs** | `team_docs/` → sync | Docs | 🔒 Private |
| **assistant** | `teleeng_assistant/` | Code+Docs | 🔒 Private |
| **billing** | `Billing/` | Code+Docs | 🔒 Private |
| **vecv2** | `vecv2/` | Code | 🔒 Private |
| **teleeng_school** | `teleeng_school/` | Code+Docs | 🔒 Private |
| **athene** | `athene/` | Code+Docs | 🔒 Private |

#### B2B Projects (если нужна публикация)

| Проект | Где сейчас | Git strategy |
|--------|-----------|--------------|
| **SPG projects** | `team_docs/spg/` | Остаются в `team_docs` (private) |
| **Фосагро** | `фосагро/` | Остаётся локально (NDA) |
| **My Office** | `My_office/` | Остаётся локально (contracts) |

---

## 📋 Проблема: Action Items

### Текущий формат

**teleeng_assistant/docs/operations/ACTION_ITEMS.md:**
```markdown
## ❓ Questions for Dev
| Q | Context | Priority |
|---|---------|----------|
| Где Payment DB? | [Link] | 🔴 |

## 🔴 P0 ▸ Critical
| Task | Est | Owner | Trigger |
|------|-----|-------|---------|
| Payment DB docs | 2-3h | AS | Support case |
```

**Формат:** markdown таблицы + текст + эмодзи-приоритеты

### Плюсы
✅ Читаемо человеком
✅ Контекст сохранён
✅ Ссылки на триггеры (incidents/support cases)

### Минусы
❌ Не машиночитаемо (парсинг сложен)
❌ Не агрегируется между проектами
❌ Нет уникальных ID задач
❌ Нет timestamps создания/обновления
❌ Нет integration с внешними tools (GitHub Issues, Linear, etc)

---

## 💡 Решение: гибридный подход

### Option A: Markdown + Frontmatter (рекомендую)

**Формат:**
```yaml
---
type: action_item
project: teleeng_assistant
id: ta-2025-10-15-001
priority: P0
status: open
created: 2025-10-15T02:30+03:00
owner: AS
estimate: 2-3h
tags: [payment, db, support_case]
trigger: support_cases/2025_10_15_user_balance_investigation.md
---

# Payment DB docs

## Context
[Support case](./support_cases/2025_10_15_user_balance_investigation.md) TG:435536141

## Tasks
- [ ] Спросить dev: где БД платежей, доступы (15′, AS)
- [ ] Update ACCESSES.md → Payment DB section (30′, AS)
- [ ] Create playbook: check_user_payments.md (1h, AS)
```

**Преимущества:**
- ✅ Машиночитаемо (YAML frontmatter)
- ✅ Человекочитаемо (markdown body)
- ✅ Уникальные ID → агрегация
- ✅ Timestamps → сортировка, фильтрация
- ✅ Поддержка в Obsidian, Cursor, GitHub
- ✅ Можно написать скрипт `aggregate_tasks.py` → собрать все задачи

### Option B: Database (SQLite/JSON)

**Формат:** SQLite БД `cyb37_tasks.db`

```sql
CREATE TABLE tasks (
  id TEXT PRIMARY KEY,
  project TEXT,
  priority TEXT,
  status TEXT,
  title TEXT,
  owner TEXT,
  estimate TEXT,
  created_at TEXT,
  updated_at TEXT,
  trigger_file TEXT,
  context TEXT
);
```

**Преимущества:**
- ✅ Полностью машиночитаемо
- ✅ SQL queries для агрегации
- ✅ Можно интегрировать с UI

**Недостатки:**
- ❌ Не Git-friendly (binary)
- ❌ Требует отдельный tool для управления
- ❌ Сложнее редактировать вручную

### Option C: Keep As Is + Scripts

**Рекомендация:** НЕ делай.

**Причина:** У тебя не так много задач. Текущий формат работает для 1 проекта, но не масштабируется. Markdown + Frontmatter — золотая середина.

---

## 🎬 Cursor Workflow интеграция

### Как будет работать

**1. При работе с проектом:**
```
Cursor открывает проект → читает AGENT_INSTRUCTIONS.md
↓
Находит ACTION_ITEMS.md (или action_items/*.md)
↓
Парсит YAML frontmatter → показывает задачи
↓
AI предлагает: "Вижу 3 открытые P0 задачи. Хочешь начать?"
```

**2. При завершении задачи:**
```
AI обновляет task file:
  status: open → completed
  completed_at: 2025-10-18T14:30+03:00
↓
Добавляет в ✅ Done секцию ACTION_ITEMS.md
↓
Git commit: "tasks: complete ta-2025-10-15-001 Payment DB docs"
```

**3. Агрегация задач:**
```bash
# Скрипт: aggregate_tasks.py
python aggregate_tasks.py --owner AS --priority P0 --status open

# Output:
PROJECT               ID                  PRIORITY  TITLE
teleeng_assistant     ta-2025-10-15-001   P0        Payment DB docs
team_docs             td-2025-10-16-003   P0        Apply Doc-Framework to spg
AI_compas             ac-2025-10-17-002   P1        Launch HR community
```

---

## 🏗️ Реорганизация: план действий

### Phase 1: Git Setup (1-2 дня)

**Создать репозитории:**

1. **@sudnik008/ai-dev** (public)
   ```bash
   cd "AI Dev"
   git init
   git add .
   git commit -m "init: AI Dev meta-documentation"
   git remote add origin git@github.com:sudnik008/ai-dev.git
   git push -u origin main
   ```

2. **@sudnik008/cyb37-strategy** (public)
   ```bash
   cd strategy
   git init
   git add .
   git commit -m "init: Cyb37 business strategy"
   git remote add origin git@github.com:sudnik008/cyb37-strategy.git
   git push -u origin main
   ```

3. **Update @sudnik008/ai-compas** (public)
   ```bash
   cd "AI compas"
   git remote -v  # Check existing remote
   git pull origin main  # Sync
   git add .
   git commit -m "sync: update from local workspace"
   git push origin main
   ```

**Не трогать (пока):**
- `team_docs/` → уже в Git (@teleengco/team_docs)
- `teleeng_assistant/` → уже в Git (@teleengco/assistant)
- `Billing/` → уже в Git (@teleengco/billing)

---

### Phase 2: Doc-Framework масштабирование (2-4 недели)

**Приоритеты по PROJECT_INDEX.md:**

| Проект | Приоритет | Срок | Действия |
|--------|-----------|------|----------|
| **team_docs/spg** | P1 | 1 нед | operations/ · ACTION_ITEMS · playbooks |
| **team_docs/hub** | P1 | 1 нед | operations/ · ACTION_ITEMS |
| **AI compas** | P1 | 2 нед | playbooks для Host/Initiator |
| **фосагро** | P2 | 3 нед | operations/ (когда активизируется) |

**Для каждого проекта:**
1. Создать `operations/` структуру
2. Создать `ACTION_ITEMS.md` (новый формат с frontmatter)
3. Создать 2-3 базовых playbooks
4. Добавить `AGENT_INSTRUCTIONS.md` в `_private/`

---

### Phase 3: Task Management система (1 неделя)

**1. Стандартизировать формат:**
- Создать template: `AI Dev/templates/action_item_template.md`
- Документировать в `AI Dev/guidelines/action_items_format.md`
- Обновить OnePager Style Guide

**2. Создать скрипт агрегации:**
```python
# AI Dev/scripts/aggregate_tasks.py
# Читает все **/ACTION_ITEMS.md
# Парсит YAML frontmatter
# Выводит таблицу задач с фильтрами
```

**3. Интеграция в Cursor:**
- Добавить в `.cursorrules` workspace правило чтения ACTION_ITEMS
- AI автоматически предлагает задачи при старте

**4. Миграция существующих:**
- `teleeng_assistant/docs/operations/ACTION_ITEMS.md` → новый формат
- `Billing/docs/operations/ACTION_ITEMS.md` → новый формат

---

### Phase 4: AGENT_INSTRUCTIONS покрытие (2 недели)

**Где нужно создать:**

| Проект | Локация | Приоритет |
|--------|---------|-----------|
| **AI Dev** | `AI Dev/_private/AGENT_INSTRUCTIONS.md` | P1 |
| **AI compas** | `AI compas/_private/AGENT_INSTRUCTIONS.md` | P1 |
| **strategy** | `strategy/_private/AGENT_INSTRUCTIONS.md` | P2 |
| **team_docs/hub** | `team_docs/hub/_private/AGENT_INSTRUCTIONS.md` | P1 |
| **фосагро** | `фосагро/_private/AGENT_INSTRUCTIONS.md` | P2 |

**Template:**
```markdown
# AGENT_INSTRUCTIONS.md — [Название проекта]

## 🎯 Контекст проекта
[1-2 абзаца о проекте]

## 📁 Структура
[Описание папок и файлов]

## 🔒 Правила работы
- Что можно менять
- Что нельзя трогать
- Git workflow

## 🔗 Связанные документы
[Ссылки на README, паспорты, guidelines]
```

---

## 🚦 Рекомендации по реорганизации

### Что делать СЕЙЧАС (высокий ROI)

1. **Git setup для ai-dev** → твой фреймворк должен быть публичным
2. **Стандартизировать ACTION_ITEMS** → единый формат с frontmatter
3. **Создать aggregate_tasks.py** → видеть все задачи в одном месте

### Что делать ПОТОМ (после 1-2 недель)

4. **Масштабировать Doc-Framework** → team_docs/spg, hub
5. **Добавить AGENT_INSTRUCTIONS** → покрытие 100% проектов
6. **Синхронизировать ai-compas** → push updates в GitHub

### Что НЕ ДЕЛАТЬ (не тратить время)

❌ **Не создавай базу данных для задач** → overkill для твоего объёма
❌ **Не реструктурируй папки** → текущая структура workable, проблема не в ней
❌ **Не переименовывай всё** → WORKSPACE_MANIFEST.md решает проблему логики vs физики

---

## 📊 Метрики успеха

**Через 1 месяц:**
- Git: 5+ репозиториев created/synced
- ACTION_ITEMS: единый формат в 4+ проектах
- AGENT_INSTRUCTIONS: покрытие 8+ проектов (67%)
- Doc-Framework: применён на 3+ проектах (25%)

**Через 3 месяца:**
- Doc-Framework: 80% активных проектов
- Задачи: агрегация работает, AI предлагает tasks
- Playbooks: 20+ across projects
- Incident Reports: 5+ documented

---

## 🎯 Следующие шаги

### Немедленно (сегодня-завтра)

1. **Создать Git repository:** `@sudnik008/ai-dev`
   - Init · add remote · push
   - README.md update с ссылкой на GitHub

2. **Стандартизировать ACTION_ITEMS:**
   - Создать `AI Dev/guidelines/action_items_format.md`
   - Обновить template в `AI Dev/templates/`
   - Мигрировать 1 проект (teleeng_assistant) → проверить формат

3. **AGENT_INSTRUCTIONS для AI Dev:**
   - Создать `AI Dev/_private/AGENT_INSTRUCTIONS.md`
   - Описать структуру фреймворка

### На этой неделе

4. **Создать aggregate_tasks.py:**
   - Парсинг YAML frontmatter
   - Вывод таблицы
   - Фильтры: owner, priority, status, project

5. **Синхронизировать ai-compas:**
   - Pull from GitHub
   - Merge local changes
   - Push updates

6. **Применить Doc-Framework на team_docs/spg:**
   - operations/ structure
   - ACTION_ITEMS.md (новый формат)
   - 2-3 playbooks

### В следующие 2 недели

7. **Git setup:** cyb37-strategy (public)
8. **Doc-Framework:** team_docs/hub
9. **AGENT_INSTRUCTIONS:** AI compas, team_docs/hub
10. **Playbooks:** 5+ новых (deploy, debug, check_logs)

---

## 💬 Мои наблюдения

### Что работает отлично ✅

**WORKSPACE_MANIFEST.md:**
Идеальное решение для mental model vs physical structure. Не нужно реструктурировать файлы — манифест даёт логику.

**Documentation-Driven Management:**
Pilot успешен. teleeng_assistant показал: фреймворк работает, команда принимает, время решения инцидентов сократилось.

**OnePager Style:**
Сверхплотный формат = быстрое сканирование + низкая когнитивная нагрузка.

### Что нужно улучшить ⚠️

**Git coverage:**
80% проектов без репозитория → нет истории, нет backup, нет collaboration. Создай репозитории для ключевых проектов (ai-dev, strategy).

**ACTION_ITEMS формат:**
Текст + таблицы работают для 1 проекта, но не агрегируются. Markdown + YAML frontmatter → машиночитаемость + человекочитаемость.

**AGENT_INSTRUCTIONS coverage:**
Только 17% проектов имеют инструкции. AI работает вслепую. Покрой все активные проекты.

### Философия: не overkill 🎯

**База данных для задач?** НЕТ.
У тебя ~5-10 открытых задач одновременно. Markdown + frontmatter + простой Python скрипт = достаточно.

**Реструктурировать всё?** НЕТ.
Текущая структура workable. WORKSPACE_MANIFEST.md даёт mental map. Не трать время на переименование/перемещение.

**Cursor Workflow?** ДА.
Фокус на интеграции: AGENT_INSTRUCTIONS → AI читает контекст → ACTION_ITEMS → AI предлагает задачи → Playbooks → AI помогает выполнять.

---

**Создано:** 18 октября 2025, 10:30 МСК
**Для:** Андрей Судник · Cyb37 Workspace
**Действие:** Review → prioritize → execute

---

## 📎 Приложение: Референсные файлы

**Созданные фреймворки (используй как reference):**
- [AI Dev/README.md](./AI%20Dev/README.md)
- [AI Dev/documentation_driven_management.md](./AI%20Dev/documentation_driven_management.md)
- [AI Dev/PROJECT_INDEX.md](./AI%20Dev/PROJECT_INDEX.md)
- [AI Dev/guidelines/OnePager_Style_Guide.md](./AI%20Dev/guidelines/OnePager_Style_Guide.md)

**Примеры реализации (pilot):**
- [teleeng_assistant/docs/operations/](./teleeng_assistant/docs/operations/)
- [teleeng_assistant/docs/operations/ACTION_ITEMS.md](./teleeng_assistant/docs/operations/ACTION_ITEMS.md)
- [teleeng_assistant/docs/operations/playbooks/check_user_status.md](./teleeng_assistant/docs/operations/playbooks/check_user_status.md)

**GitHub repositories (проверь доступы):**
- https://github.com/sudnik008 — личные проекты
- https://github.com/orgs/teleengco/repositories — организация


