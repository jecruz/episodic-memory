# Episodic Memory — Shared Multi-Agent Fork

## What This Is

Fork of github.com/obra/episodic-memory. Modified to support shared multi-agent conversation memory over SSE/MCP.

**Changes from upstream:**
- SSE transport instead of stdio (network-accessible MCP server)
- `agent_id` column on exchanges
- Shared data path via `EPISODIC_SHARED_DIR`
- Agent registration before indexing

## Tech Stack

- **Language:** TypeScript
- **Database:** SQLite + sqlite-vec (vector similarity)
- **MCP SDK:** @modelcontextprotocol/sdk
- **Embeddings:** Transformers.js (local, offline)
- **Transport:** SSE (Server-Sent Events)

## Project Structure

```
src/
├── db.ts           — SQLite init, migrations, insert/search
├── paths.ts        — Data path config (EPISODIC_SHARED_DIR)
├── mcp-server.ts   — MCP server (SSE transport)
├── search.ts       — Vector + text search
├── show.ts         — Format conversation as markdown
├── indexer.ts      — Index conversation files
├── types.ts        — TypeScript interfaces
└── embeddings.ts   — Transformers.js embeddings
```

## Key Paths

| Path | Purpose |
|------|---------|
| `src/db.ts` | Schema + migrations |
| `src/paths.ts` | Data directory config |
| `src/mcp-server.ts` | MCP server (SSE) |

## Commands

```bash
npm install          # Install dependencies
npm run build        # TypeScript compile + esbuild bundle
npm run index        # Index conversations
npm run search       # Search conversations
npm test            # Run tests
```

## MCP Tools

| Tool | Description |
|------|-------------|
| `episodic_memory_search` | Semantic search across conversations |
| `episodic_memory_show` | Display full conversation as markdown |

## Environment Variables

| Variable | Default | Purpose |
|---------|---------|---------|
| `EPISODIC_SHARED_DIR` | `~/.config/superpowers` | Shared data directory |
| `EPISODIC_PORT` | `8002` | SSE server port |
| `AGENT_ID` | — | This agent's identity |

## Data Schema

```sql
exchanges (
  id TEXT PRIMARY KEY,
  project TEXT NOT NULL,
  agent_id TEXT,           -- NEW: which agent created this
  session_id TEXT,         -- session UUID
  git_branch TEXT,         -- git branch at time of exchange
  user_message TEXT,
  assistant_message TEXT,
  embedding BLOB,
  ...
)
```
