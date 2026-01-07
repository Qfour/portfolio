# Git Workflow & Branch Strategy

## Branch Structure

```
main (production)
  ↑
develop (integration)
  ↑
feature/* (feature development)
```

## Branch Descriptions

### `main`
- **Purpose**: Production branch for GitHub Pages
- **Protection**: Direct commits prohibited
- **Updates**: Only via PR from `develop`
- **Tagging**: Version tags (v1.0.0, v1.1.0, etc.)
- **Deployment**: Auto-deploy to GitHub Pages via Actions

### `develop`
- **Purpose**: Development integration branch
- **Base for**: All feature branches
- **Updates**: Via PR from `feature/*` branches
- **Testing**: CI checks must pass before merge
- **State**: Should always be deployable

### `feature/*`
- **Purpose**: Individual feature development
- **Naming**: `feature/short-description`
  - Example: `feature/add-blog-section`
  - Example: `feature/update-skills`
- **Lifecycle**: Create → Develop → PR to develop → Delete after merge
- **Base**: Always branch from `develop`

## Workflow Examples

### Starting New Feature

```bash
# Ensure develop is up to date
git checkout develop
git pull origin develop

# Create feature branch
git checkout -b feature/add-contact-form

# Work on feature
# ... make changes ...
git add .
git commit -m "feat: add contact form component"

# Push to remote
git push -u origin feature/add-contact-form

# Create PR to develop via GitHub
```

### Releasing to Production

```bash
# After features merged to develop
git checkout develop
git pull origin develop

# Create PR from develop to main via GitHub
# After PR approved and merged, tag the release
git checkout main
git pull origin main
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

## Commit Message Convention

Follow Conventional Commits specification:

### Format
```
<type>(<scope>): <subject>

<body>

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `refactor`: Code refactoring
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `test`: Adding or updating tests
- `chore`: Build process or auxiliary tool changes
- `perf`: Performance improvements

### Examples

```bash
feat(skills): add Rust programming language

Add Rust to the skills section with proficiency level.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

```bash
fix(hero): correct typo in subtitle

Fix typo "specialzing" → "specializing"

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

## CI/CD Integration

### Pull Request Checks (develop)
- ✓ TypeScript type checking (`npm run check`)
- ✓ Build success (`npm run build`)
- ✓ No console errors in build output

### Deployment (main)
- Trigger: Push to `main` or tag creation
- Build: `npm run build`
- Deploy: GitHub Pages (dist/ folder)

## Agent Automation Guidelines

When Claude Code Agent commits:

1. **Always use detailed commit messages** following the convention
2. **Include Co-Authored-By** footer
3. **Run checks before committing**:
   ```bash
   npm run check  # TypeScript
   npm run build  # Build test
   ```
4. **Create feature branches** for non-trivial changes
5. **Use PRs for merges** to develop/main (except trivial docs)

## Branch Protection Rules (GitHub Settings)

### `main` branch
- ✓ Require pull request reviews (1 approval)
- ✓ Require status checks to pass
- ✓ Require branches to be up to date
- ✓ Include administrators

### `develop` branch
- ✓ Require status checks to pass
- ✓ Require branches to be up to date

## Quick Reference

```bash
# Check current branch
git branch

# View all branches
git branch -a

# Switch branch
git checkout <branch-name>

# Create and switch
git checkout -b <branch-name>

# Delete local branch
git branch -d <branch-name>

# Delete remote branch
git push origin --delete <branch-name>

# View commit history
git log --oneline --graph --all --decorate

# Check status
git status
```
