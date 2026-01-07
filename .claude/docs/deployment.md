# Deployment Guide

## GitHub Pages Configuration

### Astro Config

```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  site: 'https://USERNAME.github.io',
  base: '/REPO_NAME', // ルートリポジトリなら省略
  output: 'static',
  build: {
    assets: 'assets',
  },
  vite: {
    plugins: [tailwindcss()],
  },
});
```

### GitHub Actions Workflow

```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup pnpm
        uses: pnpm/action-setup@v4
        with:
          version: 9

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: pnpm

      - name: Install dependencies
        run: pnpm install

      - name: Build
        run: pnpm build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## Repository Setup

### 1. GitHub Pages有効化

1. Repository → Settings → Pages
2. Source: "GitHub Actions" を選択

### 2. Base Path設定

| リポジトリ形式 | URL | base設定 |
|---------------|-----|----------|
| `username.github.io` | `https://username.github.io/` | 不要 |
| `username/portfolio` | `https://username.github.io/portfolio/` | `base: '/portfolio'` |

### 3. Custom Domain（オプション）

1. `public/CNAME`を作成:
   ```
   yourdomain.com
   ```

2. astro.config.mjsを更新:
   ```javascript
   site: 'https://yourdomain.com',
   // base不要
   ```

3. DNSレコード設定:
   - A: `185.199.108.153`
   - A: `185.199.109.153`
   - A: `185.199.110.153`
   - A: `185.199.111.153`
   - または CNAME: `username.github.io`

## Build Optimization

### Image Optimization

```javascript
// astro.config.mjs
export default defineConfig({
  image: {
    service: {
      entrypoint: 'astro/assets/services/sharp',
    },
  },
});
```

### Compression

```bash
pnpm add -D @playform/compress
```

```javascript
// astro.config.mjs
import compress from '@playform/compress';

export default defineConfig({
  integrations: [compress()],
});
```

## Environment Variables

### Local Development

```bash
# .env.local (gitignore対象)
PUBLIC_SITE_URL=http://localhost:4321
```

### Production

GitHub Secrets経由（必要に応じて）:
- Repository → Settings → Secrets and variables → Actions

```yaml
# workflow内で使用
env:
  PUBLIC_API_KEY: ${{ secrets.API_KEY }}
```

## Performance Checklist

- [ ] Lighthouse Score 90+
- [ ] 画像最適化（WebP/AVIF）
- [ ] フォントサブセット化
- [ ] Critical CSS インライン化
- [ ] JavaScript最小化
- [ ] gzip/brotli圧縮

## Troubleshooting

### 404 on Subpages

`base`設定が正しいか確認。全リンクで`base`を考慮:

```astro
<a href={`${import.meta.env.BASE_URL}projects`}>Projects</a>
```

### Assets Not Loading

`base`パスがアセットURLに含まれているか確認。

### Build Failures

```bash
# ローカルでビルド確認
pnpm build
pnpm preview
```

### Cache Issues

```yaml
# workflowにキャッシュクリア追加
- name: Clear cache
  run: rm -rf node_modules/.cache
```
