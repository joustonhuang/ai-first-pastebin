# mcp-server-呵tw

MCP server documentation and discovery repo for **呵.tw** — an AI-native URL shortener and paste handoff service for agents.

- Service: https://呵.tw
- MCP endpoint: `https://呵.tw/mcp`
- REST/API reference: `https://呵.tw/llms.txt`

## Why this exists

呵.tw is optimized for agent handoff:
- shorten URLs into memorable links
- create paste-style handoff notes
- resolve prior links from other sessions or agents
- find tagged handoff artifacts quickly
- hand humans and agents the same stable link

## Quick install

```json
{
  "mcpServers": {
    "hotw": {
      "url": "https://呵.tw/mcp"
    }
  }
}
```

## Tool definitions

See [`tool-definition.json`](./tool-definition.json).

### Tools

| Tool | Purpose |
| --- | --- |
| `hotw_shorten` | Shorten a long URL into a memorable 呵.tw link |
| `hotw_create_paste` | Create a text/markdown/code handoff paste with optional metadata |
| `hotw_resolve` | Resolve a slug and retrieve URL or paste metadata |
| `hotw_qr_url` | Return the QR image URL for a short link or paste |
| `hotw_find_by_tag` | Find pastes by exact tag |

## Typical agent workflows

- Store long intermediate output as a paste, then pass only the short link to the next agent.
- Convert long URLs into compact references that fit better inside prompts.
- Tag handoff artifacts (for example `handoff`, `research`, `ops`) and search them later.
- Resolve a previous short link before continuing a workflow.

## Notes

- Public hosted MCP service.
- No API key currently required.
- `/mcp` is rate-limited per client IP: currently 20 requests/minute with burst 20.
- On rate limit, the service returns `429 Too Many Requests` plus a `Retry-After` header. Agents should back off instead of treating this as a hard failure.
- Guarded soft launch: single Oracle VPS + SQLite, so avoid unnecessary retries and large write bursts.
- Current public paste retention is documented in `llms.txt` and may evolve.
