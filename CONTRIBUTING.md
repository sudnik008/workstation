# Contributing to Workstation

## 🎯 Философия проекта

**Workstation** — это инструмент управления multi-project workspace. Цель: помочь другим разработчикам организовать свой workspace из множества проектов.

---

## 📂 Структура репозитория

### Что В Git (публично)
- ✅ `README.md` — описание проекта
- ✅ `_control/WORKSPACE_MANIFEST.md` — пример карты проектов
- ✅ Гайды: `GIT_AND_WORKSPACE_SETUP.md`, `WORKSPACE_MIGRATION_PLAN.md`
- ✅ `reference/` — справочные материалы

### Что НЕ в Git (локально)
- ❌ `🚩 todo.md` — персональные задачи
- ❌ `💻 Dev.md` — dev-заметки
- ❌ `notes_*.md` — операционные заметки

**Правило:** Структурные документы → GitHub | Приватные данные → локально

---

## 🛠️ Workflow

### Обновление документации
```bash
# 1. Перейти в workstation
cd "/path/to/workstation"

# 2. Проверить статус
git status

# 3. Добавить изменения
git add README.md _control/WORKSPACE_MANIFEST.md

# 4. Коммит
git commit -m "docs: update manifest with new projects"

# 5. Push
git push origin main
```

### Добавление новых гайдов
```bash
# Создать файл
echo "# New Guide" > NEW_GUIDE.md

# Добавить в Git
git add NEW_GUIDE.md
git commit -m "docs: add NEW_GUIDE"
git push origin main
```

### Обновление манифеста
```bash
# Редактировать манифест
vim _control/WORKSPACE_MANIFEST.md

# Проверить изменения
git diff _control/WORKSPACE_MANIFEST.md

# Коммит
git add _control/WORKSPACE_MANIFEST.md
git commit -m "docs: update manifest - add PROJECT_NAME"
git push origin main
```

---

## 🚫 Что НЕ коммитить

**Приватные данные:**
- Персональные TODO-листы
- Заметки с упоминанием клиентов
- API ключи, пароли
- Финансовая информация

**Временные файлы:**
- `*.tmp`, `*.bak`
- `notes_*.md` (операционные заметки)

**Системные файлы:**
- `.DS_Store`, `.idea/`, `.vscode/`

См. [`.gitignore`](./.gitignore) для полного списка.

---

## 📝 Стиль коммитов

Используем [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>: <subject>

<body> (optional)
```

### Types
- `docs:` — документация (README, гайды, манифест)
- `feat:` — новая функциональность (новые гайды, структуры)
- `fix:` — исправления
- `chore:` — maintenance (обновление .gitignore)

### Примеры
```bash
git commit -m "docs: update README with quick start"
git commit -m "feat: add CONTRIBUTING guide"
git commit -m "docs: update manifest - add vecv2 project"
git commit -m "chore: update .gitignore - exclude dev notes"
```

---

## 🔒 Приватность

**Манифест содержит публичную информацию:**
- Названия проектов (без деталей реализации)
- Технологический стек (общедоступный)
- Публичные URL (если есть)

**НЕ включать:**
- Внутренние URL (staging, dev)
- Детали клиентов (кроме публично доступных)
- Коммерческую информацию

---

## 🎬 Первый раз?

### Клонировать репозиторий
```bash
git clone git@github.com:sudnik008/workstation.git
cd workstation
```

### Настроить локальные файлы
```bash
# Создать персональные файлы (не будут в Git)
touch "🚩 todo.md"
touch "💻 Dev.md"
```

### Проверить .gitignore
```bash
git status
# Должны видеть только tracked files
# todo.md и dev.md НЕ должны появиться
```

---

## ✅ Чек-лист перед push

- [ ] `git status` — проверить, какие файлы изменены
- [ ] Убедиться, что нет приватных данных
- [ ] Коммит следует Conventional Commits
- [ ] `git push origin main` — отправить изменения

---

## 🤝 Вопросы?

Создайте [Issue](https://github.com/sudnik008/workstation/issues) или [Discussion](https://github.com/sudnik008/workstation/discussions).

---

**Спасибо за интерес к проекту!**

