# MAiDAS

**Markdown AI Data Access Standard**

Tiêu chuẩn truy cập dữ liệu web dựa trên Markdown cho các tác nhân AI

---

**Bản dịch**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Tổng quan

MAiDAS là một tiêu chuẩn được thiết kế để các tác nhân AI có thể đọc và ghi dữ liệu web một cách dễ dàng.

Web truyền thống dựa trên HTML/JavaScript có những vấn đề:
- Khó phân tích cho AI
- Chứa thông tin giao diện người dùng không cần thiết
- Trích xuất dữ liệu có cấu trúc phức tạp

MAiDAS xử lý tất cả việc trao đổi dữ liệu chỉ sử dụng Markdown.

## Nguyên tắc cốt lõi

- **Đơn giản**: Markdown + quy tắc tối thiểu
- **Tuân thủ tiêu chuẩn**: Phương thức HTTP tiêu chuẩn, RFC 7763 (text/markdown)
- **Vận hành song song**: Có thể chạy cùng với các trang HTML hiện có
- **Tối ưu hóa AI**: Chỉ trao đổi dữ liệu, không có giao diện người dùng

## Bắt đầu nhanh

### 1. Tạo điểm vào

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Trang của tôi

> Mô tả trang

## API

- [Bài viết](/articles/) - [schema](/articles/_schema.md)
```

### 2. Định nghĩa Schema

```
/articles/_schema.md
```

```markdown
# Articles API

## Trường

| Trường | Kiểu | Bắt buộc | Mô tả |
|--------|------|----------|-------|
| title | string | Y | Tiêu đề |
| content | string(markdown) | Y | Nội dung |

## Hành động

- GET /articles/_index.md - Danh sách
- GET /articles/{id}.md - Đọc
- POST /articles/ - Tạo
```

### 3. Cung cấp dữ liệu

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

- [Bài viết đầu tiên](/articles/first.md) | 2025-01-27
- [Bài viết thứ hai](/articles/second.md) | 2025-01-26
```

## Tài liệu

Xem [SPECIFICATION.md](SPECIFICATION.md) để biết đặc tả đầy đủ.

## Trạng thái

Tiêu chuẩn này hiện đang ở trạng thái **Bản nháp**. Chào đón phản hồi và đóng góp.

## Đóng góp

Chào đón Issues và PRs. Vui lòng thảo luận về đặc tả tại [Issues](https://github.com/TheMagicTower/maidas/issues).

## Giấy phép

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
