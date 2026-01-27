# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

面向AI代理的基于Markdown的Web数据访问标准

---

**翻译**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. 概述

### 1.1 目的

MAiDAS是一个为AI代理设计的标准，使其能够轻松读写Web数据。传统的基于HTML/JavaScript的Web对AI来说难以解析，并且包含不必要的信息。MAiDAS仅使用Markdown处理所有数据交换。

### 1.2 原则

- **简洁性**: Markdown + 最少规则
- **标准合规**: HTTP标准方法，RFC 7763 (text/markdown)
- **并行运行**: 可与现有HTML网站并行运行
- **AI优化**: 仅交换数据，无UI

### 1.3 MIME类型

所有请求和响应使用以下Content-Type：

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. 文件结构

### 2.1 基本结构

```
example.com/
├── .well-known/
│   └── index.md           # 必需。网站入口点
├── articles/
│   ├── _schema.md         # API模式
│   ├── _index.md          # 列表
│   ├── first-post.md      # 单个文档
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 系统文件

带有下划线(`_`)前缀的文件是系统文件：

| 文件 | 用途 |
|------|------|
| `_schema.md` | 资源CRUD模式定义 |
| `_index.md` | 资源列表 |

---

## 3. 入口点

### 3.1 位置

```
/.well-known/index.md
```

### 3.2 格式

```markdown
---
version: 0.1.0
---

# 网站名称

> 网站描述（一行）

## 页面

- [关于](/about.md)
- [文章列表](/articles/_index.md)

## API

