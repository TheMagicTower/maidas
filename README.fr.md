# MAiDAS

**Markdown AI Data Access Standard**

Standard d'accès aux données web basé sur Markdown pour les agents IA

---

**Traductions**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Aperçu

MAiDAS est un standard conçu pour permettre aux agents IA de lire et écrire facilement des données web.

Le web traditionnel basé sur HTML/JavaScript présente des problèmes :
- Difficile à analyser pour l'IA
- Contient des informations d'interface utilisateur inutiles
- Extraction de données structurées complexe

MAiDAS gère tous les échanges de données en utilisant uniquement Markdown.

## Principes fondamentaux

- **Simplicité** : Markdown + règles minimales
- **Conformité aux standards** : Méthodes HTTP standard, RFC 7763 (text/markdown)
- **Fonctionnement parallèle** : Peut fonctionner aux côtés des sites HTML existants
- **Optimisation IA** : Échange de données uniquement, sans interface utilisateur

## Démarrage rapide

### 1. Créer le point d'entrée

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Mon Site

> Description du site

## API

- [Articles](/articles/) - [schema](/articles/_schema.md)
```

### 2. Définir le schéma

```
/articles/_schema.md
```

```markdown
# Articles API

## Champs

| Champ | Type | Requis | Description |
|-------|------|--------|-------------|
| title | string | Y | Titre |
| content | string(markdown) | Y | Contenu |

## Actions

- GET /articles/_index.md - Liste
- GET /articles/{id}.md - Lecture
- POST /articles/ - Création
```

### 3. Fournir les données

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

- [Premier article](/articles/first.md) | 2025-01-27
- [Deuxième article](/articles/second.md) | 2025-01-26
```

## Documentation

Consultez [SPECIFICATION.fr.md](SPECIFICATION.fr.md) pour la spécification complète.

## Statut

Ce standard est actuellement à l'état de **Brouillon**. Les commentaires et contributions sont les bienvenus.

## Contribuer

Les Issues et PRs sont les bienvenues. Veuillez discuter de la spécification dans [Issues](https://github.com/TheMagicTower/maidas/issues).

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
