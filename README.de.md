# MAiDAS

**Markdown AI Data Access Standard**

Ein Markdown-basierter Webdatenzugriffsstandard für KI-Agenten

---

**Übersetzungen**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Überblick

MAiDAS ist ein Standard, der entwickelt wurde, damit KI-Agenten Webdaten einfach lesen und schreiben können.

Das traditionelle HTML/JavaScript-basierte Web hat Probleme:
- Schwer für KI zu parsen
- Enthält unnötige UI-Informationen
- Komplexe Extraktion strukturierter Daten

MAiDAS wickelt den gesamten Datenaustausch nur mit Markdown ab.

## Kernprinzipien

- **Einfachheit**: Markdown + minimale Regeln
- **Standardkonformität**: HTTP-Standardmethoden, RFC 7763 (text/markdown)
- **Parallelbetrieb**: Kann neben bestehenden HTML-Seiten betrieben werden
- **KI-Optimierung**: Nur Datenaustausch, ohne Benutzeroberfläche

## Schnellstart

### 1. Einstiegspunkt erstellen

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Meine Seite

> Seitenbeschreibung

## API

- [Artikel](/articles/) - [schema](/articles/_schema.md)
```

### 2. Schema definieren

```
/articles/_schema.md
```

```markdown
# Articles API

## Felder

| Feld | Typ | Erforderlich | Beschreibung |
|------|-----|--------------|--------------|
| title | string | Y | Titel |
| content | string(markdown) | Y | Inhalt |

## Aktionen

- GET /articles/_index.md - Liste
- GET /articles/{id}.md - Lesen
- POST /articles/ - Erstellen
```

### 3. Daten bereitstellen

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

- [Erster Beitrag](/articles/first.md) | 2025-01-27
- [Zweiter Beitrag](/articles/second.md) | 2025-01-26
```

## Dokumentation

Die vollständige Spezifikation finden Sie in [SPECIFICATION.md](SPECIFICATION.md).

## Status

Dieser Standard befindet sich derzeit im **Entwurf**-Status. Feedback und Beiträge sind willkommen.

## Mitwirken

Issues und PRs sind willkommen. Bitte diskutieren Sie die Spezifikation in [Issues](https://github.com/TheMagicTower/maidas/issues).

## Lizenz

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
