# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Стандарт доступу до веб-даних на основі Markdown для AI-агентів

---

**Переклади**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md)

---

## 1. Огляд

### 1.1 Мета

MAiDAS — це стандарт, розроблений для того, щоб AI-агенти могли легко читати та записувати веб-дані. Традиційний веб на основі HTML/JavaScript складно парсити для AI та містить непотрібну інформацію. MAiDAS обробляє весь обмін даними, використовуючи лише Markdown.

### 1.2 Принципи

- **Простота**: Markdown + мінімум правил
- **Відповідність стандартам**: Стандартні HTTP-методи, RFC 7763 (text/markdown)
- **Паралельна робота**: Може працювати разом з існуючими HTML-сайтами
- **Оптимізація для AI**: Лише обмін даними, без інтерфейсу користувача

### 1.3 MIME-тип

Усі запити та відповіді використовують наступний Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Структура файлів

### 2.1 Базова структура

```
example.com/
├── .well-known/
│   └── index.md           # Обов'язково. Точка входу сайту
├── articles/
│   ├── _schema.md         # Схема API
│   ├── _index.md          # Список
│   ├── first-post.md      # Окремий документ
│   └── second-post.md
└── about.md
```

### 2.2 Системні файли

Файли з префіксом підкреслення (`_`) — системні файли:

| Файл | Призначення |
|------|-------------|
| `_schema.md` | Визначення CRUD-схеми ресурсів |
| `_index.md` | Список ресурсів |

---

## 3. Точка входу

### 3.1 Розташування

```
/.well-known/index.md
```

### 3.2 Формат

```markdown
---
version: 0.1.0
---

# Назва сайту

> Опис сайту в один рядок

## Сторінки

- [Про нас](/about.md)
- [Список статей](/articles/_index.md)

## API

- [Керування статтями](/articles/) - [schema](/articles/_schema.md)
```

---

## 4. Визначення схеми

```markdown
# Articles API

## Автентифікація

- Метод: Bearer
- Заголовок: `Authorization: Bearer {token}`
- Обов'язково: Y

## Поля

| Поле | Тип | Обов'язково | Опис |
|------|-----|-------------|------|
| title | string | Y | Заголовок |
| content | string(markdown) | Y | Зміст |
| tags | array(string) | N | Список тегів |
| date | date | N | Дата створення |

## Дії

- GET /articles/_index.md - Список
- GET /articles/{id}.md - Читання
- POST /articles/ - Створення
- PUT /articles/{id}.md - Оновлення
- DELETE /articles/{id}.md - Видалення
```

---

## 5. Відповідь списку

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Перший запис](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Другий запис](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. Окремий документ

```markdown
---
title: Перший запис
date: 2025-01-27
tags: [tech, ai]
---

# Перший запис

Зміст...
```

---

## 7. CRUD Запит/Відповідь

### 7.1 Створення (POST)

**Запит**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Новий запис
tags: [tech]
---

# Новий запис

Зміст...
```

**Відповідь**
```markdown
---
status: 201
---

# Created

Документ створено: /articles/new-post.md
```

---

## 8. Параметри запиту

### 8.1 Пагінація

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 Сортування

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. Автентифікація

Стандартна HTTP Bearer token автентифікація

```
Authorization: Bearer {token}
```

---

## 10. Обробка помилок

| Код | Заголовок | Опис |
|-----|-----------|------|
| 200 | OK | Успіх |
| 201 | Created | Створено |
| 400 | Bad Request | Невірний запит |
| 401 | Unauthorized | Потрібна автентифікація |
| 403 | Forbidden | Немає дозволу |
| 404 | Not Found | Документ не знайдено |
| 500 | Internal Server Error | Помилка сервера |

---

## Додаток: Історія версій

| Версія | Дата | Зміни |
|--------|------|-------|
| 0.1.0 | 2025-01-27 | Перший чорновий реліз |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
