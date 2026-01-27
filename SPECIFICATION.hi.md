# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

AI एजेंटों के लिए Markdown आधारित वेब डेटा एक्सेस मानक

---

**अनुवाद**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. अवलोकन

### 1.1 उद्देश्य

MAiDAS एक मानक है जो AI एजेंटों को वेब डेटा आसानी से पढ़ने और लिखने में सक्षम बनाता है। पारंपरिक HTML/JavaScript आधारित वेब AI के लिए पार्स करना कठिन है और इसमें अनावश्यक जानकारी होती है। MAiDAS केवल Markdown का उपयोग करके सभी डेटा एक्सचेंज को संभालता है।

### 1.2 सिद्धांत

- **सरलता**: Markdown + न्यूनतम नियम
- **मानक अनुपालन**: HTTP मानक विधियां, RFC 7763 (text/markdown)
- **समानांतर संचालन**: मौजूदा HTML साइटों के साथ चल सकता है
- **AI अनुकूलन**: केवल डेटा एक्सचेंज, बिना UI के

### 1.3 MIME प्रकार

सभी अनुरोध और प्रतिक्रियाएं निम्न Content-Type का उपयोग करती हैं:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. फ़ाइल संरचना

### 2.1 मूल संरचना

```
example.com/
├── .well-known/
│   └── index.md           # आवश्यक। साइट एंट्री पॉइंट
├── articles/
│   ├── _schema.md         # API स्कीमा
│   ├── _index.md          # सूची
│   ├── first-post.md      # व्यक्तिगत दस्तावेज़
│   └── second-post.md
└── about.md
```

### 2.2 सिस्टम फ़ाइलें

अंडरस्कोर (`_`) प्रीफिक्स वाली फ़ाइलें सिस्टम फ़ाइलें हैं:

| फ़ाइल | उद्देश्य |
|-------|---------|
| `_schema.md` | संसाधन CRUD स्कीमा परिभाषा |
| `_index.md` | संसाधन सूची |

---

## 3. एंट्री पॉइंट

### 3.1 स्थान

```
/.well-known/index.md
```

### 3.2 प्रारूप

```markdown
---
version: 0.1.0
---

# साइट का नाम

> साइट विवरण एक पंक्ति में

## पृष्ठ

- [परिचय](/about.md)
- [लेख सूची](/articles/_index.md)

## API

- [लेख प्रबंधन](/articles/) - [schema](/articles/_schema.md)
```

---

## 4. स्कीमा परिभाषा

```markdown
# Articles API

## प्रमाणीकरण

- विधि: Bearer
- हेडर: `Authorization: Bearer {token}`
- आवश्यक: Y

## फ़ील्ड

| फ़ील्ड | प्रकार | आवश्यक | विवरण |
|-------|--------|--------|-------|
| title | string | Y | शीर्षक |
| content | string(markdown) | Y | सामग्री |
| tags | array(string) | N | टैग सूची |
| date | date | N | निर्माण तिथि |

## क्रियाएं

- GET /articles/_index.md - सूची
- GET /articles/{id}.md - पढ़ें
- POST /articles/ - बनाएं
- PUT /articles/{id}.md - अपडेट करें
- DELETE /articles/{id}.md - हटाएं
```

---

## 5. सूची प्रतिक्रिया

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [पहली पोस्ट](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [दूसरी पोस्ट](/articles/second-post.md) | 2025-01-26 | #diary
```

---

## 6. व्यक्तिगत दस्तावेज़

```markdown
---
title: पहली पोस्ट
date: 2025-01-27
tags: [tech, ai]
---

# पहली पोस्ट

सामग्री...
```

---

## 7. CRUD अनुरोध/प्रतिक्रिया

### 7.1 बनाएं (POST)

**अनुरोध**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: नई पोस्ट
tags: [tech]
---

# नई पोस्ट

सामग्री...
```

**प्रतिक्रिया**
```markdown
---
status: 201
---

# Created

दस्तावेज़ बनाया गया: /articles/new-post.md
```

---

## 8. क्वेरी पैरामीटर

### 8.1 पेजिनेशन

```
GET /articles/_index.md?page=2&limit=20
```

### 8.2 सॉर्टिंग

```
GET /articles/_index.md?sort=-date,title
```

---

## 9. प्रमाणीकरण

HTTP मानक Bearer टोकन प्रमाणीकरण

```
Authorization: Bearer {token}
```

---

## 10. त्रुटि प्रबंधन

| कोड | शीर्षक | विवरण |
|-----|--------|-------|
| 200 | OK | सफलता |
| 201 | Created | बनाया गया |
| 400 | Bad Request | अमान्य अनुरोध |
| 401 | Unauthorized | प्रमाणीकरण आवश्यक |
| 403 | Forbidden | अनुमति नहीं |
| 404 | Not Found | दस्तावेज़ नहीं मिला |
| 500 | Internal Server Error | सर्वर त्रुटि |

---

## परिशिष्ट: संस्करण इतिहास

| संस्करण | तिथि | परिवर्तन |
|---------|------|----------|
| 0.1.0 | 2025-01-27 | प्रारंभिक ड्राफ्ट रिलीज़ |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
