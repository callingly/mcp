# Installing the Callingly MCP server

Callingly is a **hosted, remote** MCP server. There is nothing to clone, build, or run locally,
and no API key to obtain — authentication happens in the browser via OAuth when the client first
connects.

| | |
|---|---|
| **Endpoint** | `https://api.callingly.com/mcp` |
| **Transport** | Streamable HTTP |
| **Auth** | OAuth 2.0 + PKCE with dynamic client registration (handled automatically by the client) |

## Cline

1. Open Cline → **MCP Servers** → **Remote Servers**.
2. Add a server with:
   - **Server Name:** `callingly`
   - **Server URL:** `https://api.callingly.com/mcp`
3. On first use, a browser window opens to sign in to Callingly and approve access. That's it.

Equivalent `cline_mcp_settings.json` entry:

```json
{
  "mcpServers": {
    "callingly": {
      "url": "https://api.callingly.com/mcp",
      "type": "streamableHttp"
    }
  }
}
```

## Other MCP clients

Use the standard remote-server config:

```json
{
  "mcpServers": {
    "callingly": {
      "type": "streamable-http",
      "url": "https://api.callingly.com/mcp"
    }
  }
}
```

If the client only supports local (stdio) servers, bridge with
[`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "callingly": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://api.callingly.com/mcp"]
    }
  }
}
```

## Verifying the connection

After OAuth completes, the server exposes 8 tools (`make_dispatch_call`, `make_ai_call`,
`send_sms`, `schedule_sms`, `schedule_call`, `add_to_sequence`, `get_recording`, `query_reports`).
A quick read-only check that doesn't contact anyone: call `query_reports` for the last 7 days.

> **Note:** the call/SMS/scheduling tools place real phone calls and send real texts. They are
> annotated `destructiveHint: true`, so well-behaved clients ask for confirmation before running
> them. You need a [Callingly account](https://callingly.com/register) (14-day free trial).
