# Git Strategy ▸ Фидбэк

TL;DR ▸ cyb37_strategy repo ✅ оставить → history важна · оперативка в корне → iCloud only · Cursor видит всё · reference → в корне workspace.

---

## 🎯 Ответы на вопросы

### 1. Оставить cyb37_strategy repo?

**✅ ДА, абсолютно.**

**Почему:**
- Стратегия = долгоживущий документ → история изменений критична
- Видеть эволюцию thinking: "как менялись приоритеты · roadmap · бизнес-модель"
- Публичный repo → можно делиться с партнёрами · инвесторами · клиентами
- Git diff → "что изменилось в strategy с последнего квартала?"

**Что туда:**
```
strategy/
├── .git/
├── README.md               # Навигация
├── strategy.md             # North Star
├── business_model.md       # Монетизация
├── products.md             # Продуктовая стратегия
├── roadmap.md              # План развития
├── projects.md             # Текущие проекты
└── community.md            # AI Compas стратегия
```

**Что НЕ туда:**
- ❌ Оперативные заметки (notes_2025_10_18.md)
- ❌ Временные action items
- ❌ Черновики встреч

---

### 2. Оперативный контур: где хранить?

**Концепция:**
```
Git repos          →  Долгоживущие · History matters
iCloud корень      →  Оперативка · Временные файлы · Удаляются после исполнения
```

**В корне workspace (iCloud only):**
```
Cyb37/
├── notes/                           # Оперативные заметки
│   ├── notes_2025_10_18.md         # → архив через месяц
│   └── notes_2025_10_19.md         # → удалить после обработки
├── WORKSPACE_MANIFEST.md            # Живой документ (не в Git)
├── WORKSPACE_FEEDBACK_2025_10_18.md # Temp → архив
├── GIT_AND_WORKSPACE_SETUP.md       # Может в AI Dev repo
├── WORKSPACE_MIGRATION_PLAN.md      # Temp → delete after done
└── .gitignore                       # Исключить оперативку
```

**Правило:**
- История нужна? → Git repo
- Временный файл? → Корень workspace → iCloud → delete after use
- Живой документ (часто меняется)? → Смотря по контексту

---

### 3. Cursor Workspace: видит ли корневые файлы?

**✅ ДА, отлично видит.**

**Как работает:**
```json
// cyb37_workspace.code-workspace
{
  "folders": [
    {"path": "AI Dev", "name": "🧠 AI Dev"},        // Git repo
    {"path": "strategy", "name": "🎯 Strategy"},    // Git repo
    {"path": "notes", "name": "📝 Notes"},          // Просто папка
    {"path": ".", "name": "📦 Root"}                // Корень workspace
  ]
}
```

**Cursor видит:**
- ✅ Все Git repos (с панелью Git для каждого)
- ✅ Папки без Git (просто файлы)
- ✅ Файлы в корне workspace (если добавить `"path": "."`)

**Git panel показывает:**
- AI Dev → Git status для AI Dev/.git/
- strategy → Git status для strategy/.git/
- notes → нет Git (просто iCloud sync)
- Root → нет Git (или можешь добавить корневой .gitignore)

---

### 4. Reference: где хранить?

**Вариант A: В корне workspace (рекомендую)**
```
Cyb37/
├── reference/
│   ├── company_requisites.md          # Реквизиты ИП
│   ├── company_requisites_teleeng_ru.md
│   ├── teleeng_competencies_letter.md
│   └── README.md
└── .gitignore    # Добавить: reference/ (не пушить реквизиты публично)
```

**Почему:**
- Reference = редко меняется → история не критична
- Содержит private данные (реквизиты) → лучше НЕ в публичном Git
- iCloud backup достаточно
- Доступно из всех проектов в Cursor Workspace

**Вариант B: Отдельный private repo (overkill)**
```
sudnik008/cyb37-reference (private)
```
- Плюс: История + Git backup
- Минус: Лишний repo для редко меняющихся данных

**Вывод:** Вариант A (в корне, iCloud only, .gitignore).

---

## 🗂️ Итоговая структура

### Git Repos (история важна)

**Личные repos (@sudnik008):**
| Repo | Что | Почему Git | Публичность |
|------|-----|------------|-------------|
| **ai-dev** | Фреймворк управления · guidelines · templates | История методологии | 🌐 Public |
| **cyb37-strategy** | Стратегия · roadmap · бизнес-модель | Эволюция thinking | 🌐 Public |
| **ai-compas** | Community platform | История процессов | 🌐 Public |

**Организация (@teleengco):**
- assistant · billing · team_docs · vecv2 (code + docs)

### Корень workspace (iCloud only)

**Оперативный контур:**
```
Cyb37/                              # Workspace root
├── 📁 AI Dev/                      # → Git repo
├── 📁 strategy/                    # → Git repo
├── 📁 AI compas/                   # → Git repo
├── 📁 teleeng_assistant/           # → Git repo
├── 📁 team_docs/                   # → Git repo
├── 📁 reference/                   # iCloud only (реквизиты)
├── 📁 notes/                       # iCloud only (оперативка)
│
├── WORKSPACE_MANIFEST.md           # Живой документ (может в ai-dev?)
├── cyb37_workspace.code-workspace  # Cursor config
├── .gitignore                      # Исключить оперативку
│
└── 📁 archive/                     # Старые проекты (iCloud)
```

