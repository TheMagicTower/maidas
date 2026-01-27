# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

A markdown-based web data access standard for AI agents

---

**Translations**: [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Overview

### 1.1 Purpose

MAiDAS is a standard designed for AI agents to easily read and write web data. Traditional HTML/JavaScript-based web is difficult for AI to parse and contains unnecessary information. MAiDAS handles all data exchange using only markdown.

### 1.2 Principles

- **Simplicity**: Markdown + minimal rules
- **Standards Compliance**: HTTP standard methods, RFC 7763 (text/markdown)
- **Parallel Operation**: Can run alongside existing HTML sites
- **AI Optimization**: Exchange data only, without UI

### 1.3 MIME Type

All requests and responses use the following Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. File Structure

### 2.1 Basic Structure

```
example.com/
├── .well-known/
│   └── index.md           # Required. Site entry point
├── articles/
│   ├── _schema.md         # API schema
│   ├── _index.md          # List
│   ├── first-post.md      # Individual document
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 System Files

Files with underscore (`_`) prefix are system files:

| File | Purpose |
|------|---------|
| `_schema.md` | Resource CRUD schema definition |
| `_index.md` | Resource list |

---

## 3. Entry Point

### 3.1 Location

```
/.well-known/index.md
```

### 3.2 Format

```markdown
---
version: 0.1.0
---

# Site Name

> One-line site description

## Pages

- [About](/about.md)
- [Article List](/articles/_index.md)

## API

- [Article Management](/articles/) - [schema](/articles/_schema.md)
- [Comment Management](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 Fields

| Field | Required | Description |
|-------|----------|-------------|
| version | Y | MAiDAS spec version |

---

## 4. Schema Definition

### 4.1 Location

`_schema.md` within each resource folder

```
/articles/_schema.md
/comments/_schema.md
```

### 4.2 Format

```markdown
# Articles API

## Authentication

- Method: Bearer
- Header: `Authorization: Bearer {token}`
- Required: Y

## Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Y | Title |
| content | string(markdown) | Y | Body |
| tags | array(string) | N | Tag list |
| date | date | N | Created date |

## Actions

- GET /articles/_index.md - List
- GET /articles/{id}.md - Read
- POST /articles/ - Create
- PUT /articles/{id}.md - Update
- DELETE /articles/{id}.md - Delete
```

### 4.3 Type System

**Basic Types**

| Type | Description |
|------|-------------|
| string | String |
| number | Number |
| boolean | True/False |
| date | Date (ISO 8601) |
| array | Array |

**Format Hints (Optional)**

Notation in parentheses after type:

| Format | Description |
|--------|-------------|
| string(markdown) | Markdown allowed |
| string(url) | URL format |
| string(email) | Email format |
| date(datetime) | Date+Time |
| array(string) | String array |
| array(number) | Number array |

---

## 5. List Response

### 5.1 Location

`_index.md` within each resource folder

### 5.2 Format

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [First Post](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Second Post](/articles/second-post.md) | 2025-01-26 | #diary
- [Third Post](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 Rules

- Define field headers with blockquote (`>`) on first line
- List field values after link with `|` separator
- Leave empty values blank
- Pagination info in frontmatter

---

## 6. Individual Document

### 6.1 Format

```markdown
---
title: First Post
date: 2025-01-27
tags: [tech, ai]
---

# First Post

Body content...
```

### 6.2 Rules

- frontmatter: YAML format (wrapped with `---`)
- Body: Markdown
- Required fields: As defined in `_schema.md`

---

## 7. CRUD Request/Response

### 7.1 Create (POST)

**Request**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: New Post
tags: [tech]
---

# New Post

Body content...
```

**Response**
```markdown
---
status: 201
---

# Created

Document created: /articles/new-post.md
```

### 7.2 Read (GET)

**List**
```
GET /articles/_index.md
```

**Individual**
```
GET /articles/first-post.md
```

### 7.3 Update (PUT)

**Request**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Updated Title
tags: [tech, ai]
---

# Updated Title

Updated body...
```

**Response**
```markdown
---
status: 200
---

# OK

Document updated.
```

### 7.4 Delete (DELETE)

**Request**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**Response**
```markdown
---
status: 200
---

# OK

Document deleted.
```

---

## 8. Query Parameters

### 8.1 Pagination

```
GET /articles/_index.md?page=2&limit=20
```

| Parameter | Default | Description |
|-----------|---------|-------------|
| page | 1 | Page number |
| limit | Server config | Items per page |

Included in response frontmatter:
- `page`: Current page
- `limit`: Items per page
- `total`: Total items

### 8.2 Filter

Use field names as query parameters:

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
GET /articles/_index.md?tag=tech&date=2025-01
```

### 8.3 Sort

```
GET /articles/_index.md?sort=-date,title
```

**Rules**
- Use `sort` parameter
- Comma-separated for multiple fields (priority order)
- `-` prefix: Descending
- No prefix: Ascending

**Examples**
```
?sort=date          # date ascending
?sort=-date         # date descending
?sort=-date,title   # date descending, then title ascending
```

### 8.4 Combination

All query parameters can be combined:

```
GET /articles/_index.md?tag=tech&sort=-date&page=2&limit=10
```

---

## 9. Authentication

### 9.1 Method

HTTP standard Bearer token authentication

```
Authorization: Bearer {token}
```

### 9.2 Schema Notation

```markdown
## Authentication

- Method: Bearer
- Header: `Authorization: Bearer {token}`
- Required: Y
```

When authentication is not required:

```markdown
## Authentication

- Required: N
```

---

## 10. Error Handling

### 10.1 Format

All error responses are also markdown:

```markdown
---
status: {HTTP status code}
---

# {Error Title}

{Error description}
```

### 10.2 Status Codes

| Code | Title | Description |
|------|-------|-------------|
| 200 | OK | Success |
| 201 | Created | Created |
| 400 | Bad Request | Invalid request |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | No permission |
| 404 | Not Found | Document not found |
| 500 | Internal Server Error | Server error |

### 10.3 Examples

**401 Unauthorized**
```markdown
---
status: 401
---

# Unauthorized

Authentication required.
```

**400 Bad Request**
```markdown
---
status: 400
---

# Bad Request

The title field is required.
```

**404 Not Found**
```markdown
---
status: 404
---

# Not Found

The requested document was not found.
```

---

## 11. Complete Example

### 11.1 Entry Point

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# My Blog

> A personal blog.

## Pages

- [About](/about.md)

## API

- [Article Management](/articles/) - [schema](/articles/_schema.md)
```

### 11.2 Schema

`GET /articles/_schema.md`

```markdown
# Articles API

## Authentication

- Method: Bearer
- Header: `Authorization: Bearer {token}`
- Required: Y (create/update/delete), N (read)

## Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Y | Title |
| content | string(markdown) | Y | Body |
| tags | array(string) | N | Tag list |
| date | date | N | Created date (default: now) |

## Actions

- GET /articles/_index.md - List
- GET /articles/{id}.md - Read
- POST /articles/ - Create
- PUT /articles/{id}.md - Update
- DELETE /articles/{id}.md - Delete
```

### 11.3 List

`GET /articles/_index.md?sort=-date&limit=10`

```markdown
---
page: 1
limit: 10
total: 25
---

# Articles

> title | date | tags

- [MAiDAS Standard Release](/articles/maidas-release.md) | 2025-01-27 | #tech, #standard
- [Future of AI Web](/articles/ai-web-future.md) | 2025-01-26 | #ai
- [Markdown Tips](/articles/markdown-tips.md) | 2025-01-25 | #markdown
```

### 11.4 Individual Document

`GET /articles/maidas-release.md`

```markdown
---
title: MAiDAS Standard Release
date: 2025-01-27
tags: [tech, standard]
---

# MAiDAS Standard Release

Today we release MAiDAS 0.1.0...
```

---

## 12. Grammar Profiles & Conformance (Proposal)

To make MAiDAS reliable for AI agents without introducing JSON, define each version as a strict Markdown “grammar profile” (DTD-like rules for structure).

### 12.1 Required Document Types (v0.1 Profile)

The v0.1 profile defines four normative document types:
- Entry point: `/.well-known/index.md`
- Schema: `/<resource>/_schema.md`
- List: `/<resource>/_index.md`
- Resource: `/<resource>/{id}.md`

### 12.2 Structural Grammar Rules (v0.1 Profile)

For v0.1, document structure is normative:
- All documents SHOULD start with YAML frontmatter.
- The first H1 (`#`) is the document title.
- Required sections MUST use fixed H2 headings.
- Required tables MUST use fixed column names.

Type-specific required H2 sections:
- Entry point: `## Pages`, `## API`
- Schema: `## Fields`, `## Actions`
- List: no required H2 sections, but list items MUST be links
- Resource: no required H2 sections

### 12.3 Frontmatter Requirements (v0.1 Profile)

Required frontmatter keys by type:
- Entry point: `version`
- Schema: none required, but `resource` and `version` are RECOMMENDED
- List: `page`, `limit`, `total`
- Resource: keys SHOULD match fields defined in `_schema.md`

Unknown frontmatter keys are allowed but MUST NOT change the meaning of required keys.

### 12.4 Conformance Test Layout (Recommended)

Treat conformance examples as part of the standard:

```text
conformance/
  v0.1/
    valid/
      entrypoint.min.md
      schema.min.md
      list.min.md
      resource.min.md
    invalid/
      entrypoint.missing-api.md
      schema.missing-actions.md
```

Agents and tools can implement a validator that checks “profile v0.1” by verifying required files, headings, frontmatter keys, and table schemas.

### 12.5 Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`

---

## Appendix: Version History

| Version | Date | Changes |
|---------|------|---------|
| 0.1.0 | 2025-01-27 | Initial draft release |
