# Changelog

All notable changes to NovaKit CLI will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.8] - 2025-12-28

### Improved
- **Performance & Flickering Elimination**: Major rendering optimizations for smoother TUI experience
  - Adaptive streaming throttle (16-80ms) that adjusts based on chunk arrival rate
  - Isolated spinner component in StatusBar - animations don't trigger parent re-renders
  - Removed thinking message rotation (static "Thinking..." for less re-renders)
  - Added React.memo to Input, StatusBar, and Markdown components
  - LRU cache (50 entries) for parsed markdown content
  - Change detection to skip redundant state updates
  - `clearStreamingBuffers()` to cancel pending flushes at end of stream

### Fixed
- TypeScript errors in Markdown.tsx (Shiki language types, markedTerminal options)
- Flickering when typing in chat input
- Flickering after stream completion

## [0.1.7] - 2025-12-27

### Added
- **Task Management Tool (`todos`)**: Track progress on complex, multi-step tasks
  - Array-based API similar to Claude Code's TodoWrite
  - Per-session todo persistence (todos auto-load when returning to a session)
  - Collapsible TodoPanel UI component (toggle with `Alt+T`)
  - Status tracking: pending, in_progress, completed
  - Commands: `/loadtodos`, `/cleartodos`, `/todos-toggle`
  - Event-driven UI sync for real-time updates

- **Automated Subagent Spawning (`spawn_agent`)**: AI can now automatically spawn specialized agents
  - **Explorer Agent**: Deep codebase exploration and understanding
  - **Planner Agent**: Implementation planning before coding
  - **Background Agent**: Long-running tasks that don't block chat
  - **Reviewer Agent**: Code review after implementation
  - Agents run in background by default (use `wait: true` for foreground)
  - Monitor with `/tasks`, kill with `/task-kill`
  - Each agent type has specialized system prompts

### Changed
- Subagents now run in background by default instead of blocking
- Enhanced tool display in UI shows agent type being spawned (e.g., "Spawn Explorer")
- Improved result summaries for `spawn_agent` and `todos` tools

## [0.1.5] - 2025-12-26

### Added
- **Subagent manager**: Background task execution with approval mode system
  - Approval modes: read-only, auto, and full access
  - Task spawning, monitoring, and cleanup functionality
  - Enhanced permission checks based on approval levels
- **System prompts framework**: Configurable prompt versions for different behaviors
  - Current and enhanced prompt variants
  - Metadata and token estimation support
- **Enhanced apply-patch tool**: Improved multi-file patch application
  - Better format support and error handling
  - Unified diff format compatibility

## [0.1.4] - 2025-12-25

### Added
- Dynamic version display in Header component reflecting package version

## [0.1.3] - 2025-12-25

### Added
- GitHub Copilot OAuth flow in onboarding process
- Support for GitHub Copilot as an AI provider

## [0.1.2] - 2025-12-25

### Added
- Enhanced help command with detailed keyboard shortcuts
- Common commands reference in help output

### Changed
- Removed deprecated files
- Updated package dependencies

## [0.1.1] - 2025-12-24

### Changed
- Updated repository owner to omer-yld

## [0.1.0] - 2025-12-24

### Added
- Initial release of NovaKit CLI
- Interactive TUI chat interface with Ink
- Multi-provider support (OpenAI, Anthropic, Google, OpenRouter, GitHub Copilot)
- Tool execution system (file read/write, bash, search)
- Vector index for semantic code search using LanceDB
- Auto-indexer with file watching
- MCP (Model Context Protocol) client and server management
- Session management with save/resume capability
- Command palette with category filtering
- Git integration (commit, diff modals)
- Snippet picker UI
- Markdown rendering with syntax highlighting
- Onboarding flow for first-time users with API key validation
- Provider authentication and configuration
- Path safety checks with `allowedReadRoots` configuration
- CI and release workflows
- Homebrew formula support

[Unreleased]: https://github.com/omer-yld/novakit-cli/compare/v0.1.8...HEAD
[0.1.8]: https://github.com/omer-yld/novakit-cli/compare/v0.1.7...v0.1.8
[0.1.7]: https://github.com/omer-yld/novakit-cli/compare/v0.1.5...v0.1.7
[0.1.5]: https://github.com/omer-yld/novakit-cli/compare/v0.1.4...v0.1.5
[0.1.4]: https://github.com/omer-yld/novakit-cli/compare/v0.1.3...v0.1.4
[0.1.3]: https://github.com/omer-yld/novakit-cli/compare/v0.1.2...v0.1.3
[0.1.2]: https://github.com/omer-yld/novakit-cli/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/omer-yld/novakit-cli/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/omer-yld/novakit-cli/releases/tag/v0.1.0
