# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Ein Markdown-basierter Webdatenzugriffsstandard für KI-Agenten

---

**Übersetzungen**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Überblick

### 1.1 Zweck

MAiDAS ist ein Standard, der entwickelt wurde, damit KI-Agenten Webdaten einfach lesen und schreiben können. Das traditionelle HTML/JavaScript-basierte Web ist schwer für KI zu parsen und enthält unnötige Informationen. MAiDAS wickelt den gesamten Datenaustausch nur mit Markdown ab.

### 1.2 Prinzipien

- **Einfachheit**: Markdown + minimale Regeln
- **Standardkonformität**: HTTP-Standardmethoden, RFC 7763 (text/markdown)
- **Parallelbetrieb**: Kann neben bestehenden HTML-Seiten betrieben werden
- **KI-Optimierung**: Nur Datenaustausch, ohne Benutzeroberfläche

### 1.3 MIME-Typ

Alle Anfragen und Antworten verwenden den folgenden Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Dateistruktur

### 2.1 Grundstruktur

```
example.com/
├── .well-known/
│   └── index.md           # Erforderlich. Einstiegspunkt der Seite
├── articles/
│   ├── _schema.md         # API-Schema
│   ├── _index.md          # Liste
│   ├── first-post.md      # Einzelnes Dokument
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 Systemdateien

Dateien mit Unterstrich (`_`) als Präfix sind Systemdateien:

| Datei | Zweck |
|-------|-------|
| `_schema.md` | Ressourcen-CRUD-Schema-Definition |
| `_index.md` | Ressourcenliste |

---

## 3. Einstiegspunkt

### 3.1 Ort

```
/.well-known/index.md
```

### 3.2 Format

```markdown
---
version: 0.1.0
---

# Seitenname

> Einzeilige Seitenbeschreibung

## Seiten

- [Über](/about.md)
- [Artikelliste](/articles/_index.md)

## API

- [Artikelverwaltung](/articles/) - [schema](/articles/_schema.md)
- [Kommentarverwaltung](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 Felder

| Feld | Erforderlich | Beschreibung |
|------|--------------|--------------|
| version | Y | MAiDAS-Spezifikationsversion |

---

## 4. Schema-Definition

### 4.1 Ort

`_schema.md` in jedem Ressourcenordner

### 4.2 Format

```markdown
# Articles API

## Authentifizierung

- Methode: Bearer
- Header: `Authorization: Bearer {token}`
- Erforderlich: Y

## Felder

| Feld | Typ | Erforderlich | Beschreibung |
|------|-----|--------------|--------------|
| title | string | Y | Titel |
| content | string(markdown) | Y | Inhalt |
| tags | array(string) | N | Tag-Liste |
| date | date | N | Erstellungsdatum |

## Aktionen

- GET /articles/_index.md - Liste
- GET /articles/{id}.md - Lesen
- POST /articles/ - Erstellen
- PUT /articles/{id}.md - Aktualisieren
- DELETE /articles/{id}.md - Löschen
```

### 4.3 Typsystem

**Grundtypen**

| Typ | Beschreibung |
|-----|--------------|
| string | Zeichenkette |
| number | Zahl |
| boolean | Wahr/Falsch |
| date | Datum (ISO 8601) |
| array | Array |

**Format-Hinweise (Optional)**

Notation in Klammern nach dem Typ:

| Format | Beschreibung |
|--------|--------------|
| string(markdown) | Markdown erlaubt |
| string(url) | URL-Format |
| string(email) | E-Mail-Format |
| date(datetime) | Datum+Zeit |
| array(string) | String-Array |
| array(number) | Zahlen-Array |

---

## 5. Listenantwort

### 5.1 Ort

`_index.md` in jedem Ressourcenordner

### 5.2 Format

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Erster Beitrag](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Zweiter Beitrag](/articles/second-post.md) | 2025-01-26 | #diary
- [Dritter Beitrag](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 Regeln

- Feldheader mit Blockquote (`>`) in der ersten Zeile definieren
- Feldwerte nach dem Link mit `|` Trennzeichen auflisten
- Leere Werte leer lassen
- Paginierungsinfo im Frontmatter

---

## 6. Einzelnes Dokument

### 6.1 Format

```markdown
---
title: Erster Beitrag
date: 2025-01-27
tags: [tech, ai]
---

# Erster Beitrag

Inhalt...
```

### 6.2 Regeln

- frontmatter: YAML-Format (mit `---` umschlossen)
- Inhalt: Markdown
- Erforderliche Felder: Wie in `_schema.md` definiert

---

## 7. CRUD-Anfrage/Antwort

### 7.1 Erstellen (POST)

**Anfrage**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Neuer Beitrag
tags: [tech]
---

# Neuer Beitrag

Inhalt...
```

**Antwort**
```markdown
---
status: 201
---

# Created

Dokument erstellt: /articles/new-post.md
```

### 7.2 Lesen (GET)

**Liste**
```
GET /articles/_index.md
```

**Einzeln**
```
GET /articles/first-post.md
```

### 7.3 Aktualisieren (PUT)

**Anfrage**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Aktualisierter Titel
tags: [tech, ai]
---

# Aktualisierter Titel

Aktualisierter Inhalt...
```

**Antwort**
```markdown
---
status: 200
---

# OK

Dokument aktualisiert.
```

### 7.4 Löschen (DELETE)

**Anfrage**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**Antwort**
```markdown
---
status: 200
---

# OK

Dokument gelöscht.
```

---

## 8. Abfrageparameter

### 8.1 Paginierung

```
GET /articles/_index.md?page=2&limit=20
```

| Parameter | Standard | Beschreibung |
|-----------|----------|--------------|
| page | 1 | Seitennummer |
| limit | Server-Config | Elemente pro Seite |

### 8.2 Filter

Feldnamen als Abfrageparameter verwenden:

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
```

### 8.3 Sortierung

```
GET /articles/_index.md?sort=-date,title
```

**Regeln**
- `sort` Parameter verwenden
- Komma für mehrere Felder
- `-` Präfix: Absteigend
- Kein Präfix: Aufsteigend

---

## 9. Authentifizierung

### 9.1 Methode

HTTP Standard Bearer Token Authentifizierung

```
Authorization: Bearer {token}
```

---

## 10. Fehlerbehandlung

### 10.1 Format

```markdown
---
status: {HTTP-Statuscode}
---

# {Fehlertitel}

{Fehlerbeschreibung}
```

### 10.2 Statuscodes

| Code | Titel | Beschreibung |
|------|-------|--------------|
| 200 | OK | Erfolg |
| 201 | Created | Erstellt |
| 400 | Bad Request | Ungültige Anfrage |
| 401 | Unauthorized | Authentifizierung erforderlich |
| 403 | Forbidden | Keine Berechtigung |
| 404 | Not Found | Dokument nicht gefunden |
| 500 | Internal Server Error | Serverfehler |

---

## 11. Vollständiges Beispiel

### 11.1 Einstiegspunkt

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# Mein Blog

> Ein persönlicher Blog.

## Seiten

- [Über](/about.md)

## API

- [Artikelverwaltung](/articles/) - [schema](/articles/_schema.md)
```

---

## Anhang: Versionshistorie

| Version | Datum | Änderungen |
|---------|-------|------------|
| 0.1.0 | 2025-01-27 | Erster Entwurf |
