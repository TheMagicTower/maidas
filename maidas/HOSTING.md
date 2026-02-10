---
title: MAiDAS Hosting Guide
---

# MAiDAS Hosting Guide

How to serve MAiDAS endpoints from your website.

## The Problem

Static site generators (Jekyll, Hugo, Astro, etc.) convert `.md` files to HTML. MAiDAS requires `.md` endpoints to return **raw markdown**, including YAML frontmatter. These two behaviors conflict.

## Solution: Post-Build Copy

Keep your MAiDAS files separate from your site source. After the static site build completes, copy the raw `.md` files into the build output, overwriting any HTML versions.

```
your-site/
├── _site/                  # Build output (auto-generated)
├── maidas/                 # Raw MAiDAS files (copied after build)
│   ├── .well-known/
│   │   └── index.md
│   └── articles/
│       ├── _schema.md
│       ├── _index.md
│       └── hello.md
├── index.html              # Your normal site
└── _config.yml             # Jekyll config
```

## GitHub Pages + Jekyll

### Step 1: Create a `maidas/` directory

Place your MAiDAS endpoint files inside `maidas/`. This directory mirrors the URL structure of your site:

```
maidas/
├── .well-known/
│   └── index.md        → https://yoursite.com/.well-known/index.md
├── articles/
│   ├── _schema.md      → https://yoursite.com/articles/_schema.md
│   ├── _index.md       → https://yoursite.com/articles/_index.md
│   └── my-post.md      → https://yoursite.com/articles/my-post.md
```

### Step 2: Add a GitHub Actions workflow

Create `.github/workflows/pages.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [gh-pages]  # or main, depending on your setup

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build with Jekyll
        uses: actions/jekyll-build-pages@v1

      - name: Copy MAiDAS raw markdown files
        run: cp -r maidas/. _site/

      - uses: actions/upload-pages-artifact@v3

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

### Step 3: Switch GitHub Pages to Actions

In your repository settings:
1. Go to **Settings > Pages**
2. Change **Source** to **GitHub Actions**

That's it. Jekyll builds your HTML site, then the raw `.md` files are copied on top.

## Hugo

Same pattern — place MAiDAS files in a separate directory and copy after build:

```yaml
- name: Build with Hugo
  run: hugo --minify

- name: Copy MAiDAS files
  run: cp -r maidas/. public/
```

## Astro / Next.js / Other SSGs

For any framework, the approach is identical:

1. Build the site normally
2. Copy `maidas/` contents into the output directory (`dist/`, `out/`, `public/`, etc.)

```bash
# Generic pattern
your-build-command
cp -r maidas/. your-output-dir/
```

## Plain Static Hosting

If you don't use a static site generator (e.g., plain Nginx, Caddy, S3), no special handling is needed. Just place `.md` files at the correct paths and they will be served as-is.

## Verification

After deploying, verify your endpoints return raw markdown:

```bash
curl -s https://yoursite.com/.well-known/index.md | head -3
# Expected:
# ---
# version: 0.1.0
# ---
```
