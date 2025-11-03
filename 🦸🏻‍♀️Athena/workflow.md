# Workflow ▸ Athene

TL;DR ▸ фича → тест-кейс → реализация в Alpha → ревью разработчиков в Beta → фидбэк → продакшн.

---

## Цикл разработки

| Этап | Роль | Действия |
|------|------|----------|
| **Alpha** | Владелец проекта | Бизнес-документация фичи → описание тест-кейса → реализация → тестирование |
| **Beta** | Разработчики | Ревью кода → фидбэк → доработка |
| **Production** | Команда | Релиз финальной версии |

**Подход:** фича в документации → тест-кейс → реализация.

---

## Работа с ветками

- `alpha` ▸ вся разработка владельца без отдельных веток
- `beta` · `main` ▸ только через MR

---

## Деплой

| Среда | Триггер | URL | Бот |
|-------|---------|-----|-----|
| **Alpha** | push в `alpha` | athene-alpha.teleeng.co | @AtheneAlphaBot |
| **Beta** | MR `alpha`→`beta` | athene-beta.teleeng.co | @AtheneBetaBot |
| **Production** | MR `beta`→`main` | athene-prod.teleeng.co | @AthenaAIrobot |

---

## Инстансы

| Среда | Admin | Бот | IP |
|-------|-------|-----|-----|
| Alpha | athene-alpha.teleeng.co/admin | @AtheneAlphaBot | 65.21.49.91 |
| Beta | athene-beta.teleeng.co/admin | @AtheneBetaBot | 65.21.49.91 |
| Production | athene-prod.teleeng.co/admin | @AthanaAIrobot | 185.161.65.58 |

---

## Инструкции для AI-агента при деплое

### Когда пользователь просит "задеплоить"

1. **Проверь ветку** ▸ `git branch --show-current` (должно быть `alpha`)
2. **Проверь статус** ▸ `git status` (только нужные файлы?)
3. **Выполни деплой** ▸ `git add <файлы>` → `git commit -m "..."` → `git push origin alpha`
4. **Сообщи** ▸ автодеплой запущен → проверка в athene-alpha.teleeng.co

---

## Правила

• Вне Alpha ▸ каждая фича = отдельный MR
• ❌ НЕ push в `beta`/`main` напрямую (только MR)
• Документируй изменения в коммитах
• Кодстайл: `make codestyle` перед коммитом

---

## Стек

Django 4.2.6 · Python 3.11 · Celery · Redis · PostgreSQL · LangChain · LiteLLM · OpenAI · LlamaIndex · Docker · GitLab CI/CD

