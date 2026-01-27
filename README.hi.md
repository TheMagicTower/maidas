# MAiDAS

**Markdown AI Data Access Standard**

AI एजेंटों के लिए Markdown आधारित वेब डेटा एक्सेस मानक

---

**अनुवाद**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## अवलोकन

MAiDAS एक मानक है जो AI एजेंटों को वेब डेटा आसानी से पढ़ने और लिखने में सक्षम बनाता है।

पारंपरिक HTML/JavaScript आधारित वेब में समस्याएं हैं:
- AI के लिए पार्स करना कठिन
- अनावश्यक UI जानकारी शामिल
- संरचित डेटा निष्कर्षण जटिल

MAiDAS केवल Markdown का उपयोग करके सभी डेटा एक्सचेंज को संभालता है।

## मुख्य सिद्धांत

- **सरलता**: Markdown + न्यूनतम नियम
- **मानक अनुपालन**: HTTP मानक विधियां, RFC 7763 (text/markdown)
- **समानांतर संचालन**: मौजूदा HTML साइटों के साथ चल सकता है
- **AI अनुकूलन**: केवल डेटा एक्सचेंज, बिना UI के

## त्वरित प्रारंभ

### 1. एंट्री पॉइंट बनाएं

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# मेरी साइट

> साइट विवरण

## API

- [लेख](/articles/) - [schema](/articles/_schema.md)
```

### 2. स्कीमा परिभाषित करें

```
/articles/_schema.md
```

```markdown
# Articles API

## फील्ड

| फील्ड | प्रकार | आवश्यक | विवरण |
|-------|--------|--------|-------|
| title | string | Y | शीर्षक |
| content | string(markdown) | Y | सामग्री |

## क्रियाएं

- GET /articles/_index.md - सूची
- GET /articles/{id}.md - पढ़ें
- POST /articles/ - बनाएं
```

### 3. डेटा प्रदान करें

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

- [पहली पोस्ट](/articles/first.md) | 2025-01-27
- [दूसरी पोस्ट](/articles/second.md) | 2025-01-26
```

## दस्तावेज़ीकरण

पूर्ण विनिर्देश के लिए [SPECIFICATION.md](SPECIFICATION.md) देखें।

## स्थिति

यह मानक वर्तमान में **ड्राफ्ट** स्थिति में है। प्रतिक्रिया और योगदान का स्वागत है।

## योगदान

Issues और PRs का स्वागत है। कृपया [Issues](https://github.com/TheMagicTower/maidas/issues) में विनिर्देश पर चर्चा करें।

## लाइसेंस

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
