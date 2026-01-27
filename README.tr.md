# MAiDAS

**Markdown AI Data Access Standard**

AI ajanları için Markdown tabanlı web veri erişim standardı

---

**Çeviriler**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Genel Bakış

MAiDAS, AI ajanlarının web verilerini kolayca okuyup yazabilmesi için tasarlanmış bir standarttır.

Geleneksel HTML/JavaScript tabanlı web'in sorunları vardır:
- AI için ayrıştırılması zor
- Gereksiz UI bilgileri içerir
- Yapılandırılmış veri çıkarımı karmaşık

MAiDAS, tüm veri alışverişini yalnızca Markdown kullanarak gerçekleştirir.

## Temel İlkeler

- **Basitlik**: Markdown + minimum kurallar
- **Standart Uyumluluğu**: HTTP standart yöntemleri, RFC 7763 (text/markdown)
- **Paralel Çalışma**: Mevcut HTML siteleriyle birlikte çalışabilir
- **AI Optimizasyonu**: Yalnızca veri alışverişi, UI yok

## Hızlı Başlangıç

### 1. Giriş Noktası Oluştur

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Sitem

> Site açıklaması

## API

- [Makaleler](/articles/) - [schema](/articles/_schema.md)
```

### 2. Şema Tanımla

```
/articles/_schema.md
```

```markdown
# Articles API

## Alanlar

| Alan | Tür | Gerekli | Açıklama |
|------|-----|---------|----------|
| title | string | Y | Başlık |
| content | string(markdown) | Y | İçerik |

## Eylemler

- GET /articles/_index.md - Liste
- GET /articles/{id}.md - Oku
- POST /articles/ - Oluştur
```

### 3. Veri Sağla

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

- [İlk Gönderi](/articles/first.md) | 2025-01-27
- [İkinci Gönderi](/articles/second.md) | 2025-01-26
```

## Dokümantasyon

Tam spesifikasyon için [SPECIFICATION.tr.md](SPECIFICATION.tr.md) dosyasına bakın.

## Durum

Bu standart şu anda **Taslak** durumundadır. Geri bildirim ve katkılar memnuniyetle karşılanır.

## Katkıda Bulunma

Issues ve PR'ler memnuniyetle karşılanır. Lütfen spesifikasyonu [Issues](https://github.com/TheMagicTower/maidas/issues) bölümünde tartışın.

## Lisans

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
