# MAiDAS

**Markdown AI Data Access Standard**

Estándar de acceso a datos web basado en Markdown para agentes de IA

---

**Traducciones**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Português](README.pt-BR.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Descripción general

MAiDAS es un estándar diseñado para que los agentes de IA puedan leer y escribir datos web fácilmente.

La web tradicional basada en HTML/JavaScript tiene problemas:
- Difícil de analizar para la IA
- Contiene información de interfaz de usuario innecesaria
- Extracción de datos estructurados compleja

MAiDAS maneja todo el intercambio de datos usando solo Markdown.

## Principios fundamentales

- **Simplicidad**: Markdown + reglas mínimas
- **Cumplimiento de estándares**: Métodos HTTP estándar, RFC 7763 (text/markdown)
- **Operación paralela**: Puede funcionar junto a sitios HTML existentes
- **Optimización para IA**: Solo intercambio de datos, sin interfaz de usuario

## Inicio rápido

### 1. Crear punto de entrada

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Mi Sitio

> Descripción del sitio

## API

- [Artículos](/articles/) - [schema](/articles/_schema.md)
```

### 2. Definir esquema

```
/articles/_schema.md
```

```markdown
# Articles API

## Campos

| Campo | Tipo | Requerido | Descripción |
|-------|------|-----------|-------------|
| title | string | Y | Título |
| content | string(markdown) | Y | Contenido |

## Acciones

- GET /articles/_index.md - Lista
- GET /articles/{id}.md - Leer
- POST /articles/ - Crear
```

### 3. Proporcionar datos

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

- [Primera publicación](/articles/first.md) | 2025-01-27
- [Segunda publicación](/articles/second.md) | 2025-01-26
```

## Documentación

Consulte [SPECIFICATION.md](SPECIFICATION.md) para la especificación completa.

## Estado

Este estándar está actualmente en estado de **Borrador**. Se agradecen comentarios y contribuciones.

## Contribuir

Se aceptan Issues y PRs. Por favor, discuta la especificación en [Issues](https://github.com/TheMagicTower/maidas/issues).

## Licencia

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
