# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal blog for Paulo Monteiro built with Hugo (static site generator) using the Chirpy theme. The blog is deployed to GitHub Pages at https://pauloportugal.github.io.

## Common Commands

### Development
```bash
# Start local development server (includes live reload)
hugo serve

# Start server accessible from network
hugo serve --bind 0.0.0.0

# Build site for production (outputs to public/)
hugo

# Clean Hugo module cache
hugo mod clean

# Update Hugo modules
hugo mod get -u

# Tidy Hugo modules (remove unused)
hugo mod tidy
```

### Content Creation
```bash
# Create new post (will use archetype template)
hugo new post/YYYY-MM-DD-post-title/index.en.md

# Create new page
hugo new about/index.en.md
```

### Deployment
The `public/` directory is a Git submodule pointing to https://github.com/PauloPortugal/pauloportugal.github.io.git

Manual deployment process:
```bash
# 1. Build the site
hugo

# 2. Deploy to GitHub Pages
cd public
git add .
git commit -m "Deploy site"
git push origin main
cd ..

# 3. (Optional) Commit source changes
git add .
git commit -m "Update site source"
git push
```

## Conventional Commits & Automated Releases

This repository uses **Conventional Commits** format to automatically generate releases, version numbers, and changelogs via GitHub Actions.

### Commit Message Format

Follow this format for all commits:

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

**Examples:**
```bash
# New blog post (patch bump)
content: add post about Hugo modules

# New feature (minor bump)
feat: add dark mode toggle to sidebar

# Bug fix (patch bump)
fix: correct broken link in about page

# Theme change (patch bump)
theme: update Chirpy theme to v1.1.0

# Breaking change (major bump)
feat!: migrate from Jekyll to Hugo

# Or with footer
feat: redesign homepage layout

BREAKING CHANGE: completely new homepage structure
```

### Commit Types

- **feat**: New features or functionality (→ minor version bump)
- **fix**: Bug fixes (→ patch version bump)
- **docs**: Documentation changes (→ patch version bump)
- **style**: Styling/design changes (→ patch version bump)
- **refactor**: Code refactoring (→ patch version bump)
- **perf**: Performance improvements (→ patch version bump)
- **content**: New blog posts or content updates (→ patch version bump)
- **theme**: Theme-related changes (→ patch version bump)
- **config**: Configuration changes (→ patch version bump)
- **chore**: Maintenance tasks (→ no version bump)

### Breaking Changes

Mark breaking changes with `!` or `BREAKING CHANGE:` footer (→ major version bump):

```bash
feat!: redesign entire site layout
theme!: migrate to new theme with incompatible config

# Or
feat: change content directory structure

BREAKING CHANGE: All posts must be moved from posts/ to post/ directory
```

### Automated Release Process

When you push to the `main` branch:

1. GitHub Actions runs the semantic release workflow
2. Analyzes commit messages since the last release
3. Determines the next version number based on commit types
4. Generates/updates CHANGELOG.md
5. Creates a Git tag (e.g., `v1.2.3`)
6. Creates a GitHub Release with release notes

**Configuration files:**
- `.github/workflows/release.yml` - GitHub Actions workflow
- `.semver.yaml` - go-semver-release configuration
- `CHANGELOG.md` - Auto-generated changelog

### Manual Release Testing

To test the release process locally:

```bash
# Download go-semver-release binary
curl -SL https://github.com/s0ders/go-semver-release/releases/latest/download/go-semver-release-linux-amd64 -o ./go-semver-release
chmod +x ./go-semver-release

# Run locally (dry-run)
./go-semver-release release https://github.com/PauloPortugal/blog.git --config .semver.yaml --dry-run

# View commits since last release
git log --oneline $(git describe --tags --abbrev=0 2>/dev/null || echo "HEAD")..HEAD
```

## Architecture

### Hugo Module System
This project uses Hugo's native module system (not git submodules for themes). The Chirpy theme is imported as a Go module:

- **Module**: `github.com/pauloportugal/blog`
- **Theme Module**: `github.com/geekifan/hugo-theme-chirpy` (v1.0.2)
- **Dependencies**: Managed in `go.mod` and `go.sum`

The theme is cached in `~/.cache/hugo_cache/modules/` and downloaded automatically by Hugo.

### Configuration Structure
Hugo configuration is split across multiple files:

- **`hugo.toml`**: Primary configuration (base URL, pagination, outputs, taxonomies, module imports)
- **`config/_default/languages.toml`**: Multilingual settings (English and Portuguese)
- **`config/_default/params.toml`**: Theme parameters (author, social links, comments, avatar)
- **`config/_default/markup.toml`**: Markdown rendering settings (syntax highlighting, Goldmark parser)

