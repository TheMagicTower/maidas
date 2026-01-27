# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

معيار وصول بيانات الويب القائم على Markdown لوكلاء الذكاء الاصطناعي

---

**الترجمات**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. نظرة عامة

### 1.1 الغرض

MAiDAS هو معيار مصمم لتمكين وكلاء الذكاء الاصطناعي من قراءة وكتابة بيانات الويب بسهولة. الويب التقليدي القائم على HTML/JavaScript صعب التحليل للذكاء الاصطناعي ويحتوي على معلومات غير ضرورية. MAiDAS يعالج جميع تبادلات البيانات باستخدام Markdown فقط.

### 1.2 المبادئ

- **البساطة**: Markdown + قواعد قليلة
- **الامتثال للمعايير**: طرق HTTP القياسية، RFC 7763 (text/markdown)
- **التشغيل المتوازي**: يمكن تشغيله جنبًا إلى جنب مع مواقع HTML الحالية
- **تحسين الذكاء الاصطناعي**: تبادل البيانات فقط، بدون واجهة مستخدم

### 1.3 نوع MIME

جميع الطلبات والاستجابات تستخدم Content-Type التالي:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. هيكل الملفات

### 2.1 الهيكل الأساسي

```
example.com/
├── .well-known/
│   └── index.md           # مطلوب. نقطة دخول الموقع
├── articles/
│   ├── _schema.md         # مخطط API
│   ├── _index.md          # القائمة
│   ├── first-post.md      # مستند فردي
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 ملفات النظام

الملفات ذات البادئة underscore (`_`) هي ملفات نظام:

| الملف | الغرض |
|-------|-------|
| `_schema.md` | تعريف مخطط CRUD للموارد |
| `_index.md` | قائمة الموارد |

---

## 3. نقطة الدخول

### 3.1 الموقع

```
/.well-known/index.md
```

### 3.2 التنسيق

```markdown
---
version: 0.1.0
---

# اسم الموقع

> وصف الموقع في سطر واحد

## الصفحات

- [حول](/about.md)
- [قائمة المقالات](/articles/_index.md)

## API

- [إدارة المقالات](/articles/) - [schema](/articles/_schema.md)
```

### 3.3 الحقول

| الحقل | مطلوب | الوصف |
|-------|-------|-------|
| version | Y | إصدار مواصفات MAiDAS |

---

## 4. تعريف المخطط

### 4.1 الموقع

`_schema.md` في كل مجلد موارد

### 4.2 التنسيق

```markdown
# Articles API

## المصادقة

- الطريقة: Bearer
- الرأس: `Authorization: Bearer {token}`
- مطلوب: Y

## الحقول

| الحقل | النوع | مطلوب | الوصف |
|-------|------|-------|-------|
| title | string | Y | العنوان |
| content | string(markdown) | Y | المحتوى |
| tags | array(string) | N | قائمة العلامات |
| date | date | N | تاريخ الإنشاء |

## الإجراءات

- GET /articles/_index.md - القائمة
- GET /articles/{id}.md - القراءة
- POST /articles/ - الإنشاء
- PUT /articles/{id}.md - التحديث
- DELETE /articles/{id}.md - الحذف
```

---

## 5. استجابة القائمة

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [المنشور الأول](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [المنشور الثاني](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. المستند الفردي

```markdown
---
title: المنشور الأول
date: 2025-01-27
tags: [tech, ai]
---

# المنشور الأول

المحتوى...
```

---

## 7. طلب/استجابة CRUD

### 7.1 الإنشاء (POST)

**الطلب**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: منشور جديد
tags: [tech]
---

# منشور جديد

المحتوى...
```

**الاستجابة**
```markdown
---
status: 201
---

# Created

تم إنشاء المستند: /articles/new-post.md
```

---

## 8. معلمات الاستعلام

### 8.1 التصفح

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 الترتيب

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. المصادقة

مصادقة Bearer token HTTP القياسية

```
Authorization: Bearer {token}
```

---

## 10. معالجة الأخطاء

| الرمز | العنوان | الوصف |
|-------|---------|-------|
| 200 | OK | نجاح |
| 201 | Created | تم الإنشاء |
| 400 | Bad Request | طلب غير صالح |
| 401 | Unauthorized | المصادقة مطلوبة |
| 403 | Forbidden | لا يوجد إذن |
| 404 | Not Found | المستند غير موجود |
| 500 | Internal Server Error | خطأ في الخادم |

---

## الملحق: سجل الإصدارات

| الإصدار | التاريخ | التغييرات |
|---------|---------|-----------|
| 0.1.0 | 2025-01-27 | الإصدار الأولي للمسودة |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
