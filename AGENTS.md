# AGENTS.md - Portfolio Site Agent Instructions

> **役割分担**: このファイルはディレクトリ固有のコンテキスト提供用。グローバル設定は `.claude/settings.json`、詳細ガイドは `.claude/docs/` を参照。

## Project Context

GitHub Pages + Astro 5.x のポートフォリオサイト。静的生成でホスト。

## Quick Reference

| Item | Value |
|------|-------|
| Framework | Astro 5.x |
| Styling | Tailwind CSS 4.x |
| Language | TypeScript strict |
| Package Manager | pnpm |
| Deploy Target | GitHub Pages (静的) |

## Directory-Specific Instructions

### `/src/components/`
- 1ファイル1コンポーネント
- Props型を必ず定義、`interface Props { }`
- 外部classは`class:list`で結合

### `/src/pages/`
- Astroのファイルベースルーティング
- 各ページでBaseLayoutを使用
- メタ情報（title, description, og:image）必須

### `/src/content/`
- Content Collectionsでプロジェクト管理
- Markdown/MDX形式
- frontmatterスキーマは`src/content/config.ts`

### `/src/styles/`
- `global.css`にカスタムプロパティ定義
- カラーは`oklch()`形式
- ダークモード対応必須

## Code Patterns

```astro
// 推奨: Astroコンポーネント
---
interface Props {
  title: string;
  class?: string;
}
const { title, class: className } = Astro.props;
---
<h1 class:list={["text-4xl font-bold", className]}>{title}</h1>
```

## Constraints

- 画像: `<Image />`コンポーネント使用（自動最適化）
- アイコン: Lucide Icons
- アニメーション: View Transitions API優先
- バンドルサイズ: 極力小さく、不要な依存追加禁止

## Related Docs

- デザインシステム: `.claude/docs/design-system.md`
- コンポーネント仕様: `.claude/docs/components.md`
- デプロイ設定: `.claude/docs/deployment.md`
