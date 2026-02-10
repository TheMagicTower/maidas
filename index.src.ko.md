# MAiDAS

Markdown AI Data Access Standard.

MAiDAS는 AI가 읽기 쉬운 웹 데이터 표준을 목표로 합니다. 사람에게는 문서처럼, 에이전트에게는 프로토콜처럼 동작합니다.

## 빠른 시작

1. 엔트리 포인트를 만듭니다: `/.well-known/index.md`
2. 리소스 스키마를 정의합니다: `/<resource>/_schema.md`
3. 목록과 개별 문서를 제공합니다: `/_index.md`, `/{id}.md`

```text
/.well-known/index.md
/articles/_schema.md
/articles/_index.md
/articles/first-post.md
```

## 사람용 문서
- [Specification](../SPECIFICATION.md)
- [README](../README.md)
- [Conformance (v0.1)](../conformance/v0.1/valid/entrypoint.min.md)

## 에이전트 엔트리 포인트
- [MAiDAS Index](./.well-known/index.md)

## 설계 메모
- MAiDAS는 HTML UI 대신 Markdown 데이터에 집중합니다.
- 문서 버전은 “문법 프로파일(Grammar Profile)”로 정의됩니다.
- 실제 구현은 conformance 예제로 검증하는 것을 권장합니다.
