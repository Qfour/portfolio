# Deployment Guide - Local Build Edition

## Overview

このプロジェクトは**ローカルビルド優先**の運用を採用しています：
- 開発者がローカルでビルド実行
- ビルド成果物（`dist/`）をGitにコミット
- GitHub Pagesで静的ファイルを直接配信

GitHub Actionsワークフローは無効化されています（将来利用時に `.disabled` 拡張子を削除）。

## Local Build Configuration

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
- Package manager: `npm`
- Build output: `dist/` (Git管理対象)

## Local Build & Deploy Workflow

### 1. Development & Build

```bash
# 依存関係インストール
npm install

# 開発サーバー起動
npm run dev

# 型チェック
npm run check

# 本番ビルド（dist/ディレクトリ生成）
npm run build

# ビルド結果の確認
npm run preview
```

### 2. Deploy Process

```bash
# 1. ビルド実行
npm run build

# 2. .nojekyllファイル作成（GitHub Pages用）
touch dist/.nojekyll

# 3. 変更をコミット
git add .
git commit -m "feat: update portfolio with latest changes

Co-Authored-By: Claude Sonnet 4 <noreply@anthropic.com>"

# 4. GitHubにプッシュ
git push origin main
```

### 3. GitHub Pages Setup

1. Repository → Settings → Pages
2. Source: **"Deploy from a branch"** を選択
3. Branch: **"main"** を選択
4. Folder: **"/ (root)"** を選択
   - distディレクトリがルートにコミットされているため

### 4. Access Your Site

サイトURL: `https://09or1.github.io/portfolio`

## Configuration Notes

### Base Path Setting

| リポジトリ形式 | URL | base設定 |
|---------------|-----|----------|
| `username.github.io` | `https://username.github.io/` | 不要 |
| `username/portfolio` | `https://username.github.io/portfolio/` | `base: '/portfolio'` |

Current: `09or1/portfolio` → base: `/portfolio`

### File Structure (after build)

```
portfolio/
├── dist/               # ビルド成果物（Git管理対象）
│   ├── .nojekyll       # GitHub Pages用
│   ├── index.html      # メインページ
│   └── ...             # その他のアセット
├── src/                # ソースコード
├── .github/workflows/  # 無効化済み（*.disabled）
└── astro.config.mjs    # Astro設定
```

## Future: GitHub Actions

将来的にGitHub Actionsを使用する場合：

```bash
# ワークフローを有効化
mv .github/workflows/ci.yml.disabled .github/workflows/ci.yml
mv .github/workflows/deploy.yml.disabled .github/workflows/deploy.yml

# .gitignoreでdistを除外
echo "dist/" >> .gitignore
```

## Troubleshooting

### Build Failures

```bash
# ローカルでビルド確認
npm run build
npm run preview
```

### 404 on GitHub Pages

1. GitHub Pages設定を確認
2. `dist/.nojekyll` ファイルが存在するか確認
3. `base: '/portfolio'` 設定が正しいか確認

### Assets Not Loading

`base`パス設定が正しいか確認。Astroが自動的にアセットパスを調整します。
