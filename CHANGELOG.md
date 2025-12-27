# Changelog

All notable changes to NovaKit CLI will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- **Task Management Tool (`todos`)**: AI can track progress on complex, multi-step tasks
  - Collapsible TodoPanel UI component (toggle with `Alt+T`)
  - Status tracking: pending, in_progress, completed
  - Per-session persistence (todos auto-load when returning to a session)
  - Commands: `/loadtodos`, `/cleartodos`, `/todos-toggle`

- **Automated Subagent Spawning (`spawn_agent`)**: AI can now automatically spawn specialized agents
  - **Explorer Agent**: Deep codebase exploration and understanding
  - **Planner Agent**: Implementation planning before coding
  - **Background Agent**: Long-running tasks that don't block chat
  - **Reviewer Agent**: Code review after implementation
  - Agents run in background by default (use `wait: true` for foreground)
  - Monitor with `/tasks`, kill with `/task-kill`

### Changed
- Subagents now run in background by default instead of blocking
- Enhanced tool display in UI shows agent type being spawned (e.g., "Spawn Explorer")

## [0.1.6] - 2025-12-26

### Added
- Background tasks (subagents) with `/spawn` command - run up to 3 parallel agents
- Subagent manager with configurable approval modes
- System prompts framework for custom agent behaviors
- Enhanced `apply_patch` tool for complex multi-file modifications
- GitHub Copilot OAuth device flow authentication
- Dynamic version display in status bar
- Z.ai provider support (GLM-4.7 models)

### Changed
- Improved streaming response handling for better UX
- Better error messages for authentication failures
- Reduced memory usage for long sessions

### Fixed
- Session compaction now preserves important context
- Memory leak in long-running sessions
- Provider switching no longer requires restart

## [0.1.5] - 2025-12-20

### Added
- MCP (Model Context Protocol) server integration
- Custom commands via `~/.novakit/commands.json`
- Custom agents via `~/.novakit/agents.json`
- Tool whitelisting per session with `/tools` command
- Semantic code search with vector indexing (LanceDB)
- `vector_search` tool for intelligent context retrieval

### Changed
- Upgraded to latest OpenRouter model catalog
- Improved checkpoint storage efficiency (50% reduction)
- Better markdown rendering with syntax highlighting

### Fixed
- LSP hover info not displaying for Python files
- Review mode not batching all changes correctly
- Config file permissions on Windows

## [0.1.4] - 2025-12-15

### Added
- LSP integration with 4 tools:
  - `lsp_goto_definition` - Navigate to symbol definition
  - `lsp_find_references` - Find all usages
  - `lsp_hover` - Get type info and documentation
  - `lsp_diagnostics` - Compiler errors and linting
- Review mode for batch change approval (`Shift+Tab`)
- Plan mode for read-only exploration
- Default LSP servers for TypeScript, Python, Go, Rust

### Changed
- Improved markdown rendering in terminal
- Better syntax highlighting for 20+ languages
- Faster startup time (30% improvement)

### Fixed
- File watching causing high CPU usage on large repos
- Undo/redo not working after provider switch

## [0.1.3] - 2025-12-10

### Added
- Checkpoints and instant rewind (`Esc+Esc`)
- Manual checkpoint creation with `/checkpoint`
- Session persistence and resume
- Favorite models (star with `*` in model selector)
- `novakit doctor` diagnostic command
- Shell completions for bash, zsh, fish

### Changed
- Improved session compaction algorithm
- Better error recovery in agent loop

### Fixed
- File watching causing high CPU usage
- Undo/redo not working after provider switch
- Memory files not loading on startup

## [0.1.2] - 2025-12-05

### Added
- GitHub Copilot provider support (OAuth flow)
- Groq provider for fast inference
- OpenRouter provider with 200+ models
- Custom OpenAI-compatible endpoint support
- `novakit provider add` command with presets

### Changed
- Reduced memory footprint for large codebases
- Improved token counting accuracy

### Fixed
- API key storage permissions on Linux
- Model selection not persisting across sessions

## [0.1.1] - 2025-12-01

### Added
- Multi-provider support (OpenAI, Anthropic)
- Basic file editing tools (read, write, edit)
- Session management (list, resume, export)
- Glob and grep tools for code search
- Web search and fetch tools

### Fixed
- Initial release bug fixes
- Terminal rendering issues on Windows

## [0.1.0] - 2025-11-25

### Added
- Initial release
- Interactive TUI chat interface with Ink/React
- Agent mode with full tool access
- File read/write/edit tools
- Bash command execution with safety filters
- Vision tool for image analysis
- Memory system (global and project-level)
- Configuration system (global and project)

[Unreleased]: https://github.com/omer-yld/novakit-cli-public/compare/v0.1.6...HEAD
[0.1.6]: https://github.com/omer-yld/novakit-cli-public/compare/v0.1.5...v0.1.6
[0.1.5]: https://github.com/omer-yld/novakit-cli-public/compare/v0.1.4...v0.1.5
[0.1.4]: https://github.com/omer-yld/novakit-cli-public/compare/v0.1.3...v0.1.4
[0.1.3]: https://github.com/omer-yld/novakit-cli-public/compare/v0.1.2...v0.1.3
[0.1.2]: https://github.com/omer-yld/novakit-cli-public/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/omer-yld/novakit-cli-public/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/omer-yld/novakit-cli-public/releases/tag/v0.1.0
