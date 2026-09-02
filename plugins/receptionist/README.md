# AI Receptionist Manager

A Claude Code plugin that lets you keep your Enhanced Rentals AI receptionist accurate —
teach it new facts and fix ones it gets wrong, in plain English.

## Install

```
/plugin marketplace add enhanced-rentals-ai/plugins
/plugin install receptionist@enhanced-rentals
```

## Set your token (once)

The plugin connects to your Enhanced Rentals workspace automatically — it just needs
your workspace token. One token = your workspace only. Ask your Enhanced Rentals contact
if you don't have it, then set it before launching Claude Code:

```bash
export ENHANCED_RENTALS_TOKEN=your_token_here
```

To keep it set, add that line to your `~/.zshrc` or `~/.bashrc`.

## Use it

Just say what you want:

- *"What does my receptionist know about parking?"*
- *"Add: there's a fridge in every room."*
- *"It says we have coffee in every room — only the suites do. Fix it."*

Or run the guided agent: **`receptionist-manager`** — it walks you through reviewing and
updating what your receptionist tells guests.

## What's included

- **Skill `edit-your-kb`** — add, correct, and search knowledge-base facts.
- **Agent `receptionist-manager`** — a guided helper for keeping the receptionist accurate.
- **Auto-connected MCP** — no manual setup beyond your token.

## What it won't touch

Knowledge-base **facts** only. How the receptionist *behaves* — greetings, door-code
logic, call routing — stays with the Enhanced Rentals team (too easy to break). Email
your contact for those.

## Troubleshooting

**"Not connected" / auth error** — check your token is set: `echo $ENHANCED_RENTALS_TOKEN`,
then restart Claude Code. Ask your contact for a fresh token if needed.
