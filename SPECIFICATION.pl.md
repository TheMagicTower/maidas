# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Standard dostępu do danych webowych oparty na Markdown dla agentów AI

---

**Tłumaczenia**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Przegląd

### 1.1 Cel

MAiDAS to standard zaprojektowany, aby agenci AI mogli łatwo odczytywać i zapisywać dane webowe. Tradycyjny web oparty na HTML/JavaScript jest trudny do parsowania dla AI i zawiera niepotrzebne informacje. MAiDAS obsługuje całą wymianę danych używając tylko Markdown.

### 1.2 Zasady

- **Prostota**: Markdown + minimalne reguły
- **Zgodność ze standardami**: Standardowe metody HTTP, RFC 7763 (text/markdown)
- **Równoległa praca**: Może działać obok istniejących stron HTML
- **Optymalizacja AI**: Tylko wymiana danych, bez UI

### 1.3 Typ MIME

Wszystkie żądania i odpowiedzi używają następującego Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Struktura plików

### 2.1 Podstawowa struktura

```
example.com/
├── .well-known/
│   └── index.md           # Wymagany. Punkt wejścia strony
├── articles/
│   ├── _schema.md         # Schemat API
│   ├── _index.md          # Lista
│   ├── first-post.md      # Pojedynczy dokument
│   └── second-post.md
└── about.md
```

### 2.2 Pliki systemowe

Pliki z prefiksem underscore (`_`) to pliki systemowe:

| Plik | Cel |
|------|-----|
| `_schema.md` | Definicja schematu CRUD zasobów |
| `_index.md` | Lista zasobów |

---

## 3. Punkt wejścia

### 3.1 Lokalizacja

```
/.well-known/index.md
```

### 3.2 Format

```markdown
---
version: 0.1.0
---

# Nazwa strony

> Opis strony w jednej linii

## Strony

- [O nas](/about.md)
- [Lista artykułów](/articles/_index.md)

## API

- [Zarządzanie artykułami](/articles/) - [schema](/articles/_schema.md)
```

---

## 4. Definicja schematu

```markdown
# Articles API

## Uwierzytelnianie

- Metoda: Bearer
- Nagłówek: `Authorization: Bearer {token}`
- Wymagane: Y

## Pola

| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| title | string | Y | Tytuł |
| content | string(markdown) | Y | Treść |
| tags | array(string) | N | Lista tagów |
| date | date | N | Data utworzenia |

## Akcje

- GET /articles/_index.md - Lista
- GET /articles/{id}.md - Odczyt
- POST /articles/ - Tworzenie
- PUT /articles/{id}.md - Aktualizacja
- DELETE /articles/{id}.md - Usunięcie
```

---

## 5. Odpowiedź listy

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Pierwszy post](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Drugi post](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. Pojedynczy dokument

```markdown
---
title: Pierwszy post
date: 2025-01-27
tags: [tech, ai]
---

# Pierwszy post

Treść...
```

---

## 7. Żądanie/Odpowiedź CRUD

### 7.1 Tworzenie (POST)

**Żądanie**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Nowy post
tags: [tech]
---

# Nowy post

Treść...
```

**Odpowiedź**
```markdown
---
status: 201
---

# Created

Dokument utworzony: /articles/new-post.md
```

---

## 8. Parametry zapytania

### 8.1 Paginacja

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 Sortowanie

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. Uwierzytelnianie

Standardowe uwierzytelnianie Bearer token HTTP

```
Authorization: Bearer {token}
```

---

## 10. Obsługa błędów

| Kod | Tytuł | Opis |
|-----|-------|------|
| 200 | OK | Sukces |
| 201 | Created | Utworzono |
| 400 | Bad Request | Nieprawidłowe żądanie |
| 401 | Unauthorized | Wymagane uwierzytelnianie |
| 403 | Forbidden | Brak uprawnień |
| 404 | Not Found | Nie znaleziono dokumentu |
| 500 | Internal Server Error | Błąd serwera |

---

## Dodatek: Historia wersji

| Wersja | Data | Zmiany |
|--------|------|--------|
| 0.1.0 | 2025-01-27 | Początkowe wydanie szkicu |
