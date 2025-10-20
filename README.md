# 🎛️ Workstation — Центр управления Cyb37 Workspace

**GitHub:** [github.com/sudnik008/workstation](https://github.com/sudnik008/workstation)

Персональная рабочая станция для управления multi-project workspace. Содержит карту проектов, гайды по Git/Cursor, планы миграции и операционные документы.

---

## 📍 Что это такое?

**Workstation** — это центр управления workspace из 12+ проектов (продукты, B2B, платформы, сервисы).

Вместо разрозненных заметок и ToDo-списков — структурированная система управления:

- **`_control/WORKSPACE_MANIFEST.md`** — полная карта проектов (продукты, статусы, Git repos, публичные URL)
- **Гайды** — Git setup, Cursor workspace, миграции
- **Операционные файлы** — текущие задачи, заметки, черновики

---

## 🗺️ Структура

```
workstation/
├── _control/                    # Управляющие документы
│   └── WORKSPACE_MANIFEST.md    # Карта всех проектов Cyb37
│
├── README.md                    # Этот файл
├── GIT_AND_WORKSPACE_SETUP.md   # Гайд: Git + Cursor Workspace
├── WORKSPACE_MIGRATION_PLAN.md  # План миграции workspace
│
├── 🚩 todo.md                   # Текущие задачи (не в Git)
├── 💻 Dev.md                    # Dev-задачи (не в Git)
├── notes_*.md                   # Операционные заметки (не в Git)
│
└── reference/                   # Справочные материалы
```

**Git:** Структурные документы → GitHub | Временные файлы (todo, notes) → локально

---

## 🎯 Для чего это нужно?

### Проблема
Multi-project workspace (12+ проектов) без единой точки управления:
- Где искать проект? В каком репозитории?
- Какой статус? Кто владелец?
- Как настроить Git/Cursor для работы со всеми проектами?

### Решение
**Workstation** = единая точка входа:

1. **WORKSPACE_MANIFEST.md** — карта всех проектов (статусы, Git repos, публичные URL)
2. **Гайды** — Git & Cursor setup, миграции, best practices
3. **Операционка** — текущие задачи, заметки (локально, не в Git)

---

## 📚 Ключевые документы

| Документ                      | Назначение                                    |
| ----------------------------- | --------------------------------------------- |
| `_control/WORKSPACE_MANIFEST` | Карта проектов: 12+ projects, Git, статусы    |
| `GIT_AND_WORKSPACE_SETUP`     | Как настроить Git + Cursor для multi-project  |
| `WORKSPACE_MIGRATION_PLAN`    | План миграции workspace (3 фазы, 5 дней)      |
| `🚩 todo.md`                  | Текущие задачи (локально)                     |

---

## 🏗️ Cyb37 Workspace: обзор

**7 бизнес-юнитов · 2 платформы · 2 сервиса · 12+ проектов**

### 🚀 Продукты
- **Sophie AI** ([aioffice.me](https://aioffice.me)) — ИИ-ассистент в Telegram
- **Athene** (dev) — следующее поколение платформы
- **Teleeng School** — онлайн-школа английского

### 🏢 B2B Проекты
- **SPG** — 5 проектов (AI-бот, Fosagro, KP automation, Fintech Doc, KB)
- **My Office** — L&D automation
- **Mercury** — trade assistant
- И другие (см. WORKSPACE_MANIFEST)

### 🏗️ Инфраструктура
- **teleeng_assistant** — Django platform (4 instances)
- **Billing** — сервис биллинга (Stripe, CloudPayments)
- **vecv2** — vector DB service

**Полная карта:** [`_control/WORKSPACE_MANIFEST.md`](./_control/WORKSPACE_MANIFEST.md)

---

## 🎬 Быстрый старт

### 1. Клонировать репозиторий
```bash
git clone git@github.com:sudnik008/workstation.git
cd workstation
```

### 2. Прочитать манифест
```bash
cat _control/WORKSPACE_MANIFEST.md
```

### 3. Настроить Cursor Workspace
См. [`GIT_AND_WORKSPACE_SETUP.md`](./GIT_AND_WORKSPACE_SETUP.md)

---

## 🔗 Связанные проекты

- **[AI Dev](https://github.com/sudnik008/ai-dev)** 🔒 — фреймворк Doc-Driven Management
- **[strategy](https://github.com/sudnik008/cyb37_strategy)** 🔒 — бизнес-модель, roadmap
- **[ai-compas](https://github.com/sudnik008/ai-compas)** 🔒 — платформа сообществ

---

## 📄 Лицензия

MIT License — свободное использование материалов.

Workstation разрабатывается как open-source инструмент управления multi-project workspace.

---

**Создано:** Октябрь 2025  
**Автор:** Андрей Судник  
**Статус:** Active development



