# 呵.tw

[![SafeSkill 50/100](https://img.shields.io/badge/SafeSkill-50%2F100_Use%20with%20Caution-orange)](https://safeskill.dev/scan/joustonhuang-mcp-server-hotw)
GitHub entry point for the 呵.tw MCP service.

- Human portal: https://呵.tw/developer
- Agent documentation: `https://呵.tw/llms.txt`
- Saves agent tokens by replacing long URLs and large handoff content with short links
- MCP endpoint: `https://呵.tw/mcp`
- Bundled helper CLI: `scripts/hotw.py`
- Safety: for shareable handoff content only — do not upload API keys, tokens, passwords, or private data

```json
{
  "mcpServers": {
    "hotw": {
      "url": "https://呵.tw/mcp"
    }
  }
}
```
