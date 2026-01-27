# MAiDAS

**Markdown AI Data Access Standard**

Padrão de acesso a dados web baseado em Markdown para agentes de IA

---

**Traduções**: [English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md) | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md) | [Español](README.es.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [العربية](README.ar.md) | [हिन्दी](README.hi.md) | [Italiano](README.it.md) | [Tiếng Việt](README.vi.md) | [ไทย](README.th.md) | [Bahasa Indonesia](README.id.md) | [Türkçe](README.tr.md) | [Polski](README.pl.md) | [Nederlands](README.nl.md) | [Українська](README.uk.md)

---

## Visão geral

MAiDAS é um padrão projetado para que agentes de IA possam ler e escrever dados web facilmente.

A web tradicional baseada em HTML/JavaScript tem problemas:
- Difícil para IA analisar
- Contém informações de interface desnecessárias
- Extração de dados estruturados complexa

MAiDAS lida com toda a troca de dados usando apenas Markdown.

## Princípios fundamentais

- **Simplicidade**: Markdown + regras mínimas
- **Conformidade com padrões**: Métodos HTTP padrão, RFC 7763 (text/markdown)
- **Operação paralela**: Pode funcionar junto com sites HTML existentes
- **Otimização para IA**: Apenas troca de dados, sem interface de usuário

## Início rápido

### 1. Criar ponto de entrada

```
/.well-known/index.md
```

```markdown
---
version: 0.1.0
---

# Meu Site

> Descrição do site

## API

- [Artigos](/articles/) - [schema](/articles/_schema.md)
```

### 2. Definir esquema

```
/articles/_schema.md
```

```markdown
# Articles API

## Campos

| Campo | Tipo | Obrigatório | Descrição |
|-------|------|-------------|-----------|
| title | string | Y | Título |
| content | string(markdown) | Y | Conteúdo |

## Ações

- GET /articles/_index.md - Lista
- GET /articles/{id}.md - Ler
- POST /articles/ - Criar
```

### 3. Fornecer dados

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

- [Primeira postagem](/articles/first.md) | 2025-01-27
- [Segunda postagem](/articles/second.md) | 2025-01-26
```

## Documentação

Consulte [SPECIFICATION.pt-BR.md](SPECIFICATION.pt-BR.md) para a especificação completa.

## Status

Este padrão está atualmente em status de **Rascunho**. Feedback e contribuições são bem-vindos.

## Contribuir

Issues e PRs são bem-vindos. Por favor, discuta a especificação em [Issues](https://github.com/TheMagicTower/maidas/issues).

## Licença

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
