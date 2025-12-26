# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in NovaKit CLI, please report it responsibly.

**DO NOT** open a public GitHub issue for security vulnerabilities.

### How to Report

Email **support@novakit.ai** with:

1. **Description** of the vulnerability
2. **Steps to reproduce** the issue
3. **Potential impact** (what could an attacker do?)
4. **NovaKit CLI version** affected
5. Your **contact information** for follow-up

### What to Expect

- **Acknowledgment** within 48 hours
- **Initial assessment** within 7 days
- **Regular updates** on our progress
- **Credit** in our security advisories (if desired)

### Scope

Security issues we're interested in:

- Authentication/authorization bypasses
- Remote code execution
- Credential leakage or exposure
- Privilege escalation
- Path traversal vulnerabilities
- Unsafe handling of user input

Out of scope:

- Issues in third-party dependencies (report to the maintainer)
- Social engineering attacks
- Physical attacks
- Denial of service (unless easily exploitable)

## Security Best Practices

### API Key Security

- Never commit API keys to version control
- Use environment variables or the secure credential store
- Rotate keys regularly
- Use provider-specific key restrictions when available

```bash
# Good: Use environment variable
export OPENAI_API_KEY=sk-...
novakit chat

# Good: Use secure credential store
novakit provider add openai --api-key YOUR_KEY
# Keys stored in ~/.novakit/credentials with 0600 permissions
```

### File Access

NovaKit CLI respects file access controls:

- Use `.novakitignore` to exclude sensitive files
- Configure `allowedReadRoots` to restrict access
- Review file operations in `auto` approval mode

```bash
# Restrict to specific directories
novakit config set allowedReadRoots '["/home/user/projects"]'
```

### Approval Modes

For sensitive operations, use restrictive approval modes:

```bash
# Read-only mode - safest
novakit chat --approval-mode read-only

# Auto mode (default) - confirms changes
novakit chat --approval-mode auto

# Full mode - use with caution
novakit chat --approval-mode full
```

### Network Security

- NovaKit CLI only connects to configured AI providers
- All API connections use HTTPS
- MCP servers run locally by default
- Web search/fetch tools respect robots.txt

## Supported Versions

| Version | Supported |
|---------|-----------|
| 0.1.x   | Yes       |

We recommend always using the latest version for security fixes.

## Security Updates

Security fixes are released as patch versions and announced via:

- [GitHub Security Advisories](https://github.com/omer-yld/novakit-cli-public/security/advisories)
- [Email newsletter](https://novakit.ai/newsletter)

Thank you for helping keep NovaKit CLI secure!
