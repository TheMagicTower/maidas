---
resource: articles
version: 0.1.0
---

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
- PUT /articles/{id}.md - Update
- DELETE /articles/{id}.md - Delete
