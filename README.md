# MAiDAS

**Markdown AI Data Access Standard**

A markdown-based web data access standard for AI agents

---

**Translations**: [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Overview

MAiDAS is a standard designed for AI agents to easily read and write web data.

Traditional HTML/JavaScript-based web has issues:
- Difficult for AI to parse
- Contains unnecessary UI information
- Complex to extract structured data

MAiDAS handles all data exchange using only markdown.

## Core Principles

- **Simplicity**: Markdown + minimal rules
- **Standards Compliance**: HTTP standard methods, RFC 7763 (text/markdown)
- **Parallel Operation**: Can run alongside existing HTML sites
- **AI Optimization**: Exchange data only, without UI

## Quick Start

### 1. Create Entry Point

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# My Site

> Site description

## API

- [Articles](/articles/) - [schema](/articles/_schema.md)
```

### 2. Define Schema

```
/articles/_schema.md
```

```markdown
# Articles API

## Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Y | Title |
| content | string(markdown) | Y | Body |

## Actions

- GET /articles/_index.md - List
- GET /articles/{id}.md - Read
- POST /articles/ - Create
```

### 3. Provide Data

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

- [First Post](/articles/first.md) | 2025-01-27
- [Second Post](/articles/second.md) | 2025-01-26
```

## Documentation

See [SPECIFICATION.md](SPECIFICATION.md) for the full specification.

## Status

This standard is currently in **Draft** status. Feedback and contributions are welcome.

## Contributing

Issues and PRs are welcome. Please discuss the specification in [Issues](https://github.com/TheMagicTower/maidas/issues).

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
