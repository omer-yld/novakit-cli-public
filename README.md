# NovaKit CLI

An AI-powered coding agent for the terminal. NovaKit brings intelligent code assistance directly to your command line with support for multiple LLM providers, persistent sessions, semantic code search, and extensible tools.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Commands](#commands)
  - [Authentication](#authentication)
  - [Chat](#chat)
  - [Providers](#providers)
  - [Sessions](#sessions)
  - [Configuration](#configuration)
  - [Index (Semantic Search)](#index-semantic-search)
  - [Shell Completions](#shell-completions)
  - [Other Commands](#other-commands)
- [In-Chat Commands](#in-chat-commands)
- [Configuration](#configuration-1)
  - [Config Locations](#config-locations)
  - [Config Hierarchy](#config-hierarchy)
  - [Config Options](#config-options)
- [Supported Providers](#supported-providers)
- [Model Selector](#model-selector)
- [Agent Tools](#agent-tools)
- [Approval Modes](#approval-modes)
- [Background Tasks (Subagents)](#background-tasks-subagents)
- [Task Management (Todos)](#task-management-todos)
- [Automated Subagent Spawning](#automated-subagent-spawning)
- [Undo/Redo & Checkpoints](#undoredo--checkpoints)
- [Context Files (NOVAKIT.md)](#context-files-novakitmd)
- [Memory System](#memory-system)
- [Snippets](#snippets)
- [Semantic Code Search](#semantic-code-search)
- [Custom Commands](#custom-commands)
- [Custom Agents](#custom-agents)
- [MCP Integration](#mcp-integration)
- [LSP Integration](#lsp-integration)
- [Custom Ignore File](#custom-ignore-file)
- [IDE Extensions (Server Mode)](#ide-extensions-server-mode)
- [UI Features](#ui-features)
- [Security](#security)
- [Environment Variables](#environment-variables)
- [Telemetry](#telemetry)
- [Usage Examples & Use Cases](#usage-examples--use-cases)
  - [Daily Development Workflows](#daily-development-workflows)
  - [Automation & Scripting](#automation--scripting)
  - [Learning & Exploration](#learning--exploration)
- [Best Practices](#best-practices)
- [Common Pitfalls & What to Avoid](#common-pitfalls--what-to-avoid)
- [Tips & Tricks](#tips--tricks)
- [Troubleshooting](#troubleshooting)

## Features

- **Multi-Provider Support** - Use OpenAI, Anthropic, GitHub Copilot, OpenRouter, Groq, and more
- **Interactive TUI** - Rich terminal interface with markdown rendering and alternate screen buffer
- **Agent Mode** - AI with access to file operations, shell commands, and web search
- **Approval Modes** - Control agent autonomy: read-only, auto (default), or full
- **Background Tasks (Subagents)** - Spawn parallel agents for concurrent work
- **Session Management** - Resume conversations, auto-compact on context threshold, export transcripts
- **Semantic Code Search** - Vector-indexed codebase for intelligent context retrieval
- **LSP Integration** - Go-to-definition, find references, hover info, and diagnostics
- **Memory System** - Persistent memories across sessions
- **MCP Integration** - Extend capabilities with Model Context Protocol servers
- **Custom Commands & Agents** - Define your own slash commands and specialized agents
- **Favorite Models** - Star frequently used models for quick access
- **IDE Extensions** - JSON-RPC server mode for VS Code and JetBrains integration
- **Custom Ignore File** - `.novakitignore` for project-specific file exclusions

## Installation

```bash
# Install globally
npm install -g @novakit/cli

# Or with pnpm
pnpm add -g @novakit/cli
```

## Quick Start

```bash
# Start the TUI (opens provider connect if not authenticated)
novakit

# Or explicitly start chat
novakit chat

# With a specific model
novakit chat -m gpt-4o

# Resume a previous session
novakit -r <session-id>
```

## Keyboard Shortcuts

### Global Shortcuts

| Shortcut | Action |
|----------|--------|
| `ctrl+l` | Switch model |
| `ctrl+n` | New session |
| `ctrl+o` | Connect provider |
| `ctrl+c` | Cancel current operation (2x to quit) |
| `ctrl+w` | Quit immediately |
| `ctrl+z` | Undo last file change |
| `ctrl+y` | Redo last undone change |
| `ctrl+x` | Toggle expand/collapse messages |
| `Esc Esc` | Rewind to last checkpoint |
| `Tab` | Cycle approval mode (read-only → auto → full) |
| `Shift+Tab` | Cycle modes (Agent → Review → Plan) |
| `Alt+T` | Toggle task panel (show/hide todos) |
| `/` | Open command palette |
| `@` | Attach file |
| `#` | Save/attach memory |
| `$` | Save/insert snippet |
| `*` | Toggle favorite (in model selector) |

### Input Shortcuts

| Shortcut | Action |
|----------|--------|
| `ctrl+u` | Clear input line |
| `Escape` | Clear input (if not empty) |
| `ctrl+a` | Jump to beginning of line |
| `ctrl+e` | Jump to end of line |
| `ctrl+n` | Insert new line (multiline input) |
| `↑` / `↓` | Navigate input history |
| `←` / `→` | Move cursor |
| `ctrl+shift+v` | Paste text (terminal standard) |

## Commands

### Authentication

```bash
# Login with API key
novakit auth login
novakit auth login --provider anthropic

# Check auth status
novakit auth status

# Logout
novakit auth logout
novakit auth logout --provider openai
```

### Chat

```bash
# Start interactive chat
novakit chat

# Use specific model
novakit chat -m claude-sonnet-4-20250514

# Resume previous session
novakit chat -r <session-id>

# One-shot prompt
novakit chat -p "Explain this codebase"

# Set approval mode
novakit chat -a read-only    # Read-only mode (exploration only)
novakit chat -a auto         # Auto mode (default, confirmations for changes)
novakit chat -a full         # Full mode (auto-approve all operations)
novakit chat --yolo          # Alias for --approval-mode full

# Skip confirmations (legacy, same as --yolo)
novakit chat --no-confirm

# Headless mode (for automation/scripts)
novakit chat -p "Fix the bug" --headless
```

### Providers

```bash
# List configured providers
novakit provider list

# Show available presets
novakit provider presets

# Add provider from preset
novakit provider add anthropic --preset anthropic

# Add custom provider
novakit provider add my-provider \
  --endpoint https://api.example.com/v1 \
  --protocol openai \
  --model gpt-4

# Switch active provider
novakit provider use anthropic

# Remove provider
novakit provider remove openai
```

### Sessions

```bash
# List sessions
novakit session list
novakit session list --all

# Resume session
novakit session resume <session-id>

# Rename session
novakit session rename <session-id> "Feature Implementation"

# Compact session (reduce context)
novakit session compact <session-id>

# Export session
novakit session export <session-id> --format markdown
novakit session export <session-id> --format json

# Delete session
novakit session delete <session-id>
```

### Configuration

```bash
# Show all config
novakit config list

# Get specific value
novakit config get defaultModel

# Set value
novakit config set maxTokens 8192
novakit config set autoConfirmWrites true

# Initialize project config
novakit config init

# Show config paths
novakit config path

# Reset to defaults
novakit config reset
```

### Index (Semantic Search)

```bash
# Build vector index
novakit index build

# Check index status
novakit index status

# Search codebase
novakit index search "authentication logic"
```

### Shell Completions

```bash
# Bash
novakit completion bash >> ~/.bashrc
# Or
novakit completion bash > /etc/bash_completion.d/novakit

# Zsh
mkdir -p ~/.zsh/completions
novakit completion zsh > ~/.zsh/completions/_novakit
# Add to ~/.zshrc:
#   fpath=(~/.zsh/completions $fpath)
#   autoload -Uz compinit && compinit

# Fish
mkdir -p ~/.config/fish/completions
novakit completion fish > ~/.config/fish/completions/novakit.fish


  # Type novakit and press Tab - shows all commands
  novakit <Tab>
  # Shows: auth, chat, config, provider, session, index, doctor, completion, usage

  # Type partial command and Tab to complete
  novakit pro<Tab>
  # Completes to: novakit provider

  # Tab again to see subcommands
  novakit provider <Tab>
  # Shows: list, add, remove, use, presets

  # Works with options too
  novakit chat --<Tab>
  # Shows: --model, --resume, --no-confirm

  # Double Tab shows all options
  novakit session <Tab><Tab>
  # Shows: list, resume, rename, compact, export, delete
```

### Other Commands

```bash
# Check API quota/usage
novakit usage

# Run diagnostics
novakit doctor

# Start server for IDE extensions
novakit serve
```

## In-Chat Commands

Type `/` to open the command palette, or use these directly:

| Command | Description |
|---------|-------------|
| `/new-session` | Start new session |
| `/session-list` | List sessions |
| `/compact` | Summarize conversation to reduce context |
| `/switch-model` | Switch model |
| `/provider-connect` | Connect a provider |
| `/config-global` | Open global config directory |
| `/config-project` | Open project config directory |
| `/config-project-init` | Initialize project config |
| `/index-build` | Build semantic search index |
| `/index-status` | Show index status |
| `/memories` | List saved memories |
| `/commit` | Generate AI commit message |
| `/diff` | Show git status/diff |
| `/undo` | Undo last file change |
| `/redo` | Redo last undone change |
| `/history` | Show recent file changes |
| `/tools` | Open Tool Manager (enable/disable tools) |
| `/tools-list` | List all available tools |
| `/review` | Toggle Review Mode |
| `/expand-toggle` | Toggle expand/collapse messages |
| `/context-init-project` | Create NOVAKIT.md in project |
| `/context-init-global` | Create ~/.novakit/NOVAKIT.md |
| `/context-reload` | Reload NOVAKIT.md files |
| `/context-show` | Show loaded context files |
| `/checkpoint` | Create manual checkpoint |
| `/rewind` | Rewind to last checkpoint |
| `/checkpoint-list` | List saved checkpoints |
| `/snippets` | List saved snippets |
| `/mcp-status` | Show MCP server status |
| `/mcp-connect` | Connect MCP servers |
| `/mcp-disconnect` | Disconnect MCP servers |
| `/spawn` | Spawn a background task |
| `/tasks` | List background tasks |
| `/task-kill` | Kill a running task |
| `/task-cleanup` | Remove completed tasks |
| `/todos-toggle` | Toggle task panel visibility |
| `/loadtodos` | Load todos from current session |
| `/cleartodos` | Clear all todos |
| `/approval-mode` | Cycle approval mode |
| `/quota` | Show API quota/balance |
| `/doctor` | Run diagnostics |
| `/tools` | List available tools |
| `/usage` | Show token usage |
| `/context` | Show context status |
| `/help` | Show keyboard shortcuts |
| `/quit` | Exit chat |

## Configuration

### Config Locations

| Type | Location |
|------|----------|
| Global config | `~/.novakit/config.json` |
| Project config | `.novakit/config.json` |
| Credentials | `~/.novakit/credentials.d/` |
| OAuth tokens | `~/.novakit/oauth/` |
| Sessions | `~/.novakit/sessions/` |
| Memories | `~/.novakit/memory/` (global) or `.novakit/memory/` (project) |
| Vector Index | `.novakit/index/` |
| Custom Commands | `~/.novakit/commands.json` |
| Custom Agents | `~/.novakit/agents.json` |
| MCP Servers | `~/.novakit/mcp.json` |
| LSP Servers | `~/.novakit/lsp.json` |
| Checkpoints | `.novakit/checkpoints/` |
| Context Files | `NOVAKIT.md` (project root) or `~/.novakit/NOVAKIT.md` (global) |
| Ignore Patterns | `.novakitignore` (project) or `~/.novakit/ignore` (global) |

### Config Hierarchy

- **Global-only settings** (always from `~/.novakit/config.json`):
  - `favoriteModels` - Your starred models
  - `providers` - Provider configurations
  - `currentProvider` - Active provider

- **Project-overridable settings** (project config overrides global):
  - `defaultModel`, `maxTokens`, `maxToolIterations`, etc.

### Config Options

```json
{
  "apiEndpoint": "https://api.anthropic.com/v1",
  "apiProtocol": "anthropic",
  "defaultModel": "claude-sonnet-4-20250514",
  "maxTokens": 16384,
  "maxToolIterations": 10,
  "autoConfirmWrites": false,
  "showTokenUsage": true,
  "contextThreshold": 0.9,
  "autoCompactEnabled": true,
  "autoSaveEnabled": true,
  "useVectorIndex": true,
  "maxContextChunks": 10,
  "maxContextTokens": 8000,
  "minContextScore": 0.15,
  "preferredModels": ["claude-sonnet-4-20250514", "gpt-4o"],
  "favoriteModels": ["gpt-4o", "claude-sonnet-4-20250514"],
  "allowedReadRoots": [],
  "disabledTools": [],
  "reviewMode": false,
  "approvalMode": "auto"
}
```

#### Key Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `defaultModel` | string | - | Default model to use |
| `maxTokens` | number | 16384 | Maximum tokens per request |
| `maxToolIterations` | number | 10 | Max tool calls per response (1-50) |
| `autoConfirmWrites` | boolean | false | Skip confirmation for file operations |
| `showTokenUsage` | boolean | true | Display token usage after requests |
| `contextThreshold` | number | 0.9 | Auto-compact when context usage exceeds this (0-1) |
| `autoCompactEnabled` | boolean | true | Enable auto-compaction on context threshold |
| `useVectorIndex` | boolean | true | Enable semantic code search |
| `maxContextChunks` | number | 10 | Max code chunks to include in context |
| `maxContextTokens` | number | 8000 | Max tokens for retrieved context |
| `minContextScore` | number | 0.15 | Minimum relevance score for context chunks |
| `favoriteModels` | string[] | [] | Models marked as favorites (shown first) |
| `allowedReadRoots` | string[] | [] | Restrict file access to these paths |
| `disabledTools` | string[] | [] | Tools disabled by default (use /tools to toggle) |
| `reviewMode` | boolean | false | Start in Review Mode (batch file changes) |
| `approvalMode` | string | "auto" | Default approval mode: read-only, auto, or full |

## Supported Providers

### Built-in Presets

| Provider | Protocol | Default Model | Auth Method |
|----------|----------|---------------|-------------|
| Anthropic | anthropic | claude-sonnet-4-5-20250929 | API Key |
| OpenAI | openai | gpt-4o-mini | API Key |
| GitHub Copilot | openai | gpt-4o | OAuth Device Flow |
| OpenRouter | openai | openai/gpt-4o-mini | API Key |
| Groq | openai | moonshotai/kimi-k2-instruct-0905 | API Key |
| GitHub Models | openai | gpt-4o-mini | API Key |
| Z.ai | openai | GLM-4.7 | API Key |
| NovaKit | novakit | openai/gpt-oss-120b | API Key |

### GitHub Copilot

GitHub Copilot uses OAuth device flow for authentication:

1. Select "GitHub Copilot" from provider connect (`ctrl+o`)
2. A device code will be displayed
3. Open the verification URL in your browser
4. Enter the code and authorize
5. Token is automatically refreshed when expired

### Adding Custom Providers

Any OpenAI-compatible API can be added:

```bash
novakit provider add ollama \
  --endpoint http://localhost:11434/v1 \
  --protocol openai \
  --model llama3.2
```

## Model Selector

Press `ctrl+l` to open the model selector:

- **Search** - Type to filter models by name, ID, or provider
- **Navigate** - Arrow keys to move, Enter to select
- **Favorite** - Press `*` to star/unstar a model
- **Connect** - Press `ctrl+o` to add a new provider

Favorite models appear at the top in a dedicated section.

## Agent Tools

The AI agent has access to these tools:

| Tool | Description |
|------|-------------|
| `read` | Read file contents with line numbers |
| `write` | Create new files |
| `edit` | Modify existing files (string replacement) |
| `apply_patch` | Apply unified diff patches (create/modify/delete files) |
| `glob` | Find files by pattern |
| `grep` | Search file contents with regex |
| `bash` | Execute shell commands |
| `list_directory` | List directory contents |
| `web_search` | Search the web |
| `web_fetch` | Fetch and analyze web pages |
| `vector_search` | Semantic code search using embeddings |
| `vision` | Read and analyze images (PNG, JPG, GIF, WebP) |
| `multi_tool_use.parallel` | Execute multiple tools in parallel |
| `save_memory` | Save persistent memories |
| `read_memory` | Retrieve saved memories |
| `lsp_goto_definition` | Navigate to symbol definition |
| `lsp_find_references` | Find all references to a symbol |
| `lsp_hover` | Get type information and documentation |
| `lsp_diagnostics` | Get file errors/warnings from LSP server |
| `todos` | Track progress on multi-step tasks |
| `spawn_agent` | Spawn specialized subagents (explorer, planner, background, reviewer) |

### Tool Modes

- **Agent Mode** (default) - Full access to all tools including write/edit/bash
- **Review Mode** - Like Agent but collects file changes for batch review when complete
- **Plan Mode** - Read-only tools for exploration and planning

Toggle with `Shift+Tab` (cycles: Agent → Review → Plan → Agent).

## Approval Modes

Approval modes control how much autonomy the agent has over tool execution:

| Mode | Description | Use Case |
|------|-------------|----------|
| `read-only` | Only exploration tools (read, glob, grep, etc.) | Safe exploration, code review |
| `auto` | Confirmations for file changes and shell commands | Normal development (default) |
| `full` | Auto-approve all operations | Trusted automation, scripting |

### Changing Approval Mode

**Keyboard:** Press `Tab` to cycle through modes

**Command Palette:** `/approval-mode`

**CLI Flag:**
```bash
novakit chat -a read-only
novakit chat -a auto
novakit chat -a full
novakit chat --yolo  # Alias for -a full
```

**Config:**
```json
{
  "approvalMode": "auto"
}
```

### Permission Details

| Tool Category | read-only | auto | full |
|--------------|-----------|------|------|
| Read operations (read, glob, grep) | Auto | Auto | Auto |
| Write operations (write, edit) | Blocked | Confirm | Auto |
| Shell commands (bash) | Blocked | Confirm | Auto* |
| MCP tools | Blocked | Confirm | Auto |

*Dangerous bash commands (rm -rf, sudo, etc.) still require confirmation even in full mode.

## Background Tasks (Subagents)

Spawn parallel agents to work on tasks concurrently while you continue in the main session.

### Commands

| Command | Description |
|---------|-------------|
| `/spawn` | Open spawn mode, then type your task |
| `/spawn <prompt>` | Spawn a task directly |
| `/tasks` | List all background tasks with status |
| `/task-kill` | Kill a running task |
| `/task-kill <id>` | Kill a specific task by ID |
| `/task-cleanup` | Remove completed/failed tasks |

### Usage Examples

```bash
# Spawn a task to run tests in background
/spawn Run all tests and fix any failures

# Spawn a task to build documentation
/spawn Generate API documentation for src/api/

# List running tasks
/tasks

# Kill the only running task
/task-kill

# Kill a specific task
/task-kill abc123
```

### Task Status

| Status | Icon | Description |
|--------|------|-------------|
| pending | ⏳ | Task queued, waiting to start |
| running | 🔄 | Task actively executing |
| completed | ✅ | Task finished successfully |
| failed | ❌ | Task encountered an error |
| cancelled | ⏹️ | Task was manually stopped |

### Notes

- Maximum 3 concurrent tasks by default
- Tasks inherit the current approval mode
- Task progress is shown in the status bar
- Background tasks cannot prompt for confirmations (auto-denied if not in full mode)

## Task Management (Todos)

The AI agent can track progress on complex, multi-step tasks using the `todos` tool. This helps organize work and shows real-time progress.

### Task Panel

A collapsible panel displays current tasks:
- **Collapsed**: Shows task count and active status in a compact bar
- **Expanded**: Shows full task list with status indicators
- **Toggle**: Press `Alt+T` or use `/todos-toggle`

### Status Indicators

| Status | Icon | Description |
|--------|------|-------------|
| pending | ○ | Task not yet started |
| in_progress | ◐ | Currently working on |
| completed | ● | Task finished |

### When Todos Are Used

The AI automatically uses todos for:
- **Multi-step tasks**: When a task requires 3+ distinct steps
- **Complex implementations**: Features requiring multiple files or components
- **User-provided lists**: When given a numbered or comma-separated list of things to do

### Commands

| Command | Description |
|---------|-------------|
| `Alt+T` | Toggle task panel visibility |
| `/todos-toggle` | Toggle task panel (same as Alt+T) |
| `/loadtodos` | Reload todos from current session |
| `/cleartodos` | Clear all todos |

### Session Persistence

Todos are stored per-session and auto-load when resuming a session, so you can pick up where you left off.

## Automated Subagent Spawning

The AI can automatically spawn specialized agents using the `spawn_agent` tool to handle complex tasks autonomously.

### Agent Types

| Type | Use Case | Example |
|------|----------|---------|
| **explorer** | Deep codebase exploration | "Find how authentication is implemented" |
| **planner** | Implementation planning | "Plan how to add caching to the API" |
| **background** | Long-running tasks | "Run all tests and fix any failures" |
| **reviewer** | Code review | "Review the changes in src/auth/*.ts" |

### How It Works

1. **Background by default**: Agents run in background so the main conversation continues
2. **Monitor progress**: Use `/tasks` to check status
3. **Wait when needed**: Use `wait: true` for tasks where you need the result before continuing (e.g., getting a plan before implementing)

### Examples

```
# AI spawns explorer to understand codebase
spawn_agent: {"agent_type": "explorer", "task": "Find all authentication middleware"}

# AI spawns planner and waits for result
spawn_agent: {"agent_type": "planner", "task": "Plan user profile feature", "wait": true}

# AI spawns reviewer after making changes
spawn_agent: {"agent_type": "reviewer", "task": "Review changes in src/components/"}
```

### Agent vs Manual Subagent

| Feature | `spawn_agent` (AI-triggered) | `/spawn` (User-triggered) |
|---------|------------------------------|---------------------------|
| Initiated by | AI agent | User command |
| System prompt | Specialized per agent type | Generic |
| Default mode | Background | Foreground |
| Use case | AI autonomy | User delegation |

### Tool Whitelisting

Enable or disable specific tools:

- `/tools` - Open interactive Tool Manager modal
- `/tools-list` - View all available tools with status
- Type to filter, Enter/Space to toggle, j/k to navigate

**Config** (`~/.novakit/config.json`):
```json
{
  "disabledTools": ["bash", "write"]
}
```

Disabled tools won't be available to the agent until re-enabled.

## Undo/Redo & Checkpoints

NovaKit tracks all file changes and allows you to revert them.

### Quick Undo/Redo

- `ctrl+z` - Undo last file change
- `ctrl+y` - Redo last undone change
- `/history` - View recent file changes

### Checkpoints

Checkpoints save the state of modified files for larger rollbacks:

- Automatic checkpoints are created before each file modification
- `Esc Esc` (double escape) - Quickly rewind to last checkpoint
- `/checkpoint` - Create a manual checkpoint
- `/checkpoint-list` - View all checkpoints
- `/rewind` - Restore the most recent checkpoint

Checkpoints are stored in `.novakit/checkpoints/` (up to 50 per project).

## Context Files (NOVAKIT.md)

Create `NOVAKIT.md` files to provide persistent context to the AI:

### Locations (all are loaded)

1. `~/.novakit/NOVAKIT.md` - Global context (all projects)
2. `./NOVAKIT.md` - Project root
3. `./src/NOVAKIT.md` - Subdirectory context

### Example NOVAKIT.md

```markdown
# Project: E-commerce API

## Architecture
- Node.js with Express
- PostgreSQL with Prisma ORM
- Redis for caching

## Conventions
- All prices stored in cents (integers)
- Use snake_case for database columns
- Use camelCase for JavaScript

## Important
- Never modify files in `src/legacy/`
- All API responses use the standard wrapper

@docs/api-reference.md
```

The `@path/to/file.md` syntax imports content from other markdown files.

Commands:
- `/context-show` - View loaded context files
- `/context-reload` - Reload after changes

## Memory System

Save important information that persists across sessions:

```bash
# In chat, use # prefix to save memories
#project-context This is a React app using TypeScript...

# Or use the memory picker
# (type # with empty input to open picker)
```

Memories are stored as markdown files:
- Global: `~/.novakit/memory/`
- Project: `.novakit/memory/`

## Snippets

Save and reuse code snippets:

```bash
# Save a snippet
$snippet-name Your code here...

# Insert a snippet (type $ with empty input)
# Opens snippet picker
```

Snippets are stored in `~/.novakit/snippets/`.

## Semantic Code Search

Build a vector index of your codebase for intelligent context retrieval:

```bash
# Build index
novakit index build

# The agent will automatically use relevant code context
```

Configuration:
```json
{
  "useVectorIndex": true,
  "maxContextChunks": 10,
  "maxContextTokens": 8000,
  "minContextScore": 0.15
}
```

## Custom Commands

Create custom slash commands in `~/.novakit/commands.json`:

```json
{
  "commands": [
    {
      "id": "review",
      "name": "Code Review",
      "description": "Review current file",
      "type": "prompt",
      "content": "Review this code for bugs, security issues, and best practices:\n\n{{input}}"
    },
    {
      "id": "test",
      "name": "Generate Tests",
      "description": "Generate tests for code",
      "type": "prompt",
      "content": "Generate comprehensive tests for:\n\n{{input}}"
    },
    {
      "id": "git-log",
      "name": "Git Log",
      "description": "Recent commits",
      "type": "bash",
      "content": "git log --oneline -10",
      "showOutput": true
    }
  ]
}
```

## Custom Agents

Define specialized agents in `~/.novakit/agents.json`:

```json
{
  "agents": [
    {
      "id": "reviewer",
      "name": "Code Reviewer",
      "icon": "🔍",
      "description": "Code review specialist",
      "systemPrompt": "You are an expert code reviewer. Focus on bugs, security, performance, and best practices.",
      "model": "claude-sonnet-4-20250514",
      "includeContext": true
    },
    {
      "id": "architect",
      "name": "System Architect",
      "icon": "🏗️",
      "description": "System design expert",
      "systemPrompt": "You are a software architect. Help design scalable, maintainable systems.",
      "tools": ["read", "glob", "grep", "list_directory"]
    }
  ]
}
```

Activate agents from the command palette (`/`).

## MCP Integration

Connect Model Context Protocol servers for extended capabilities.

Configure in `~/.novakit/mcp.json`:

```json
{
  "servers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/dir"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "your-token"
      }
    }
  }
}
```

Use `/mcp-status`, `/mcp-connect`, `/mcp-disconnect` to manage servers.

## LSP Integration

NovaKit integrates with Language Server Protocol (LSP) servers for intelligent code understanding.

### LSP Tools

The AI agent can use these LSP-powered tools:

| Tool | Description |
|------|-------------|
| `lsp_goto_definition` | Navigate to where a symbol is defined |
| `lsp_find_references` | Find all usages of a symbol across the codebase |
| `lsp_hover` | Get type signatures and documentation |
| `lsp_diagnostics` | Get compiler errors, warnings, and linting issues |

### Configuration

Configure LSP servers in `~/.novakit/lsp.json`:

```json
{
  "servers": [
    {
      "name": "typescript",
      "languages": [".ts", ".tsx", ".js", ".jsx"],
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "enabled": true
    },
    {
      "name": "python",
      "languages": [".py", ".pyi"],
      "command": "pylsp",
      "args": [],
      "enabled": true
    }
  ],
  "autoDetect": true,
  "autoStart": true
}
```

### Default Servers

| Language | Server | File Extensions |
|----------|--------|-----------------|
| TypeScript/JavaScript | `typescript-language-server` | `.ts`, `.tsx`, `.js`, `.jsx`, `.mjs`, `.cjs` |
| Python | `pylsp` | `.py`, `.pyi` |
| Go | `gopls` | `.go` |
| Rust | `rust-analyzer` | `.rs` |
| JSON | `vscode-json-language-server` | `.json`, `.jsonc` |
| YAML | `yaml-language-server` | `.yaml`, `.yml` |
| CSS | `vscode-css-language-server` | `.css`, `.scss`, `.less` |
| HTML | `vscode-html-language-server` | `.html`, `.htm` |

### Installing LSP Servers

```bash
# TypeScript/JavaScript
npm install -g typescript-language-server typescript

# Python
pip install python-lsp-server

# Go
go install golang.org/x/tools/gopls@latest

# Rust
rustup component add rust-analyzer
```

Servers are auto-started when needed and auto-detected based on file extensions.

## Custom Ignore File

Control which files NovaKit ignores when searching and indexing.

### File Locations

1. **Project-level**: `.novakitignore` in project root
2. **Global**: `~/.novakit/ignore`

Both support gitignore syntax, including negation patterns.

### Example `.novakitignore`

```gitignore
# Large data files
*.csv
*.parquet
data/

# Test fixtures
__fixtures__/
*.snap

# Generated files
dist/
build/
*.generated.ts

# Sensitive files
.env*
secrets/

# But keep this vendor directory
!important-vendor/
```

### Default Patterns

These are always ignored (built-in):

- **Directories**: `node_modules`, `.git`, `dist`, `build`, `.next`, `coverage`, `__pycache__`, `.venv`, `target`
- **Binary files**: `*.png`, `*.jpg`, `*.gif`, `*.ico`, `*.woff`, `*.ttf`, `*.pdf`, `*.zip`
- **Lock files**: `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`
- **Minified files**: `*.min.js`, `*.min.css`

The ignore system applies to the `glob` and `grep` tools used by the AI agent.

## IDE Extensions (Server Mode)

NovaKit can run as a JSON-RPC server for integration with IDEs like VS Code and JetBrains.

### Starting the Server

```bash
# Start server in stdio mode
novakit serve
```

### Protocol

The server uses JSON-RPC 2.0 over stdio with Content-Length headers.

#### Requests

| Method | Description |
|--------|-------------|
| `initialize` | Initialize with project path and capabilities |
| `chat` | Send a message and receive streaming response |
| `tool/confirm` | Confirm or deny a tool execution |
| `session/list` | List available sessions |
| `abort` | Cancel current operation |
| `shutdown` | Gracefully shutdown the server |

#### Notifications (Server → Client)

| Method | Description |
|--------|-------------|
| `$/text` | Streaming text content |
| `$/toolCall` | Tool about to be executed |
| `$/toolResult` | Tool execution completed |
| `$/error` | Error occurred |
| `$/progress` | Progress update |

### Example: Initialize

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "initialize",
  "params": {
    "projectPath": "/path/to/project",
    "clientName": "vscode",
    "clientVersion": "1.0.0"
  }
}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "serverVersion": "0.1.0",
    "capabilities": {
      "tools": ["read", "write", "edit", "glob", "grep", "bash", "..."],
      "streaming": true,
      "sessions": true,
      "lsp": true
    }
  }
}
```

### Example: Chat

**Request:**
```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "chat",
  "params": {
    "message": "Add a logout button to the header",
    "sessionId": "optional-session-id"
  }
}
```

**Notifications received:**
```json
{"jsonrpc": "2.0", "method": "$/text", "params": {"content": "I'll add a logout button...", "done": false}}
{"jsonrpc": "2.0", "method": "$/toolCall", "params": {"requestId": "uuid", "name": "edit", "arguments": {...}}}
{"jsonrpc": "2.0", "method": "$/toolResult", "params": {"requestId": "uuid", "name": "edit", "success": true}}
{"jsonrpc": "2.0", "method": "$/text", "params": {"content": "", "done": true}}
```

**Response:**
```json
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": {
    "sessionId": "session-uuid"
  }
}
```

### VS Code Extension Example

```typescript
import { spawn } from 'child_process';

const server = spawn('novakit', ['serve'], {
  cwd: workspaceRoot,
  stdio: ['pipe', 'pipe', 'pipe']
});

// Send request
function sendRequest(method: string, params: any) {
  const content = JSON.stringify({
    jsonrpc: '2.0',
    id: nextId++,
    method,
    params
  });
  const message = `Content-Length: ${Buffer.byteLength(content)}\r\n\r\n${content}`;
  server.stdin.write(message);
}

// Handle responses and notifications
server.stdout.on('data', (data) => {
  // Parse JSON-RPC messages...
});

// Initialize
sendRequest('initialize', { projectPath: workspaceRoot });

// Chat
sendRequest('chat', { message: 'Fix the bug in auth.ts' });
```

## UI Features

### Performance Optimizations

NovaKit is optimized for smooth, flicker-free terminal rendering:
- **Adaptive streaming throttle** - Dynamically adjusts buffer timing based on chunk rate
- **Isolated spinner animations** - Status bar spinners don't trigger parent re-renders
- **Memoized components** - Input, StatusBar, and Markdown components use React.memo
- **LRU cache for markdown** - Parsed markdown is cached to avoid re-parsing
- **Native terminal scrollback** - Uses terminal's built-in scroll for mouse wheel support

### Status Indicators

- **Animated spinner** while thinking or executing tools
- **Context usage** percentage in status bar
- **Mode indicator** (Agent/Review/Plan) with color coding
- **Provider and model** display

### Provider Connect

The provider connect dialog shows authentication status:
- `✓ authenticated` - Provider has valid API key
- `○ needs key` - Provider needs authentication

## Security

### File Access

- Use `allowedReadRoots` to restrict file access to specific directories
- Write/edit operations require confirmation (unless `autoConfirmWrites: true`)
- Dangerous bash commands are blocked by default

### Credentials

- API keys are stored with restricted permissions (0600)
- OAuth tokens stored separately in `~/.novakit/oauth/`
- Keys are never logged or displayed in full
- Support for environment variable fallback (`NOVAKIT_API_KEY`)

### Blocked Commands

The following patterns are blocked in bash:
- `rm -rf /` and system path deletions
- `shutdown`, `reboot`, `halt`, `poweroff`
- Fork bombs
- Device file writes

## Environment Variables

| Variable | Description |
|----------|-------------|
| `NOVAKIT_API_KEY` | Default API key (fallback) |
| `NOVAKIT_CONFIG_DIR` | Custom config directory |
| `NO_COLOR` | Disable colored output |

## Telemetry

NovaKit collects anonymous usage data to improve the product. This is completely optional and can be disabled.

### What We Collect

- CLI version, platform, and Node.js version
- Session duration, message count, and token usage
- Tool execution (name, success/failure, duration)
- Feature usage (vector index, compaction, etc.)
- Error types (no sensitive data)

### What We DON'T Collect

- File paths or code content
- Prompts or conversations
- API keys or credentials
- Any personally identifiable information

### Opting Out

```bash
novakit config set telemetryEnabled false
```

## Usage Examples & Use Cases

### Daily Development Workflows

#### Starting a New Feature
```bash
# Start with read-only mode to explore first
novakit chat -a read-only
> "Explain how authentication works in this codebase"
> "What files would I need to modify to add a logout feature?"

# Once you understand, switch to auto mode (Tab key or restart)
novakit chat
> "Add a logout button to the header component"
```

#### Bug Fixing
```bash
# Attach the error log or screenshot
novakit chat
> @error.log "Fix this error"

# Or describe the issue
> "The login form submits twice when clicking the button"
```

#### Code Review
```bash
# Use read-only mode to safely explore changes
novakit chat -a read-only
> "Review the changes in src/auth/*.ts for security issues"

# Or use Review Mode for batch reviewing AI changes
# Press Shift+Tab to switch to Review Mode
```

#### Refactoring
```bash
novakit chat
> "Refactor the UserService class to use dependency injection"
> "Extract the validation logic from UserController into a separate module"
```

### Automation & Scripting

#### CI/CD Integration
```bash
# Headless mode for automated pipelines
novakit chat -p "Fix any TypeScript errors in src/" --headless -a full

# One-shot commands for scripts
novakit chat -p "Generate a changelog from recent commits" --headless
```

#### Batch Operations
```bash
# Use full approval mode for trusted batch operations
novakit chat -a full
> "Add JSDoc comments to all exported functions in src/utils/"
> "Update all imports from 'lodash' to use named imports"
```

### Learning & Exploration

#### Understanding a New Codebase
```bash
novakit chat -a read-only
> "Give me an overview of this project's architecture"
> "What are the main entry points?"
> "How does data flow from the API to the UI?"
```

#### Learning Patterns
```bash
> "Explain the error handling pattern used in this codebase"
> "Show me examples of how tests are structured here"
```

## Best Practices

### 1. Start in Read-Only Mode for Unfamiliar Code

```bash
# Safe exploration - no accidental changes
novakit chat -a read-only
```

**Why:** Prevents accidental modifications while you understand the codebase. Switch to `auto` mode once you're ready to make changes.

### 2. Use NOVAKIT.md for Project Context

Create a `NOVAKIT.md` in your project root:

```markdown
# Project: My App

## Architecture
- React frontend with TypeScript
- Express backend with PostgreSQL
- Redis for session management

## Conventions
- Use functional components with hooks
- All API responses follow { data, error, meta } format
- Database column names use snake_case

## Do NOT modify
- src/legacy/ (deprecated, scheduled for removal)
- migrations/ (use CLI to generate new migrations)

## Testing
- Run `pnpm test` before committing
- Integration tests require `docker-compose up -d`
```

**Why:** The AI reads this context on every request, ensuring consistent understanding of your project's standards.

### 3. Build the Vector Index for Large Codebases

```bash
novakit index build
```

**Why:** Semantic search helps the AI find relevant code even when you don't know exact file names or function names.

### 4. Use Sessions for Complex Tasks

```bash
# Name your sessions for easy resumption
novakit chat
# Work on a feature...

# Later, resume where you left off
novakit session list
novakit chat -r <session-id>
```

**Why:** Sessions preserve context, so the AI remembers previous discussions and decisions.

### 5. Use Background Tasks for Long Operations

```bash
# Don't block your main session
/spawn Run all tests and fix any failures
/spawn Generate API documentation

# Continue working while tasks run
/tasks  # Check progress
```

**Why:** Background tasks let you stay productive while the AI handles time-consuming operations.

### 6. Leverage Checkpoints Before Major Changes

```bash
# Create a checkpoint before risky changes
/checkpoint

# If something goes wrong
/rewind
# Or double-press Escape
```

**Why:** Checkpoints provide a safety net for reverting multiple file changes at once.

### 7. Use Specific, Actionable Prompts

```
GOOD: "Add input validation to the signup form: email must be valid,
       password must be 8+ characters with one number"

BAD:  "Make the form better"
```

**Why:** Specific prompts lead to predictable results. Vague prompts may produce unexpected changes.

### 8. Review Changes Before Committing

```bash
# Always check what changed
/diff
git diff

# Use undo if needed
ctrl+z  # or /undo
```

**Why:** Even in `full` approval mode, reviewing changes before committing ensures quality.

## Common Pitfalls & What to Avoid

### 1. Don't Use Full Mode on Untrusted Projects

```bash
# RISKY: Auto-approves everything including shell commands
novakit chat -a full  # in an unfamiliar codebase
```

**Better:** Start with `auto` mode and upgrade to `full` only when you trust the operations.

### 2. Don't Ignore Context Warnings

When you see high context usage (80%+):
```bash
# Compact the session to free up context
/compact
```

**Why:** Running out of context causes the AI to lose earlier conversation details, leading to inconsistent responses.

### 3. Don't Skip Reading the Suggested Changes

```bash
# The AI shows you what it's about to do - READ IT
# [edit] src/auth/login.ts
# - const token = jwt.sign(user)
# + const token = jwt.sign(user, { expiresIn: '24h' })
```

**Why:** Blindly accepting changes can introduce bugs or security issues.

### 4. Don't Forget to Save Memories for Important Context

```bash
# Save project-specific knowledge
#api-format All API responses use { success, data, error } format
#auth-flow Authentication uses JWT with refresh tokens
```

**Why:** Memories persist across sessions, so you don't need to re-explain project conventions.

### 5. Don't Leave Sensitive Data in Sessions

Sessions are stored locally. If you've discussed sensitive information:

```bash
# Delete the session when done
novakit session delete <session-id>
```

### 6. Don't Run Without Testing After Changes

```bash
# Always verify changes work
> "Run the tests to make sure nothing broke"

# Or spawn a background task
/spawn Run tests and report any failures
```

### 7. Don't Overload a Single Prompt

```bash
# TOO MUCH at once
> "Add authentication, implement the dashboard, set up the database,
   write tests, add documentation, and deploy to production"

# BETTER: Break into steps
> "Add user authentication with JWT"
# Wait for completion
> "Implement the user dashboard"
# And so on...
```

**Why:** Complex prompts can lead to partial implementations or missed requirements.

### 8. Don't Forget About .novakitignore

```gitignore
# .novakitignore
secrets/
*.env*
credentials/
private/
```

**Why:** Prevents the AI from reading or accidentally exposing sensitive files.

## Tips & Tricks

### Quick Mode Switching

- `Tab` - Cycle approval modes (read-only → auto → full)
- `Shift+Tab` - Cycle tool modes (Agent → Review → Plan)

### Efficient File Attachment

```bash
# Attach files with context
@src/auth/login.ts "Fix the token expiry issue"
@package.json @tsconfig.json "Update TypeScript to version 5"
```

### Multi-Line Prompts

```bash
# Press Ctrl+N to insert newlines
> First line of my prompt
  Second line with more context  # Ctrl+N before this
  Third line with requirements
```

### Quick Commands

```bash
# Type / to open command palette with fuzzy search
/com  # matches /compact, /commit
/sw   # matches /switch-model
```

### Star Your Favorite Models

```bash
# Press * in the model selector to star/unstar
# Starred models appear at the top
ctrl+l → * → Enter
```

### Export Sessions for Documentation

```bash
# Export as markdown for documentation
novakit session export <id> --format markdown > feature-implementation.md

# Export as JSON for analysis
novakit session export <id> --format json > session.json
```

### Use Glob Patterns in Questions

```bash
> "Review all test files in src/**/*.test.ts"
> "Find any TODO comments in src/components/*.tsx"
```

## Troubleshooting

### Run Diagnostics

```bash
novakit doctor
```

### Common Issues

**"Not authenticated"**
```bash
novakit auth login --provider <provider-name>
# Or use ctrl+o in chat to connect
```

**"Model not found"**
- Check model name matches provider's naming
- Use `novakit provider presets` to see default models
- Ensure the model is in your `preferredModels` or fetched from API

**"Authentication failed" for GitHub Copilot**
- Copilot tokens expire quickly (~30 min)
- Token refresh is automatic if OAuth token is valid
- If issues persist, reconnect with `ctrl+o`

**High context usage**
```bash
# Compact the session
novakit session compact <session-id>

# Or in chat
/compact

# Auto-compact triggers at contextThreshold (default 90%)
```

**Favorites not showing**
- Favorites are stored in global config (`~/.novakit/config.json`)
- Project config doesn't override `favoriteModels`


