# Moss MCP Skill

> Add Moss semantic search to any MCP client -- Cursor, Claude, VS Code, Windsurf, and more.

## Install

```bash
npx -y @moss-tools/mcp-server
```

## Configure

Add to your MCP client config:

```json
{
  "mcpServers": {
    "moss": {
      "command": "npx",
      "args": ["-y", "@moss-tools/mcp-server"],
      "env": {
        "MOSS_PROJECT_ID": "your-project-id",
        "MOSS_PROJECT_KEY": "your-project-key",
        "MOSS_DEFAULT_INDEX": "your-default-index"
      }
    }
  }
}
```

### Claude Code

```bash
claude mcp add moss -e MOSS_PROJECT_ID=your-project-id -e MOSS_PROJECT_KEY=your-project-key -- npx -y @moss-tools/mcp-server
```

### Codex

```bash
codex mcp add moss --env MOSS_PROJECT_ID=your-project-id --env MOSS_PROJECT_KEY=your-project-key -- npx -y @moss-tools/mcp-server
```

## Environment Variables

- `MOSS_PROJECT_ID` (required): Your project ID from https://portal.usemoss.dev
- `MOSS_PROJECT_KEY` (required): Your project key from the portal
- `MOSS_DEFAULT_INDEX` (optional): Default index name. When set, tools use it as the default.

## Available Tools

- `search`: Semantic, keyword, or hybrid search over an index. Auto-loads the index if needed.
- `create-index`: Create a new index from a list of documents.
- `add-docs`: Add or upsert documents into an existing index.
- `delete-docs`: Delete documents from an index by ID.
- `get-docs`: Retrieve documents from an index.
- `list-indexes`: List all indexes in the current project.
- `get-index`: Get metadata and stats for a specific index.
- `delete-index`: Delete an index and all its data.

## Common Failure Modes

1. `INDEX_NOT_FOUND` -- The index name doesn't exist. Use `list-indexes` to see available indexes.
2. `AUTH_FAILED` -- Check that MOSS_PROJECT_ID and MOSS_PROJECT_KEY are set correctly.
3. `RATE_LIMITED` -- Back off and retry. Default limit is 1000 requests/hour.

## Links

- MCP discovery: https://moss.dev/.well-known/mcp.json
- Full documentation: https://moss.dev/llms-full.txt
- Portal: https://portal.usemoss.dev
- Repository: https://github.com/usemoss/moss-mcp
