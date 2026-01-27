# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Tiêu chuẩn truy cập dữ liệu web dựa trên Markdown cho các tác nhân AI

---

**Bản dịch**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Tổng quan

### 1.1 Mục đích

MAiDAS là một tiêu chuẩn được thiết kế để các tác nhân AI có thể đọc và ghi dữ liệu web một cách dễ dàng. Web truyền thống dựa trên HTML/JavaScript khó phân tích cho AI và chứa thông tin không cần thiết. MAiDAS xử lý tất cả việc trao đổi dữ liệu chỉ sử dụng Markdown.

### 1.2 Nguyên tắc

- **Đơn giản**: Markdown + quy tắc tối thiểu
- **Tuân thủ tiêu chuẩn**: Phương thức HTTP tiêu chuẩn, RFC 7763 (text/markdown)
- **Vận hành song song**: Có thể chạy cùng với các trang HTML hiện có
- **Tối ưu hóa AI**: Chỉ trao đổi dữ liệu, không có giao diện người dùng

### 1.3 Loại MIME

Tất cả yêu cầu và phản hồi sử dụng Content-Type sau:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Cấu trúc tệp

### 2.1 Cấu trúc cơ bản

```
example.com/
├── .well-known/
│   └── index.md           # Bắt buộc. Điểm vào trang
├── articles/
│   ├── _schema.md         # Schema API
│   ├── _index.md          # Danh sách
│   ├── first-post.md      # Tài liệu riêng lẻ
│   └── second-post.md
└── about.md
```

### 2.2 Tệp hệ thống

Các tệp có tiền tố gạch dưới (`_`) là tệp hệ thống:

| Tệp | Mục đích |
|-----|----------|
| `_schema.md` | Định nghĩa schema CRUD tài nguyên |
| `_index.md` | Danh sách tài nguyên |

---

## 3. Điểm vào

### 3.1 Vị trí

```
/.well-known/index.md
```

### 3.2 Định dạng

```markdown
---
version: 0.1.0
---

# Tên trang

> Mô tả trang một dòng

## Trang

- [Giới thiệu](/about.md)
- [Danh sách bài viết](/articles/_index.md)

## API

- [Quản lý bài viết](/articles/) - [schema](/articles/_schema.md)
```

---

## 4. Định nghĩa Schema

```markdown
# Articles API

## Xác thực

- Phương thức: Bearer
- Header: `Authorization: Bearer {token}`
- Bắt buộc: Y

## Trường

| Trường | Kiểu | Bắt buộc | Mô tả |
|--------|------|----------|-------|
| title | string | Y | Tiêu đề |
| content | string(markdown) | Y | Nội dung |
| tags | array(string) | N | Danh sách thẻ |
| date | date | N | Ngày tạo |

## Hành động

- GET /articles/_index.md - Danh sách
- GET /articles/{id}.md - Đọc
- POST /articles/ - Tạo
- PUT /articles/{id}.md - Cập nhật
- DELETE /articles/{id}.md - Xóa
```

---

## 5. Phản hồi danh sách

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Bài viết đầu tiên](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Bài viết thứ hai](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. Tài liệu riêng lẻ

```markdown
---
title: Bài viết đầu tiên
date: 2025-01-27
tags: [tech, ai]
---

# Bài viết đầu tiên

Nội dung...
```

---

## 7. Yêu cầu/Phản hồi CRUD

### 7.1 Tạo (POST)

**Yêu cầu**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Bài viết mới
tags: [tech]
---

# Bài viết mới

Nội dung...
```

**Phản hồi**
```markdown
---
status: 201
---

# Created

Tài liệu đã tạo: /articles/new-post.md
```

---

## 8. Tham số truy vấn

### 8.1 Phân trang

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 Sắp xếp

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. Xác thực

Xác thực Bearer token HTTP tiêu chuẩn

```
Authorization: Bearer {token}
```

---

## 10. Xử lý lỗi

| Mã | Tiêu đề | Mô tả |
|----|---------|-------|
| 200 | OK | Thành công |
| 201 | Created | Đã tạo |
| 400 | Bad Request | Yêu cầu không hợp lệ |
| 401 | Unauthorized | Cần xác thực |
| 403 | Forbidden | Không có quyền |
| 404 | Not Found | Không tìm thấy tài liệu |
| 500 | Internal Server Error | Lỗi máy chủ |

---

## Phụ lục: Lịch sử phiên bản

| Phiên bản | Ngày | Thay đổi |
|-----------|------|----------|
| 0.1.0 | 2025-01-27 | Phát hành bản nháp đầu tiên |
