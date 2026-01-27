# MAiDAS

**Markdown AI Data Access Standard**

Стандарт доступа к веб-данным на основе Markdown для ИИ-агентов

---

**Переводы**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Обзор

MAiDAS — это стандарт, разработанный для того, чтобы ИИ-агенты могли легко читать и записывать веб-данные.

Традиционный веб на основе HTML/JavaScript имеет проблемы:
- Сложно парсить для ИИ
- Содержит ненужную информацию об интерфейсе
- Сложное извлечение структурированных данных

MAiDAS обрабатывает весь обмен данными, используя только Markdown.

## Основные принципы

- **Простота**: Markdown + минимум правил
- **Соответствие стандартам**: Стандартные HTTP-методы, RFC 7763 (text/markdown)
- **Параллельная работа**: Может работать вместе с существующими HTML-сайтами
- **Оптимизация для ИИ**: Только обмен данными, без пользовательского интерфейса

## Быстрый старт

### 1. Создание точки входа

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Мой сайт

> Описание сайта

## API

- [Статьи](/articles/) - [schema](/articles/_schema.md)
```

### 2. Определение схемы

```
/articles/_schema.md
```

```markdown
# Articles API

## Поля

| Поле | Тип | Обязательно | Описание |
|------|-----|-------------|----------|
| title | string | Y | Заголовок |
| content | string(markdown) | Y | Содержание |

## Действия

- GET /articles/_index.md - Список
- GET /articles/{id}.md - Чтение
- POST /articles/ - Создание
```

### 3. Предоставление данных

```
/articles/_index.md
```

```markdown
---
page: 1
limit: 20
total: 2
---

# Articles

> title | date

- [Первая запись](/articles/first.md) | 2025-01-27
- [Вторая запись](/articles/second.md) | 2025-01-26
```

## Документация

Полную спецификацию см. в [SPECIFICATION.ru.md](SPECIFICATION.ru.md).

## Статус

Этот стандарт в настоящее время находится в статусе **Черновик**. Отзывы и вклад приветствуются.

## Участие

Issues и PR приветствуются. Пожалуйста, обсуждайте спецификацию в [Issues](https://github.com/TheMagicTower/maidas/issues).

## Лицензия

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