### Content Organization

**Important**: Content structure follows specific naming conventions:

1. **List pages** (categories, tags, archives): Use `_index.md` with underscore prefix
2. **Single pages** (posts, about): Use `index.md` without underscore
3. **Homepage**: Must be `content/_index.en.md` (with underscore)

Content structure:
```
content/
├── _index.en.md           # Homepage (MUST have underscore)
├── post/                  # Blog posts directory (singular "post" not "posts")
│   └── YYYY-MM-DD-title/
│       ├── index.en.md    # Post content
│       └── *.png          # Bundled images
├── about/
│   └── index.en.md        # About page
├── categories/
│   └── _index.en.md       # Categories list (MUST have underscore)
├── tags/
│   └── _index.en.md       # Tags list (MUST have underscore)
└── archives/
    └── index.en.html      # Archives page
```

### Multilingual Support
- Language suffixes: `.en.md` (English), `.pt-PT.md` (Portuguese)
- Default language: English (en)
- Language configuration in `config/_default/languages.toml`

### Post Front Matter Structure
Posts require YAML front matter with specific fields:

```yaml
---
title: Post Title
date: 2019-08-08 11:33:00 +0800
draft: false
description: Brief description
categories:
  - Category1
  - Category2
tags:
  - tag1
  - tag2
pin: true              # Pin to homepage (optional)
math: true             # Enable MathJax (optional)
image:                 # Featured image (optional)
  path: image.png      # MUST exist in post directory
  lqip: data:image/...  # Low-quality placeholder (optional)
  alt: Image description
---
```

**Critical**: If `image.path` is specified, the image file MUST exist in the same directory as the post, or Hugo will fail with a nil pointer error.

### Taxonomy Configuration
Taxonomies (categories and tags) must be explicitly defined in `hugo.toml`:

```toml
[taxonomies]
  category = "categories"
  tag = "tags"
```

Without this configuration, categories and tags won't appear in the sidebar navigation.

## Theme-Specific Notes

### Chirpy Theme Features
- Sidebar navigation with Home, Categories, Tags, Archives, About
- Dark/Light mode toggle
- Table of Contents (TOC) for posts
- Syntax highlighting with line numbers
- Math equations via MathJax
- Multiple comment systems support (currently disabled)
- Social links in sidebar
- Search functionality (requires JSON output format)

### Asset Management
- **Avatar**: Located at `assets/img/commons/avatar.jpg`
- **Post images**: Store in the same directory as the post markdown file
- **Self-hosting**: Currently disabled (`self_host = false` in params.toml)

### Menu Configuration
Pages appear in the sidebar via front matter:

```yaml
menu:
  main:
    name: Page Name
    weight: 1              # Lower numbers appear first
    pre: fa-icon-name      # FontAwesome icon
```

## Reference Documentation

The Chirpy theme provides comprehensive examples and documentation:

- **Text and Typography**: https://geekifan.github.io/chirpy-starter/post/2019-08-08-text-and-typography/
  - Demonstrates all supported text formatting, typography, and content features
  - Shows examples of headings, lists, block quotes, prompts, tables, links, code blocks, math equations, images, videos, and more
  - Useful reference for understanding markdown capabilities and theme-specific features

- **Writing a New Post**: https://geekifan.github.io/chirpy-starter/post/2019-08-08-write-a-new-post/
  - Complete guide to creating posts with the Chirpy theme
  - Covers front matter options, naming conventions, and best practices
  - Essential reading before creating new content

## Troubleshooting

### "Module not found" errors
Run `hugo mod get` to download missing modules.

### Sidebar items not appearing
1. Check that taxonomies are defined in `hugo.toml`
2. Verify list pages use `_index.md` (with underscore)
3. Ensure menu configuration exists in page front matter

### "nil pointer evaluating resource.Resource.RelPermalink"
A post references an image in the front matter that doesn't exist. Either:
- Add the missing image to the post directory, or
- Remove the `image:` section from the front matter

### localhost:1313 references in production build
Don't use `hugo serve` for production builds. Use `hugo` (without serve) to build with the correct `baseURL`.

## Important File Patterns

- **Don't ignore**: `public/` is a Git submodule, not in `.gitignore`
- **Theme location**: Downloaded to Hugo cache, not in `themes/` directory
- **Empty directories**: `data/`, `i18n/`, `layouts/` exist but are empty (theme provides defaults)
- **Generated files**: `resources/_gen/`, `.hugo_build.lock` are ignored
