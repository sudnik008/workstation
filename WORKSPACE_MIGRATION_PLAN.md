# Cyb37 Workspace ▸ Migration Plan

TL;DR ▸ Option A (MD+FM) → 3 фазы · 5 дней · Git sync → ACTION_ITEMS format → AGENT_INSTRUCTIONS покрытие.

---

## 🎯 Целевое состояние

**По манифесту:** 7 Units → 12 проектов → единый workflow → Cursor-ready.

**Технически:**
- Git: 5 repos synced (ai-dev · strategy · ai-compas · team_docs · code repos)
- ACTION_ITEMS: MD+FM format → machine+human readable → агрегация работает
- AGENT_INSTRUCTIONS: 100% покрытие активных проектов
- Doc-Framework: 4+ проекта (teleeng_assistant · spg · hub · ai-compas)

---

## ⚡️ Phase 1: Foundation (День 1-2)

**Git Sync**
1) `cd "AI Dev" && git init` → проверить remote → `git remote -v` → если нет: `git remote add origin git@github.com:sudnik008/ai-dev.git`
2) `git pull origin main` (если репо не пустой) → мёрж локал + remote → `git push origin main`
3) `cd strategy && git init` → `git remote add origin git@github.com:sudnik008/cyb37-strategy.git` → `git push -u origin main`

**ACTION_ITEMS Template**
4) Создать `AI Dev/templates/action_item_template.md` (MD+FM format → ID · priority · status · timestamps · owner · estimate)
5) Создать `AI Dev/guidelines/action_items_standard.md` (формат → примеры → workflow)

**Результат:** Git configured · template ready · guideline documented.

---

## ⚡️ Phase 2: Implementation (День 3-4)

**Миграция ACTION_ITEMS**
6) `teleeng_assistant/docs/operations/ACTION_ITEMS.md` → конвертировать в новый формат (сохранить старый как `.bak`)
7) `Billing/docs/operations/ACTION_ITEMS.md` → конвертировать
8) Создать `team_docs/spg/ACTION_ITEMS.md` (новый, по template)

**Агрегация**
9) Создать `AI Dev/scripts/aggregate_tasks.py` → парсинг YAML FM → вывод таблицы (owner · priority · status · project)
10) Тест: `python aggregate_tasks.py --owner AS --priority P0` → проверка вывода

**AGENT_INSTRUCTIONS**
11) Создать `AI Dev/_private/AGENT_INSTRUCTIONS.md` (meta-level инструкции)
12) Создать `AI compas/_private/AGENT_INSTRUCTIONS.md` (community context)
13) Создать `team_docs/spg/_private/AGENT_INSTRUCTIONS.md` (SPG проекты)

**Результат:** 3 проекта мигрированы · агрегация работает · AGENT_INSTRUCTIONS покрытие 50%.

---

## ⚡️ Phase 3: Scale (День 5)

**Doc-Framework**
14) `team_docs/spg/` → создать `operations/` → `playbooks/` `incidents/` `support_cases/` → 2-3 playbooks
15) `team_docs/hub/` → `operations/` structure → ACTION_ITEMS.md (новый формат)

**Интеграция Cursor**
16) Обновить `.cursorrules` → правило чтения `AGENT_INSTRUCTIONS.md` → автопредложение задач из `ACTION_ITEMS.md`

**Финализация Git**
17) Sync `ai-compas`: `cd "AI compas" && git pull && git add . && git commit && git push`
18) Commit all changes: `AI Dev/` `strategy/` `team_docs/` → push

**Результат:** 4 проекта с Doc-Framework · Cursor integration ready · Git synced.

---

## 📋 Чек-лист выполнения

**Foundation:**
- [ ] ai-dev: Git remote configured & synced
- [ ] cyb37-strategy: Git repo created & pushed
- [ ] ACTION_ITEMS: template + guideline created

**Implementation:**
- [ ] ACTION_ITEMS: 3 проекта мигрированы (teleeng_assistant · Billing · spg)
- [ ] aggregate_tasks.py: created & tested
- [ ] AGENT_INSTRUCTIONS: 3 файла created (AI Dev · AI compas · spg)

**Scale:**
- [ ] Doc-Framework: spg + hub (operations/ structure)
- [ ] Cursor: .cursorrules updated
- [ ] Git: all repos synced

---

## 🎬 Начать сейчас

```bash
# 1) Проверить Git status AI Dev
cd "/Users/andreysudnik/Library/Mobile Documents/iCloud~md~obsidian/Documents/Cyb37/AI Dev"
git remote -v

# Если remote существует:
git pull origin main
git add .
git commit -m "sync: local workspace updates"
git push origin main

# Если remote НЕ существует:
git remote add origin git@github.com:sudnik008/ai-dev.git
git branch -M main
git push -u origin main
```

**Затем:** Создать templates → migrate 1 ACTION_ITEMS → test → scale.

---

**Статус:** Ready to execute
**Владелец:** AS
**Срок:** 5 дней
**Риски:** Git merge conflicts (если remote ≠ local) ⇢ backup before sync


