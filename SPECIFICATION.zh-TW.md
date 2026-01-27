# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

面向AI代理的基於Markdown的Web資料存取標準

---

**翻譯**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. 概述

### 1.1 目的

MAiDAS是一個為AI代理設計的標準，使其能夠輕鬆讀寫Web資料。傳統的基於HTML/JavaScript的Web對AI來說難以解析，並且包含不必要的資訊。MAiDAS僅使用Markdown處理所有資料交換。

### 1.2 原則

- **簡潔性**: Markdown + 最少規則
- **標準合規**: HTTP標準方法，RFC 7763 (text/markdown)
- **並行運行**: 可與現有HTML網站並行運行
- **AI優化**: 僅交換資料，無UI

### 1.3 MIME類型

所有請求和回應使用以下Content-Type：

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. 檔案結構

### 2.1 基本結構

```
example.com/
├── .well-known/
│   └── index.md           # 必需。網站入口點
├── articles/
│   ├── _schema.md         # API模式
│   ├── _index.md          # 列表
│   ├── first-post.md      # 單個文件
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 系統檔案

帶有下劃線(`_`)前綴的檔案是系統檔案：

| 檔案 | 用途 |
|------|------|
| `_schema.md` | 資源CRUD模式定義 |
| `_index.md` | 資源列表 |

---

## 3. 入口點

### 3.1 位置

```
/.well-known/index.md
```

### 3.2 格式

```markdown
---
version: 0.1.0
---

# 網站名稱

> 網站描述（一行）

## 頁面

- [關於](/about.md)
- [文章列表](/articles/_index.md)

## API

