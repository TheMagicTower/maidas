# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

AIエージェント向けのMarkdownベースWebデータアクセス標準

---

**翻訳**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. 概要

### 1.1 目的

MAiDASは、AIエージェントがWebデータを簡単に読み書きできるように設計された標準です。従来のHTML/JavaScriptベースのWebはAIがパースしにくく、不要な情報が含まれています。MAiDASはMarkdownのみですべてのデータ交換を処理します。

### 1.2 原則

- **シンプルさ**: Markdown + 最小限のルール
- **標準準拠**: HTTP標準メソッド、RFC 7763 (text/markdown)
- **並行運用**: 既存のHTMLサイトと並行して運用可能
- **AI最適化**: UIなしでデータのみを交換

### 1.3 MIMEタイプ

すべてのリクエストとレスポンスは以下のContent-Typeを使用します：

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. ファイル構造

### 2.1 基本構造

```
example.com/
├── .well-known/
│   └── index.md           # 必須。サイトエントリーポイント
├── articles/
│   ├── _schema.md         # APIスキーマ
│   ├── _index.md          # 一覧
│   ├── first-post.md      # 個別ドキュメント
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 システムファイル

アンダースコア(`_`)プレフィックスが付いたファイルはシステムファイルです：

| ファイル | 用途 |
|----------|------|
| `_schema.md` | リソースCRUDスキーマ定義 |
| `_index.md` | リソース一覧 |

---

## 3. エントリーポイント

### 3.1 場所

```
/.well-known/index.md
```

### 3.2 形式

```markdown
---
version: 0.1.0
---

# サイト名

> サイトの説明（一行）

## ページ

- [紹介](/about.md)
- [記事一覧](/articles/_index.md)

## API

