# MAiDAS

**Markdown AI Data Access Standard**

面向AI代理的基於Markdown的Web資料存取標準

---

**翻譯**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## 概述

MAiDAS是一個為AI代理設計的標準，使其能夠輕鬆讀寫Web資料。

傳統的基於HTML/JavaScript的Web存在以下問題：
- AI難以解析
- 包含不必要的UI資訊
- 結構化資料提取複雜

MAiDAS僅使用Markdown處理所有資料交換。

## 核心原則

- **簡潔性**: Markdown + 最少規則
- **標準合規**: HTTP標準方法，RFC 7763 (text/markdown)
- **並行運行**: 可與現有HTML網站並行運行
- **AI優化**: 僅交換資料，無UI

## 快速開始

### 1. 建立入口點

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# My Site

> 網站描述

## API

- [文章管理](/articles/) - [schema](/articles/_schema.md)
```

### 2. 定義Schema

```
/articles/_schema.md
```

```markdown
# Articles API

## 欄位

| 欄位 | 類型 | 必需 | 描述 |
|------|------|------|------|
| title | string | Y | 標題 |
| content | string(markdown) | Y | 正文 |

## 操作

- GET /articles/_index.md - 列表
- GET /articles/{id}.md - 讀取
- POST /articles/ - 建立
```

### 3. 提供資料

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

- [第一篇文章](/articles/first.md) | 2025-01-27
- [第二篇文章](/articles/second.md) | 2025-01-26
```

## 文件

完整規範請參閱[SPECIFICATION.md](SPECIFICATION.md)。

## 狀態

本標準目前處於**草案**狀態。歡迎回饋和貢獻。

## 貢獻

歡迎提交Issue和PR。請在[Issues](https://github.com/TheMagicTower/maidas/issues)中討論規範。

## 授權

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
