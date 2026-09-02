# Atlas — your Enhanced Rentals AI colleague

A Claude Code plugin that gives you **Atlas**, an AI teammate for your Enhanced Rentals
workspace. Atlas keeps your AI receptionist sharp — it reads your real guest calls to
find what the receptionist is missing, adds and corrects the facts it tells guests, and
helps you stay on top of your operation. More abilities land over time.

## Install

```
/plugin marketplace add enhanced-rentals-ai/plugins
/plugin install atlas@enhanced-rentals
```

## Set your token (once)

Atlas connects to your Enhanced Rentals workspace automatically — it just needs your
workspace token. One token = your workspace only. Ask your Enhanced Rentals contact if
you don't have it, then set it before launching Claude Code:

```bash
export ENHANCED_RENTALS_TOKEN=your_token_here
```

To keep it set, add that line to your `~/.zshrc` or `~/.bashrc`.

## Use it

Just talk to Atlas:

- *"What is my receptionist missing?"* → Atlas scans recent guest calls for questions it
  couldn't answer and shows you the gaps
- *"What does my receptionist know about parking?"*
- *"Add: there's a fridge in every room."*
- *"It says we have coffee in every room — only the suites do. Fix it."*

Run the **`atlas`** agent for a guided teammate that walks you from *finding* gaps to
*fixing* them.

## What Atlas can do today

- **`find-knowledge-gaps`** — mine real guest calls and chats for questions the
  receptionist couldn't answer, and turn the genuine gaps into fixes.
- **`edit-your-kb`** — add, correct, and search knowledge-base facts.
- **Auto-connected MCP** — no manual setup beyond your token.

More skills — reaching further into the platform — are on the way.

## What Atlas won't touch

Knowledge-base **facts** only. How the receptionist *behaves* — greetings, door-code
logic, call routing — stays with the Enhanced Rentals team (too easy to break). Ask Atlas
and it'll tell you to email your contact for those.

## Troubleshooting

**"Not connected" / auth error** — check your token is set: `echo $ENHANCED_RENTALS_TOKEN`,
then restart Claude Code. Ask your contact for a fresh token if needed.
