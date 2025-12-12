---
title: Claude Code - Project Memory with CLAUDE.md
date: 2025-12-12 10:00:00 +0000
draft: false
description: Learn how to use the /init command and CLAUDE.md files to give Claude Code project context and improve code generation quality
categories:
  - AI Tools
  - Development
tags:
  - claude-code
  - ai
  - developer-tools
image:
   path: What-is-Claude-Code_.webp
   alt: Welcome to Claude Code
---

In the [previous chapter](/post/2025-12-12-claude-code-introduction-setup/), we covered what Claude Code is and how to get started. We walked through installation, first-time setup and best practices for working with Claude Code. Now that we have Claude Code up and running, it's time to give it proper context about
your project.

## Why Use CLAUDE.md?

Before making any code changes with Claude Code, you should initialize a `CLAUDE.md` file in your project root. This file acts as mini-documentation that Claude uses as context when making decisions about your code, leading to much better code generation that follows your project's patterns.

## The /init Command

Run this command to have Claude scan your entire codebase and create a structured `CLAUDE.md` file:

```bash
/init
```

Claude will:
- Analyze your folder structure
- Examine package.json and dependencies
- Identify state management patterns
- Review existing documentation
- Generate a comprehensive CLAUDE.md file

The file is automatically added to session context, so Claude refers to it whenever making changes.

## What Gets Included?

A typical `CLAUDE.md` file contains:

- **Development commands** (npm scripts)
- **Architecture overview** (frameworks, libraries, versions)
- **Project structure** (where files belong)
- **Data architecture** (APIs, databases, CMS)
- **Styling conventions** (CSS approach, design system)
- **Testing setup** (test frameworks, patterns)
- **Development notes** (coding standards, preferences)

## Starting with a New Project

If you're starting fresh with minimal code, the generated `CLAUDE.md` will be sparse. In this case:

1. Manually edit the file to outline:
   - High-level project structure plans
   - Tools, packages, and frameworks you'll use
   - Code style preferences
   - Application summary

2. Update it as you go when things change

## Adding Memories from Chat

Use the `#` symbol to add instructions directly from chat:

```
# when making new page components, always add a link to that page in the header
```

You'll see options to save to:

- **Project memory** - `CLAUDE.md` (tracked in git, shared with team)
- **Local project memory** - `CLAUDE.local.md` (personal, not tracked)
- **Global memory** - `~/.config/claude/CLAUDE.md` (applies to all projects)

### Memory Types Explained

**Project Memory** (`CLAUDE.md`):
- Tracked by version control
- Shared with all developers on the project
- Contains project-specific guidance (folder structure, naming conventions, frameworks)

**Local Project Memory** (`CLAUDE.local.md`):
- Personal preferences for this project only
- Not pushed to repository
- Your own tooling and workflow preferences
- *Note: Being deprecated in favor of importing untracked files*

**Global Memory** (`~/.config/claude/CLAUDE.md`):
- Personal guidance across ALL projects
- Your coding style preferences
- Tools you use globally

## Accessing Memory Files

Use the `/memory` command to open and edit any memory file:

```bash
/memory
```

Select which file to edit (project, local, or global).

## Keep It Updated

The `CLAUDE.md` file isn't a "set and forget" document. Update it when:

- File/folder structure changes
- Dependencies are added or removed
- Coding standards evolve
- New patterns are established

If you don't update it, Claude might not pick up on changes and could generate code that doesn't match your current project structure.

## Best Practices

1. **Always run `/init`** when bringing Claude Code into a project
2. **Review the generated file** and add missing information
3. **Be specific** with folder locations and naming conventions
4. **Update regularly** as your project evolves
5. **Commit to git** so the team shares the same context
6. **Use `# ` in chat** for quick memory additions during development

By maintaining a good `CLAUDE.md` file, you keep Claude aligned with your project's architecture and coding style, resulting in cleaner, more appropriate code generation.
