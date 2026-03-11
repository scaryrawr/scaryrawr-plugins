# Agent Onboarding Guide

This repository is a **Claude Code plugin marketplace** containing custom plugins and MCP server integrations. This guide helps coding agents understand the repository structure and workflows.

## Repository Overview

- **First-party plugins**: `./plugins/` - Custom Claude Code plugins
- **External MCP wrappers**: `./external_plugins/` - Integrations with external MCP servers
- **Marketplace config**: `.claude-plugin/marketplace.json` - Registry of all plugins
- **Development docs**: `.github/copilot-instructions.md` - Plugin architecture guide

## Key File Patterns

### 1. Plugin Configuration
Each plugin has a `.claude-plugin/plugin.json` defining:
- `name`, `description`, `author`
- `source` path (relative to repo root)
- `category` (e.g., "development")
- Optional: `lspServers` for language server integration

Example from `plugins/azure-devops/.claude-plugin/plugin.json`:
```json
{
  "name": "azure",
  "description": "Azure Plugin for Claude",
  "author": {"name": "Mike Wallio"}
}
```

### 2. MCP Server Configuration
External plugins use `.mcp.json` to define MCP server commands:
```json
{
  "chrome-devtools": {
    "command": "npx",
    "args": ["chrome-devtools-mcp@latest", "--browser-url=http://127.0.0.1:9222"]
  }
}
```

### 3. Agent Definitions
Specialized agents use markdown files with YAML frontmatter:
- **Location**: `agents/` directory within a plugin
- **Format**: YAML frontmatter + markdown instructions
- **Tools**: List of available tools (Read, Grep, MCP functions)

Example from `external_plugins/chrome-devtools/agents/accessibility.md`:
```yaml
---
name: accessibility
description: Web applications performance specialist for accessibility compliance...
tools:
  - Read
  - Grep
  - Glob
  - Bash
  - mcp__plugin_chrome-devtools_...
---
```

### 4. Skill Instructions
Skill files teach specific workflows (e.g., Azure DevOps CLI usage):
- **Location**: `skills/` directory within a plugin
- **Format**: YAML frontmatter + markdown documentation
- **Purpose**: Provide contextual instructions for complex tasks

Example from `plugins/azure-devops/skills/azure-cli/SKILL.md`:
- URL parsing patterns for Azure DevOps links
- PR management workflows (review, checkout, voting)
- Work item operations (query, update, relations)

## Plugin Types

### MCP-Based Plugins
Wrap external MCP servers (e.g., Chrome DevTools, Playwright):
- Define server command in `.mcp.json`
- Use `npx` to launch MCP servers
- Provide browser automation, debugging tools

### Skill-Based Plugins
Provide contextual instructions:
- Teach URL parsing and CLI workflows
- Define specialized agent behaviors
- Example: Azure DevOps integration with `az` CLI

### LSP-Based Plugins
Enable language server integration:
- Defined in `marketplace.json` under `lspServers`
- Example: TypeScript native LSP using `tsgo`
- Supports IntelliSense, diagnostics, refactoring

## Development Workflow

1. **Add new plugin**:
   - Create directory in `plugins/` or `external_plugins/`
   - Add `.claude-plugin/plugin.json` with metadata
   - Register in `marketplace.json`

2. **MCP integration**:
   - Add `.mcp.json` with server command
   - Test MCP server launch
   - Verify tool availability

3. **Agent/skill creation**:
   - Create markdown file with YAML frontmatter
   - Define available tools
   - Write contextual instructions

4. **Testing**:
   - Install plugin: `claude plugin install <name>`
   - Verify tools are available
   - Test agent/skill instructions

## Installation Flow

1. Users add marketplace:
   ```sh
   claude plugin marketplace add scaryrawr/scaryrawr-plugins
   ```

2. Install specific plugin:
   ```sh
   claude plugin install typescript-native-lsp
   ```

3. Claude Code resolves source path → loads configuration → activates features

## Current Plugins

| Plugin | Type | Description |
|--------|------|-------------|
| `azure-devops` | Skill-based | Azure DevOps integration with PR/work item management |
| `typescript-native-lsp` | LSP-based | TypeScript/JavaScript code intelligence using `tsgo` |
| `chrome-devtools` | MCP-based | Chrome DevTools for automation, debugging, and performance analysis |
| `playwright-ext` | MCP-based | Browser automation using Playwright with extension bridge |

## Tools Available

### General Tools
- `Read`: Read file contents (text and images)
- `Grep`, `Glob`, `Bash`: File operations and searching
- `Edit`: Surgical text edits (find/replace)
- `Write`: Create or overwrite files

### MCP Tools (when plugin active)
- Chrome DevTools: Page navigation, scripting, performance analysis
- Playwright: Browser automation, form filling, testing
- Azure CLI: Work item/PR management via `az` commands

## Best Practices

1. **Plugin Structure**: Follow existing patterns for consistency
2. **Documentation**: Include README.md with installation requirements
3. **Testing**: Verify tools work before committing
4. **Compatibility**: Ensure MCP servers are available via npm
5. **Error Handling**: Provide clear error messages for missing dependencies

## Example Workflows

### Adding a Chrome DevTools Agent
1. Create `agents/my-agent.md` in chrome-devtools plugin
2. Define YAML frontmatter with tools list
3. Write markdown instructions for specific task
4. Test agent activation and tool usage

### Creating an LSP Plugin
1. Add entry to `marketplace.json` under `lspServers`
2. Define command and file extensions
3. Test language server integration
4. Document installation requirements in README.md

## Troubleshooting

- **MCP server not starting**: Check npm package availability
- **Tools unavailable**: Verify plugin installation and activation
- **Configuration errors**: Check JSON syntax in config files
- **Permission issues**: Ensure proper file permissions for plugin directories
