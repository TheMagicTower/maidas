---
resource: articles
version: 0.1.0
---

# Articles API

## Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Y | Article title |
| content | string(markdown) | Y | Article body |
| date | date | Y | Published date |
| tags | array(string) | N | Tag list |

## Actions

- GET /articles/_index.md - List articles
- GET /articles/{id}.md - Read article
