# MAiDAS

**Markdown AI Data Access Standard**

AIエージェント向けのMarkdownベースWebデータアクセス標準

---

**翻訳**: [English](README.md) | [한국어](README.ko.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## 概要

MAiDASは、AIエージェントがWebデータを簡単に読み書きできるように設計された標準です。

従来のHTML/JavaScriptベースのWebには問題があります：
- AIがパースしにくい
- 不要なUI情報が含まれている
- 構造化データの抽出が複雑

MAiDASはMarkdownのみですべてのデータ交換を処理します。

## 基本原則

- **シンプルさ**: Markdown + 最小限のルール
- **標準準拠**: HTTP標準メソッド、RFC 7763 (text/markdown)
- **並行運用**: 既存のHTMLサイトと並行して運用可能
- **AI最適化**: UIなしでデータのみを交換

## クイックスタート

### 1. エントリーポイントの作成

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# My Site

> サイトの説明

## API

- [記事管理](/articles/) - [schema](/articles/_schema.md)
```

### 2. スキーマの定義

```
/articles/_schema.md
```

```markdown
# Articles API

## フィールド

| フィールド | タイプ | 必須 | 説明 |
|------------|--------|------|------|
| title | string | Y | タイトル |
| content | string(markdown) | Y | 本文 |

## アクション

- GET /articles/_index.md - 一覧取得
- GET /articles/{id}.md - 個別取得
- POST /articles/ - 作成
```

### 3. データの提供

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

- [最初の投稿](/articles/first.md) | 2025-01-27
- [2番目の投稿](/articles/second.md) | 2025-01-26
```

## ドキュメント

完全な仕様は[SPECIFICATION.md](SPECIFICATION.md)を参照してください。

## ステータス

この標準は現在**ドラフト**状態です。フィードバックと貢献を歓迎します。

## 貢献

IssueとPRを歓迎します。仕様に関する議論は[Issues](https://github.com/TheMagicTower/maidas/issues)で行ってください。

## ライセンス

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
