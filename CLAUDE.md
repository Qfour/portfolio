# CLAUDE.md - Portfolio Project

## Overview

GitHub Pages向けポートフォリオサイト。Astro 5.x + Tailwind CSS 4.x + TypeScript。

## Tech Stack

- **Framework**: Astro 5.x (静的生成)
- **Styling**: Tailwind CSS 4.x
- **Language**: TypeScript (strict)
- **Package Manager**: pnpm
- **Deploy**: GitHub Pages via GitHub Actions

## Project Structure

```
├── src/
│   ├── components/    # UIコンポーネント
│   │   ├── common/    # Button, Card等
│   │   ├── layout/    # Header, Footer
│   │   └── sections/  # Hero, About, Projects
│   ├── layouts/       # BaseLayout.astro
│   ├── pages/         # ルーティング
│   ├── content/       # MDX/Markdownコンテンツ
│   ├── styles/        # global.css
│   └── utils/         # ヘルパー関数
├── public/            # 静的アセット
├── .claude/           # Claude Code設定
│   ├── settings.json  # 権限設定
│   └── docs/          # 詳細ドキュメント
└── .github/workflows/ # CI/CD
```

## Commands

```bash
pnpm dev       # 開発サーバー (localhost:4321)
pnpm build     # 本番ビルド → dist/
pnpm preview   # ビルドプレビュー
pnpm check     # TypeScript型チェック
```

## Key Rules

1. **TypeScript**: strict mode、any禁止
2. **Components**: Props型定義必須、1ファイル1コンポーネント
3. **Styling**: Tailwind優先、カスタムCSSは最小限
4. **Images**: `<Image />`コンポーネント使用
5. **A11y**: セマンティックHTML、alt属性必須

## Docs Reference

詳細は `.claude/docs/` を参照:
- `design-system.md` - カラー、タイポグラフィ、スペーシング
- `components.md` - コンポーネント仕様とパターン
- `deployment.md` - GitHub Pages設定とワークフロー
