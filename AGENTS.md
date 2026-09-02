# Agent instructions (Codex & non-plugin clients)

For Claude Code, install the plugin instead (see [`README.md`](README.md)) — it wires up
everything automatically. This file is for Codex or any other MCP-capable assistant using
the skills manually.

The skills live under [`plugins/`](plugins/). Find the one that matches what the user
wants and follow its `SKILL.md` exactly — it is the source of truth.

Skills available today:

- **[`find-knowledge-gaps`](plugins/receptionist/skills/find-knowledge-gaps/SKILL.md)** —
  mine real guest calls and chats (over MCP) for questions the receptionist couldn't
  answer, classify knowledge gaps vs behavior issues, and turn the gaps into fixes.
- **[`edit-your-kb`](plugins/receptionist/skills/edit-your-kb/SKILL.md)** — add, correct,
  and search knowledge-base facts for the AI receptionist (amenities, parking, WiFi,
  check-in/out times, house rules, local tips).

## Rules that apply everywhere in this repo

- **Scope is self-serve, safe edits only.** Never change how the AI *behaves* —
  greetings, door-code logic, call routing, escalation. Those are the bot prompt, not
  self-serve content. Tell the user to email their Enhanced Rentals contact.
- **Never invent data.** Ask the user for real values (e.g. a WiFi password).
- **Confirm the workspace with `ping` first**, show a draft before writing, verify after.
- One token = one workspace; you can only ever touch this customer's own data.
