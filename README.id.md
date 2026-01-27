# MAiDAS

**Markdown AI Data Access Standard**

Standar akses data web berbasis Markdown untuk agen AI

---

**Terjemahan**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Gambaran Umum

MAiDAS adalah standar yang dirancang agar agen AI dapat dengan mudah membaca dan menulis data web.

Web tradisional berbasis HTML/JavaScript memiliki masalah:
- Sulit diurai oleh AI
- Mengandung informasi UI yang tidak perlu
- Ekstraksi data terstruktur yang kompleks

MAiDAS menangani semua pertukaran data hanya menggunakan Markdown.

## Prinsip Inti

- **Kesederhanaan**: Markdown + aturan minimal
- **Kepatuhan Standar**: Metode HTTP standar, RFC 7763 (text/markdown)
- **Operasi Paralel**: Dapat berjalan bersama situs HTML yang ada
- **Optimisasi AI**: Hanya pertukaran data, tanpa UI

## Mulai Cepat

### 1. Buat Titik Masuk

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Situs Saya

> Deskripsi situs

## API

- [Artikel](/articles/) - [schema](/articles/_schema.md)
```

### 2. Definisikan Skema

```
/articles/_schema.md
```

```markdown
# Articles API

## Field

| Field | Tipe | Wajib | Deskripsi |
|-------|------|-------|-----------|
| title | string | Y | Judul |
| content | string(markdown) | Y | Konten |

## Aksi

- GET /articles/_index.md - Daftar
- GET /articles/{id}.md - Baca
- POST /articles/ - Buat
```

### 3. Sediakan Data

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

- [Posting Pertama](/articles/first.md) | 2025-01-27
- [Posting Kedua](/articles/second.md) | 2025-01-26
```

## Dokumentasi

Lihat [SPECIFICATION.id.md](SPECIFICATION.id.md) untuk spesifikasi lengkap.

## Status

Standar ini saat ini dalam status **Draft**. Umpan balik dan kontribusi sangat diterima.

## Kontribusi

Issues dan PR sangat diterima. Silakan diskusikan spesifikasi di [Issues](https://github.com/TheMagicTower/maidas/issues).

## Lisensi

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
