---
title: Claude Code - Tools and Permissions
date: 2025-12-14 10:00:00 +0000
draft: false
description: Understanding Claude Code's built-in tools and how to manage permissions for automated workflows
categories:
  - AI Tools
  - Development
tags:
  - claude-code
  - ai
  - developer-tools
  - automation
image:
   path: cc_tools.png
   alt: Claude Code Tools and Permissions
---

In the [previous chapter](/post/2025-12-16-claude-code-context-management/), we learned how to manage context effectively by adding files, images, and using commands like `/clear` and `/compact`. Now let's explore the built-in tools Claude Code uses to perform actions and how to manage permissions for those tools.

## How Claude Code Uses Tools

When you ask Claude Code to do something, it automatically selects the appropriate built-in tool:

- **Read** - View file contents
- **Edit** - Modify existing files
- **Write** - Create new files
- **Bash** - Execute shell commands
- **Glob** - Find files by pattern
- **Grep** - Search file contents
- **Task** - Launch specialized agents
- **TodoWrite** - Create task checklists

You don't need to specify which tool to use—Claude Code determines this based on your instructions.

## Permission System

Some tools require permission before execution, while others don't. 
Here is an example of such commands.

**Require Permission:**
- Edit
- Write
- Bash
- NotebookEdit

**No Permission Required:**
- Read
- Glob
- Grep
- TodoWrite

For a complete list of tool permissions, visit [Claude Code Tool Permissions](https://code.claude.com/docs/en/settings#tools-available-to-claude)

When Claude needs permission, you'll see three options:

1. **Yes** - Allow this action once
2. **Yes, and don't ask again this session** - Allow and remember for current session
3. **No** - Deny the action

You can also provide further instructions instead of a simple yes/no.

This file stores allowed commands for your workflow and should not be committed to version control.

## Settings File Hierarchy

Claude Code uses a [hierarchical system of settings](https://code.claude.com/docs/en/settings) files to manage configurations at different levels:

| Settings File | Location | Scope | Version Control | Purpose |
|---------------|----------|-------|-----------------|---------|
| **User Settings** | `~/.claude/settings.json` | All projects globally | ✗ Not tracked | Personal preferences across all Claude Code usage |
| **Project Settings** | `.claude/settings.json` | Team-wide | ✓ Tracked & shared | Team-shared configuration, checked into source control |
| **Local Project Settings** | `.claude/settings.local.json` | Project-specific | ✗ Auto-ignored by git | Personal project preferences, experimentation without affecting team |
| **User Preferences** | `~/.claude.json` | Global | ✗ Not tracked | User preferences (theme, notifications), OAuth sessions, MCP servers, caches |

### Settings Precedence (Highest to Lowest)

1. **Command line arguments** - Temporary session overrides
2. **Local project settings** (`.claude/settings.local.json`) - Personal project preferences
3. **Shared project settings** (`.claude/settings.json`) - Team shared settings
4. **User settings** (`~/.claude/settings.json`) - Personal global defaults

Settings are merged hierarchically, with more specific settings adding to or overriding broader ones.

### Pattern Matching

Use patterns for granular control:

```json
{
  "permissions": {
    "allow": [
      "Bash(git log:*)",    // Allow git log with any arguments
      "Bash(git diff:*)",   // Allow git diff with any arguments
      "Bash(npm test:*)",   // Allow npm test operations
      "Read"                // Allow all read operations
    ],
    "deny": [
      "Bash(rm:*)",         // Block all rm commands
      "Write"               // Block file creation
    ]
  }
}
```

## Quick Permission Toggle

Press `Alt + M` (Windows/Linux) or `Ctrl + M` (Mac) to toggle **Accept Edits** mode:

- **Enabled:** Claude can edit files without asking for this session
- **Disabled:** Claude asks for permission (default)

This setting resets when you start a new session.

## CLI Permission Flags

For automated workflows, use command-line flags to control tool permissions:

| Flag | Description | Example | Use Case |
|------|-------------|---------|----------|
| `--allowedTools` | Allow specific tools without prompting | `claude --allowedTools "Bash(git log:*)" "Read"` | Automated CI/CD workflows, trusted operations |
| `--disallowedTools` | Block specific tools without prompting | `claude --disallowedTools "Bash(rm:*)" "Edit"` | Prevent destructive operations, read-only mode |
| `--dangerously-skip-permissions` | Skip all permission prompts | `claude --dangerously-skip-permissions` | ⚠️ Fully automated workflows (use with caution) |
| `--permission-mode` | Specify permission handling mode | `claude --permission-mode plan` | Control how Claude requests permissions |

## Settings Configuration

Load settings from specific files:

```bash
# Use custom settings file
claude --settings ./settings.json

# Use multiple setting sources
claude --setting-sources user,project
```

For complete CLI reference and advanced usage, see the [official CLI documentation](https://code.claude.com/docs/en/cli-reference).

## Best Practices

1. **Review before granting permission** - Always check what commands Claude wants to run
2. **Use "don't ask again" selectively** - Grant for trusted commands like `git status`, `git log`
3. **Be cautious with Bash** - Review destructive commands (`rm`, `git push --force`) carefully
4. **Keep settings.local.json untracked** - Add to `.gitignore`
5. **Use patterns for common operations** - Allow `git` commands but not `rm` or `sudo`
6. **Toggle Accept Edits for bulk work** - Use `Alt + M` when making many file changes
7. **Review tool list periodically** - Check what's allowed in your settings file

## Safety Considerations

**Always review commands before approval:**

- Git operations that push to remote
- File deletions or moves
- Package installations
- Database migrations
- System-level commands

The permission system exists to keep you in control. When in doubt, select "No" and manually review or execute the command yourself.

By understanding Claude Code's tools and permission system, you can balance automation with safety, creating efficient workflows while maintaining oversight of critical operations.
