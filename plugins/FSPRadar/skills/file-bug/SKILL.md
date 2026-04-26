---
name: file-bug
description: You MUST use this skill the moment a Future Of Sports teammate signals they want to report something broken, weird, or missing in the product — even if they don't say "file a bug." Triggers on: any screenshot/image of a UI error, glitch, crash, broken layout, or unexpected app state; any phrase like "this is broken", "doesn't work", "not working", "crashed", "weird", "log this", "report this", "ticket this", "add to linear", "file a bug", "log feedback", "feature request", "we should build", "I wish it would"; any frustrated venting about the product; any pasted Slack/email complaint about the product. Files the report into Linear with everything pivotable as native fields or labels (Type label, Priority native, Assignee native, Version label) — never in rich-text description. Asks the user warmly for whatever's missing — never with a checklist, always conversationally, one or two questions at a time.
---

# File a bug / feedback / feature request to Linear (Future Of Sports)

Your job: turn whatever the teammate just dumped at you (screenshot, vent, half-sentence, Slack paste) into a properly-formed Linear issue without making them think.

## Hard rules

1. **Never ask for everything at once.** Extract everything you can from their input first. Only ask for what's truly missing, one or two questions at a time, in plain warm English.
2. **Never file with placeholder values** like "TBD", "unknown", or "see screenshot". If a required field is missing, ask.
3. **Always file as a top-level issue** in the `Future Of Sports` team. Never as a sub-issue of any parent.
4. **Pivotable fields are NEVER in the rich-text description.** Everything you'd ever filter on (Type, Priority, Assignee, Version, Reporter) goes into native Linear fields or labels. The description is reserved for prose: steps, expected/actual, logs.
5. **Always reply with the issue URL** after filing, plus a one-liner about anything you guessed so they can correct you.

## Required schema — pivot-first design

Every field below is set as a native Linear property or a label so it can be filtered, grouped, and pivoted in Linear views. **Do not duplicate any of these in the description.**

| Field | Where it lives in Linear | How to fill it |
|---|---|---|
| **Title** | `title` (native) | Synthesize from input. Don't ask. |
| **Team** | `team: "Future Of Sports"` (native) | Hardcoded. Don't ask. |
| **Type** | Workspace label: `Bug` / `Feature Feedback` / `New Feature` | Infer; ask only if genuinely ambiguous. |
| **Priority** | `priority: 1\|2\|3\|4` (native) | Infer from impact, or ask. See scale below. |
| **Assignee** | `assignee` (native) | Ask if not stated. Common: `praful`, `anand`, `tabitha`. |
| **Version Affected** | Team label under the **`Version`** label group (e.g. `web-1842`, `ios-2.4.1`, `android-2.4.1`) | ASK if not provided. Then check if the label already exists; if not, create it under the `Version` group. See "Version label workflow" below. |
| **Reporter** | `createdBy` (native — automatic from the authenticated user) | Don't ask. Linear sets this from whoever is logged in via the MCP. |
| **Date filed** | `createdAt` (native — automatic) | Don't ask. |
| **Steps to Reproduce** | Description prose (Bug only) | Extract or ask. |
| **Expected vs Actual** | Description prose (Bug only) | Extract or ask. |
| **Logs / Notes** | Description prose | Paste log lines or note that a screenshot was attached. |

## Priority scale (team convention — memorize)

- **P0 → priority 1 (Urgent)** — show-stopper, core flow broken, **no workaround**.
- **P1 → priority 2 (High)** — show-stopper but a **workaround exists**.
- **P2 → priority 3 (Normal — Linear shows this as "Medium")** — trivial but visible to **~20%** of audience.
- **P3 → priority 4 (Low)** — minor / cosmetic / edge case.

## Type definitions

- **Bug** — something is broken or behaving wrong.
- **Feature Feedback** — feature works but needs polish / UX change / improvement.
- **New Feature** — net-new functionality not yet built.

## Version label workflow

Versions are labels under the `Version` label group on the Future Of Sports team. Naming convention: `<platform>-<version>` (no spaces, lowercase). Examples: `web-1842`, `ios-2.4.1`, `android-2.4.1`, `commit-a3f9c12`.

Before calling `save_issue`:

1. Get the version string from the teammate (e.g. "web build #1842").
2. Normalize to a label name (e.g. `web-1842`).
3. Call `list_issue_labels` filtered by team and name to check if it exists.
4. If it doesn't, call `create_issue_label` with `name: "<normalized>"`, `parent: "Version"`, `teamId: "b91b7ab6-69a0-45ae-b90c-c8f07d3c7db2"`.
5. Pass the label name in the `labels` array of `save_issue` alongside the Type label.

If the teammate gives multiple environment dimensions (e.g. "web build #1842 on Safari iPadOS 17.4"), only the **app version** becomes a label. The **OS / browser / device** goes in the description as a one-line `**Environment:**` prefix, since OS combos are too sparse to be useful as pivotable labels.

## How to ask warmly

When fields are missing, ask in plain English. Acknowledge what they gave you first. Examples:

- *"got it — quick one, what version of the app were you on? build number, commit sha, or app store version is fine"*
- *"thanks, that's enough to file. who should own this — anand or tabitha?"*
- *"want me to mark this P0 (totally broken, no workaround) or P1 (broken but you can work around it)?"*
- *"can you walk me through what you tapped right before it broke? rough is fine"*

NEVER say things like *"please provide: version, priority, assignee, steps..."* That's the failure mode.

## Description template

Description is **prose only** — no metadata. Use this shape for **Bug** type:

```markdown
**Environment:** <OS · browser · device, if relevant>

## Steps to Reproduce
1.
2.
3.

## Expected vs Actual
- Expected:
- Actual:

## Logs / Notes
- <urls, log lines, environmental observations, or note that a screenshot was attached>
```

For **Feature Feedback** / **New Feature**: skip the headings, write 1–3 paragraphs of prose. Keep it short.

## Workflow

1. **Extract** everything you can from the teammate's input (screenshot OCR, text, attached console logs).
2. **Identify gaps** in the schema. Prioritize asking for: Version → Steps (if Bug) → Priority → Assignee.
3. **Ask conversationally**, one or two questions per turn. Acknowledge each answer before the next question.
4. **Resolve the Version label** (look up or create under `Version` group) before filing.
5. **File** via the Linear MCP's `save_issue` tool (the exact tool name will look like `mcp__linear__save_issue` or `mcp__plugin_FSPRadar_linear__save_issue` depending on how Linear was registered — pick whichever is available) with:
   - `team: "Future Of Sports"`
   - `title`
   - `labels: [<TypeLabel>, <VersionLabel>]`
   - `priority` (1/2/3/4)
   - `assignee`
   - `description` (prose only — no metadata)
   If the Linear MCP is not yet authenticated (the call returns an auth error), call its `authenticate` tool first — Claude Code will guide the teammate through Linear OAuth in their browser — then retry.
6. **Confirm** to the teammate: *"filed as FUT-XXX → <url>"*. If you guessed at priority or type, flag it: *"marked it P1 since you mentioned a workaround — let me know if it's actually P0"*.

## When NOT to trigger

- Code-level bugs in a dev session that the teammate is actively debugging (they want help fixing, not filing). If unclear, ask: *"want me to file this in Linear or are we debugging it now?"*
- The teammate explicitly says they don't want to file it.
- The complaint isn't about the Future Of Sports product (e.g., they're venting about a third-party tool).
