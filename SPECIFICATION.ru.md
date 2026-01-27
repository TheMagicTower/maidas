# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Стандарт доступа к веб-данным на основе Markdown для ИИ-агентов

---

**Переводы**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Обзор

### 1.1 Цель

MAiDAS — это стандарт, разработанный для того, чтобы ИИ-агенты могли легко читать и записывать веб-данные. Традиционный веб на основе HTML/JavaScript сложно парсить для ИИ и содержит ненужную информацию. MAiDAS обрабатывает весь обмен данными, используя только Markdown.

### 1.2 Принципы

- **Простота**: Markdown + минимум правил
- **Соответствие стандартам**: Стандартные HTTP-методы, RFC 7763 (text/markdown)
- **Параллельная работа**: Может работать вместе с существующими HTML-сайтами
- **Оптимизация для ИИ**: Только обмен данными, без UI

### 1.3 MIME-тип

Все запросы и ответы используют следующий Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Структура файлов

### 2.1 Базовая структура

```
example.com/
├── .well-known/
│   └── index.md           # Обязательно. Точка входа сайта
├── articles/
│   ├── _schema.md         # Схема API
│   ├── _index.md          # Список
│   ├── first-post.md      # Отдельный документ
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 Системные файлы

Файлы с префиксом подчёркивания (`_`) — системные файлы:

| Файл | Назначение |
|------|------------|
| `_schema.md` | Определение CRUD-схемы ресурсов |
| `_index.md` | Список ресурсов |

---

## 3. Точка входа

### 3.1 Расположение

```
/.well-known/index.md
```

### 3.2 Формат

```markdown
---
version: 0.1.0
---

# Название сайта

> Описание сайта в одну строку

## Страницы

- [О нас](/about.md)
- [Список статей](/articles/_index.md)

## API

- [Управление статьями](/articles/) - [schema](/articles/_schema.md)
- [Управление комментариями](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 Поля

| Поле | Обязательно | Описание |
|------|-------------|----------|
| version | Y | Версия спецификации MAiDAS |

---

## 4. Определение схемы

### 4.1 Расположение

`_schema.md` в каждой папке ресурсов

### 4.2 Формат

```markdown
# Articles API

## Аутентификация

- Метод: Bearer
- Заголовок: `Authorization: Bearer {token}`
- Обязательно: Y

## Поля

| Поле | Тип | Обязательно | Описание |
|------|-----|-------------|----------|
| title | string | Y | Заголовок |
| content | string(markdown) | Y | Содержание |
| tags | array(string) | N | Список тегов |
| date | date | N | Дата создания |

## Действия

- GET /articles/_index.md - Список
- GET /articles/{id}.md - Чтение
- POST /articles/ - Создание
- PUT /articles/{id}.md - Обновление
- DELETE /articles/{id}.md - Удаление
```

### 4.3 Система типов

**Базовые типы**

| Тип | Описание |
|-----|----------|
| string | Строка |
| number | Число |
| boolean | Истина/Ложь |
| date | Дата (ISO 8601) |
| array | Массив |

**Подсказки формата (Опционально)**

Нотация в скобках после типа:

| Формат | Описание |
|--------|----------|
| string(markdown) | Разрешён Markdown |
| string(url) | Формат URL |
| string(email) | Формат email |
| date(datetime) | Дата+Время |
| array(string) | Массив строк |
| array(number) | Массив чисел |

---

## 5. Ответ списка

### 5.1 Расположение

`_index.md` в каждой папке ресурсов

### 5.2 Формат

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Первая запись](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Вторая запись](/articles/second-post.md) | 2025-01-26 | #diary
- [Третья запись](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 Правила

- Определить заголовки полей с blockquote (`>`) в первой строке
- Перечислить значения полей после ссылки с разделителем `|`
- Оставить пустые значения пустыми
- Информация о пагинации во frontmatter

---

## 6. Отдельный документ

### 6.1 Формат

```markdown
---
title: Первая запись
date: 2025-01-27
tags: [tech, ai]
---

# Первая запись

Содержание...
```

### 6.2 Правила

- frontmatter: формат YAML (обёрнуто `---`)
- Тело: Markdown
- Обязательные поля: Как определено в `_schema.md`

---

## 7. CRUD Запрос/Ответ

### 7.1 Создание (POST)

**Запрос**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Новая запись
tags: [tech]
---

# Новая запись

Содержание...
```

**Ответ**
```markdown
---
status: 201
---

# Created

Документ создан: /articles/new-post.md
```

### 7.2 Чтение (GET)

**Список**
```
GET /articles/_index.md
```

**Отдельный**
```
GET /articles/first-post.md
```

### 7.3 Обновление (PUT)

**Запрос**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Обновлённый заголовок
tags: [tech, ai]
---

# Обновлённый заголовок

Обновлённое содержание...
```

**Ответ**
```markdown
---
status: 200
---

# OK

Документ обновлён.
```

### 7.4 Удаление (DELETE)

**Запрос**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**Ответ**
```markdown
---
status: 200
---

# OK

Документ удалён.
```

---

## 8. Параметры запроса

### 8.1 Пагинация

```
GET /articles/_index.md?page=2&limit=20
```

| Параметр | По умолчанию | Описание |
|----------|--------------|----------|
| page | 1 | Номер страницы |
| limit | Конфиг сервера | Элементов на страницу |

### 8.2 Фильтр

Использовать имена полей как параметры запроса:

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
```

### 8.3 Сортировка

```
GET /articles/_index.md?sort=-date,title
```

**Правила**
- Использовать параметр `sort`
- Запятая для нескольких полей
- Префикс `-`: По убыванию
- Без префикса: По возрастанию

---

## 9. Аутентификация

### 9.1 Метод

Стандартная HTTP Bearer token аутентификация

```
Authorization: Bearer {token}
```

---

## 10. Обработка ошибок

### 10.1 Формат

```markdown
---
status: {HTTP код состояния}
---

# {Заголовок ошибки}

{Описание ошибки}
```

### 10.2 Коды состояния

| Код | Заголовок | Описание |
|-----|-----------|----------|
| 200 | OK | Успех |
| 201 | Created | Создано |
| 400 | Bad Request | Неверный запрос |
| 401 | Unauthorized | Требуется аутентификация |
| 403 | Forbidden | Нет разрешения |
| 404 | Not Found | Документ не найден |
| 500 | Internal Server Error | Ошибка сервера |

---

## 11. Полный пример

### 11.1 Точка входа

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# Мой блог

> Личный блог.

## Страницы

- [О нас](/about.md)

## API

- [Управление статьями](/articles/) - [schema](/articles/_schema.md)
```

---

## Приложение: История версий

| Версия | Дата | Изменения |
|--------|------|-----------|
| 0.1.0 | 2025-01-27 | Первый черновой релиз |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
