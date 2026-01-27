# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Standard d'accès aux données web basé sur Markdown pour les agents IA

---

**Traductions**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Aperçu

### 1.1 Objectif

MAiDAS est un standard conçu pour permettre aux agents IA de lire et écrire facilement des données web. Le web traditionnel basé sur HTML/JavaScript est difficile à analyser pour l'IA et contient des informations inutiles. MAiDAS gère tous les échanges de données en utilisant uniquement Markdown.

### 1.2 Principes

- **Simplicité**: Markdown + règles minimales
- **Conformité aux standards**: Méthodes HTTP standard, RFC 7763 (text/markdown)
- **Fonctionnement parallèle**: Peut fonctionner aux côtés des sites HTML existants
- **Optimisation IA**: Échange de données uniquement, sans interface utilisateur

### 1.3 Type MIME

Toutes les requêtes et réponses utilisent le Content-Type suivant:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Structure des Fichiers

### 2.1 Structure de Base

```
example.com/
├── .well-known/
│   └── index.md           # Requis. Point d'entrée du site
├── articles/
│   ├── _schema.md         # Schéma API
│   ├── _index.md          # Liste
│   ├── first-post.md      # Document individuel
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 Fichiers Système

Les fichiers avec préfixe tiret bas (`_`) sont des fichiers système:

| Fichier | Usage |
|---------|-------|
| `_schema.md` | Définition du schéma CRUD des ressources |
| `_index.md` | Liste des ressources |

---

## 3. Point d'Entrée

### 3.1 Emplacement

```
/.well-known/index.md
```

### 3.2 Format

```markdown
---
version: 0.1.0
---

# Nom du Site

> Description du site en une ligne

## Pages

- [À propos](/about.md)
- [Liste des Articles](/articles/_index.md)

## API

- [Gestion des Articles](/articles/) - [schema](/articles/_schema.md)
- [Gestion des Commentaires](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 Champs

| Champ | Requis | Description |
|-------|--------|-------------|
| version | Y | Version de la spec MAiDAS |

---

## 4. Définition du Schéma

### 4.1 Emplacement

`_schema.md` dans chaque dossier de ressources

### 4.2 Format

```markdown
# Articles API

## Authentification

- Méthode: Bearer
- En-tête: `Authorization: Bearer {token}`
- Requis: Y

## Champs

| Champ | Type | Requis | Description |
|-------|------|--------|-------------|
| title | string | Y | Titre |
| content | string(markdown) | Y | Contenu |
| tags | array(string) | N | Liste des tags |
| date | date | N | Date de création |

## Actions

- GET /articles/_index.md - Liste
- GET /articles/{id}.md - Lecture
- POST /articles/ - Création
- PUT /articles/{id}.md - Mise à jour
- DELETE /articles/{id}.md - Suppression
```

### 4.3 Système de Types

**Types de Base**

| Type | Description |
|------|-------------|
| string | Chaîne |
| number | Nombre |
| boolean | Vrai/Faux |
| date | Date (ISO 8601) |
| array | Tableau |

**Indices de Format (Optionnel)**

Notation entre parenthèses après le type:

| Format | Description |
|--------|-------------|
| string(markdown) | Markdown autorisé |
| string(url) | Format URL |
| string(email) | Format email |
| date(datetime) | Date+Heure |
| array(string) | Tableau de chaînes |
| array(number) | Tableau de nombres |

---

## 5. Réponse de Liste

### 5.1 Emplacement

`_index.md` dans chaque dossier de ressources

### 5.2 Format

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Premier Article](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Deuxième Article](/articles/second-post.md) | 2025-01-26 | #diary
- [Troisième Article](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 Règles

- Définir les en-têtes de champs avec blockquote (`>`) sur la première ligne
- Lister les valeurs après le lien avec séparateur `|`
- Laisser les valeurs vides en blanc
- Infos de pagination dans le frontmatter

---

## 6. Document Individuel

### 6.1 Format

```markdown
---
title: Premier Article
date: 2025-01-27
tags: [tech, ai]
---

# Premier Article

Contenu du corps...
```

### 6.2 Règles

- frontmatter: format YAML (entouré de `---`)
- Corps: Markdown
- Champs requis: Selon `_schema.md`

---

## 7. Requête/Réponse CRUD

### 7.1 Créer (POST)

**Requête**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Nouvel Article
tags: [tech]
---

# Nouvel Article

Contenu...
```

**Réponse**
```markdown
---
status: 201
---

# Created

Document créé: /articles/new-post.md
```

### 7.2 Lire (GET)

**Liste**
```
GET /articles/_index.md
```

**Individuel**
```
GET /articles/first-post.md
```

### 7.3 Mettre à jour (PUT)

**Requête**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Titre Mis à Jour
tags: [tech, ai]
---

# Titre Mis à Jour

Corps mis à jour...
```

**Réponse**
```markdown
---
status: 200
---

# OK

Document mis à jour.
```

### 7.4 Supprimer (DELETE)

**Requête**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**Réponse**
```markdown
---
status: 200
---

# OK

Document supprimé.
```

---

## 8. Paramètres de Requête

### 8.1 Pagination

```
GET /articles/_index.md?page=2&limit=20
```

| Paramètre | Défaut | Description |
|-----------|--------|-------------|
| page | 1 | Numéro de page |
| limit | Config serveur | Éléments par page |

### 8.2 Filtre

Utiliser les noms de champs comme paramètres:

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
```

### 8.3 Tri

```
GET /articles/_index.md?sort=-date,title
```

**Règles**
- Paramètre `sort`
- Virgule pour plusieurs champs
- Préfixe `-`: Descendant
- Sans préfixe: Ascendant

---

## 9. Authentification

### 9.1 Méthode

Authentification Bearer token HTTP standard

```
Authorization: Bearer {token}
```

---

## 10. Gestion des Erreurs

### 10.1 Format

```markdown
---
status: {code HTTP}
---

# {Titre Erreur}

{Description}
```

### 10.2 Codes de Statut

| Code | Titre | Description |
|------|-------|-------------|
| 200 | OK | Succès |
| 201 | Created | Créé |
| 400 | Bad Request | Requête invalide |
| 401 | Unauthorized | Authentification requise |
| 403 | Forbidden | Pas de permission |
| 404 | Not Found | Document non trouvé |
| 500 | Internal Server Error | Erreur serveur |

---

## 11. Exemple Complet

### 11.1 Point d'Entrée

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# Mon Blog

> Un blog personnel.

## Pages

- [À propos](/about.md)

## API

- [Gestion des Articles](/articles/) - [schema](/articles/_schema.md)
```

---

## Annexe: Historique des Versions

| Version | Date | Changements |
|---------|------|-------------|
| 0.1.0 | 2025-01-27 | Version initiale du brouillon |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