- [文章管理](/articles/) - [schema](/articles/_schema.md)
- [評論管理](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 欄位

| 欄位 | 必需 | 描述 |
|------|------|------|
| version | Y | MAiDAS規範版本 |

---

## 4. 模式定義

### 4.1 位置

每個資源資料夾內的`_schema.md`

```
/articles/_schema.md
/comments/_schema.md
```

### 4.2 格式

```markdown
# Articles API

## 認證

- 方式: Bearer
- 標頭: `Authorization: Bearer {token}`
- 必需: Y

## 欄位

| 欄位 | 類型 | 必需 | 描述 |
|------|------|------|------|
| title | string | Y | 標題 |
| content | string(markdown) | Y | 正文 |
| tags | array(string) | N | 標籤列表 |
| date | date | N | 建立日期 |

## 操作

- GET /articles/_index.md - 列表
- GET /articles/{id}.md - 讀取
- POST /articles/ - 建立
- PUT /articles/{id}.md - 更新
- DELETE /articles/{id}.md - 刪除
```

### 4.3 類型系統

**基本類型**

| 類型 | 描述 |
|------|------|
| string | 字串 |
| number | 數字 |
| boolean | 布林值 |
| date | 日期 (ISO 8601) |
| array | 陣列 |

**格式提示（可選）**

類型後括號表示：

| 格式 | 描述 |
|------|------|
| string(markdown) | 允許Markdown |
| string(url) | URL格式 |
| string(email) | 郵件格式 |
| date(datetime) | 日期+時間 |
| array(string) | 字串陣列 |
| array(number) | 數字陣列 |

---

## 5. 列表回應

### 5.1 位置

每個資源資料夾內的`_index.md`

### 5.2 格式

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [第一篇文章](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [第二篇文章](/articles/second-post.md) | 2025-01-26 | #diary
- [第三篇文章](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 規則

- 第一行用blockquote(`>`)定義欄位標頭
- 連結後用`|`分隔符列出欄位值
- 空值留空
- frontmatter中包含分頁資訊

---

## 6. 單個文件

### 6.1 格式

```markdown
---
title: 第一篇文章
date: 2025-01-27
tags: [tech, ai]
---

# 第一篇文章

正文內容...
```

### 6.2 規則

- frontmatter: YAML格式（用`---`包裹）
- 正文: Markdown
- 必需欄位: 按`_schema.md`中定義

---

## 7. CRUD請求/回應

### 7.1 建立 (POST)

**請求**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: 新文章
tags: [tech]
---

# 新文章

正文內容...
```

**回應**
```markdown
---
status: 201
---

# Created

文件已建立: /articles/new-post.md
```

### 7.2 讀取 (GET)

**列表**
```
GET /articles/_index.md
```

**單個**
```
GET /articles/first-post.md
```

### 7.3 更新 (PUT)

**請求**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: 更新的標題
tags: [tech, ai]
---

# 更新的標題

更新的正文...
```

**回應**
```markdown
---
status: 200
---

# OK

文件已更新。
```

### 7.4 刪除 (DELETE)

**請求**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**回應**
```markdown
---
status: 200
---

# OK

文件已刪除。
```

---

## 8. 查詢參數

### 8.1 分頁

```
GET /articles/_index.md?page=2&limit=20
```

| 參數 | 預設值 | 描述 |
|------|--------|------|
| page | 1 | 頁碼 |
| limit | 伺服器設定 | 每頁項目數 |

回應frontmatter中包含：
- `page`: 當前頁
- `limit`: 每頁項目數
- `total`: 總項目數

### 8.2 篩選

使用欄位名作為查詢參數：

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
GET /articles/_index.md?tag=tech&date=2025-01
```

### 8.3 排序

```
GET /articles/_index.md?sort=-date,title
```

**規則**
- 使用`sort`參數
- 逗號分隔多個欄位（優先順序）
- `-`前綴: 降序
- 無前綴: 升序

**範例**
```
?sort=date          # date升序
?sort=-date         # date降序
?sort=-date,title   # date降序，相同則title升序
```

### 8.4 組合

所有查詢參數可以組合：

```
GET /articles/_index.md?tag=tech&sort=-date&page=2&limit=10
```

---

## 9. 認證

### 9.1 方式

HTTP標準Bearer權杖認證

```
Authorization: Bearer {token}
```

### 9.2 模式表示

```markdown
## 認證

- 方式: Bearer
- 標頭: `Authorization: Bearer {token}`
- 必需: Y
```

不需要認證時：

```markdown
## 認證

- 必需: N
```

---

## 10. 錯誤處理

### 10.1 格式

所有錯誤回應也是Markdown：

```markdown
---
status: {HTTP狀態碼}
---

# {錯誤標題}

{錯誤描述}
```

### 10.2 狀態碼

| 代碼 | 標題 | 描述 |
|------|------|------|
| 200 | OK | 成功 |
| 201 | Created | 已建立 |
| 400 | Bad Request | 無效請求 |
| 401 | Unauthorized | 需要認證 |
| 403 | Forbidden | 無權限 |
| 404 | Not Found | 文件未找到 |
| 500 | Internal Server Error | 伺服器錯誤 |

### 10.3 範例

**401 Unauthorized**
```markdown
---
status: 401
---

# Unauthorized

需要認證。
```

**400 Bad Request**
```markdown
---
status: 400
---

# Bad Request

title欄位是必需的。
```

**404 Not Found**
```markdown
---
status: 404
---

# Not Found

請求的文件未找到。
```

---

## 11. 完整範例

### 11.1 入口點

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# My Blog

> 個人部落格。

## 頁面

- [關於](/about.md)

## API

- [文章管理](/articles/) - [schema](/articles/_schema.md)
```

### 11.2 模式

`GET /articles/_schema.md`

```markdown
# Articles API

## 認證

- 方式: Bearer
- 標頭: `Authorization: Bearer {token}`
- 必需: Y (建立/更新/刪除), N (讀取)

## 欄位

| 欄位 | 類型 | 必需 | 描述 |
|------|------|------|------|
| title | string | Y | 標題 |
| content | string(markdown) | Y | 正文 |
| tags | array(string) | N | 標籤列表 |
| date | date | N | 建立日期 (預設: 現在) |

## 操作

- GET /articles/_index.md - 列表
- GET /articles/{id}.md - 讀取
- POST /articles/ - 建立
- PUT /articles/{id}.md - 更新
- DELETE /articles/{id}.md - 刪除
```

### 11.3 列表

`GET /articles/_index.md?sort=-date&limit=10`

```markdown
---
page: 1
limit: 10
total: 25
---

# Articles

> title | date | tags

- [MAiDAS標準發布](/articles/maidas-release.md) | 2025-01-27 | #tech, #standard
- [AI Web的未來](/articles/ai-web-future.md) | 2025-01-26 | #ai
- [Markdown技巧](/articles/markdown-tips.md) | 2025-01-25 | #markdown
```

### 11.4 單個文件

`GET /articles/maidas-release.md`

```markdown
---
title: MAiDAS標準發布
date: 2025-01-27
tags: [tech, standard]
---

# MAiDAS標準發布

今天我們發布MAiDAS 0.1.0...
```

---

## 附錄: 版本歷史

| 版本 | 日期 | 變更 |
|------|------|------|
| 0.1.0 | 2025-01-27 | 初始草案發布 |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
