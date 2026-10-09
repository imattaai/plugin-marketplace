# TASKn Grok Plugin

A Grok Build plugin that connects your coding agent to [TASKn](https://taskn.app) — the AI task manager. Once installed and authenticated, ask things like "what's on my plate today?" or "add a task to call the dentist tomorrow" and it reads from and writes to your TASKn account directly.

## What's inside

| Component | Location | Purpose |
| --- | --- | --- |
| Skill | `skills/taskn/SKILL.md` | How to use the 10 TASKn tools (feed, search, projects, create/update/complete/delete, stats, shared) |
| MCP server | `.mcp.json` | Remote MCP config → `https://mcp.taskn.app/functions/v1/mcp` |

## Authentication

The TASKn MCP server uses OAuth 2.1 (authorization-code + PKCE) via the user's TASKn sign-in (Google, Apple, or email). The MCP client performs the browser sign-in flow; this plugin ships no credentials and no secrets. Each user connects their own TASKn account — the plugin only ever sees the signed-in user's data.

## Network endpoints

- `https://mcp.taskn.app/functions/v1/mcp` — TASKn MCP server (tool calls)
- `https://qxxjwdzxmflvzungriuq.supabase.co/auth/v1/oauth/...` — TASKn OAuth issuer (authorize/token, via Supabase Auth)

No other network calls. No telemetry.

## Install (via marketplace)

Once listed: `grok plugin add taskn` (or browse `/marketplace`), then authenticate the TASKn connector when prompted.

## Manual install

1. Copy this directory's `skills/taskn` and `.mcp.json` into your Grok Build plugin locations.
2. Authenticate the `taskn` MCP server with your TASKn account on first use.

## License

MIT — see [LICENSE](LICENSE).
