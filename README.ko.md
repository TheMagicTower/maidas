# MAiDAS

**Markdown AI Data Access Standard**

AI 에이전트를 위한 마크다운 기반 웹 데이터 접근 표준

---

**번역**: [English](README.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## 개요

MAiDAS는 AI 에이전트가 웹 데이터를 쉽게 읽고 쓸 수 있도록 설계된 표준입니다.

기존 HTML/JavaScript 기반 웹은:
- AI가 파싱하기 어려움
- 불필요한 UI 정보가 많음
- 구조화된 데이터 추출이 복잡함

MAiDAS는 마크다운만으로 모든 데이터 교환을 처리합니다.

## 핵심 원칙

- **단순함**: 마크다운 + 최소한의 규칙
- **표준 준수**: HTTP 표준 메서드, RFC 7763 (text/markdown)
- **병행 운영**: 기존 HTML 사이트와 함께 운영 가능
- **AI 최적화**: UI 없이 데이터만 주고받음

## 빠른 시작

### 1. 진입점 생성

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# My Site

> 사이트 설명

## API

- [글 관리](/articles/) - [schema](/articles/_schema.md)
```

### 2. 스키마 정의

```
/articles/_schema.md
```

```markdown
# Articles API

## 필드

| 필드 | 타입 | 필수 | 설명 |
|------|------|------|------|
| title | string | Y | 제목 |
| content | string(markdown) | Y | 본문 |

## 액션

- GET /articles/_index.md - 목록 조회
- GET /articles/{id}.md - 개별 조회
- POST /articles/ - 생성
```

### 3. 데이터 제공

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

- [첫 번째 글](/articles/first.md) | 2025-01-27
- [두 번째 글](/articles/second.md) | 2025-01-26
```

## 문서

전체 스펙은 [SPECIFICATION.ko.md](SPECIFICATION.ko.md)를 참조하세요.

## 상태

이 표준은 현재 **초안(Draft)** 상태입니다. 피드백과 기여를 환영합니다.

## 기여

이슈와 PR을 환영합니다. 스펙에 대한 논의는 [Issues](https://github.com/TheMagicTower/maidas/issues)에서 진행해 주세요.

## 라이선스

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
