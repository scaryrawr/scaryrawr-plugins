# Claude Code Plugin Marketplace Development

This repository is a **plugin marketplace** for Claude Code, distributing both custom plugins and wrappers for external MCP servers. The architecture separates first-party plugins from third-party integrations.

## Repository Structure

```
plugins/          # First-party custom plugins
external_plugins/ # Wrappers for external MCP servers (Chrome DevTools, Playwright)
```

Each plugin contains:

- `.claude-plugin/plugin.json` - Plugin metadata and configuration
- `README.md` - User-facing installation and usage docs
- Optional: `.mcp.json` - MCP server configuration (for MCP-based plugins)
- Optional: `agents/` or `skills/` - Agent definitions or skill instructions

## Plugin Architecture Patterns

### MCP-Based Plugins

External plugins wrap MCP servers via `.mcp.json`. Example from [chrome-devtools/.mcp.json](../external_plugins/chrome-devtools/.mcp.json):

```json
{
  "chrome-devtools": {
    "command": "npx",
    "args": [
      "chrome-devtools-mcp@latest",
      "--browser-url=http://127.0.0.1:9222"
    ]
  }
}
```

### Skill-Based Plugins

Provide contextual instructions for AI agents. See [plugins/azure-devops/skills/azure-cli/SKILL.md](../plugins/azure-devops/skills/azure-cli/SKILL.md) which teaches URL parsing and Azure DevOps CLI workflows.

### Agent-Based Plugins

Define specialized agent behaviors. See [external_plugins/chrome-devtools/agents/](../external_plugins/chrome-devtools/agents/) for accessibility and performance analysis agents with frontmatter metadata:

```markdown
---
name: accessibility
description: Web applications performance specialist...
tools:
  - Read
  - Grep
  - Glob
  - Bash
  - mcp__plugin_chrome-devtools_...
---
```

### LSP-Based Plugins

Enable language server integration. The [typescript-native-lsp plugin](../plugins/typescript-native-lsp) defines LSP servers in `marketplace.json`:

```json
"lspServers": {
  "typescript": {
    "command": "tsgo",
    "args": ["--lsp", "--stdio"],
    "extensionToLanguage": { ".ts": "typescript", ... }
  }
}
```

## Marketplace Configuration

[.claude-plugin/marketplace.json](../.claude-plugin/marketplace.json) is the registry of all plugins with:

- Plugin metadata (name, description, author, category)
- Source paths (relative to repo root)
- Plugin-specific configs (LSP servers, strictness)

## Installation Flow

1. Users add marketplace: `claude plugin marketplace add scaryrawr/scaryrawr-plugins`
2. Install specific plugin: `claude plugin install typescript-native-lsp`
3. Claude Code resolves source path → loads plugin configuration → activates features

## When Creating New Plugins

- Place in `plugins/` (custom) or `external_plugins/` (MCP wrapper)
- Create `.claude-plugin/plugin.json` with name, description, author
- Add plugin entry to `marketplace.json`
- For MCP servers: include `.mcp.json` with server config
- For agents: use markdown files in `agents/` with frontmatter
- For skills: use markdown files in `skills/` with frontmatter
- Document installation requirements (e.g., `npm install -g @typescript/native-preview`) in README.md
