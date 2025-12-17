---
title: Claude Code - Model Configuration
date: 2025-12-17 10:00:00 +0000
draft: false
description: Learn how to configure and switch between Claude AI models using aliases, settings, and environment variables
categories:
  - AI Tools
  - Development
tags:
  - claude-code
  - ai
  - developer-tools
  - configuration
image:
  path: cc_model_config.png
  alt: Claude Code Model Configuration
---

In the [previous chapter](/post/2025-12-14-claude-code-tools-permissions/), we explored Claude Code's built-in tools and permission system. Now let's learn how to configure which AI model Claude Code uses for different tasks.

## Model Aliases

Claude Code provides convenient aliases that automatically map to the latest model versions:

| Alias | Current Model | Use Case |
|-------|--------------|----------|
| **`default`** | Recommended for your account type | Automatic best choice based on account |
| **`sonnet`** | Sonnet 4.5 | Daily coding tasks, balanced performance |
| **`opus`** | Opus 4.5 | Complex reasoning, architectural decisions |
| **`haiku`** | Haiku (latest) | Fast, simple tasks, background operations |
| **`sonnet[1m]`** | Sonnet with 1M token context | Long sessions, large codebases |
| **`opusplan`** | Opus (plan) → Sonnet (execution) | Hybrid: complex planning, efficient execution |

Using aliases ensures you automatically get the latest model versions without updating configuration.

## Setting Your Model

Configure your model using one of these methods (listed by priority):

#### Switch models mid-session:

This temporarily changes the model for the current session only.

```bash
/model sonnet
```


#### At Startup

Specify the model when starting Claude Code, setting the model for the entire session:

```bash
claude --model opus
```

#### Environment Variable


Set a default model for all sessions, add to your `.bashrc` or `.zshrc` to make it permanent:

```bash
export ANTHROPIC_MODEL=sonnet
```


### Settings File (Lowest Priority)

Configure permanently in `.claude/settings.json`:

```json
{
  "model": "sonnet",
  "permissions": {
    "allow": [...]
  }
}
```

## Checking Your Current Model

Two ways to verify which model you're using:

### 1. Status Command

```bash
/status
```

Displays your current model along with account information.

### 2. Status Line

Configure the status line to always show the current model (see [status line configuration](https://code.claude.com/docs/en/statusline)).

## Special Model Features

### The `opusplan` Hybrid Model

The `opusplan` alias provides intelligent model switching:

- **Plan Mode**: Uses Opus for complex reasoning and architecture
- **Execution Mode**: Switches to Sonnet for code generation

This combines Opus's deep reasoning with Sonnet's efficient execution.

For other special model features visit [Claude Code Special Model Features](http://localhost:1313/post/2025-12-17-claude-code-5-model-configuration/#special-model-features).

## Model Selection Strategy

Choose your model based on the task:

**Use Sonnet for:**
- Daily coding tasks
- Bug fixes
- Component creation
- Refactoring

**Use Opus for:**
- Complex architectural decisions
- System design
- Debugging difficult issues
- Performance optimization strategies

**Use Haiku for:**
- Simple file edits
- Quick searches
- Documentation updates
- Formatting fixes

**Use opusplan for:**
- Large features requiring planning
- System-wide changes
- Complex integrations

## Best Practices

1. **Start with `default`** - Let Claude Code choose the appropriate model
2. **Use `sonnet` for most work** - Balanced performance and cost
3. **Reserve `opus` for complexity** - Use when you need deeper reasoning
4. **Try `opusplan` for features** - Combines planning depth with execution efficiency
5. **Switch mid-session when needed** - Use `/model` to adapt to task complexity
6. **Monitor usage** - Check `/status` to track which model you're using
7. **Set per-project defaults** - Use settings files for project-specific requirements

For complete model configuration details and updates, see the [official model configuration documentation](https://code.claude.com/docs/en/model-config).

By understanding model configuration, you can optimize Claude Code's performance, cost, and capabilities for different types of tasks.
