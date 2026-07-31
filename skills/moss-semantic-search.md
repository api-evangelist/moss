# Moss Skill

> Real-time semantic search for AI agents. Sub-10ms local retrieval.

## Quick Validation

```bash
curl -H "X-Project-Key: YOUR_KEY" https://service.usemoss.dev/v1/health
# Expected: {"status":"ok"}
```

## Prerequisites

- Node 18+ or Python 3.10+
- Moss account: sign up at https://portal.usemoss.dev or provision via POST /v1/provision

## Install

```bash
npm install @moss-dev/moss    # JavaScript/TypeScript
pip install moss     # Python
```

## Authenticate

Set environment variables:
- MOSS_PROJECT_ID: your project ID from the portal
- MOSS_PROJECT_KEY: your project key from the portal

```javascript
import { MossClient } from '@moss-dev/moss';
const client = new MossClient(process.env.MOSS_PROJECT_ID, process.env.MOSS_PROJECT_KEY);
```

```python
from moss import MossClient
client = MossClient(project_id=os.environ["MOSS_PROJECT_ID"], project_key=os.environ["MOSS_PROJECT_KEY"])
```

## Create Index

```javascript
await client.createIndex('my-index', [
  { id: '1', text: 'Moss delivers sub-10ms semantic search' },
  { id: '2', text: 'Indexes run locally in the browser or on device' },
]);
```

```python
await client.create_index('my-index', [
    {'id': '1', 'text': 'Moss delivers sub-10ms semantic search'},
    {'id': '2', 'text': 'Indexes run locally in the browser or on device'},
])
```

## Load and Query

```javascript
await client.loadIndex('my-index');  // Downloads index for local queries
const results = await client.query('my-index', 'fast search');
// results: [{ id: '1', text: '...', score: 0.92 }]
```

```python
await client.load_index('my-index')
results = await client.query('my-index', 'fast search')
```

## Common Failure Modes

1. `INDEX_NOT_LOADED` -- You must call loadIndex() before query(). Use the MCP `search` tool to avoid this (it auto-loads).
2. `INVALID_ALPHA` -- alpha must be 0-1. 0 = keyword only, 1 = semantic only, 0.5 = hybrid (default).
3. `METADATA_TYPE_ERROR` -- All metadata values must be strings, not numbers or booleans. Use `"25"` not `25`.

## Error Handling

- All errors extend MossError with `code`, `message`, `retryable` fields
- If `retryable: true`, use exponential backoff with jitter
- Common codes: INDEX_NOT_FOUND, INDEX_NOT_LOADED, INVALID_ALPHA, METADATA_TYPE_ERROR, RATE_LIMITED

## Embedding Models

- `moss-minilm` (default): Fast, lightweight, ideal for edge/offline use
- `moss-mediumlm`: Higher accuracy with reasonable performance
- Custom (BYOE): Pass pre-computed embeddings via DocumentInfo.embedding

## Key Parameters

- `alpha` (float, 0-1): Controls hybrid search blend. 0 = pure keyword/BM25, 1 = pure semantic, 0.5 = balanced hybrid.
- `topK` (int, 1-100): Number of results to return. Default 5.
- `metadata` (Record<string, string>): Key-value pairs for filtering. Values must be strings.

## Links

- Full documentation: https://moss.dev/llms-full.txt
- Documentation index: https://moss.dev/llms-docs.txt
- Portal: https://portal.usemoss.dev
- MCP discovery: https://moss.dev/.well-known/mcp.json
- Docs: https://docs.moss.dev
- Samples: https://github.com/usemoss/moss-samples
