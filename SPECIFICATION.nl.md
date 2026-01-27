# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Op Markdown gebaseerde webtoegangsstandaard voor AI-agenten

---

**Vertalingen**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Overzicht

### 1.1 Doel

MAiDAS is een standaard ontworpen zodat AI-agenten eenvoudig webgegevens kunnen lezen en schrijven. Traditioneel HTML/JavaScript-gebaseerd web is moeilijk te parsen voor AI en bevat onnodige informatie. MAiDAS verwerkt alle gegevensuitwisseling met alleen Markdown.

### 1.2 Principes

- **Eenvoud**: Markdown + minimale regels
- **Standaardconformiteit**: HTTP standaardmethoden, RFC 7763 (text/markdown)
- **Parallelle werking**: Kan naast bestaande HTML-sites draaien
- **AI-optimalisatie**: Alleen gegevensuitwisseling, geen UI

### 1.3 MIME-type

Alle verzoeken en antwoorden gebruiken het volgende Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Bestandsstructuur

### 2.1 Basisstructuur

```
example.com/
├── .well-known/
│   └── index.md           # Vereist. Site toegangspunt
├── articles/
│   ├── _schema.md         # API-schema
│   ├── _index.md          # Lijst
│   ├── first-post.md      # Individueel document
│   └── second-post.md
└── about.md
```

### 2.2 Systeembestanden

Bestanden met underscore (`_`) prefix zijn systeembestanden:

| Bestand | Doel |
|---------|------|
| `_schema.md` | Resource CRUD-schemadefinitie |
| `_index.md` | Resourcelijst |

---

## 3. Toegangspunt

### 3.1 Locatie

```
/.well-known/index.md
```

### 3.2 Formaat

```markdown
---
version: 0.1.0
---

# Sitenaam

> Sitebeschrijving in één regel

## Pagina's

- [Over](/about.md)
- [Artikellijst](/articles/_index.md)

## API

- [Artikelbeheer](/articles/) - [schema](/articles/_schema.md)
```

---

## 4. Schemadefinitie

```markdown
# Articles API

## Authenticatie

- Methode: Bearer
- Header: `Authorization: Bearer {token}`
- Vereist: Y

## Velden

| Veld | Type | Vereist | Beschrijving |
|------|------|---------|--------------|
| title | string | Y | Titel |
| content | string(markdown) | Y | Inhoud |
| tags | array(string) | N | Taglijst |
| date | date | N | Aanmaakdatum |

## Acties

- GET /articles/_index.md - Lijst
- GET /articles/{id}.md - Lezen
- POST /articles/ - Aanmaken
- PUT /articles/{id}.md - Bijwerken
- DELETE /articles/{id}.md - Verwijderen
```

---

## 5. Lijstantwoord

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Eerste bericht](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Tweede bericht](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. Individueel document

```markdown
---
title: Eerste bericht
date: 2025-01-27
tags: [tech, ai]
---

# Eerste bericht

Inhoud...
```

---

## 7. CRUD-verzoek/antwoord

### 7.1 Aanmaken (POST)

**Verzoek**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Nieuw bericht
tags: [tech]
---

# Nieuw bericht

Inhoud...
```

**Antwoord**
```markdown
---
status: 201
---

# Created

Document aangemaakt: /articles/new-post.md
```

---

## 8. Queryparameters

### 8.1 Paginering

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 Sorteren

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. Authenticatie

HTTP standaard Bearer token authenticatie

```
Authorization: Bearer {token}
```

---

## 10. Foutafhandeling

| Code | Titel | Beschrijving |
|------|-------|--------------|
| 200 | OK | Succes |
| 201 | Created | Aangemaakt |
| 400 | Bad Request | Ongeldig verzoek |
| 401 | Unauthorized | Authenticatie vereist |
| 403 | Forbidden | Geen toestemming |
| 404 | Not Found | Document niet gevonden |
| 500 | Internal Server Error | Serverfout |

---

## Bijlage: Versiegeschiedenis

| Versie | Datum | Wijzigingen |
|--------|-------|-------------|
| 0.1.0 | 2025-01-27 | Eerste concept release |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
