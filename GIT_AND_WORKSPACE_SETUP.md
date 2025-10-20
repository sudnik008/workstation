# Git & Cursor Workspace ▸ Setup Guide

TL;DR ▸ Git локально = `.git/` папка · 1 папка = 1 repo · Cursor Workspace ✅ есть → добавить 5 папок из манифеста.

---

## 🗂️ Как работает Git локально

### Структура
```
AI Dev/                           # Твоя папка (видима)
├── .git/                         # Скрытая папка Git (мозг репозитория)
│   ├── config                    # Настройки (remote, branch)
│   ├── objects/                  # История всех изменений
│   ├── refs/                     # Ветки (main, feature-X)
│   └── HEAD                      # Указатель на текущую ветку
├── README.md                     # Твои файлы
├── guidelines/
└── ...
```

### Как это работает

**Локально (твой Mac):**
```
.git/ папка = вся история + настройки
↓
git add → помещает изменения в staging
↓
git commit → сохраняет snapshot в .git/objects/
↓
Всё хранится локально в .git/
```

**Связь с GitHub:**
```
git remote = адрес GitHub репозитория
origin = https://github.com/sudnik008/ai-dev.git
↓
git push → отправляет коммиты из .git/ → GitHub
git pull → скачивает коммиты из GitHub → .git/
↓
Синхронизация: локал ↔ GitHub
```

### Правило: 1 папка = 1 Git репозиторий

**✅ ПРАВИЛЬНО:**
```
Cyb37/
├── AI Dev/
│   └── .git/              # Отдельный Git repo для AI Dev
├── strategy/
│   └── .git/              # Отдельный Git repo для strategy
├── teleeng_assistant/
│   └── .git/              # Отдельный Git repo для assistant
└── team_docs/
    └── .git/              # Отдельный Git repo для team_docs
```

**❌ НЕПРАВИЛЬНО:**
```
Cyb37/
├── .git/                  # Один Git на всё → хаос
├── AI Dev/
├── strategy/
└── ...
```

**Почему?**
- Каждый проект = отдельная история
- Разные remote (GitHub repos)
- Независимые коммиты
- Проще управлять

### Где живет Git?

**Локально:**
- `.git/` папка внутри каждого проекта
- Скрытая (начинается с точки)
- Смотреть: `ls -la | grep .git`

**На GitHub:**
- Remote сервер
- URL в `.git/config`: `origin = https://github.com/user/repo.git`
- Push/Pull синхронизирует

**iCloud:**
- Синхронизирует ВСЁ (включая `.git/`)
- Может быть проблемой (конфликты при sync)
- Лучше: Git repos НЕ в iCloud (но у тебя workspace там, терпимо)

---

## 🖥️ Cursor Workspace: текущее состояние

### Проверка
**Файл:** `cyb37_workspace.code-workspace` ✅ существует

**Текущие папки (8):**
1. 📦 Assistant (teleeng_assistant)
2. 🔍 VecV2
3. 🏛️ Athene
4. 💳 Billing
5. 📚 TeamDocs (team_docs)
6. 🧠 AI Dev
7. 🧭 AI Compas
8. 📋 Reference

**Не хватает (из манифеста):**
- ❌ strategy (бизнес-модель)
- ❌ фосагро (B2B project)
- ❌ tax_audit (internal)
- ❌ My_office (B2B project)
- ❌ notes (оперативка)
- ❌ teleeng_school (product)
- ❌ archive (история)

### Что дает Cursor Workspace?

**Плюсы:**
- ✅ Все проекты в одном окне (sidebar)
- ✅ Единые настройки (formatOnSave, rulers)
- ✅ Git autorefresh → видишь изменения сразу
- ✅ Terminal открывается в контексте файла
- ✅ Search по всем проектам одновременно

**Как работает:**
```
Cursor открывает workspace файл
↓
Загружает все папки из "folders": []
↓
Каждая папка = отдельный Git repo (если .git есть)
↓
Git panel показывает изменения по всем repos
↓
Commit/Push можно делать для каждого отдельно
```

