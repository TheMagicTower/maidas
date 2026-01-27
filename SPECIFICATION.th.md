# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

มาตรฐานการเข้าถึงข้อมูลเว็บที่ใช้ Markdown สำหรับเอเจนต์ AI

---

**การแปล**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. ภาพรวม

### 1.1 วัตถุประสงค์

MAiDAS เป็นมาตรฐานที่ออกแบบมาเพื่อให้เอเจนต์ AI สามารถอ่านและเขียนข้อมูลเว็บได้อย่างง่ายดาย เว็บแบบดั้งเดิมที่ใช้ HTML/JavaScript ยากสำหรับ AI ในการแยกวิเคราะห์และมีข้อมูลที่ไม่จำเป็น MAiDAS จัดการการแลกเปลี่ยนข้อมูลทั้งหมดโดยใช้เพียง Markdown

### 1.2 หลักการ

- **ความเรียบง่าย**: Markdown + กฎขั้นต่ำ
- **การปฏิบัติตามมาตรฐาน**: วิธี HTTP มาตรฐาน, RFC 7763 (text/markdown)
- **การทำงานคู่ขนาน**: สามารถทำงานร่วมกับไซต์ HTML ที่มีอยู่ได้
- **การเพิ่มประสิทธิภาพ AI**: แลกเปลี่ยนข้อมูลเท่านั้น ไม่มี UI

### 1.3 ประเภท MIME

คำขอและการตอบกลับทั้งหมดใช้ Content-Type ต่อไปนี้:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. โครงสร้างไฟล์

### 2.1 โครงสร้างพื้นฐาน

```
example.com/
├── .well-known/
│   └── index.md           # จำเป็น จุดเข้าไซต์
├── articles/
│   ├── _schema.md         # Schema API
│   ├── _index.md          # รายการ
│   ├── first-post.md      # เอกสารเดี่ยว
│   └── second-post.md
└── about.md
```

### 2.2 ไฟล์ระบบ

ไฟล์ที่มีคำนำหน้า underscore (`_`) เป็นไฟล์ระบบ:

| ไฟล์ | วัตถุประสงค์ |
|------|-------------|
| `_schema.md` | คำจำกัดความ schema CRUD ทรัพยากร |
| `_index.md` | รายการทรัพยากร |

---

## 3. จุดเข้า

### 3.1 ตำแหน่ง

```
/.well-known/index.md
```

### 3.2 รูปแบบ

```markdown
---
version: 0.1.0
---

# ชื่อเว็บไซต์

> คำอธิบายเว็บไซต์หนึ่งบรรทัด

## หน้า

- [เกี่ยวกับ](/about.md)
- [รายการบทความ](/articles/_index.md)

## API

- [การจัดการบทความ](/articles/) - [schema](/articles/_schema.md)
```

---

## 4. คำจำกัดความ Schema

```markdown
# Articles API

## การยืนยันตัวตน

- วิธี: Bearer
- Header: `Authorization: Bearer {token}`
- จำเป็น: Y

## ฟิลด์

| ฟิลด์ | ประเภท | จำเป็น | คำอธิบาย |
|-------|--------|--------|----------|
| title | string | Y | หัวข้อ |
| content | string(markdown) | Y | เนื้อหา |
| tags | array(string) | N | รายการแท็ก |
| date | date | N | วันที่สร้าง |

## การดำเนินการ

- GET /articles/_index.md - รายการ
- GET /articles/{id}.md - อ่าน
- POST /articles/ - สร้าง
- PUT /articles/{id}.md - อัปเดต
- DELETE /articles/{id}.md - ลบ
```

---

## 5. การตอบกลับรายการ

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [โพสต์แรก](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [โพสต์ที่สอง](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. เอกสารเดี่ยว

```markdown
---
title: โพสต์แรก
date: 2025-01-27
tags: [tech, ai]
---

# โพสต์แรก

เนื้อหา...
```

---

## 7. คำขอ/การตอบกลับ CRUD

### 7.1 สร้าง (POST)

**คำขอ**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: โพสต์ใหม่
tags: [tech]
---

# โพสต์ใหม่

เนื้อหา...
```

**การตอบกลับ**
```markdown
---
status: 201
---

# Created

สร้างเอกสารแล้ว: /articles/new-post.md
```

---

## 8. พารามิเตอร์การสืบค้น

### 8.1 การแบ่งหน้า

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 การเรียงลำดับ

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. การยืนยันตัวตน

การยืนยันตัวตน Bearer token HTTP มาตรฐาน

```
Authorization: Bearer {token}
```

---

## 10. การจัดการข้อผิดพลาด

| รหัส | หัวข้อ | คำอธิบาย |
|------|--------|----------|
| 200 | OK | สำเร็จ |
| 201 | Created | สร้างแล้ว |
| 400 | Bad Request | คำขอไม่ถูกต้อง |
| 401 | Unauthorized | ต้องการการยืนยันตัวตน |
| 403 | Forbidden | ไม่มีสิทธิ์ |
| 404 | Not Found | ไม่พบเอกสาร |
| 500 | Internal Server Error | ข้อผิดพลาดเซิร์ฟเวอร์ |

---

## ภาคผนวก: ประวัติเวอร์ชัน

| เวอร์ชัน | วันที่ | การเปลี่ยนแปลง |
|----------|--------|----------------|
| 0.1.0 | 2025-01-27 | การเผยแพร่ร่างเริ่มต้น |
