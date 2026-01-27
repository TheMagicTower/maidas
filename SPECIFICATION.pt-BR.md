# MAiDAS Specification v0.1.0

**Markdown AI Data Access Standard**

Padrão de acesso a dados web baseado em Markdown para agentes de IA

---

**Traduções**: [English](SPECIFICATION.md) | [한국어](SPECIFICATION.ko.md) | [日本語](SPECIFICATION.ja.md) | [简体中文](SPECIFICATION.zh-CN.md) | [繁體中文](SPECIFICATION.zh-TW.md) | [Español](SPECIFICATION.es.md) | [Français](SPECIFICATION.fr.md) | [Deutsch](SPECIFICATION.de.md) | [Русский](SPECIFICATION.ru.md) | [العربية](SPECIFICATION.ar.md) | [हिन्दी](SPECIFICATION.hi.md) | [Italiano](SPECIFICATION.it.md) | [Tiếng Việt](SPECIFICATION.vi.md) | [ไทย](SPECIFICATION.th.md) | [Bahasa Indonesia](SPECIFICATION.id.md) | [Türkçe](SPECIFICATION.tr.md) | [Polski](SPECIFICATION.pl.md) | [Nederlands](SPECIFICATION.nl.md) | [Українська](SPECIFICATION.uk.md)

---

## 1. Visão Geral

### 1.1 Propósito

MAiDAS é um padrão projetado para que agentes de IA possam ler e escrever dados web facilmente. A web tradicional baseada em HTML/JavaScript é difícil para IA analisar e contém informações desnecessárias. MAiDAS lida com toda a troca de dados usando apenas Markdown.

### 1.2 Princípios

- **Simplicidade**: Markdown + regras mínimas
- **Conformidade com padrões**: Métodos HTTP padrão, RFC 7763 (text/markdown)
- **Operação paralela**: Pode funcionar junto com sites HTML existentes
- **Otimização para IA**: Apenas troca de dados, sem interface de usuário

### 1.3 Tipo MIME

Todas as solicitações e respostas usam o seguinte Content-Type:

```
Content-Type: text/markdown; charset=utf-8
```

---

## 2. Estrutura de Arquivos

### 2.1 Estrutura Básica

```
example.com/
├── .well-known/
│   └── index.md           # Obrigatório. Ponto de entrada do site
├── articles/
│   ├── _schema.md         # Schema da API
│   ├── _index.md          # Lista
│   ├── first-post.md      # Documento individual
│   └── second-post.md
├── comments/
│   ├── _schema.md
│   ├── _index.md
│   └── ...
└── about.md
```

### 2.2 Arquivos de Sistema

Arquivos com prefixo underscore (`_`) são arquivos de sistema:

| Arquivo | Propósito |
|---------|-----------|
| `_schema.md` | Definição do schema CRUD de recursos |
| `_index.md` | Lista de recursos |

---

## 3. Ponto de Entrada

### 3.1 Localização

```
/.well-known/index.md
```

### 3.2 Formato

```markdown
---
version: 0.1.0
---

# Nome do Site

> Descrição do site em uma linha

## Páginas

- [Sobre](/about.md)
- [Lista de Artigos](/articles/_index.md)

## API

- [Gerenciamento de Artigos](/articles/) - [schema](/articles/_schema.md)
- [Gerenciamento de Comentários](/comments/) - [schema](/comments/_schema.md)
```

### 3.3 Campos

| Campo | Obrigatório | Descrição |
|-------|-------------|-----------|
| version | Y | Versão da especificação MAiDAS |

---

## 4. Definição de Schema

### 4.1 Localização

`_schema.md` dentro de cada pasta de recursos

### 4.2 Formato

```markdown
# Articles API

## Autenticação

- Método: Bearer
- Cabeçalho: `Authorization: Bearer {token}`
- Obrigatório: Y

## Campos

| Campo | Tipo | Obrigatório | Descrição |
|-------|------|-------------|-----------|
| title | string | Y | Título |
| content | string(markdown) | Y | Conteúdo |
| tags | array(string) | N | Lista de tags |
| date | date | N | Data de criação |

## Ações

- GET /articles/_index.md - Lista
- GET /articles/{id}.md - Ler
- POST /articles/ - Criar
- PUT /articles/{id}.md - Atualizar
- DELETE /articles/{id}.md - Excluir
```

### 4.3 Sistema de Tipos

**Tipos Básicos**

| Tipo | Descrição |
|------|-----------|
| string | String |
| number | Número |
| boolean | Verdadeiro/Falso |
| date | Data (ISO 8601) |
| array | Array |

**Dicas de Formato (Opcional)**

Notação entre parênteses após o tipo:

| Formato | Descrição |
|---------|-----------|
| string(markdown) | Markdown permitido |
| string(url) | Formato URL |
| string(email) | Formato email |
| date(datetime) | Data+Hora |
| array(string) | Array de strings |
| array(number) | Array de números |

---

## 5. Resposta de Lista

### 5.1 Localização

`_index.md` dentro de cada pasta de recursos

### 5.2 Formato

```markdown
---
page: 1
limit: 20
total: 57
---

# Articles

> title | date | tags

- [Primeira Postagem](/articles/first-post.md) | 2025-01-27 | #tech, #ai
- [Segunda Postagem](/articles/second-post.md) | 2025-01-26 | #diary
- [Terceira Postagem](/articles/third-post.md) | 2025-01-25 |
```

### 5.3 Regras

- Definir cabeçalhos de campo com blockquote (`>`) na primeira linha
- Listar valores de campo após o link com separador `|`
- Deixar valores vazios em branco
- Informações de paginação no frontmatter

---

## 6. Documento Individual

### 6.1 Formato

```markdown
---
title: Primeira Postagem
date: 2025-01-27
tags: [tech, ai]
---

# Primeira Postagem

Conteúdo do corpo...
```

### 6.2 Regras

- frontmatter: formato YAML (envolvido com `---`)
- Corpo: Markdown
- Campos obrigatórios: Conforme definido em `_schema.md`

---

## 7. Solicitação/Resposta CRUD

### 7.1 Criar (POST)

**Solicitação**
```
POST /articles/
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Nova Postagem
tags: [tech]
---

# Nova Postagem

Conteúdo...
```

**Resposta**
```markdown
---
status: 201
---

# Created

Documento criado: /articles/new-post.md
```

### 7.2 Ler (GET)

**Lista**
```
GET /articles/_index.md
```

**Individual**
```
GET /articles/first-post.md
```

### 7.3 Atualizar (PUT)

**Solicitação**
```
PUT /articles/first-post.md
Content-Type: text/markdown; charset=utf-8
Authorization: Bearer {token}

---
title: Título Atualizado
tags: [tech, ai]
---

# Título Atualizado

Corpo atualizado...
```

**Resposta**
```markdown
---
status: 200
---

# OK

Documento atualizado.
```

### 7.4 Excluir (DELETE)

**Solicitação**
```
DELETE /articles/first-post.md
Authorization: Bearer {token}
```

**Resposta**
```markdown
---
status: 200
---

# OK

Documento excluído.
```

---

## 8. Parâmetros de Consulta

### 8.1 Paginação

```
GET /articles/_index.md?page=2&limit=20
```

| Parâmetro | Padrão | Descrição |
|-----------|--------|-----------|
| page | 1 | Número da página |
| limit | Config do servidor | Itens por página |

### 8.2 Filtro

Usar nomes de campo como parâmetros de consulta:

```
GET /articles/_index.md?tag=tech
GET /articles/_index.md?date=2025-01
```

### 8.3 Ordenação

```
GET /articles/_index.md?sort=-date,title
```

**Regras**
- Usar parâmetro `sort`
- Vírgula para múltiplos campos
- Prefixo `-`: Descendente
- Sem prefixo: Ascendente

---

## 9. Autenticação

### 9.1 Método

Autenticação Bearer token HTTP padrão

```
Authorization: Bearer {token}
```

---

## 10. Tratamento de Erros

### 10.1 Formato

```markdown
---
status: {código HTTP}
---

# {Título do Erro}

{Descrição do erro}
```

### 10.2 Códigos de Status

| Código | Título | Descrição |
|--------|--------|-----------|
| 200 | OK | Sucesso |
| 201 | Created | Criado |
| 400 | Bad Request | Solicitação inválida |
| 401 | Unauthorized | Autenticação necessária |
| 403 | Forbidden | Sem permissão |
| 404 | Not Found | Documento não encontrado |
| 500 | Internal Server Error | Erro do servidor |

---

## 11. Exemplo Completo

### 11.1 Ponto de Entrada

`GET /.well-known/index.md`

```markdown
---
version: 0.1.0
---

# Meu Blog

> Um blog pessoal.

## Páginas

- [Sobre](/about.md)

## API

- [Gerenciamento de Artigos](/articles/) - [schema](/articles/_schema.md)
```

---

## Apêndice: Histórico de Versões

| Versão | Data | Alterações |
|--------|------|------------|
| 0.1.0 | 2025-01-27 | Lançamento inicial do rascunho |

---

## Conformance Examples (v0.1)

Reference conformance examples live in:
- `conformance/v0.1/valid/`
- `conformance/v0.1/invalid/`
