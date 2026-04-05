# Malagueta Theme — Hugo

Tema editorial para o Portal Malagueta. Layout em três colunas com hero, sidebar e seções por categoria.

## Instalação

```bash
# No diretório raiz do seu site Hugo:
mkdir themes
cp -r malagueta-theme themes/malagueta-theme
```

## Configuração

Copie `hugo.toml.example` para a raiz do seu site como `hugo.toml` e ajuste:

```toml
baseURL = "https://seusite.com/"
title   = "Portal Malagueta"
theme   = "malagueta-theme"

[params]
  description   = "Descrição do portal"
  navCategories = ["Notícias", "Eventos", "Resenhas", "Entrevistas"]
  sidebarCount  = 6
```

> **navCategories** define quais categorias aparecem no navbar e nas seções da homepage.  
> Os nomes devem bater exatamente com as `categories` dos posts.

## Estrutura de conteúdo

```
content/
  posts/
    meu-post.md
  sobre.md
```

## Front matter dos posts

```yaml
---
title: "Título do post"
subtitle: "Uma linha de chamada"
date: 2025-03-15
draft: false
author: "Nome do autor"
cover: "https://url-da-imagem.jpg"   # imagem de capa
categories: ["Notícias"]             # deve ser uma das navCategories
tags: ["cultura", "bh"]
summary: "Resumo curto para listagens e cards."
---
```

## Criar novo post

```bash
hugo new posts/meu-post.md
```

## Executar localmente

```bash
hugo server -D
```

## Páginas geradas automaticamente

| URL | Conteúdo |
|-----|----------|
| `/` | Homepage com hero + seções |
| `/posts/` | Todos os posts |
| `/categories/noticias/` | Posts da categoria |
| `/tags/cultura/` | Posts com a tag |
| `/sobre/` | Página sobre |

## Imagens

Recomenda-se usar URLs externas (ex: Cloudinary, Unsplash, seu CDN) no campo `cover`.  
Para imagens locais, coloque em `static/images/` e use `/images/foto.jpg`.