- [記事管理](/articles/) - [schema](/articles/_schema.md)
- [コメント管理](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 フィールド

| フィールド | 必須 | 説明 |
|------------|------|------|
| version | Y | MAiDASスペックバージョン |

---

## 4. スキーマ定義

### 4.1 場所

各リソースフォルダ内の`_schema.md`

```
/articles/_schema.md
/comments/_schema.md
```

### 4.2 形式

```markdown
# Articles API

## 認証

- 方式: Bearer
- ヘッダー: `Authorization: Bearer {token}`
- 必須: Y

## フィールド

| フィールド | タイプ | 必須 | 説明 |
|------------|--------|------|------|
| title | string | Y | タイトル |
| content | string(markdown) | Y | 本文 |
| tags | array(string) | N | タグ一覧 |
| date | date | N | 作成日 |

## アクション

- GET /articles/_index.md - 一覧取得
- GET /articles/{id}.md - 個別取得
- POST /articles/ - 作成
- PUT /articles/{id}.md - 更新
- DELETE /articles/{id}.md - 削除
```

### 4.3 タイプシステム

**基本タイプ**

| タイプ | 説明 |
|--------|------|
| string | 文字列 |
| number | 数値 |
| boolean | 真偽値 |
| date | 日付 (ISO 8601) |
| array | 配列 |

**フォーマットヒント（オプション）**

タイプの後に括弧で表記：

| フォーマット | 説明 |
|--------------|------|
| string(markdown) | Markdown許可 |
| string(url) | URL形式 |
| string(email) | メール形式 |
| date(datetime) | 日付+時刻 |
| array(string) | 文字列配列 |
| array(number) | 数値配列 |

---

## 5. 一覧レスポンス

### 5.1 場所

各リソースフォルダ内の`_index.md`

### 5.2 形式

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [最初の投稿](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [2番目の投稿](/articles/second-post.md) | 2025-01-26 | #diary
- [3番目の投稿](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 ルール

- 最初の行でblockquote(`>`)を使用してフィールドヘッダーを定義
- リンクの後に`|`区切りでフィールド値を列挙
- 空の値は空白のまま
- frontmatterにページネーション情報

---

## 6. 個別ドキュメント

### 6.1 形式

```markdown
---
title: 最初の投稿
date: 2025-01-27
tags: [tech, ai]
---

# 最初の投稿

本文内容...
```

### 6.2 ルール

- frontmatter: YAML形式（`---`で囲む）
- 本文: Markdown
- 必須フィールド: `_schema.md`で定義された通り

---

## 7. CRUDリクエスト/レスポンス

### 7.1 作成 (POST)

**リクエスト**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: 新しい投稿
tags: [tech]
---

# 新しい投稿

本文内容...
```

**レスポンス**
```markdown
---
status: 201
---

# Created

ドキュメントが作成されました: /articles/new-post.md
```

### 7.2 取得 (GET)

**一覧取得**
```
GET /articles/_index.md
```

**個別取得**
```
GET /articles/first-post.md
```

### 7.3 更新 (PUT)

**リクエスト**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: 更新されたタイトル
tags: [tech, ai]
---

# 更新されたタイトル

更新された本文...
```

**レスポンス**
```markdown
---
status: 200
---

# OK

ドキュメントが更新されました。
```

### 7.4 削除 (DELETE)

**リクエスト**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**レスポンス**
```markdown
---
status: 200
---

# OK

ドキュメントが削除されました。
```

---

## 8. クエリパラメータ

### 8.1 ページネーション

```
GET /articles/_index.md?page=2&limit=20
```

| パラメータ | デフォルト | 説明 |
|------------|------------|------|
| page | 1 | ページ番号 |
| limit | サーバー設定 | ページあたりの項目数 |

レスポンスfrontmatterに含まれる：
- `page`: 現在のページ
- `limit`: ページあたりの項目数
- `total`: 総項目数

### 8.2 フィルター

フィールド名をクエリパラメータとして使用：

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
GET /articles/_index.md?tag=tech&date=2025-01
```

### 8.3 ソート

```
GET /articles/_index.md?sort=-date,title
```

**ルール**
- `sort`パラメータを使用
- コンマで複数フィールド（優先順位順）
- `-`プレフィックス: 降順
- プレフィックスなし: 昇順

**例**
```
?sort=date          # date昇順
?sort=-date         # date降順
?sort=-date,title   # date降順、同じならtitle昇順
```

### 8.4 組み合わせ

すべてのクエリパラメータは組み合わせ可能：

```
GET /articles/_index.md?tag=tech&sort=-date&page=2&limit=10
```

---

## 9. 認証

### 9.1 方式

HTTP標準Bearerトークン認証

```
Authorization: Bearer {token}
```

### 9.2 スキーマ表記

```markdown
## 認証

- 方式: Bearer
- ヘッダー: `Authorization: Bearer {token}`
- 必須: Y
```

認証が不要な場合：

```markdown
## 認証

- 必須: N
```

---

## 10. エラー処理

### 10.1 形式

すべてのエラーレスポンスもMarkdown：

```markdown
---
status: {HTTPステータスコード}
---

# {エラータイトル}

{エラー説明}
```

### 10.2 ステータスコード

| コード | タイトル | 説明 |
|--------|----------|------|
| 200 | OK | 成功 |
| 201 | Created | 作成済み |
| 400 | Bad Request | 不正なリクエスト |
| 401 | Unauthorized | 認証必要 |
| 403 | Forbidden | 権限なし |
| 404 | Not Found | ドキュメントなし |
| 500 | Internal Server Error | サーバーエラー |

### 10.3 例

**401 Unauthorized**
```markdown
---
status: 401
---

# Unauthorized

認証が必要です。
```

**400 Bad Request**
```markdown
---
status: 400
---

# Bad Request

titleフィールドは必須です。
```

**404 Not Found**
```markdown
---
status: 404
---

# Not Found

リクエストされたドキュメントが見つかりません。
```

---

## 11. 完全な例

### 11.1 エントリーポイント

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# My Blog

> 個人ブログです。

## ページ

- [紹介](/about.md)

## API

- [記事管理](/articles/) - [schema](/articles/_schema.md)
```

### 11.2 スキーマ

`GET /articles/_schema.md`

```markdown
# Articles API

## 認証

- 方式: Bearer
- ヘッダー: `Authorization: Bearer {token}`
- 必須: Y (作成/更新/削除), N (取得)

## フィールド

| フィールド | タイプ | 必須 | 説明 |
|------------|--------|------|------|
| title | string | Y | タイトル |
| content | string(markdown) | Y | 本文 |
| tags | array(string) | N | タグ一覧 |
| date | date | N | 作成日 (デフォルト: 現在) |

## アクション

- GET /articles/_index.md - 一覧取得
- GET /articles/{id}.md - 個別取得
- POST /articles/ - 作成
- PUT /articles/{id}.md - 更新
- DELETE /articles/{id}.md - 削除
```

### 11.3 一覧

`GET /articles/_index.md?sort=-date&limit=10`

```markdown
---
page: 1
limit: 10
total: 25
---

# Articles

> title | date | tags

- [MAiDAS標準リリース](/articles/maidas-release.md) | 2025-01-27 | #tech, #standard
- [AI Webの未来](/articles/ai-web-future.md) | 2025-01-26 | #ai
- [Markdownの活用法](/articles/markdown-tips.md) | 2025-01-25 | #markdown
```

### 11.4 個別ドキュメント

`GET /articles/maidas-release.md`

```markdown
---
title: MAiDAS標準リリース
date: 2025-01-27
tags: [tech, standard]
---

# MAiDAS標準リリース

本日MAiDAS 0.1.0をリリースします...
```

---

## 付録: バージョン履歴

| バージョン | 日付 | 変更内容 |
|------------|------|----------|
| 0.1.0 | 2025-01-27 | 初回ドラフトリリース |
