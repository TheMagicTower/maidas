# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

AI ajanları için Markdown tabanlı web veri erişim standardı

---

**Çeviriler**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Genel Bakış

### 1.1 Amaç

MAiDAS, AI ajanlarının web verilerini kolayca okuyup yazabilmesi için tasarlanmış bir standarttır. Geleneksel HTML/JavaScript tabanlı web AI için ayrıştırılması zor ve gereksiz bilgiler içerir. MAiDAS tüm veri alışverişini yalnızca Markdown kullanarak gerçekleştirir.

### 1.2 İlkeler

- **Basitlik**: Markdown + minimum kurallar
- **Standart Uyumluluğu**: HTTP standart yöntemleri, RFC 7763 (text/markdown)
- **Paralel Çalışma**: Mevcut HTML siteleriyle birlikte çalışabilir
- **AI Optimizasyonu**: Yalnızca veri alışverişi, UI yok

### 1.3 MIME Türü

Tüm istekler ve yanıtlar aşağıdaki Content-Type'ı kullanır:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Dosya Yapısı

### 2.1 Temel Yapı

```
example.com/
├── .well-known/
│   └── index.md           # Gerekli. Site giriş noktası
├── articles/
│   ├── _schema.md         # API şeması
│   ├── _index.md          # Liste
│   ├── first-post.md      # Bireysel belge
│   └── second-post.md
└── about.md
```

### 2.2 Sistem Dosyaları

Alt çizgi (`_`) önekli dosyalar sistem dosyalarıdır:

| Dosya | Amaç |
|-------|------|
| `_schema.md` | Kaynak CRUD şema tanımı |
| `_index.md` | Kaynak listesi |

---

## 3. Giriş Noktası

### 3.1 Konum

```
/.well-known/index.md
```

### 3.2 Format

```markdown
---
version: 0.1.0
---

# Site Adı

> Tek satır site açıklaması

## Sayfalar

- [Hakkında](/about.md)
- [Makale Listesi](/articles/_index.md)

## API

- [Makale Yönetimi](/articles/) - [schema](/articles/_schema.md)
```

---

## 4. Şema Tanımı

```markdown
# Articles API

## Kimlik Doğrulama

- Yöntem: Bearer
- Başlık: `Authorization: Bearer {token}`
- Gerekli: Y

## Alanlar

| Alan | Tür | Gerekli | Açıklama |
|------|-----|---------|----------|
| title | string | Y | Başlık |
| content | string(markdown) | Y | İçerik |
| tags | array(string) | N | Etiket listesi |
| date | date | N | Oluşturma tarihi |

## Eylemler

- GET /articles/_index.md - Liste
- GET /articles/{id}.md - Oku
- POST /articles/ - Oluştur
- PUT /articles/{id}.md - Güncelle
- DELETE /articles/{id}.md - Sil
```

---

## 5. Liste Yanıtı

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [İlk Gönderi](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [İkinci Gönderi](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. Bireysel Belge

```markdown
---
title: İlk Gönderi
date: 2025-01-27
tags: [tech, ai]
---

# İlk Gönderi

İçerik...
```

---

## 7. CRUD İstek/Yanıt

### 7.1 Oluştur (POST)

**İstek**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Yeni Gönderi
tags: [tech]
---

# Yeni Gönderi

İçerik...
```

**Yanıt**
```markdown
---
status: 201
---

# Created

Belge oluşturuldu: /articles/new-post.md
```

---

## 8. Sorgu Parametreleri

### 8.1 Sayfalama

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 Sıralama

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. Kimlik Doğrulama

HTTP standart Bearer token kimlik doğrulaması

```
Authorization: Bearer {token}
```

---

## 10. Hata İşleme

| Kod | Başlık | Açıklama |
|-----|--------|----------|
| 200 | OK | Başarılı |
| 201 | Created | Oluşturuldu |
| 400 | Bad Request | Geçersiz istek |
| 401 | Unauthorized | Kimlik doğrulama gerekli |
| 403 | Forbidden | İzin yok |
| 404 | Not Found | Belge bulunamadı |
| 500 | Internal Server Error | Sunucu hatası |

---

## Ek: Sürüm Geçmişi

| Sürüm | Tarih | Değişiklikler |
|-------|-------|---------------|
| 0.1.0 | 2025-01-27 | İlk taslak sürümü |
