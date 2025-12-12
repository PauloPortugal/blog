---
title: Claude Code - Managing Context for Better Results
date: 2025-12-12 11:00:00 +0000
draft: false
description: Learn how to provide context to Claude Code using files, images, and code selections, and manage context bloat for optimal AI performance
categories:
  - AI Tools
  - Development
tags:
  - claude-code
  - ai
  - developer-tools
  - productivity
image:
   path: claude-code-context.png
   alt: Claude Code Context in Practice
---

In the [previous chapter](/post/2025-12-12-claude-code-memory-claude-md/), we learned how to use the `/init` command to create a `CLAUDE.md` file that gives Claude Code project-wide context. Now let's explore how to provide specific context for individual tasks and manage context effectively to avoid bloat.

## What is Context?

Context is additional relevant information that helps AI models provide more accurate responses. Just like in human conversations, saying "fix the link" without context is vague, but saying "fix the broken link in the navigation component" gives clear direction.

While `CLAUDE.md` provides automatic project context, you'll often need to add specific files, code sections, or images for targeted tasks.

## Adding Files as Context

Use the `@` symbol followed by a file path:

```
@src/components/ui/button.tsx add a black variant with white text
```

As you type, Claude shows file suggestions you can select with arrow keys. You can reference multiple files in a single prompt:

```
@src/components/button.tsx @src/styles/theme.ts update button styles to match theme
```

## Adding Images as Context

Add images to the chat session to provide visual context using either method:

- **Drag and drop** the image file into the chat
- **Copy and paste** with `Ctrl + V` (Linux/Windows) or `Cmd + V` (Mac)

```
[Drag image or paste with Ctrl + V]
Can you update the button design in @src/components/button.tsx to look more
like this image with a 3D effect, keeping all color variants?
```

Claude can analyze the image and implement visual features like shadows, gradients, and effects.

## The Context Bloat Problem

Every prompt, response, and manually added file stays in context for the entire session. This is useful when working on a single feature, but problems arise when you:

1. Work on one feature (e.g., shopping basket)
2. Get sidetracked to fix something unrelated (e.g., button styling)
3. Continue in the same session with all the basket context still loaded

**Result:** Claude has irrelevant context that can confuse it and reduce accuracy.

## Managing Context

### 1. `/clear` - Complete Reset

Clears entire session history and context, keeping only `CLAUDE.md`:

```bash
/clear
```

Best for: Starting fresh after completing a task.

### 2. `/compact` - Summarize Session

Compacts the session into a small summary, deleting the rest:

```bash
/compact
```

Best for: Long sessions on a single feature where context is getting bloated but you want to keep working on the same task.

### 3. Double Escape - Rewind History

Press `Escape` twice to rewind to a previous point in the session:

- Navigate through chat history
- Press Enter on any message to rewind there
- Everything after that point gets removed

Best for: Removing recent digressions or debugging tangents.

### 4. Exit and Resume Sessions

Exit the current session:

```bash
/exit
```

Start a new session:

```bash
claude
```

Resume a previous session:

```bash
/resume
```

This shows all sessions with:
- Last used timestamp
- Message count
- Git branch
- Summary

Select one to resume with full context intact.

## Context Window Limits

Claude Code at this point in time has a **200,000 token** context window (1 token ≈ 3-4 characters). Long sessions with many files eventually fill this limit.

**Warning signs:**
- Claude notifies you when approaching the limit
- Results quality drops off

**Solution:** Use `/clear`, `/compact`, or double escape to reduce context.

## Best Practices

1. **Keep context focused** - Only add files relevant to the current task
2. **Clean between tasks** - Use `/clear` when switching to unrelated work
3. **Use `/compact` for long sessions** - When working on one feature over many prompts
4. **Leverage file selection** - Highlight specific code sections instead of entire files
5. **Reference schemas/configs** - Help Claude understand data structures and settings
6. **Add visual context** - Use images for UI/design-related tasks
7. **Monitor context size** - If results degrade, clear or compact

## Context Hierarchy

Claude Code uses context in this order:

1. **System prompts** - Built-in Claude Code instructions
2. **CLAUDE.md files** - Project, local, and global memory
3. **Current session** - Chat history and responses
4. **Manually added files** - Files referenced with `@` or opened
5. **Images** - Visual context from dragged images

By managing context effectively, you ensure Claude Code has exactly the information it needs—no more, no less—for optimal code generation.
