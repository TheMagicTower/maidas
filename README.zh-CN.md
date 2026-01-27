# MAiDAS

**Markdown AI Data Access Standard**

面向AI代理的基于Markdown的Web数据访问标准

---

**翻译**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## 概述

MAiDAS是一个为AI代理设计的标准，使其能够轻松读写Web数据。

传统的基于HTML/JavaScript的Web存在以下问题：
- AI难以解析
- 包含不必要的UI信息
- 结构化数据提取复杂

MAiDAS仅使用Markdown处理所有数据交换。

## 核心原则

- **简洁性**: Markdown + 最少规则
- **标准合规**: HTTP标准方法，RFC 7763 (text/markdown)
- **并行运行**: 可与现有HTML网站并行运行
- **AI优化**: 仅交换数据，无UI

## 快速开始

### 1. 创建入口点

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# My Site

> 网站描述

## API

- [文章管理](/articles/) - [schema](/articles/_schema.md)
```

### 2. 定义Schema

```
/articles/_schema.md
```

```markdown
# Articles API

## 字段

| 字段 | 类型 | 必需 | 描述 |
|------|------|------|------|
| title | string | Y | 标题 |
| content | string(markdown) | Y | 正文 |

## 操作

- GET /articles/_index.md - 列表
- GET /articles/{id}.md - 读取
- POST /articles/ - 创建
```

### 3. 提供数据

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

## 文档

完整规范请参阅[SPECIFICATION.md](SPECIFICATION.md)。

## 状态

本标准目前处于**草案**状态。欢迎反馈和贡献。

## 贡献

欢迎提交Issue和PR。请在[Issues](https://github.com/TheMagicTower/maidas/issues)中讨论规范。

## 许可证

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
