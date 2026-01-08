# Design System

## Colors

Sysdigインスパイアのカラーパレット。モダンなテック系アプリらしいダークテーマベース。

### Palette

```css
:root {
  /* Sysdig-inspired color palette */
  --color-bg-primary: #1E1E22;
  --color-bg-secondary: #2a2a2e;
  --color-bg-tertiary: #34343a;
  --color-text-primary: #F9F9F9;
  --color-text-secondary: #c0c0c8;
  --color-text-muted: #8a8a92;
  --color-accent-green: #BDF78B;
  --color-accent-green-dark: #a8e574;
  --color-border: #3a3a3e;
}
```

### Color Reference

| 用途 | Color | Hex Code |
|------|-------|----------|
| Primary Background | Dark Charcoal | `#1E1E22` |
| Secondary Background | Medium Gray | `#2a2a2e` |
| Tertiary Background | Light Gray | `#34343a` |
| Primary Text | Off-white | `#F9F9F9` |
| Secondary Text | Light Gray | `#c0c0c8` |
| Muted Text | Medium Gray | `#8a8a92` |
| Accent (Primary) | Lime Green | `#BDF78B` |
| Accent (Dark) | Dark Lime | `#a8e574` |
| Border | Dark Gray | `#3a3a3e` |

### Tailwind Mapping

| 用途 | カスタムCSS変数 | 代替Tailwindクラス |
|------|-------------|------------------|
| Primary Background | `bg-[var(--color-bg-primary)]` | `bg-gray-900` |
| Secondary Background | `bg-[var(--color-bg-secondary)]` | `bg-gray-800` |
| Primary Text | `text-[var(--color-text-primary)]` | `text-gray-50` |
| Muted Text | `text-[var(--color-text-muted)]` | `text-gray-400` |
| Border | `border-[var(--color-border)]` | `border-gray-700` |
| Accent | `text-[var(--color-accent-green)]` | `text-green-400` |

## Typography

### Font Stack

```css
--font-sans: 'Inter', 'Noto Sans JP', system-ui, sans-serif;
--font-mono: 'JetBrains Mono', ui-monospace, monospace;
```

### Scale

| Element | Classes |
|---------|---------|
| Hero | `text-5xl md:text-7xl font-bold tracking-tight` |
| H1 | `text-4xl md:text-5xl font-bold` |
| H2 | `text-3xl md:text-4xl font-semibold` |
| H3 | `text-xl md:text-2xl font-semibold` |
| Body | `text-base md:text-lg leading-relaxed` |
| Small | `text-sm text-slate-500` |
| Code | `font-mono text-sm` |

### Japanese Text

- 本文: `Noto Sans JP` weight 400
- 見出し: `Noto Sans JP` weight 700
- サブセット化推奨（WOFF2形式）

## Spacing

### Section

```
セクション間: py-20 md:py-32
セクション内: space-y-12 md:space-y-16
```

### Container

```
max-w-6xl mx-auto px-4 md:px-8
```

### Grid

```
グリッド: grid gap-6 md:gap-8
2列: grid-cols-1 md:grid-cols-2
3列: grid-cols-1 md:grid-cols-2 lg:grid-cols-3
```

## Components

### Button

```astro
<button class="
  px-6 py-3
  bg-[var(--color-accent-green)] hover:bg-[var(--color-accent-green-dark)]
  text-[var(--color-bg-primary)] font-medium
  rounded-lg
  transition-colors
  focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-[var(--color-accent-green)] focus-visible:ring-offset-2
">
  Button Text
</button>
```

### Card

```astro
<article class="
  bg-[var(--color-bg-secondary)]
  border border-[var(--color-border)]
  rounded-2xl
  overflow-hidden
  hover:shadow-lg
  transition-shadow
">
  <!-- content -->
</article>
```

### Link

```astro
<a class="
  text-[var(--color-accent-green)]
  hover:underline
  underline-offset-4
  transition-colors
">
  Link Text
</a>
```

## Animation

### Transitions

```css
/* 標準 */
transition-all duration-200 ease-out

/* ホバー */
transition-colors duration-150

/* スケール */
transition-transform hover:scale-105
```

### View Transitions

```astro
---
import { ViewTransitions } from 'astro:transitions';
---
<head>
  <ViewTransitions />
</head>

<main transition:animate="slide">
  <!-- content -->
</main>
```

## Responsive Breakpoints

| Breakpoint | Width | Usage |
|------------|-------|-------|
| `sm` | 640px | モバイル横向き |
| `md` | 768px | タブレット |
| `lg` | 1024px | デスクトップ |
| `xl` | 1280px | 大画面 |

## Dark Mode

`class`ストラテジーを使用:

```javascript
// tailwind.config.mjs
export default {
  darkMode: 'class',
}
```

トグル実装:
```javascript
document.documentElement.classList.toggle('dark');
localStorage.setItem('theme', isDark ? 'dark' : 'light');
```
