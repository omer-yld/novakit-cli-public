---
name: Bug report
about: Report a bug in NovaKit CLI
title: '[BUG] '
labels: bug
assignees: ''
---

## Describe the bug
A clear and concise description of what the bug is.

## To Reproduce
Steps to reproduce the behavior:
1. Run command '...'
2. Type '...'
3. See error

## Expected behavior
A clear and concise description of what you expected to happen.

## Terminal Output
```
Paste relevant terminal output or error messages here.
Run with DEBUG=* for more details: DEBUG=* novakit chat
```

## Environment

**Required info (run `novakit doctor` and paste output):**
```
Paste novakit doctor output here
```

**Additional details:**
- **OS**: [e.g., macOS 14.0, Ubuntu 22.04, Windows 11]
- **Terminal**: [e.g., iTerm2, Windows Terminal, Alacritty]
- **Shell**: [e.g., bash, zsh, fish, PowerShell]
- **Provider**: [e.g., OpenAI, Anthropic, GitHub Copilot, NovaKit]
- **Model**: [e.g., gpt-4o, claude-sonnet-4-5]

## Configuration

If relevant, share your config (remove sensitive data):

```bash
novakit config list
```

## Checklist

- [ ] I've searched existing issues to ensure this isn't a duplicate
- [ ] I've run `novakit doctor` and included the output
- [ ] I've updated to the latest version (`npm update -g novakit-cli`)
- [ ] I've tried with a fresh session (`novakit chat --new`)

## Additional context
Add any other context about the problem here.