**Что в .gitignore (корневой):**
```gitignore
# Оперативные файлы
notes/
*_FEEDBACK_*.md
*_MIGRATION_*.md
*temp*.md

# Приватные данные
reference/
My_office/
tax_audit/

# Системные файлы
.DS_Store
*.tmp
```

---

## 🎬 Рекомендация: структура

### Уровень 1: Git repos (долгоживущие)

**ai-dev (public):**
```
AI Dev/
├── .git/
├── README.md
├── PROJECT_INDEX.md
├── CHANGELOG.md
├── documentation_driven_management.md
├── guidelines/
│   ├── OnePager_Style_Guide.md
│   ├── action_items_standard.md
│   └── ...
├── templates/
│   └── action_item_template.md
└── scripts/
    └── aggregate_tasks.py
```

**cyb37-strategy (public):**
```
strategy/
├── .git/
├── README.md
├── strategy.md
├── business_model.md
├── products.md
├── roadmap.md
├── projects.md
└── community.md
```

**ai-compas (public):**
```
AI compas/
├── .git/
├── README.md
├── passport.md
├── _core/
└── communities/
```

### Уровень 2: Workspace root (оперативка)

**reference/ (iCloud only):**
- Реквизиты · справочники
- НЕ пушить в Git (private data)

**notes/ (iCloud only):**
- Оперативные заметки
- Удаляются через 1-2 месяца или после обработки

**Root files:**
- WORKSPACE_MANIFEST.md → живой документ (решить: в ai-dev или корень?)
- cyb37_workspace.code-workspace → Cursor config
- .gitignore → исключить оперативку

---

## 🤔 Решить: WORKSPACE_MANIFEST.md

**Вопрос:** Где хранить манифест?

**Вариант A: В ai-dev repo**
- Плюс: История изменений структуры workspace
- Плюс: В публичном repo (показать другим)
- Минус: Нужно переместить из корня

**Вариант B: В корне workspace (iCloud)**
- Плюс: Живой документ, быстрые изменения
- Плюс: Уже там, не перемещать
- Минус: Нет истории Git

**Рекомендация:**
```
Вариант A: WORKSPACE_MANIFEST.md → в ai-dev repo
Причина: История структуры workspace = часть фреймворка управления
```

**Как:**
```bash
mv WORKSPACE_MANIFEST.md "AI Dev/"
cd "AI Dev"
git add WORKSPACE_MANIFEST.md
git commit -m "docs: add workspace manifest"
git push origin main
```

**Ссылка из корня:**
```markdown
<!-- В корне: README.md -->
# Cyb37 Workspace

📍 **Манифест:** [AI Dev/WORKSPACE_MANIFEST.md](./AI%20Dev/WORKSPACE_MANIFEST.md)
🎬 **Старт:** [Cursor Workspace](./cyb37_workspace.code-workspace)
```

---

## ✅ Финальная рекомендация

### Git repos (создать/оставить)
1. ✅ **ai-dev** (public) — уже есть, синхронизирован
2. ✅ **cyb37-strategy** (public) — создал, НЕ удалять, заполнить
3. ✅ **ai-compas** (public) — уже есть, синхронизировать

### Корень workspace (iCloud only)
4. ✅ **reference/** — оставить в корне, добавить в .gitignore
5. ✅ **notes/** — оставить в корне, добавить в .gitignore
6. ✅ **WORKSPACE_MANIFEST.md** → переместить в ai-dev repo
7. ✅ **.gitignore** — создать в корне workspace

### Action Items
- [ ] Переместить WORKSPACE_MANIFEST.md в AI Dev/
- [ ] Создать .gitignore в корне Cyb37/
- [ ] Заполнить cyb37-strategy repo (перенести файлы из strategy/)
- [ ] Инициализировать Git в strategy/ → push в cyb37-strategy
- [ ] Обновить cyb37_workspace.code-workspace (добавить "." для root)

---

## 📊 Cursor Workspace: как настроить

**Добавить root folder:**
```json
{
  "folders": [
    {"path": "AI Dev", "name": "🧠 AI Dev"},
    {"path": "strategy", "name": "🎯 Strategy"},
    {"path": "AI compas", "name": "🧭 AI Compas"},
    {"path": "teleeng_assistant", "name": "📦 Assistant"},
    {"path": "team_docs", "name": "📚 TeamDocs"},
    {"path": "reference", "name": "📋 Reference"},
    {"path": "notes", "name": "📝 Notes"},
    {"path": ".", "name": "📦 Workspace Root"}  // ← ДОБАВИТЬ
  ]
}
```

**Результат:**
- Cursor видит все Git repos (с Git panel)
- Cursor видит reference/ notes/ (без Git)
- Cursor видит root files (WORKSPACE_MANIFEST, .gitignore)

---

**Создано:** 18 октября 2025 17:00 МСК
**Решение:** cyb37-strategy ✅ оставить · reference в корне · notes в корне · WORKSPACE_MANIFEST → в ai-dev




