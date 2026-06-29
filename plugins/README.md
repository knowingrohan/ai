# Plugins

MCP servers, agent plugins, and runtime extensions.

## Directory Convention

Each plugin lives in its own directory:

```
plugins/
  <plugin-name>/
    README.md       ← what it does, install steps, available tools
    package.json    ← if it is an npm-published MCP server
    src/            ← plugin source (if applicable)
```

## Plugin Types

- **MCP servers** — expose tools to Claude Code, Cursor, Windsurf, and any MCP-compatible agent
- **Agent plugins** — runtime extensions for specific agent platforms
- **Browser extensions** — config or manifests for browser-based agents

## Adding a Plugin

1. Create `plugins/<plugin-name>/`
2. Add a `README.md` with: what the plugin does, install command, list of tools or capabilities exposed
3. Add source or config files as needed
