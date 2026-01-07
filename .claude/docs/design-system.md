# Design System

## Colors

oklch()形式でモダンなカラースペースを使用。

### Palette

```css
:root {
  /* Primary */
  --color-primary: oklch(65% 0.15 250);
  --color-primary-hover: oklch(55% 0.15 250);
  
  /* Neutral */
  --color-bg: oklch(98% 0.005 250);
  --color-surface: oklch(100% 0 0);
  --color-text: oklch(20% 0.02 250);
  --color-text-muted: oklch(45% 0.02 250);
  --color-border: oklch(90% 0.01 250);
}

.dark {
  --color-bg: oklch(15% 0.02 250);
  --color-surface: oklch(20% 0.02 250);
  --color-text: oklch(95% 0.005 250);
  --color-text-muted: oklch(65% 0.01 250);
  --color-border: oklch(30% 0.02 250);
}
```

### Tailwind Mapping

| 用途 | Light | Dark |
|------|-------|------|
| Background | `bg-slate-50` | `dark:bg-slate-950` |
| Surface | `bg-white` | `dark:bg-slate-900` |
| Text | `text-slate-900` | `dark:text-slate-50` |
| Muted | `text-slate-500` | `dark:text-slate-400` |
| Border | `border-slate-200` | `dark:border-slate-800` |
| Accent | `text-blue-600` | `dark:text-blue-400` |

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
  bg-blue-600 hover:bg-blue-700 
  text-white font-medium 
  rounded-lg 
  transition-colors
  focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2
">
  Button Text
</button>
```

### Card

```astro
<article class="
  bg-white dark:bg-slate-900 
  border border-slate-200 dark:border-slate-800 
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
  text-blue-600 dark:text-blue-400 
  hover:underline 
  underline-offset-4
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
