# Enhanced Rentals — Plugins for Claude Code

Self-serve tools that let Enhanced Rentals customers manage their AI **themselves**, in
plain English, from [Claude Code](https://claude.com/claude-code) — no code, no waiting
on support.

This repo is a Claude Code **plugin marketplace**. Add it once, install the plugin you
want, and you're set.

---

## Quick start

```
/plugin marketplace add enhanced-rentals-ai/plugins
/plugin install receptionist@enhanced-rentals
```

Then set your workspace token once (ask your Enhanced Rentals contact for it):

```bash
export ENHANCED_RENTALS_TOKEN=your_token_here
```

That's it — the plugin auto-connects to your workspace. Ask your assistant:
*"What does my receptionist know about parking?"*

---

## Available plugins

| Plugin | Install | What it does |
|---|---|---|
| **[AI Receptionist Manager](plugins/receptionist/)** | `receptionist@enhanced-rentals` | Teach your AI receptionist new facts and keep its knowledge base accurate — amenities, parking, WiFi, check-in times, house rules, local tips. |
| _More coming_ | | We add plugins over time. ⭐ Star this repo to follow along. |

---

## How it works

Each plugin bundles everything it needs — the connection to your Enhanced Rentals
workspace, the skills, and a guided agent — so installing it is all the setup there is.
Your **token lives only in your own environment** (`ENHANCED_RENTALS_TOKEN`), never in
this repo, and only ever reaches your own workspace.

## What these tools won't touch

By design, the self-serve tools only manage **content that's safe to edit yourself**.
Anything that changes how your AI *behaves* — greetings, door-code logic, call routing,
escalation — stays with the Enhanced Rentals team, because it's easy to break. Email
your contact for those.

---

## Using Codex or another AI assistant instead of Claude Code?

Plugins are a Claude Code feature. With Codex or any other MCP-capable assistant, you can
still use the same skills manually:

1. Connect the MCP server — copy `.mcp.json.example` to `.mcp.json` and paste your token,
   or for Codex add to `~/.codex/config.toml`:
   ```toml
   [mcp_servers.enhanced-rentals]
   command = "npx"
   args = ["-y", "mcp-remote", "https://er-svc-mcp.fly.dev/mcp",
           "--header", "Authorization: Bearer YOUR_TOKEN"]
   ```
2. Open this repo and follow [`AGENTS.md`](AGENTS.md) — it points your assistant at the
   same skill instructions the plugin uses.

---

## Support

Contact your Enhanced Rentals representative, or email
[hello@enhancedrentals.com](mailto:hello@enhancedrentals.com).

---

*Built by [Enhanced Rentals](https://enhancedrentals.com). MIT licensed — fork it, adapt
it, make it yours.*
