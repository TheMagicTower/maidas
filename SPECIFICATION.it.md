# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Standard di accesso ai dati web basato su Markdown per agenti IA

---

**Traduzioni**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Panoramica

### 1.1 Scopo

MAiDAS è uno standard progettato per consentire agli agenti IA di leggere e scrivere facilmente dati web. Il web tradizionale basato su HTML/JavaScript è difficile da analizzare per l'IA e contiene informazioni non necessarie. MAiDAS gestisce tutti gli scambi di dati usando solo Markdown.

### 1.2 Principi

- **Semplicità**: Markdown + regole minime
- **Conformità agli standard**: Metodi HTTP standard, RFC 7763 (text/markdown)
- **Operazione parallela**: Può funzionare insieme ai siti HTML esistenti
- **Ottimizzazione IA**: Solo scambio dati, senza interfaccia utente

### 1.3 Tipo MIME

Tutte le richieste e risposte usano il seguente Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Struttura dei File

### 2.1 Struttura Base

```
example.com/
├── .well-known/
│   └── index.md           # Richiesto. Punto di ingresso del sito
├── articles/
│   ├── _schema.md         # Schema API
│   ├── _index.md          # Elenco
│   ├── first-post.md      # Documento singolo
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 File di Sistema

I file con prefisso underscore (`_`) sono file di sistema:

| File | Scopo |
|------|-------|
| `_schema.md` | Definizione schema CRUD risorse |
| `_index.md` | Elenco risorse |

---

## 3. Punto di Ingresso

### 3.1 Posizione

```
/.well-known/index.md
```

### 3.2 Formato

```markdown
---
version: 0.1.0
---

# Nome Sito

> Descrizione del sito in una riga

## Pagine

- [Informazioni](/about.md)
- [Elenco Articoli](/articles/_index.md)

## API

- [Gestione Articoli](/articles/) - [schema](/articles/_schema.md)
- [Gestione Commenti](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 Campi

| Campo | Richiesto | Descrizione |
|-------|-----------|-------------|
| version | Y | Versione specifica MAiDAS |

---

## 4. Definizione Schema

### 4.1 Posizione

`_schema.md` in ogni cartella risorse

### 4.2 Formato

```markdown
# Articles API

## Autenticazione

- Metodo: Bearer
- Header: `Authorization: Bearer {token}`
- Richiesto: Y

## Campi

| Campo | Tipo | Richiesto | Descrizione |
|-------|------|-----------|-------------|
| title | string | Y | Titolo |
| content | string(markdown) | Y | Contenuto |
| tags | array(string) | N | Lista tag |
| date | date | N | Data creazione |

## Azioni

- GET /articles/_index.md - Elenco
- GET /articles/{id}.md - Lettura
- POST /articles/ - Creazione
- PUT /articles/{id}.md - Aggiornamento
- DELETE /articles/{id}.md - Eliminazione
```

### 4.3 Sistema di Tipi

**Tipi Base**

| Tipo | Descrizione |
|------|-------------|
| string | Stringa |
| number | Numero |
| boolean | Vero/Falso |
| date | Data (ISO 8601) |
| array | Array |

---

## 5. Risposta Elenco

### 5.1 Posizione

`_index.md` in ogni cartella risorse

### 5.2 Formato

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Primo Post](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Secondo Post](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. Documento Singolo

### 6.1 Formato

```markdown
---
title: Primo Post
date: 2025-01-27
tags: [tech, ai]
---

# Primo Post

Contenuto...
```

---

## 7. Richiesta/Risposta CRUD

### 7.1 Creazione (POST)

**Richiesta**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Nuovo Post
tags: [tech]
---

# Nuovo Post

Contenuto...
```

**Risposta**
```markdown
---
status: 201
---

# Created

Documento creato: /articles/new-post.md
```

---

## 8. Parametri Query

### 8.1 Paginazione

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 Ordinamento

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. Autenticazione

HTTP standard Bearer token

```
Authorization: Bearer {token}
```

---

## 10. Gestione Errori

| Codice | Titolo | Descrizione |
|--------|--------|-------------|
| 200 | OK | Successo |
| 201 | Created | Creato |
| 400 | Bad Request | Richiesta non valida |
| 401 | Unauthorized | Autenticazione richiesta |
| 403 | Forbidden | Nessun permesso |
| 404 | Not Found | Documento non trovato |
| 500 | Internal Server Error | Errore server |

---

## Appendice: Cronologia Versioni

| Versione | Data | Modifiche |
|----------|------|-----------|
| 0.1.0 | 2025-01-27 | Rilascio bozza iniziale |
