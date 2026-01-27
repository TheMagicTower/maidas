# MAiDAS

**Markdown AI Data Access Standard**

มาตรฐานการเข้าถึงข้อมูลเว็บที่ใช้ Markdown สำหรับเอเจนต์ AI

---

**การแปล**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## ภาพรวม

MAiDAS เป็นมาตรฐานที่ออกแบบมาเพื่อให้เอเจนต์ AI สามารถอ่านและเขียนข้อมูลเว็บได้อย่างง่ายดาย

เว็บแบบดั้งเดิมที่ใช้ HTML/JavaScript มีปัญหา:
- ยากสำหรับ AI ในการแยกวิเคราะห์
- มีข้อมูล UI ที่ไม่จำเป็น
- การดึงข้อมูลที่มีโครงสร้างซับซ้อน

MAiDAS จัดการการแลกเปลี่ยนข้อมูลทั้งหมดโดยใช้เพียง Markdown

## หลักการสำคัญ

- **ความเรียบง่าย**: Markdown + กฎขั้นต่ำ
- **การปฏิบัติตามมาตรฐาน**: วิธี HTTP มาตรฐาน, RFC 7763 (text/markdown)
- **การทำงานคู่ขนาน**: สามารถทำงานร่วมกับไซต์ HTML ที่มีอยู่ได้
- **การเพิ่มประสิทธิภาพ AI**: แลกเปลี่ยนข้อมูลเท่านั้น ไม่มี UI

## เริ่มต้นอย่างรวดเร็ว

### 1. สร้างจุดเข้า

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# เว็บไซต์ของฉัน

> คำอธิบายเว็บไซต์

## API

- [บทความ](/articles/) - [schema](/articles/_schema.md)
```

### 2. กำหนด Schema

```
/articles/_schema.md
```

```markdown
# Articles API

## ฟิลด์

| ฟิลด์ | ประเภท | จำเป็น | คำอธิบาย |
|-------|--------|--------|----------|
| title | string | Y | หัวข้อ |
| content | string(markdown) | Y | เนื้อหา |

## การดำเนินการ

- GET /articles/_index.md - รายการ
- GET /articles/{id}.md - อ่าน
- POST /articles/ - สร้าง
```

### 3. ให้ข้อมูล

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

- [โพสต์แรก](/articles/first.md) | 2025-01-27
- [โพสต์ที่สอง](/articles/second.md) | 2025-01-26
```

## เอกสาร

ดู [SPECIFICATION.md](SPECIFICATION.md) สำหรับข้อกำหนดฉบับเต็ม

## สถานะ

มาตรฐานนี้อยู่ในสถานะ **ร่าง** ยินดีรับข้อเสนอแนะและการมีส่วนร่วม

## การมีส่วนร่วม

ยินดีต้อนรับ Issues และ PRs กรุณาพูดคุยเกี่ยวกับข้อกำหนดใน [Issues](https://github.com/TheMagicTower/maidas/issues)

## สัญญาอนุญาต

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
