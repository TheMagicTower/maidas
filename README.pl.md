# MAiDAS

**Markdown AI Data Access Standard**

Standard dostępu do danych webowych oparty na Markdown dla agentów AI

---

**Tłumaczenia**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Przegląd

MAiDAS to standard zaprojektowany, aby agenci AI mogli łatwo odczytywać i zapisywać dane webowe.

Tradycyjny web oparty na HTML/JavaScript ma problemy:
- Trudny do parsowania dla AI
- Zawiera niepotrzebne informacje UI
- Złożona ekstrakcja danych strukturalnych

MAiDAS obsługuje całą wymianę danych używając tylko Markdown.

## Podstawowe zasady

- **Prostota**: Markdown + minimalne reguły
- **Zgodność ze standardami**: Standardowe metody HTTP, RFC 7763 (text/markdown)
- **Równoległa praca**: Może działać obok istniejących stron HTML
- **Optymalizacja AI**: Tylko wymiana danych, bez UI

## Szybki start

### 1. Utwórz punkt wejścia

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Moja Strona

> Opis strony

## API

- [Artykuły](/articles/) - [schema](/articles/_schema.md)
```

### 2. Zdefiniuj schemat

```
/articles/_schema.md
```

```markdown
# Articles API

## Pola

| Pole | Typ | Wymagane | Opis |
|------|-----|----------|------|
| title | string | Y | Tytuł |
| content | string(markdown) | Y | Treść |

## Akcje

- GET /articles/_index.md - Lista
- GET /articles/{id}.md - Odczyt
- POST /articles/ - Tworzenie
```

### 3. Dostarcz dane

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

- [Pierwszy post](/articles/first.md) | 2025-01-27
- [Drugi post](/articles/second.md) | 2025-01-26
```

## Dokumentacja

Zobacz [SPECIFICATION.pl.md](SPECIFICATION.pl.md) dla pełnej specyfikacji.

## Status

Ten standard jest obecnie w statusie **Szkic**. Opinie i wkład są mile widziane.

## Współtworzenie

Issues i PR są mile widziane. Proszę dyskutować o specyfikacji w [Issues](https://github.com/TheMagicTower/maidas/issues).

## Licencja

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