- [文章管理](/articles/) - [schema](/articles/_schema.md)
- [评论管理](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 字段

| 字段 | 必需 | 描述 |
|------|------|------|
| version | Y | MAiDAS规范版本 |

---

## 4. 模式定义

### 4.1 位置

每个资源文件夹内的`_schema.md`

```
/articles/_schema.md
/comments/_schema.md
```

### 4.2 格式

```markdown
# Articles API

## 认证

- 方式: Bearer
- 头部: `Authorization: Bearer {token}`
- 必需: Y

## 字段

| 字段 | 类型 | 必需 | 描述 |
|------|------|------|------|
| title | string | Y | 标题 |
| content | string(markdown) | Y | 正文 |
| tags | array(string) | N | 标签列表 |
| date | date | N | 创建日期 |

## 操作

- GET /articles/_index.md - 列表
- GET /articles/{id}.md - 读取
- POST /articles/ - 创建
- PUT /articles/{id}.md - 更新
- DELETE /articles/{id}.md - 删除
```

### 4.3 类型系统

**基本类型**

| 类型 | 描述 |
|------|------|
| string | 字符串 |
| number | 数字 |
| boolean | 布尔值 |
| date | 日期 (ISO 8601) |
| array | 数组 |

**格式提示（可选）**

类型后括号表示：

| 格式 | 描述 |
|------|------|
| string(markdown) | 允许Markdown |
| string(url) | URL格式 |
| string(email) | 邮件格式 |
| date(datetime) | 日期+时间 |
| array(string) | 字符串数组 |
| array(number) | 数字数组 |

---

## 5. 列表响应

### 5.1 位置

每个资源文件夹内的`_index.md`

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

### 5.3 规则

- 第一行用blockquote(`>`)定义字段头
- 链接后用`|`分隔符列出字段值
- 空值留空
- frontmatter中包含分页信息

---

## 6. 单个文档

### 6.1 格式

```markdown
---
title: 第一篇文章
date: 2025-01-27
tags: [tech, ai]
---

# 第一篇文章

正文内容...
```

### 6.2 规则

- frontmatter: YAML格式（用`---`包裹）
- 正文: Markdown
- 必需字段: 按`_schema.md`中定义

---

## 7. CRUD请求/响应

### 7.1 创建 (POST)

**请求**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: 新文章
tags: [tech]
---

# 新文章

正文内容...
```

**响应**
```markdown
---
status: 201
---

# Created

文档已创建: /articles/new-post.md
```

### 7.2 读取 (GET)

**列表**
```
GET /articles/_index.md
```

**单个**
```
GET /articles/first-post.md
```

### 7.3 更新 (PUT)

**请求**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: 更新的标题
tags: [tech, ai]
---

# 更新的标题

更新的正文...
```

**响应**
```markdown
---
status: 200
---

# OK

文档已更新。
```

### 7.4 删除 (DELETE)

**请求**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**响应**
```markdown
---
status: 200
---

# OK

文档已删除。
```

---

## 8. 查询参数

### 8.1 分页

```
GET /articles/_index.md?page=2&limit=20
```

| 参数 | 默认值 | 描述 |
|------|--------|------|
| page | 1 | 页码 |
| limit | 服务器配置 | 每页项目数 |

响应frontmatter中包含：
- `page`: 当前页
- `limit`: 每页项目数
- `total`: 总项目数

### 8.2 筛选

使用字段名作为查询参数：

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
GET /articles/_index.md?tag=tech&date=2025-01
```

### 8.3 排序

```
GET /articles/_index.md?sort=-date,title
```

**规则**
- 使用`sort`参数
- 逗号分隔多个字段（优先级顺序）
- `-`前缀: 降序
- 无前缀: 升序

**示例**
```
?sort=date          # date升序
?sort=-date         # date降序
?sort=-date,title   # date降序，相同则title升序
```

### 8.4 组合

所有查询参数可以组合：

```
GET /articles/_index.md?tag=tech&sort=-date&page=2&limit=10
```

---

## 9. 认证

### 9.1 方式

HTTP标准Bearer令牌认证

```
Authorization: Bearer {token}
```

### 9.2 模式表示

```markdown
## 认证

- 方式: Bearer
- 头部: `Authorization: Bearer {token}`
- 必需: Y
```

不需要认证时：

```markdown
## 认证

- 必需: N
```

---

## 10. 错误处理

### 10.1 格式

所有错误响应也是Markdown：

```markdown
---
status: {HTTP状态码}
---

# {错误标题}

{错误描述}
```

### 10.2 状态码

| 代码 | 标题 | 描述 |
|------|------|------|
| 200 | OK | 成功 |
| 201 | Created | 已创建 |
| 400 | Bad Request | 无效请求 |
| 401 | Unauthorized | 需要认证 |
| 403 | Forbidden | 无权限 |
| 404 | Not Found | 文档未找到 |
| 500 | Internal Server Error | 服务器错误 |

### 10.3 示例

**401 Unauthorized**
```markdown
---
status: 401
---

# Unauthorized

需要认证。
```

**400 Bad Request**
```markdown
---
status: 400
---

# Bad Request

title字段是必需的。
```

**404 Not Found**
```markdown
---
status: 404
---

# Not Found

请求的文档未找到。
```

---

## 11. 完整示例

### 11.1 入口点

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# My Blog

> 个人博客。

## 页面

- [关于](/about.md)

## API

- [文章管理](/articles/) - [schema](/articles/_schema.md)
```

### 11.2 模式

`GET /articles/_schema.md`

```markdown
# Articles API

## 认证

- 方式: Bearer
- 头部: `Authorization: Bearer {token}`
- 必需: Y (创建/更新/删除), N (读取)

## 字段

| 字段 | 类型 | 必需 | 描述 |
|------|------|------|------|
| title | string | Y | 标题 |
| content | string(markdown) | Y | 正文 |
| tags | array(string) | N | 标签列表 |
| date | date | N | 创建日期 (默认: 现在) |

## 操作

- GET /articles/_index.md - 列表
- GET /articles/{id}.md - 读取
- POST /articles/ - 创建
- PUT /articles/{id}.md - 更新
- DELETE /articles/{id}.md - 删除
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

- [MAiDAS标准发布](/articles/maidas-release.md) | 2025-01-27 | #tech, #standard
- [AI Web的未来](/articles/ai-web-future.md) | 2025-01-26 | #ai
- [Markdown技巧](/articles/markdown-tips.md) | 2025-01-25 | #markdown
```

### 11.4 单个文档

`GET /articles/maidas-release.md`

```markdown
---
title: MAiDAS标准发布
date: 2025-01-27
tags: [tech, standard]
---

# MAiDAS标准发布

今天我们发布MAiDAS 0.1.0...
```

---

## 附录: 版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| 0.1.0 | 2025-01-27 | 初始草案发布 |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
