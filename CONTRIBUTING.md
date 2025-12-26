# Contributing to NovaKit CLI

Thank you for your interest in contributing to NovaKit CLI!

## How to Contribute

### Reporting Bugs

1. **Check existing issues** — Search [issues](https://github.com/omer-yld/novakit-cli-public/issues) to avoid duplicates
2. **Run diagnostics** — Run `novakit doctor` and include the output
3. **Use the template** — File a [bug report](https://github.com/omer-yld/novakit-cli-public/issues/new?template=bug_report.md)
4. **Include details**:
   - NovaKit CLI version (`novakit --version`)
   - Node.js version (`node --version`)
   - Operating system and terminal
   - Provider being used
   - Steps to reproduce

### Requesting Features

1. Check the [roadmap](https://github.com/omer-yld/novakit-cli-public/discussions/categories/roadmap) first
2. Use the [feature request template](https://github.com/omer-yld/novakit-cli-public/issues/new?template=feature_request.md)
3. Describe your use case and why this feature would be valuable
4. Consider if this could be solved with [custom commands](#custom-commands--agents)

### Community

- Share your workflows and custom commands
- Help others troubleshoot issues
- Give feedback on beta features

## Code Contributions

NovaKit CLI is currently closed-source. We appreciate your interest, but we're not accepting code contributions at this time.

If you've found a security vulnerability, please email **support@novakit.ai** instead of opening a public issue.

## Custom Commands & Agents

You can extend NovaKit CLI without touching the source code!

### Custom Commands

Create `~/.novakit/commands.json`:

```json
{
  "commands": {
    "review": {
      "type": "prompt",
      "description": "Review code for issues",
      "template": "Review this code for bugs, security issues, and best practices:\n\n{{input}}"
    },
    "test": {
      "type": "bash",
      "description": "Run project tests",
      "command": "npm test"
    },
    "commit": {
      "type": "prompt",
      "description": "Generate commit message",
      "template": "Generate a conventional commit message for these changes. Be concise."
    }
  }
}
```

### Custom Agents

Create `~/.novakit/agents.json`:

```json
{
  "agents": {
    "security": {
      "name": "Security Analyst",
      "description": "Focused on security review",
      "systemPrompt": "You are a security expert. Focus on finding vulnerabilities, insecure patterns, and security best practices.",
      "tools": ["read", "grep", "glob"],
      "preferredModel": "claude-sonnet-4-5"
    },
    "docs": {
      "name": "Documentation Writer",
      "description": "Write and improve documentation",
      "systemPrompt": "You are a technical writer. Write clear, concise documentation.",
      "tools": ["read", "write", "edit", "glob"]
    }
  }
}
```

### Sharing Your Creations

1. Create a GitHub Gist with your command/agent config
2. Post it in [Show and Tell](https://github.com/omer-yld/novakit-cli-public/discussions/categories/show-and-tell)
3. Tag it with `#custom-command` or `#custom-agent`

## MCP Servers

You can extend NovaKit CLI with Model Context Protocol servers.

### Configuration

Create `~/.novakit/mcp.json`:

```json
{
  "servers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/dir"]
    }
  }
}
```

### Sharing MCP Configs

Share useful MCP server configurations in [Discussions](https://github.com/omer-yld/novakit-cli-public/discussions) with the `#mcp` tag.

## Feedback

We love hearing from users! The best ways to provide feedback:

- **[GitHub Discussions](https://github.com/omer-yld/novakit-cli-public/discussions)** — General discussions, Q&A, show and tell
- **[GitHub Issues](https://github.com/omer-yld/novakit-cli-public/issues)** — Bug reports and feature requests
- **[Email](mailto:support@novakit.ai)** — Private feedback
- **[Twitter/X](https://twitter.com/novakitai)** — Quick feedback and updates

## Code of Conduct

- Be respectful and inclusive
- Help others learn
- Give constructive feedback
- Report harassment to support@novakit.ai

Thank you for being part of the NovaKit community!
