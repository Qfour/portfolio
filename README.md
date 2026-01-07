# Portfolio Website

Personal portfolio website for Yoshiki Doi - Infrastructure & Security Engineer.

## Tech Stack

- **Framework**: [Astro 5.x](https://astro.build/) - Static site generator
- **Styling**: [Tailwind CSS 4.x](https://tailwindcss.com/) - Utility-first CSS
- **Language**: TypeScript (strict mode)
- **Package Manager**: npm
- **Deployment**: GitHub Pages via GitHub Actions

## Quick Start

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview

# Type check
npm run check
```

## Project Structure

```
portfolio/
├── .github/
│   └── workflows/       # CI/CD workflows
├── .claude/             # Claude Code configuration
│   ├── settings.json    # Permissions and settings
│   └── docs/            # Detailed documentation
├── src/
│   ├── components/
│   │   ├── common/      # Reusable UI components
│   │   ├── layout/      # Header, Footer
│   │   └── sections/    # Hero, Skills, Experience, etc.
│   ├── layouts/         # Base layout templates
│   ├── pages/           # Route pages
│   ├── styles/          # Global styles and Tailwind config
│   └── utils/           # Helper functions
├── public/              # Static assets
├── astro.config.mjs     # Astro configuration
├── tsconfig.json        # TypeScript configuration
└── package.json         # Dependencies and scripts
```

## Development

### Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Start dev server at `localhost:4321` |
| `npm run build` | Build for production → `dist/` |
| `npm run preview` | Preview production build |
| `npm run check` | TypeScript type checking |

### Git Workflow

See [.claude/docs/git-workflow.md](./.claude/docs/git-workflow.md) for detailed Git workflow.

**Branch Structure:**
- `main` - Production (GitHub Pages)
- `develop` - Development integration
- `feature/*` - Feature development

**Workflow:**
1. Create feature branch from `develop`
2. Make changes and commit
3. Create PR to `develop`
4. After merge, create PR from `develop` to `main` for release

## Deployment

### GitHub Pages Setup

1. **Enable GitHub Pages**
   - Settings → Pages → Source: "GitHub Actions"

2. **Branch Protection** (Recommended)
   - Protect `main` and `develop` branches
   - Require PR reviews and status checks

3. **Push to Deploy**
   - Push to `main` triggers automatic deployment
   - Site available at: https://09or1.github.io/portfolio/

See [.claude/docs/deployment.md](./.claude/docs/deployment.md) for detailed deployment guide.

## CI/CD

### Workflows

**CI Checks** (`.github/workflows/ci.yml`)
- Runs on PRs to `develop` and `main`
- TypeScript type checking
- Build validation

**Deployment** (`.github/workflows/deploy.yml`)
- Runs on push to `main`
- Builds and deploys to GitHub Pages

## Documentation

- [CLAUDE.md](./CLAUDE.md) - Project overview and guidelines
- [.claude/docs/git-workflow.md](./.claude/docs/git-workflow.md) - Git workflow and branch strategy
- [.claude/docs/deployment.md](./.claude/docs/deployment.md) - Deployment guide
- [.claude/docs/design-system.md](./.claude/docs/design-system.md) - Design tokens and styling
- [.claude/docs/components.md](./.claude/docs/components.md) - Component specifications

## Key Features

- Responsive design for all devices
- Optimized performance (Lighthouse 90+)
- Type-safe with TypeScript
- Component-based architecture
- Automated CI/CD pipeline
- Dark theme with gradient accents

## Code Style

- **TypeScript**: Strict mode, no `any`
- **Components**: One component per file, typed props
- **Styling**: Tailwind utilities first, minimal custom CSS
- **Commits**: Conventional Commits format

## License

© 2024 Yoshiki Doi. All rights reserved.

## Contact

- Email: yoshiki.doi.dev@gmail.com
- GitHub: [@09or1](https://github.com/09or1)
