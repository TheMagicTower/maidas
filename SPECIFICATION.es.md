# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Estándar de acceso a datos web basado en Markdown para agentes de IA

---

**Traducciones**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Português](SPECIFICATION.pt-BR.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Descripción General

### 1.1 Propósito

MAiDAS es un estándar diseñado para que los agentes de IA puedan leer y escribir datos web fácilmente. La web tradicional basada en HTML/JavaScript es difícil de analizar para la IA y contiene información innecesaria. MAiDAS maneja todo el intercambio de datos usando solo Markdown.

### 1.2 Principios

- **Simplicidad**: Markdown + reglas mínimas
- **Cumplimiento de estándares**: Métodos HTTP estándar, RFC 7763 (text/markdown)
- **Operación paralela**: Puede funcionar junto a sitios HTML existentes
- **Optimización para IA**: Solo intercambio de datos, sin interfaz de usuario

### 1.3 Tipo MIME

Todas las solicitudes y respuestas usan el siguiente Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Estructura de Archivos

### 2.1 Estructura Básica

```
example.com/
├── .well-known/
│   └── index.md           # Requerido. Punto de entrada del sitio
├── articles/
│   ├── _schema.md         # Esquema API
│   ├── _index.md          # Lista
│   ├── first-post.md      # Documento individual
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 Archivos de Sistema

Los archivos con prefijo guión bajo (`_`) son archivos de sistema:

| Archivo | Propósito |
|---------|-----------|
| `_schema.md` | Definición de esquema CRUD de recursos |
| `_index.md` | Lista de recursos |

---

## 3. Punto de Entrada

### 3.1 Ubicación

```
/.well-known/index.md
```

### 3.2 Formato

```markdown
---
version: 0.1.0
---

# Nombre del Sitio

> Descripción del sitio en una línea

## Páginas

- [Acerca de](/about.md)
- [Lista de Artículos](/articles/_index.md)

## API

- [Gestión de Artículos](/articles/) - [schema](/articles/_schema.md)
- [Gestión de Comentarios](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 Campos

| Campo | Requerido | Descripción |
|-------|-----------|-------------|
| version | Y | Versión de la especificación MAiDAS |

---

## 4. Definición de Esquema

### 4.1 Ubicación

`_schema.md` dentro de cada carpeta de recursos

```
/articles/_schema.md
/comments/_schema.md
```

### 4.2 Formato

```markdown
# Articles API

## Autenticación

- Método: Bearer
- Encabezado: `Authorization: Bearer {token}`
- Requerido: Y

## Campos

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| title | string | Y | Título |
| content | string(markdown) | Y | Contenido |
| tags | array(string) | N | Lista de etiquetas |
| date | date | N | Fecha de creación |

## Acciones

- GET /articles/_index.md - Lista
- GET /articles/{id}.md - Leer
- POST /articles/ - Crear
- PUT /articles/{id}.md - Actualizar
- DELETE /articles/{id}.md - Eliminar
```

### 4.3 Sistema de Tipos

**Tipos Básicos**

| Tipo | Descripción |
|------|-------------|
| string | Cadena |
| number | Número |
| boolean | Verdadero/Falso |
| date | Fecha (ISO 8601) |
| array | Array |

**Sugerencias de Formato (Opcional)**

Notación entre paréntesis después del tipo:

| Formato | Descripción |
|---------|-------------|
| string(markdown) | Markdown permitido |
| string(url) | Formato URL |
| string(email) | Formato email |
| date(datetime) | Fecha+Hora |
| array(string) | Array de cadenas |
| array(number) | Array de números |

---

## 5. Respuesta de Lista

### 5.1 Ubicación

`_index.md` dentro de cada carpeta de recursos

### 5.2 Formato

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Primera Publicación](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Segunda Publicación](/articles/second-post.md) | 2025-01-26 | #diary
- [Tercera Publicación](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 Reglas

- Definir encabezados de campo con blockquote (`>`) en la primera línea
- Listar valores de campo después del enlace con separador `|`
- Dejar valores vacíos en blanco
- Información de paginación en frontmatter

---

## 6. Documento Individual

### 6.1 Formato

```markdown
---
title: Primera Publicación
date: 2025-01-27
tags: [tech, ai]
---

# Primera Publicación

Contenido del cuerpo...
```

### 6.2 Reglas

- frontmatter: formato YAML (envuelto con `---`)
- Cuerpo: Markdown
- Campos requeridos: Según lo definido en `_schema.md`

---

## 7. Solicitud/Respuesta CRUD

### 7.1 Crear (POST)

**Solicitud**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Nueva Publicación
tags: [tech]
---

# Nueva Publicación

Contenido del cuerpo...
```

**Respuesta**
```markdown
---
status: 201
---

# Created

Documento creado: /articles/new-post.md
```

### 7.2 Leer (GET)

**Lista**
```
GET /articles/_index.md
```

**Individual**
```
GET /articles/first-post.md
```

### 7.3 Actualizar (PUT)

**Solicitud**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Título Actualizado
tags: [tech, ai]
---

# Título Actualizado

Cuerpo actualizado...
```

**Respuesta**
```markdown
---
status: 200
---

# OK

Documento actualizado.
```

### 7.4 Eliminar (DELETE)

**Solicitud**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**Respuesta**
```markdown
---
status: 200
---

# OK

Documento eliminado.
```

---

## 8. Parámetros de Consulta

### 8.1 Paginación

```
GET /articles/_index.md?page=2&limit=20
```

| Parámetro | Predeterminado | Descripción |
|-----------|----------------|-------------|
| page | 1 | Número de página |
| limit | Config del servidor | Elementos por página |

Incluido en frontmatter de respuesta:
- `page`: Página actual
- `limit`: Elementos por página
- `total`: Total de elementos

### 8.2 Filtro

Usar nombres de campo como parámetros de consulta:

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
GET /articles/_index.md?tag=tech&date=2025-01
```

### 8.3 Ordenamiento

```
GET /articles/_index.md?sort=-date,title
```

**Reglas**
- Usar parámetro `sort`
- Separados por coma para múltiples campos (orden de prioridad)
- Prefijo `-`: Descendente
- Sin prefijo: Ascendente

**Ejemplos**
```
?sort=date          # date ascendente
?sort=-date         # date descendente
?sort=-date,title   # date descendente, luego title ascendente
```

### 8.4 Combinación

Todos los parámetros de consulta pueden combinarse:

```
GET /articles/_index.md?tag=tech&sort=-date&page=2&limit=10
```

---

## 9. Autenticación

### 9.1 Método

Autenticación Bearer token HTTP estándar

```
Authorization: Bearer {token}
```

### 9.2 Notación en Esquema

```markdown
## Autenticación

- Método: Bearer
- Encabezado: `Authorization: Bearer {token}`
- Requerido: Y
```

Cuando no se requiere autenticación:

```markdown
## Autenticación

- Requerido: N
```

---

## 10. Manejo de Errores

### 10.1 Formato

Todas las respuestas de error también son markdown:

```markdown
---
status: {código de estado HTTP}
---

# {Título del Error}

{Descripción del error}
```

### 10.2 Códigos de Estado

| Código | Título | Descripción |
|--------|--------|-------------|
| 200 | OK | Éxito |
| 201 | Created | Creado |
| 400 | Bad Request | Solicitud inválida |
| 401 | Unauthorized | Autenticación requerida |
| 403 | Forbidden | Sin permiso |
| 404 | Not Found | Documento no encontrado |
| 500 | Internal Server Error | Error del servidor |

### 10.3 Ejemplos

**401 Unauthorized**
```markdown
---
status: 401
---

# Unauthorized

Se requiere autenticación.
```

**400 Bad Request**
```markdown
---
status: 400
---

# Bad Request

El campo title es requerido.
```

**404 Not Found**
```markdown
---
status: 404
---

# Not Found

El documento solicitado no fue encontrado.
```

---

## 11. Ejemplo Completo

### 11.1 Punto de Entrada

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# Mi Blog

> Un blog personal.

## Páginas

- [Acerca de](/about.md)

## API

- [Gestión de Artículos](/articles/) - [schema](/articles/_schema.md)
```

### 11.2 Esquema

`GET /articles/_schema.md`

```markdown
# Articles API

## Autenticación

- Método: Bearer
- Encabezado: `Authorization: Bearer {token}`
- Requerido: Y (crear/actualizar/eliminar), N (leer)

## Campos

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| title | string | Y | Título |
| content | string(markdown) | Y | Contenido |
| tags | array(string) | N | Lista de etiquetas |
| date | date | N | Fecha de creación (predeterminado: ahora) |

## Acciones

- GET /articles/_index.md - Lista
- GET /articles/{id}.md - Leer
- POST /articles/ - Crear
- PUT /articles/{id}.md - Actualizar
- DELETE /articles/{id}.md - Eliminar
```

### 11.3 Lista

`GET /articles/_index.md?sort=-date&limit=10`

```markdown
---
page: 1
limit: 10
total: 25
---

# Articles

> title | date | tags

- [Lanzamiento del Estándar MAiDAS](/articles/maidas-release.md) | 2025-01-27 | #tech, #standard
- [Futuro de la Web IA](/articles/ai-web-future.md) | 2025-01-26 | #ai
- [Consejos de Markdown](/articles/markdown-tips.md) | 2025-01-25 | #markdown
```

### 11.4 Documento Individual

`GET /articles/maidas-release.md`

```markdown
---
title: Lanzamiento del Estándar MAiDAS
date: 2025-01-27
tags: [tech, standard]
---

# Lanzamiento del Estándar MAiDAS

Hoy lanzamos MAiDAS 0.1.0...
```

---

## Apéndice: Historial de Versiones

| Versión | Fecha | Cambios |
|---------|-------|---------|
| 0.1.0 | 2025-01-27 | Lanzamiento inicial del borrador |
