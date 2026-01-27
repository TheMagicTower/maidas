# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Standar akses data web berbasis Markdown untuk agen AI

---

**Terjemahan**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Gambaran Umum

### 1.1 Tujuan

MAiDAS adalah standar yang dirancang agar agen AI dapat dengan mudah membaca dan menulis data web. Web tradisional berbasis HTML/JavaScript sulit diurai oleh AI dan mengandung informasi yang tidak perlu. MAiDAS menangani semua pertukaran data hanya menggunakan Markdown.

### 1.2 Prinsip

- **Kesederhanaan**: Markdown + aturan minimal
- **Kepatuhan Standar**: Metode HTTP standar, RFC 7763 (text/markdown)
- **Operasi Paralel**: Dapat berjalan bersama situs HTML yang ada
- **Optimisasi AI**: Hanya pertukaran data, tanpa UI

### 1.3 Tipe MIME

Semua permintaan dan respons menggunakan Content-Type berikut:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Struktur File

### 2.1 Struktur Dasar

```
example.com/
├── .well-known/
│   └── index.md           # Wajib. Titik masuk situs
├── articles/
│   ├── _schema.md         # Schema API
│   ├── _index.md          # Daftar
│   ├── first-post.md      # Dokumen individual
│   └── second-post.md
└── about.md
```

### 2.2 File Sistem

File dengan awalan underscore (`_`) adalah file sistem:

| File | Tujuan |
|------|--------|
| `_schema.md` | Definisi schema CRUD sumber daya |
| `_index.md` | Daftar sumber daya |

---

## 3. Titik Masuk

### 3.1 Lokasi

```
/.well-known/index.md
```

### 3.2 Format

```markdown
---
version: 0.1.0
---

# Nama Situs

> Deskripsi situs satu baris

## Halaman

- [Tentang](/about.md)
- [Daftar Artikel](/articles/_index.md)

## API

- [Manajemen Artikel](/articles/) - [schema](/articles/_schema.md)
```

---

## 4. Definisi Schema

```markdown
# Articles API

## Autentikasi

- Metode: Bearer
- Header: `Authorization: Bearer {token}`
- Wajib: Y

## Field

| Field | Tipe | Wajib | Deskripsi |
|-------|------|-------|-----------|
| title | string | Y | Judul |
| content | string(markdown) | Y | Konten |
| tags | array(string) | N | Daftar tag |
| date | date | N | Tanggal pembuatan |

## Aksi

- GET /articles/_index.md - Daftar
- GET /articles/{id}.md - Baca
- POST /articles/ - Buat
- PUT /articles/{id}.md - Perbarui
- DELETE /articles/{id}.md - Hapus
```

---

## 5. Respons Daftar

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Posting Pertama](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Posting Kedua](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. Dokumen Individual

```markdown
---
title: Posting Pertama
date: 2025-01-27
tags: [tech, ai]
---

# Posting Pertama

Konten...
```

---

## 7. Permintaan/Respons CRUD

### 7.1 Buat (POST)

**Permintaan**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Posting Baru
tags: [tech]
---

# Posting Baru

Konten...
```

**Respons**
```markdown
---
status: 201
---

# Created

Dokumen dibuat: /articles/new-post.md
```

---

## 8. Parameter Query

### 8.1 Paginasi

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 Pengurutan

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. Autentikasi

Autentikasi Bearer token HTTP standar

```
Authorization: Bearer {token}
```

---

## 10. Penanganan Error

| Kode | Judul | Deskripsi |
|------|-------|-----------|
| 200 | OK | Sukses |
| 201 | Created | Dibuat |
| 400 | Bad Request | Permintaan tidak valid |
| 401 | Unauthorized | Autentikasi diperlukan |
| 403 | Forbidden | Tidak ada izin |
| 404 | Not Found | Dokumen tidak ditemukan |
| 500 | Internal Server Error | Error server |

---

## Lampiran: Riwayat Versi

| Versi | Tanggal | Perubahan |
|-------|---------|-----------|
| 0.1.0 | 2025-01-27 | Rilis draft awal |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
