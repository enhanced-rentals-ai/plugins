---
name: find-knowledge-gaps
description: >-
  Find gaps in an Enhanced Rentals AI receptionist's knowledge base by mining real
  guest conversations and call transcripts over the MCP server — questions the
  receptionist couldn't answer, answered wrong, or had to escalate. Produces a ranked
  gap report and, on approval, fills each gap via the edit-your-kb skill. Use when the
  user asks "what is my receptionist missing / getting wrong / what should I add".
---

# Find Knowledge Gaps

You help an Enhanced Rentals customer discover what their AI receptionist **doesn't
know** — by reading the receptionist's real guest conversations and call transcripts and
spotting the questions it failed to answer. Then you turn the genuine gaps into
knowledge-base articles.

Everything runs over the Enhanced Rentals MCP server (tools prefixed `enhanced-rentals`
/ `mcp__enhanced-rentals__*`). One token = one workspace; you only ever see this
customer's own data. This is a **read-and-analyze** job — you never write to the KB
without showing the user first and getting approval.

## The signals (in order of strength)

1. **`list_failure_flags`** — voice calls a human operator explicitly marked as a bad
   bot reply, each with a `reason` and a linked transcript. Strongest signal.
2. **`list_inbox_failure_flags`** — the same, for the chat/text inbox.
3. **`list_transcripts`** — recent voice calls. The `summary` field usually reveals the
   ask; calls that end in "transferred to an agent / consultant" or "couldn't find the
   info" are prime gap candidates.
4. **`list_conversations`** — recent chat threads; same idea.

Use `get_transcript` / `get_conversation` to read the full turns for any candidate, so
you quote the guest's actual question (not a paraphrase).

## Workflow

1. **Confirm scope.** `ping` (tell the user which workspace) and `list_bots` to identify
   the receptionist. Ask how far back / how many to scan if unbounded (default: the most
   recent ~50 transcripts + all open failure flags).
2. **Gather signals.** Pull `list_failure_flags` and `list_inbox_failure_flags` first,
   then page `list_transcripts` / `list_conversations`. Read summaries; open the full
   transcript/conversation for anything that looks like an unanswered question.
3. **Extract the real question.** For each candidate, capture the guest's actual ask in
   their words (e.g. "Do the rooms have a fridge?", "Can I park a boat?").
4. **Classify — this matters.** Sort each into exactly one:
   - **Knowledge gap** → the receptionist lacked a *fact* (amenities, parking, WiFi,
     times, rules, local tips). **Fixable here.**
   - **Behavior / tone / routing issue** → the receptionist knew but acted wrong
     (escalated too fast, wrong process, bad phrasing). **Not fixable here** — this is
     the bot's prompt. List it separately and tell the user to email their Enhanced
     Rentals contact. Do NOT try to fix it.
   - **Not a gap** → a legitimate transfer-to-human (booking change, complaint) or a
     one-off. Drop it.
5. **Confirm each gap is genuine.** For every knowledge gap, `search_knowledge_base` for
   the topic. If an article already exists but the receptionist still missed it, note
   that the existing article may need to be clearer or the answer corrected — don't just
   duplicate it.
6. **Report.** Present a short, ranked table the user can act on:

   | Gap | How often | Guest asked | KB today | Suggested fix |
   |---|---|---|---|---|
   | In-room fridge | 4 calls | "Do the rooms have a fridge?" | nothing | Add "In-room fridge" |
   | Boat parking | 2 calls | "Can I park my boat?" | parking article, no boat | Update parking article |

   Below the table, list the **behavior/routing issues** separately under a clear heading
   like "Needs the Enhanced Rentals team (not a knowledge gap)".
7. **Offer to fix.** Ask which gaps to fill. For each approved one, hand off to the
   **`edit-your-kb`** skill (draft → approve → `upsert_kb_article` → verify). For gaps
   where you don't have the real answer (e.g. the WiFi password), ask the user for it —
   never invent it.

## Rules

- **Read-only until approved.** Analysis never writes to the KB on its own.
- **Never invent facts.** If a gap needs a value you don't have, ask the user.
- **Respect privacy.** Transcripts may be marked `contains_pii`. Quote only the question
  being asked; never surface guest names, phone numbers, or reservation details in the
  report.
- **Stay in scope.** You fix *missing facts*. Greetings, escalation rules, door-code
  logic, and phrasing are bot behavior — route those to the Enhanced Rentals team.
- **Be concrete and brief.** The user wants "here are 4 things guests keep asking that
  your receptionist can't answer, and here's how to fix them" — not a wall of transcript
  text.

## Example

> **User:** "What is my receptionist missing?"
>
> 1. `ping` → "Scanning the Riverside Motel workspace."
> 2. `list_failure_flags` + last 50 `list_transcripts`.
> 3. Notice 4 calls where guests asked about a fridge and the bot said it didn't know;
>    2 asking about boat parking; 1 angry booking-change transfer.
> 4. Classify: fridge = knowledge gap, boat parking = knowledge gap, booking change =
>    legitimate transfer (drop).
> 5. `search_knowledge_base("fridge")` → nothing. `search_knowledge_base("parking")` →
>    a parking article that doesn't mention boats.
> 6. Report the two gaps in a table; note the parking article just needs a boat line.
> 7. User says "fix both" → hand to `edit-your-kb`: add "In-room fridge", update the
>    parking article to mention the side lot / boat parking. Verify each with a search.
