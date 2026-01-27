# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

AI 에이전트를 위한 마크다운 기반 웹 데이터 접근 표준

---

**번역**: [English](SPECIFICATION.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. 개요

### 1.1 목적

MAiDAS는 AI 에이전트가 웹 데이터를 쉽게 읽고 쓸 수 있도록 설계된 표준입니다. 기존 HTML/JavaScript 기반 웹은 AI가 파싱하기 어렵고 불필요한 정보가 많습니다. MAiDAS는 마크다운만으로 모든 데이터 교환을 처리합니다.

### 1.2 원칙

- **단순함**: 마크다운 + 최소한의 규칙
- **표준 준수**: HTTP 표준 메서드, RFC 7763 (text/markdown)
- **병행 운영**: 기존 HTML 사이트와 함께 운영 가능
- **AI 최적화**: UI 없이 데이터만 주고받음

### 1.3 MIME 타입

모든 요청과 응답은 다음 Content-Type을 사용합니다:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. 파일 구조

### 2.1 기본 구조

```
example.com/
├── .well-known/
│   └── index.md           # 필수. 사이트 진입점
├── articles/
│   ├── _schema.md         # API 스키마
│   ├── _index.md          # 목록
│   ├── first-post.md      # 개별 문서
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 시스템 파일

언더스코어(`_`) prefix가 붙은 파일은 시스템 파일입니다:

| 파일 | 용도 |
|------|------|
| `_schema.md` | 리소스 CRUD 스키마 정의 |
| `_index.md` | 리소스 목록 |

---

## 3. 진입점

### 3.1 위치

```
/.well-known/index.md
```

### 3.2 형식

```markdown
---
version: 0.1.0
---

# 사이트명

> 사이트 설명 한 줄

## 페이지

- [소개](/about.md)
- [글 목록](/articles/_index.md)

## API

- [글 관리](/articles/) - [schema](/articles/_schema.md)
- [댓글 관리](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 필드

| 필드 | 필수 | 설명 |
|------|------|------|
| version | Y | MAiDAS 스펙 버전 |

---

## 4. 스키마 정의

### 4.1 위치

각 리소스 폴더 내 `_schema.md`

```
/articles/_schema.md
/comments/_schema.md
```

### 4.2 형식

```markdown
# Articles API

## 인증

- 방식: Bearer
- 헤더: `Authorization: Bearer {token}`
- 필수: Y

## 필드

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| title | string | Y | 제목 |
| content | string(markdown) | Y | 본문 |
| tags | array(string) | N | 태그 목록 |
| date | date | N | 작성일 |

## 액션

- GET /articles/_index.md - 목록 조회
- GET /articles/{id}.md - 개별 조회
- POST /articles/ - 생성
- PUT /articles/{id}.md - 수정
- DELETE /articles/{id}.md - 삭제
```

### 4.3 타입 시스템

**기본 타입**

| 타입 | 설명 |
|------|------|
| string | 문자열 |
| number | 숫자 |
| boolean | 참/거짓 |
| date | 날짜 (ISO 8601) |
| array | 배열 |

**포맷 힌트 (선택)**

타입 뒤 괄호로 표기:

| 포맷 | 설명 |
|------|------|
| string(markdown) | 마크다운 허용 |
| string(url) | URL 형식 |
| string(email) | 이메일 형식 |
| date(datetime) | 날짜+시간 |
| array(string) | 문자열 배열 |
| array(number) | 숫자 배열 |

---

## 5. 목록 응답

### 5.1 위치

각 리소스 폴더 내 `_index.md`

### 5.2 형식

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [첫 번째 글](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [두 번째 글](/articles/second-post.md) | 2025-01-26 | #diary
- [세 번째 글](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 규칙

- 첫 줄 blockquote(`>`)로 필드 헤더 정의
- 링크 뒤에 `|` 구분자로 필드값 나열
- 빈 값은 비워둠
- frontmatter에 페이지네이션 정보

---

## 6. 개별 문서

### 6.1 형식

```markdown
---
title: 첫 번째 글
date: 2025-01-27
tags: [tech, ai]
---

# 첫 번째 글

본문 내용...
```

### 6.2 규칙

- frontmatter: YAML 형식 (`---`로 감싸기)
- 본문: 마크다운
- 필수 필드: `_schema.md`에서 정의한 대로

---

## 7. CRUD 요청/응답

### 7.1 생성 (POST)

**요청**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: 새 글
tags: [tech]
---

# 새 글

본문 내용...
```

**응답**
```markdown
---
status: 201
---

# Created

문서가 생성되었습니다: /articles/new-post.md
```

### 7.2 조회 (GET)

**목록 조회**
```
GET /articles/_index.md
```

**개별 조회**
```
GET /articles/first-post.md
```

### 7.3 수정 (PUT)

**요청**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: 수정된 제목
tags: [tech, ai]
---

# 수정된 제목

수정된 본문...
```

**응답**
```markdown
---
status: 200
---

# OK

문서가 수정되었습니다.
```

### 7.4 삭제 (DELETE)

**요청**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**응답**
```markdown
---
status: 200
---

# OK

문서가 삭제되었습니다.
```

---

## 8. 쿼리 파라미터

### 8.1 페이지네이션

```
GET /articles/_index.md?page=2&limit=20
```

| 파라미터 | 기본값 | 설명 |
|----------|--------|------|
| page | 1 | 페이지 번호 |
| limit | 서버 설정 | 페이지당 항목 수 |

응답 frontmatter에 포함:
- `page`: 현재 페이지
- `limit`: 페이지당 항목 수
- `total`: 전체 항목 수

### 8.2 필터

필드명을 쿼리 파라미터로 사용:

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
GET /articles/_index.md?tag=tech&date=2025-01
```

### 8.3 정렬

```
GET /articles/_index.md?sort=-date,title
```

**규칙**
- `sort` 파라미터 사용
- 콤마로 다중 필드 (우선순위 순)
- `-` prefix: 내림차순
- prefix 없음: 오름차순

**예시**
```
?sort=date          # date 오름차순
?sort=-date         # date 내림차순
?sort=-date,title   # date 내림차순, 같으면 title 오름차순
```

### 8.4 조합

모든 쿼리 파라미터는 조합 가능:

```
GET /articles/_index.md?tag=tech&sort=-date&page=2&limit=10
```

---

## 9. 인증

### 9.1 방식

HTTP 표준 Bearer 토큰 인증

```
Authorization: Bearer {token}
```

### 9.2 스키마 표기

```markdown
## 인증

- 방식: Bearer
- 헤더: `Authorization: Bearer {token}`
- 필수: Y
```

인증이 필요 없는 경우:

```markdown
## 인증

- 필수: N
```

---

## 10. 에러 처리

### 10.1 형식

모든 에러 응답도 마크다운:

```markdown
---
status: {HTTP 상태 코드}
---

# {에러 제목}

{에러 설명}
```

### 10.2 상태 코드

| 코드 | 제목 | 설명 |
|------|------|------|
| 200 | OK | 성공 |
| 201 | Created | 생성됨 |
| 400 | Bad Request | 잘못된 요청 |
| 401 | Unauthorized | 인증 필요 |
| 403 | Forbidden | 권한 없음 |
| 404 | Not Found | 문서 없음 |
| 500 | Internal Server Error | 서버 오류 |

### 10.3 예시

**401 Unauthorized**
```markdown
---
status: 401
---

# Unauthorized

인증이 필요합니다.
```

**400 Bad Request**
```markdown
---
status: 400
---

# Bad Request

title 필드는 필수입니다.
```

**404 Not Found**
```markdown
---
status: 404
---

# Not Found

요청한 문서가 없습니다.
```

---

## 11. 전체 예시

### 11.1 진입점

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# My Blog

> 개인 블로그입니다.

## 페이지

- [소개](/about.md)

## API

- [글 관리](/articles/) - [schema](/articles/_schema.md)
```

### 11.2 스키마

`GET /articles/_schema.md`

```markdown
# Articles API

## 인증

- 방식: Bearer
- 헤더: `Authorization: Bearer {token}`
- 필수: Y (생성/수정/삭제), N (조회)

## 필드

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| title | string | Y | 제목 |
| content | string(markdown) | Y | 본문 |
| tags | array(string) | N | 태그 목록 |
| date | date | N | 작성일 (기본: 현재) |

## 액션

- GET /articles/_index.md - 목록 조회
- GET /articles/{id}.md - 개별 조회
- POST /articles/ - 생성
- PUT /articles/{id}.md - 수정
- DELETE /articles/{id}.md - 삭제
```

### 11.3 목록

`GET /articles/_index.md?sort=-date&limit=10`

```markdown
---
page: 1
limit: 10
total: 25
---

# Articles

> title | date | tags

- [MAiDAS 표준 발표](/articles/maidas-release.md) | 2025-01-27 | #tech, #standard
- [AI 웹의 미래](/articles/ai-web-future.md) | 2025-01-26 | #ai
- [마크다운 활용법](/articles/markdown-tips.md) | 2025-01-25 | #markdown
```

### 11.4 개별 문서

`GET /articles/maidas-release.md`

```markdown
---
title: MAiDAS 표준 발표
date: 2025-01-27
tags: [tech, standard]
---

# MAiDAS 표준 발표

오늘 MAiDAS 0.1.0을 발표합니다...
```

---

## 부록: 버전 히스토리

| 버전 | 날짜 | 변경사항 |
|------|------|----------|
| 0.1.0 | 2025-01-27 | 최초 초안 릴리스 |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
