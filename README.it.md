# MAiDAS

**Markdown AI Data Access Standard**

Standard di accesso ai dati web basato su Markdown per agenti IA

---

**Traduzioni**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Panoramica

MAiDAS è uno standard progettato per consentire agli agenti IA di leggere e scrivere facilmente dati web.

Il web tradizionale basato su HTML/JavaScript ha problemi:
- Difficile da analizzare per l'IA
- Contiene informazioni UI non necessarie
- Estrazione di dati strutturati complessa

MAiDAS gestisce tutti gli scambi di dati usando solo Markdown.

## Principi fondamentali

- **Semplicità**: Markdown + regole minime
- **Conformità agli standard**: Metodi HTTP standard, RFC 7763 (text/markdown)
- **Operazione parallela**: Può funzionare insieme ai siti HTML esistenti
- **Ottimizzazione IA**: Solo scambio dati, senza interfaccia utente

## Avvio rapido

### 1. Creare punto di ingresso

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Il Mio Sito

> Descrizione del sito

## API

- [Articoli](/articles/) - [schema](/articles/_schema.md)
```

### 2. Definire lo schema

```
/articles/_schema.md
```

```markdown
# Articles API

## Campi

| Campo | Tipo | Obbligatorio | Descrizione |
|-------|------|--------------|-------------|
| title | string | Y | Titolo |
| content | string(markdown) | Y | Contenuto |

## Azioni

- GET /articles/_index.md - Elenco
- GET /articles/{id}.md - Lettura
- POST /articles/ - Creazione
```

### 3. Fornire dati

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

- [Primo post](/articles/first.md) | 2025-01-27
- [Secondo post](/articles/second.md) | 2025-01-26
```

## Documentazione

Vedere [SPECIFICATION.it.md](SPECIFICATION.it.md) per la specifica completa.

## Stato

Questo standard è attualmente in stato di **Bozza**. Feedback e contributi sono benvenuti.

## Contribuire

Issues e PR sono benvenute. Si prega di discutere la specifica in [Issues](https://github.com/TheMagicTower/maidas/issues).

## Licenza

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
