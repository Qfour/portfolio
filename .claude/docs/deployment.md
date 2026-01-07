# Deployment Guide

## GitHub Pages Configuration

### Astro Config

```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  site: 'https://09or1.github.io',
  base: '/portfolio',
  vite: {
    plugins: [tailwindcss()],
  },
});
```

Current project settings:
- Site: `https://09or1.github.io`
- Base path: `/portfolio`
- Package manager: `npm` (not pnpm)

### GitHub Actions Workflows

Two workflows are configured:

#### 1. CI Checks (`.github/workflows/ci.yml`)
Runs on PRs to `develop` and `main`:
- TypeScript type checking
- Build test
- Output validation

#### 2. Deployment (`.github/workflows/deploy.yml`)
Runs on push to `main`:
- Builds the Astro site
- Deploys to GitHub Pages

See actual workflow files in `.github/workflows/` directory.

## Repository Setup

### 1. GitHub Pages有効化

1. Repository → Settings → Pages
2. Source: **"GitHub Actions"** を選択（重要）
3. Branch設定は不要（Actionsが自動管理）

### 2. Branch Protection Rules

#### `main` branch
1. Repository → Settings → Branches → Add rule
2. Branch name pattern: `main`
3. Enable:
   - ✓ Require a pull request before merging
   - ✓ Require status checks to pass before merging
   - ✓ Require branches to be up to date before merging
   - Status checks: `TypeScript Type Check`, `Build Test`

#### `develop` branch
1. Add rule for `develop`
2. Enable:
   - ✓ Require status checks to pass before merging
   - Status checks: `TypeScript Type Check`, `Build Test`

### 3. Base Path設定

| リポジトリ形式 | URL | base設定 |
|---------------|-----|----------|
| `username.github.io` | `https://username.github.io/` | 不要 |
| `username/portfolio` | `https://username.github.io/portfolio/` | `base: '/portfolio'` |

Current: `09or1/portfolio` → base: `/portfolio`

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
npm run build
npm run preview
```

### Cache Issues

```yaml
# workflowにキャッシュクリア追加
- name: Clear cache
  run: rm -rf node_modules/.cache
```
