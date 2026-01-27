# MAiDAS

**Markdown AI Data Access Standard**

Op Markdown gebaseerde webtoegangsstandaard voor AI-agenten

---

**Vertalingen**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Українська](README.uk.md)

---

## Overzicht

MAiDAS is een standaard ontworpen zodat AI-agenten eenvoudig webgegevens kunnen lezen en schrijven.

Traditioneel HTML/JavaScript-gebaseerd web heeft problemen:
- Moeilijk te parsen voor AI
- Bevat onnodige UI-informatie
- Complexe extractie van gestructureerde gegevens

MAiDAS verwerkt alle gegevensuitwisseling met alleen Markdown.

## Kernprincipes

- **Eenvoud**: Markdown + minimale regels
- **Standaardconformiteit**: HTTP standaardmethoden, RFC 7763 (text/markdown)
- **Parallelle werking**: Kan naast bestaande HTML-sites draaien
- **AI-optimalisatie**: Alleen gegevensuitwisseling, geen UI

## Snelle start

### 1. Toegangspunt maken

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Mijn Site

> Site beschrijving

## API

- [Artikelen](/articles/) - [schema](/articles/_schema.md)
```

### 2. Schema definiëren

```
/articles/_schema.md
```

```markdown
# Articles API

## Velden

| Veld | Type | Verplicht | Beschrijving |
|------|------|-----------|--------------|
| title | string | Y | Titel |
| content | string(markdown) | Y | Inhoud |

## Acties

- GET /articles/_index.md - Lijst
- GET /articles/{id}.md - Lezen
- POST /articles/ - Aanmaken
```

### 3. Gegevens leveren

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

- [Eerste bericht](/articles/first.md) | 2025-01-27
- [Tweede bericht](/articles/second.md) | 2025-01-26
```

## Documentatie

Zie [SPECIFICATION.nl.md](SPECIFICATION.nl.md) voor de volledige specificatie.

## Status

Deze standaard is momenteel in **Concept** status. Feedback en bijdragen zijn welkom.

## Bijdragen

Issues en PRs zijn welkom. Bespreek de specificatie in [Issues](https://github.com/TheMagicTower/maidas/issues).

## Licentie

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
