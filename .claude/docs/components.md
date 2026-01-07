# Components Specification

## Principles

1. **Single Responsibility**: 1コンポーネント = 1目的
2. **Props First**: スタイルのカスタマイズはProps経由
3. **Composition**: 小さなコンポーネントを組み合わせる
4. **Type Safety**: Props型定義必須

## Directory Structure

```
src/components/
├── common/          # 汎用UI部品
│   ├── Button.astro
│   ├── Card.astro
│   ├── Badge.astro
│   └── Icon.astro
├── layout/          # レイアウト部品
│   ├── Header.astro
│   ├── Footer.astro
│   ├── Nav.astro
│   └── Container.astro
└── sections/        # ページセクション
    ├── Hero.astro
    ├── About.astro
    ├── Projects.astro
    ├── Skills.astro
    └── Contact.astro
```

## Component Template

```astro
---
interface Props {
  /** 必須プロパティ */
  title: string;
  /** オプショナル */
  description?: string;
  /** 追加クラス */
  class?: string;
}

const { title, description, class: className } = Astro.props;
---

<div class:list={["base-styles", className]}>
  <h2>{title}</h2>
  {description && <p>{description}</p>}
  <slot />
</div>
```

## Common Components

### Button

```astro
---
interface Props {
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  href?: string;
  class?: string;
}

const { variant = 'primary', size = 'md', href, class: className } = Astro.props;

const baseStyles = "inline-flex items-center justify-center font-medium rounded-lg transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2";

const variants = {
  primary: "bg-blue-600 text-white hover:bg-blue-700 focus-visible:ring-blue-500",
  secondary: "bg-slate-100 text-slate-900 hover:bg-slate-200 dark:bg-slate-800 dark:text-slate-100",
  ghost: "text-slate-600 hover:bg-slate-100 dark:text-slate-400 dark:hover:bg-slate-800",
};

const sizes = {
  sm: "px-3 py-1.5 text-sm",
  md: "px-5 py-2.5 text-base",
  lg: "px-7 py-3.5 text-lg",
};

const Element = href ? 'a' : 'button';
---

<Element
  href={href}
  class:list={[baseStyles, variants[variant], sizes[size], className]}
>
  <slot />
</Element>
```

### Card

```astro
---
interface Props {
  href?: string;
  class?: string;
}

const { href, class: className } = Astro.props;
const Element = href ? 'a' : 'article';
---

<Element
  href={href}
  class:list={[
    "block bg-white dark:bg-slate-900",
    "border border-slate-200 dark:border-slate-800",
    "rounded-2xl overflow-hidden",
    href && "hover:shadow-lg transition-shadow",
    className
  ]}
>
  <slot />
</Element>
```

### Badge

```astro
---
interface Props {
  variant?: 'default' | 'primary' | 'success';
}

const { variant = 'default' } = Astro.props;

const variants = {
  default: "bg-slate-100 text-slate-700 dark:bg-slate-800 dark:text-slate-300",
  primary: "bg-blue-100 text-blue-700 dark:bg-blue-900 dark:text-blue-300",
  success: "bg-green-100 text-green-700 dark:bg-green-900 dark:text-green-300",
};
---

<span class:list={["px-2.5 py-1 text-xs font-medium rounded-full", variants[variant]]}>
  <slot />
</span>
```

## Section Components

### Hero

```astro
---
interface Props {
  name: string;
  title: string;
  description: string;
}

const { name, title, description } = Astro.props;
---

<section class="py-20 md:py-32">
  <div class="max-w-6xl mx-auto px-4 md:px-8">
    <p class="text-blue-600 dark:text-blue-400 font-medium mb-4">
      {name}
    </p>
    <h1 class="text-5xl md:text-7xl font-bold tracking-tight mb-6">
      {title}
    </h1>
    <p class="text-xl md:text-2xl text-slate-600 dark:text-slate-400 max-w-2xl">
      {description}
    </p>
    <div class="mt-10 flex gap-4">
      <slot name="actions" />
    </div>
  </div>
</section>
```

### Projects

```astro
---
import Card from '../common/Card.astro';
import Badge from '../common/Badge.astro';

interface Project {
  title: string;
  description: string;
  image: string;
  tags: string[];
  href: string;
}

interface Props {
  projects: Project[];
}

const { projects } = Astro.props;
---

<section class="py-20 md:py-32">
  <div class="max-w-6xl mx-auto px-4 md:px-8">
    <h2 class="text-3xl md:text-4xl font-bold mb-12">Projects</h2>
    <div class="grid gap-8 md:grid-cols-2 lg:grid-cols-3">
      {projects.map((project) => (
        <Card href={project.href}>
          <img src={project.image} alt={project.title} class="w-full aspect-video object-cover" />
          <div class="p-6">
            <h3 class="text-xl font-semibold mb-2">{project.title}</h3>
            <p class="text-slate-600 dark:text-slate-400 mb-4">{project.description}</p>
            <div class="flex flex-wrap gap-2">
              {project.tags.map((tag) => <Badge>{tag}</Badge>)}
            </div>
          </div>
        </Card>
      ))}
    </div>
  </div>
</section>
```

## Layout Components

### Header

```astro
---
interface Props {
  currentPath: string;
}

const { currentPath } = Astro.props;

const links = [
  { href: '/', label: 'Home' },
  { href: '/projects', label: 'Projects' },
  { href: '/about', label: 'About' },
];
---

<header class="sticky top-0 z-50 bg-white/80 dark:bg-slate-950/80 backdrop-blur-sm border-b border-slate-200 dark:border-slate-800">
  <div class="max-w-6xl mx-auto px-4 md:px-8">
    <nav class="flex items-center justify-between h-16">
      <a href="/" class="font-bold text-xl">Portfolio</a>
      <ul class="flex items-center gap-8">
        {links.map((link) => (
          <li>
            <a
              href={link.href}
              class:list={[
                "text-sm font-medium transition-colors",
                currentPath === link.href
                  ? "text-blue-600 dark:text-blue-400"
                  : "text-slate-600 hover:text-slate-900 dark:text-slate-400 dark:hover:text-slate-100"
              ]}
            >
              {link.label}
            </a>
          </li>
        ))}
      </ul>
    </nav>
  </div>
</header>
```

## Best Practices

### Class Merging

外部からのクラスは`class:list`で結合:

```astro
<div class:list={["base-class", Astro.props.class]} />
```

### Conditional Rendering

```astro
{condition && <Component />}
{items.map((item) => <Item {...item} />)}
```

### Slots

名前付きスロットで柔軟な構成:

```astro
<slot />                    <!-- デフォルト -->
<slot name="header" />      <!-- 名前付き -->
<slot name="footer">        <!-- フォールバック -->
  <p>Default footer</p>
</slot>
```

### Image Handling

```astro
---
import { Image } from 'astro:assets';
import myImage from '../assets/image.png';
---

<Image src={myImage} alt="Description" width={800} />
```
