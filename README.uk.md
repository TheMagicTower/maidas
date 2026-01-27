# MAiDAS

**Markdown AI Data Access Standard**

Стандарт доступу до веб-даних на основі Markdown для AI-агентів

---

**Переклади**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md)

---

## Огляд

MAiDAS — це стандарт, розроблений для того, щоб AI-агенти могли легко читати та записувати веб-дані.

Традиційний веб на основі HTML/JavaScript має проблеми:
- Складно парсити для AI
- Містить непотрібну інформацію про інтерфейс
- Складне вилучення структурованих даних

MAiDAS обробляє весь обмін даними, використовуючи лише Markdown.

## Основні принципи

- **Простота**: Markdown + мінімум правил
- **Відповідність стандартам**: Стандартні HTTP-методи, RFC 7763 (text/markdown)
- **Паралельна робота**: Може працювати разом з існуючими HTML-сайтами
- **Оптимізація для AI**: Лише обмін даними, без інтерфейсу користувача

## Швидкий старт

### 1. Створення точки входу

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Мій сайт

> Опис сайту

## API

- [Статті](/articles/) - [schema](/articles/_schema.md)
```

### 2. Визначення схеми

```
/articles/_schema.md
```

```markdown
# Articles API

## Поля

| Поле | Тип | Обов'язково | Опис |
|------|-----|-------------|------|
| title | string | Y | Заголовок |
| content | string(markdown) | Y | Зміст |

## Дії

- GET /articles/_index.md - Список
- GET /articles/{id}.md - Читання
- POST /articles/ - Створення
```

### 3. Надання даних

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

- [Перший запис](/articles/first.md) | 2025-01-27
- [Другий запис](/articles/second.md) | 2025-01-26
```

## Документація

Повну специфікацію див. у [SPECIFICATION.md](SPECIFICATION.md).

## Статус

Цей стандарт наразі має статус **Чернетка**. Відгуки та внески вітаються.

## Участь

Issues та PR вітаються. Будь ласка, обговорюйте специфікацію в [Issues](https://github.com/TheMagicTower/maidas/issues).

## Ліцензія

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
