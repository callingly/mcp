# Callingly MCP Server

> Get your sales leads on the phone with a real rep — straight from your AI agent.

The **Callingly MCP server** lets AI agents and automation tools turn a decision to reach a lead
into an actual phone call. Connect [Claude](https://claude.ai), [ChatGPT](https://chatgpt.com),
[Cursor](https://cursor.com), or an [n8n](https://n8n.io) workflow, and your agent can place a call,
send a text, or schedule a callback. Callingly dials your sales team, plays them the lead's details,
and warm-transfers the live conversation — then syncs outcomes, recordings, and transcripts back so
your agent and CRM stay in step.

It's a **hosted, remote** server — there's nothing to install or self-host. You authenticate with
your Callingly account over OAuth and start calling.

- 🌐 **Learn more:** <https://callingly.com/features/mcp/>
- 🆓 **Start free:** <https://callingly.com/register> (14-day trial, no credit card)

## Connection

| | |
|---|---|
| **Endpoint** | `https://api.callingly.com/mcp` |
| **Transport** | Streamable HTTP |
| **Auth** | OAuth 2.0 + PKCE, dynamic client registration (scope `mcp:use`) |

Because it uses standard OAuth with dynamic client registration, MCP clients that support remote
servers will prompt you to sign in to Callingly on first connect — no manual API keys or callback
registration required.

## Add it to your client

### Claude, Cursor, and other MCP clients

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

If your client only supports local (stdio) servers, bridge to the remote endpoint with
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

### ChatGPT

Add Callingly as a connector in ChatGPT (Developer Mode / Apps) using the endpoint above; ChatGPT
handles the OAuth sign-in.

### n8n

Use the **MCP Client** node and point it at `https://api.callingly.com/mcp`. Your n8n workflows can
then call a lead, send an SMS, or schedule a callback as a step.

## Tools

| Tool | What it does |
|---|---|
| `make_dispatch_call` | Ring your team's reps and bridge them to the lead the moment someone picks up. |
| `make_ai_call` | Send an AI voice agent to call and qualify the lead first, then warm-transfer to a human rep. |
| `send_sms` | Text an existing lead from one of your Callingly phone numbers. |
| `schedule_sms` | Queue a text to a lead to be sent at a future time. |
| `schedule_call` | Book a callback for a lead at a future time. |
| `add_to_sequence` | Enroll the lead in a team's automated voice + SMS follow-up sequence. |
| `get_recording` | Read recordings, transcripts, summaries, and outcomes of recent calls. |
| `query_reports` | Call volume, connection rate, and per-rep performance over a date range. |

Calls, AI calls, and sequences are routed through a Callingly **team** (identified by `team_id`).
Because these tools place real calls and send real texts to real people, the call/SMS/schedule tools
are annotated as destructive — well-behaved clients will confirm intent before running them.

## How a call works

1. Your agent decides a lead should be contacted and calls a Callingly tool.
2. Callingly rings an available rep on your team (or an AI agent, for `make_ai_call`).
3. The rep hears the lead's details, then Callingly bridges them to the lead.
4. The outcome, recording, and transcript sync back to your agent and CRM.

## About Callingly

[Callingly](https://callingly.com) is an AI lead-response platform that gets your inbound leads on
the phone in seconds — by AI agent, by automation, or by your own workflows. In business since 2019.

## Support

- Docs & overview: <https://callingly.com/features/mcp/>
- Email: support@callingly.com
