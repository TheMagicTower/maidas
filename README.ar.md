# MAiDAS

**Markdown AI Data Access Standard**

معيار وصول بيانات الويب القائم على Markdown لوكلاء الذكاء الاصطناعي

---

**الترجمات**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## نظرة عامة

MAiDAS هو معيار مصمم لتمكين وكلاء الذكاء الاصطناعي من قراءة وكتابة بيانات الويب بسهولة.

الويب التقليدي القائم على HTML/JavaScript لديه مشاكل:
- صعب التحليل للذكاء الاصطناعي
- يحتوي على معلومات واجهة مستخدم غير ضرورية
- استخراج البيانات المهيكلة معقد

MAiDAS يعالج جميع تبادلات البيانات باستخدام Markdown فقط.

## المبادئ الأساسية

- **البساطة**: Markdown + قواعد قليلة
- **الامتثال للمعايير**: طرق HTTP القياسية، RFC 7763 (text/markdown)
- **التشغيل المتوازي**: يمكن تشغيله جنبًا إلى جنب مع مواقع HTML الحالية
- **تحسين الذكاء الاصطناعي**: تبادل البيانات فقط، بدون واجهة مستخدم

## البدء السريع

### 1. إنشاء نقطة الدخول

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# موقعي

> وصف الموقع

## API

- [المقالات](/articles/) - [schema](/articles/_schema.md)
```

### 2. تعريف المخطط

```
/articles/_schema.md
```

```markdown
# Articles API

## الحقول

| الحقل | النوع | مطلوب | الوصف |
|-------|------|-------|-------|
| title | string | Y | العنوان |
| content | string(markdown) | Y | المحتوى |

## الإجراءات

- GET /articles/_index.md - القائمة
- GET /articles/{id}.md - القراءة
- POST /articles/ - الإنشاء
```

### 3. توفير البيانات

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

- [المنشور الأول](/articles/first.md) | 2025-01-27
- [المنشور الثاني](/articles/second.md) | 2025-01-26
```

## التوثيق

راجع [SPECIFICATION.ar.md](SPECIFICATION.ar.md) للحصول على المواصفات الكاملة.

## الحالة

هذا المعيار حاليًا في حالة **مسودة**. نرحب بالملاحظات والمساهمات.

## المساهمة

نرحب بـ Issues و PRs. يرجى مناقشة المواصفات في [Issues](https://github.com/TheMagicTower/maidas/issues).

## الترخيص

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