---

## 🎯 AI Dev: текущий статус

**Git status (только что проверил):**
```
Branch: main
Remote: origin → https://github.com/sudnik008/ai-dev.git
Status: Up to date with origin/main

Uncommitted changes:
- Modified: guidelines/OnePager_Style_Guide.md
- Untracked: guidelines/terminology_and_architecture.md
- Untracked: guidelines/workspace_setup.md
- Untracked: meeting_dev_2025_10_16.md
```

**Что это значит:**
- ✅ Git настроен правильно
- ✅ Remote связан с GitHub
- ⚠️ Есть локальные изменения (не на GitHub)

**Что делать:**
```bash
cd "AI Dev"
git add .
git commit -m "docs: update guidelines + meeting notes"
git push origin main
```

**Результат:** Локальные изменения → GitHub (синхронизированы).

---

## ✅ Action Plan: Setup

### 1) Коммит AI Dev changes (сейчас)
```bash
cd "/Users/andreysudnik/Library/Mobile Documents/iCloud~md~obsidian/Documents/Cyb37/AI Dev"
git add guidelines/OnePager_Style_Guide.md
git add guidelines/terminology_and_architecture.md
git add guidelines/workspace_setup.md
git add meeting_dev_2025_10_16.md
git commit -m "docs: update OnePager guide + new guidelines + meeting notes"
git push origin main
```

### 2) Обновить Cursor Workspace (добавить папки)
```bash
cd "/Users/andreysudnik/Library/Mobile Documents/iCloud~md~obsidian/Documents/Cyb37"
# Открыть cyb37_workspace.code-workspace в Cursor
# Добавить в "folders": []
```

**Добавить:**
```json
{
  "path": "strategy",
  "name": "🎯 Strategy"
},
{
  "path": "notes",
  "name": "📝 Notes"
},
{
  "path": "фосагро",
  "name": "🏭 Fosagro"
},
{
  "path": "tax_audit",
  "name": "💼 Tax Audit"
},
{
  "path": "My_office",
  "name": "🏢 My Office"
},
{
  "path": "teleeng_school",
  "name": "🎓 Teleeng School"
}
```

### 3) Инициализировать Git в strategy (если нужен remote)
```bash
cd strategy
git init
git add .
git commit -m "init: business strategy docs"
git remote add origin git@github.com:sudnik008/cyb37-strategy.git
git push -u origin main
```

---

## 🛡️ Как не запутаться в Git

### Правила
1. **1 папка = 1 .git/** → не будет конфликтов
2. **Перед работой:** `git status` → видишь текущее состояние
3. **После изменений:** `git add . && git commit && git push` → синхронизация
4. **Cursor Git panel** → показывает все repos в workspace

### Проверка "где живет Git?"
```bash
# В любой папке:
ls -la | grep .git

# Если видишь:
drwxr-xr-x  .git/
# → Это Git репозиторий

# Проверить remote:
git remote -v
# → Видишь GitHub URL
```

### Если запутался
```bash
# Проверить статус:
git status

# Увидеть историю:
git log --oneline

# Проверить remote:
git remote -v

# Если что-то сломалось:
git fetch origin
git reset --hard origin/main  # ОСТОРОЖНО: удалит локальные изменения
```

---

## 🎬 Начать сейчас

**Шаг 1:** Коммит AI Dev
```bash
cd "AI Dev"
git add .
git commit -m "docs: sync workspace updates"
git push origin main
```

**Шаг 2:** Обновить workspace
- Открыть `cyb37_workspace.code-workspace`
- Добавить 6 папок (strategy, notes, фосагро, tax_audit, My_office, teleeng_school)
- Сохранить
- Перезапустить Cursor workspace

**Результат:** Все проекты в одном окне · Git настроен · работа из Cursor.

---

**Создано:** 18 октября 2025
**Статус:** Ready to execute
**Риск:** Нет (AI Dev уже настроен правильно)


