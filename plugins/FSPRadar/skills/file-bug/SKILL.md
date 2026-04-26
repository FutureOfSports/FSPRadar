---
name: file-bug
description: You MUST use this skill the moment a Future Of Sports teammate signals they want to report something broken, weird, or missing in the product — even if they don't say "file a bug." Triggers on: any screenshot/image of a UI error, glitch, crash, broken layout, or unexpected app state; any phrase like "this is broken", "doesn't work", "not working", "crashed", "weird", "log this", "report this", "ticket this", "add to linear", "file a bug", "log feedback", "feature request", "we should build", "I wish it would"; any frustrated venting about the product; any pasted Slack/email complaint about the product. Files the report into Linear with the team's required schema (Version Affected, Type, Priority P0–P3, Assignee, Reporter, Steps, Expected vs Actual) and asks the user warmly for whatever's missing — never with a checklist, always conversationally, one or two questions at a time.
---

# File a bug / feedback / feature request to Linear (Future Of Sports)

Your job: turn whatever the teammate just dumped at you (screenshot, vent, half-sentence, Slack paste) into a properly-formed Linear issue without making them think.

## Hard rules

1. **Never ask for everything at once.** Extract everything you can from their input first. Only ask for what's truly missing, one or two questions at a time, in plain warm English.
2. **Never file with placeholder values** like "TBD", "unknown", or "see screenshot". If a required field is missing, ask.
3. **Always file as a top-level issue** in the `Future Of Sports` team. Never as a sub-issue of any parent.
4. **Always reply with the issue URL** after filing, plus a one-liner about any field you had to guess so they can correct you.

## Required schema

Before calling the Linear MCP's `save_issue` tool (the exact tool name will look like `mcp__linear__save_issue` or `mcp__plugin_FSPRadar_linear__save_issue` depending on how Linear was registered — pick whichever is available), you must have all of these:

| Field | Linear arg | Source |
|---|---|---|
| Title | `title` | Synthesize from input. Don't ask. |
| Team | `team: "Future Of Sports"` | Hardcoded. Don't ask. |
| Type | `labels: ["Bug"]` / `["Feature Feedback"]` / `["New Feature"]` | Infer; ask only if genuinely ambiguous. |
| Priority | `priority`: 1 / 2 / 3 / 4 | Infer from impact, or ask. See scale below. |
| Assignee | `assignee` | Ask if not stated. Common: `praful`, `anand`, `tabitha`. |
| Version Affected | In description | **ASK** if not provided. App version / build # / commit sha. |
| Reporter | In description | The teammate's name (from their Linear identity). Don't ask. |
| Date filed | In description | Today. Don't ask. |
| Steps to Reproduce | In description (Bug only) | Extract or ask. |
| Expected vs Actual | In description (Bug only) | Extract or ask. |
| Screenshots / Logs | In description / `links` | Extract / note attachment. |

## Priority scale (team convention — memorize this)

- **P0 → priority 1 (Urgent)** — show-stopper, core flow broken, **no workaround**.
- **P1 → priority 2 (High)** — show-stopper but a **workaround exists**.
- **P2 → priority 3 (Normal)** — trivial but visible to **~20%** of audience.
- **P3 → priority 4 (Low)** — minor / cosmetic / edge case.

## Type definitions

- **Bug** — something is broken or behaving wrong.
- **Feature Feedback** — feature works but needs polish / UX change / improvement.
- **New Feature** — net-new functionality not yet built.

## How to ask warmly

When fields are missing, ask in plain English. Acknowledge what they gave you first. Examples:

- *"got it — quick one, what version of the app were you on? build number or commit sha is fine"*
- *"thanks, that's enough to file. who should own this — praful, anand, or tabitha?"*
- *"want me to mark this P0 (totally broken, no workaround) or P1 (broken but you can work around it)?"*
- *"can you walk me through what you tapped right before it broke? rough is fine"*

NEVER say things like *"please provide: version, priority, assignee, steps..."* That's the failure mode.

## Description template

When you call `save_issue`, the `description` field must follow this shape (drop the bug-only sections for Feature Feedback / New Feature):

```markdown
## Version Affected
<value>

## Type
<Bug | Feature Feedback | New Feature>

## Priority
<P0 | P1 | P2 | P3>

## Reporter
@<teammate name>  ·  Date filed: YYYY-MM-DD

## Steps to Reproduce
1.
2.
3.

## Expected vs Actual
- Expected:
- Actual:

## Screenshots / Logs / Links
- <urls, log lines, or note that a screenshot was attached>
```

## Workflow

1. **Extract** everything you can from the teammate's input (screenshot OCR, text, attached console logs).
2. **Identify gaps** in the schema. Prioritize asking for: Version Affected → Steps (if Bug) → Priority → Assignee. Don't ask for things you can confidently infer.
3. **Ask conversationally**, one or two questions per turn. Acknowledge each answer before the next question.
4. **File** via the Linear MCP's `save_issue` tool with team = "Future Of Sports", proper labels, priority, assignee, and the description template filled in. If the Linear MCP is not yet authenticated (the call returns an auth error), call its `authenticate` tool first — Claude Code will guide the teammate through Linear OAuth in their browser — then retry.
5. **Confirm** to the teammate: *"filed as FUT-XXX → <url>"*. If you guessed at priority or type, flag it: *"marked it P1 since you mentioned a workaround — let me know if it's actually P0"*.

## When NOT to trigger

- Code-level bugs in a dev session that the teammate is actively debugging (they want help fixing, not filing). If unclear, ask: *"want me to file this in Linear or are we debugging it now?"*
- The teammate explicitly says they don't want to file it.
- The complaint isn't about the Future Of Sports product (e.g., they're venting about a third-party tool).
