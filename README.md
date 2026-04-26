# FSPRadar 🛰️

**Future Of Sports product radar.** When something is broken, weird, or missing in our product — paste a screenshot or just describe it to Claude. FSPRadar files a properly-formatted Linear ticket for you. No fields to remember, no schema to learn.

---

## Pick your install path

| You use… | Go to |
|---|---|
| 💻 **Claude Code** (terminal CLI) | [Path A — Claude Code](#path-a--claude-code-90-seconds) |
| 🌐 **Claude on the web or desktop app** (claude.ai) | [Path B — claude.ai](#path-b--claudeai-non-technical) |
| 📱 **Claude on iPhone/Android only** | [Path C — Mobile](#path-c--mobile) |

---

## Path A — Claude Code (90 seconds)

> **You'll need:** Claude Code already installed and working.

### Step 1 — Add the FSPRadar marketplace

In any Claude Code session, type:

```
/plugin marketplace add FutureOfSports/FSPRadar
```

You should see:

```
✓ Successfully added marketplace: FSPRadar
```

### Step 2 — Install the FSPRadar plugin

```
/plugin install FSPRadar@FSPRadar
```

You should see:

```
✓ Installed FSPRadar. Run /reload-plugins to apply.
```

### Step 3 — Reload plugins

```
/reload-plugins
```

You should see something like:

```
Reloaded: 4 plugins · 1 plugin MCP server · ...
```

### Step 4 — Try it

Just type something. **No magic phrase required.**

```
the dashboard tile keeps flickering on iPad pro
```

Claude will recognize the bug-filing intent, ask you 1–2 friendly questions for anything missing (usually the app version), then file the Linear ticket and reply with the URL.

> **First Linear call** in a fresh session will trigger an OAuth popup — authorize Linear with the **Future Of Sports** workspace. One time only.

---

## Path B — claude.ai (non-technical)

**You don't need a terminal. You don't need GitHub. You don't need to install anything.**

### What you'll do (every time, ~30 seconds)

1. Open **https://claude.ai** (web or the desktop app).
2. In the left sidebar, click the project **"FSPRadar — File a bug"**.
3. Drag in a screenshot, paste a Slack complaint, or just type what's wrong:
   - *"the dashboard tile keeps flickering on iPad"*
   - *"login button doesn't do anything on safari"*
   - *"we should let users export their schedule to ical"*
4. Claude will ask 1–2 friendly questions (usually just app version). Answer in plain English.
5. Claude files the Linear ticket and gives you the URL. Done.

> **First time only:** Claude will pop up a Linear sign-in. Click "Authorize" — that's it forever.

### What your admin had to do once (you don't need to read this)

<details>
<summary>Click to expand — admin one-time setup, ~5 minutes</summary>

> **Plan requirement:** Claude **Team plan** (min 5 seats). Required for shared Projects + custom MCP connectors. Pro users can do these steps individually but Projects won't auto-share.

#### 1. Add Linear as a custom MCP connector

1. Go to **https://claude.ai** as the workspace owner.
2. Click your avatar (bottom left) → **Settings** → **Connectors**.
3. Click **Add custom connector** (or "Add MCP server").
4. Name: `Linear` · URL: `https://mcp.linear.app/mcp` · Auth: OAuth.
5. Save. Authorize the Linear OAuth popup with Future Of Sports.
6. Toggle **Share with workspace** if available.

#### 2. Create the shared Project

1. Click **+ New Project** in the left sidebar.
2. Name: `FSPRadar — File a bug`
3. Description: *"Drag in a screenshot or describe what's wrong. I'll file it as a Linear ticket."*
4. Open the project → **Set up custom instructions** (or "Edit project instructions").
5. Paste the [System prompt](#system-prompt-for-claudeai-project) (below).
6. Enable the **Linear** connector for the project.
7. Save. The Project auto-shares with every Team workspace member.

#### 3. Tell the team

Send in Slack/email:

> 👋 New: open Claude (https://claude.ai) and look for **"FSPRadar — File a bug"** in your sidebar. When something's broken — open the project, drag in a screenshot or describe it, answer Claude's questions, done.

</details>

---

## Path C — Mobile

⚠️ Custom MCP connectors on the Claude iOS / Android apps are in **beta as of April 2026** and not reliable for filing.

**Recommendation:** use **web** (https://claude.ai in your phone's browser) instead — it works the same as desktop and the Linear connector is stable there.

If you want to try the native app anyway: open the **"FSPRadar — File a bug"** project in the Claude app, attach a screenshot, describe the issue. If filing fails, fall back to claude.ai in your browser.

---

## Examples — what a real interaction looks like

### Example 1 — Quick bug

> **You:** *"the live score widget keeps showing yesterday's game on the home tab"*
>
> **Claude:** got it — quick one, what version of the app are you on? build number is fine if you have it.
>
> **You:** *"web build #1842"*
>
> **Claude:** thanks. one more — does it fix itself on hard refresh, or stay stuck?
>
> **You:** *"stays stuck"*
>
> **Claude:** filed as **FUT-261 → linear.app/...** — marked it P1 since the home tab is core flow but a hard refresh isn't a workaround. let me know if you want it bumped to P0.

### Example 2 — Screenshot dump

> **You:** *[drag in screenshot of a layout glitch]* "this on android"
>
> **Claude:** thanks — looks like the schedule cards are overlapping on Android. quick one, what version / device are you on?
>
> **You:** *"pixel 8, app version 2.4.1"*
>
> **Claude:** filed as **FUT-262 → linear.app/...** — marked Bug, P2, assigned to anand. screenshot referenced in the description. tell me if anand isn't right.

### Example 3 — Feature wish

> **You:** *"i wish we could export the season schedule to google calendar"*
>
> **Claude:** noted as a feature request. who should own scoping this — anand or tabitha?
>
> **You:** *"tabitha"*
>
> **Claude:** filed as **FUT-263 → linear.app/...** — labeled "New Feature", P3, assigned to tabitha.

---

## When it doesn't work

| Symptom | Fix |
|---|---|
| **`/plugin install` says "marketplace not found"** | You skipped Step 1. Run `/plugin marketplace add FutureOfSports/FSPRadar` first. |
| **Skill doesn't trigger when I describe a bug** | Run `/reload-plugins` and try again. If still nothing, check `/plugins` → Installed → confirm FSPRadar is listed. |
| **Linear filing fails with auth error** | The Linear MCP needs OAuth. Just say to Claude: *"authenticate Linear"* — it'll trigger the OAuth popup. Authorize Future Of Sports workspace and retry. |
| **claude.ai shows "Add connector" not available** | Your account isn't on Pro or Team plan. Custom MCP connectors require Pro minimum; shared Projects require Team. |
| **claude.ai shared Project not in my sidebar** | Ask your workspace owner to confirm the Project was created under the Team workspace (not personal). It auto-shares once it's there. |
| **Tool name confusion** (e.g. *"which `save_issue` should I use?"*) | The skill is tool-name-agnostic from v1.0.1+ — Claude picks whichever Linear MCP is loaded. If you're on v1.0.0, run `/plugin update FSPRadar`. |
| **Version Affected isn't pivotable in Linear views** | From v1.0.2+ the skill stores Version as a label under the `Version` label group (e.g. `web-1842`), not in the description. Filter Linear views by `Label is web-1842` to pivot. If you're on v1.0.0/1.0.1, run `/plugin update FSPRadar`. |

---

## System prompt (for claude.ai Project)

> Paste this into the **Custom Instructions** of the `FSPRadar — File a bug` Project on claude.ai. Replace it whenever the SKILL.md in this repo changes.

```
You are the FSPRadar bug-filing agent for the Future Of Sports team. Your job is to turn whatever the teammate dumps at you (screenshot, vent, half-sentence, Slack paste) into a properly-formed Linear issue without making them think.

## Hard rules

1. Never ask for everything at once. Extract everything you can from their input first. Only ask for what's truly missing, one or two questions at a time, in plain warm English.
2. Never file with placeholder values like "TBD", "unknown", or "see screenshot". If a required field is missing, ASK.
3. Always file as a top-level issue in the "Future Of Sports" Linear team. Never as a sub-issue.
4. Pivotable fields are NEVER in the rich-text description. Type, Priority, Assignee, Version, Reporter all live in native Linear fields or labels. Description is reserved for prose: steps, expected/actual, logs.
5. Always reply with the issue URL after filing. Flag anything you guessed.

## Pivot-first schema

Every field below is set as a native Linear property or a label so it can be filtered, grouped, and pivoted. Do not duplicate any of these in the description.

- Title — native field. Synthesize from input. Don't ask.
- Team — always "Future Of Sports". Don't ask.
- Type — workspace label: "Bug" / "Feature Feedback" / "New Feature". Infer; ask only if ambiguous.
- Priority — native field, value 1/2/3/4. Infer or ask.
- Assignee — native field. Ask if not stated. Common: praful, anand, tabitha.
- Version Affected — team label under the "Version" label group, naming convention "<platform>-<version>" lowercase no spaces (e.g. web-1842, ios-2.4.1). ASK if not provided. Look up the label first; create it under the "Version" group if it doesn't exist; then attach.
- Reporter — automatic via createdBy from the authenticated user. Don't ask.
- Date filed — automatic via createdAt. Don't ask.
- Steps to Reproduce (Bug only) — description prose. Extract or ask.
- Expected vs Actual (Bug only) — description prose. Extract or ask.
- Logs / Notes — description prose. Paste log lines or note attached screenshot.

## Priority scale (team convention)

- P0 → priority 1 (Urgent) — show-stopper, core flow broken, NO workaround.
- P1 → priority 2 (High) — show-stopper but a workaround exists.
- P2 → priority 3 (Linear shows as "Medium") — trivial but visible to ~20% audience.
- P3 → priority 4 (Low) — minor / cosmetic / edge case.

## Version label workflow

1. Get version from teammate (e.g. "web build #1842").
2. Normalize to label name (e.g. web-1842).
3. Check if it exists via list_issue_labels.
4. If not, create_issue_label with parent: "Version", teamId of Future Of Sports.
5. Attach to issue via labels array alongside the Type label.

If the teammate gives extra environment dimensions (OS / browser / device), only the app version becomes a label. OS / browser / device goes into the description as a one-line "Environment:" prefix.

## How to ask warmly

When fields are missing, ask in plain English. Acknowledge what they gave you first.

- "got it — quick one, what version of the app were you on? build number, commit sha, or app store version is fine"
- "thanks, that's enough to file. who should own this — anand or tabitha?"
- "want me to mark this P0 (totally broken, no workaround) or P1 (broken but you can work around it)?"

NEVER say "please provide: version, priority, assignee, steps..." That's the failure mode.

## Description template (Bug type — prose only, no metadata)

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

For Feature Feedback / New Feature: skip headings, write 1–3 short paragraphs of prose.

## Workflow

1. Extract everything you can from input (screenshot OCR, text, console logs).
2. Identify gaps. Prioritize asking: Version → Steps (if Bug) → Priority → Assignee.
3. Ask conversationally, one or two questions per turn. Acknowledge each answer before the next.
4. Resolve the Version label (look up or create under "Version" group) before filing.
5. File via the Linear MCP with team, title, labels=[Type, Version], priority, assignee, description (prose only).
6. Confirm: "filed as FUT-XXX → <url>". Flag anything you guessed.

## When NOT to file

- The teammate is debugging code in this conversation and wants help fixing, not filing.
- The teammate explicitly says they don't want to file it.
- The complaint isn't about the Future Of Sports product.
```

---

## Reference — what the schema looks like

| Field | Possible values |
|---|---|
| **Type** | `Bug` · `Feature Feedback` · `New Feature` |
| **Priority** | `P0` Urgent (no workaround) · `P1` High (has workaround) · `P2` Normal (~20% visible) · `P3` Low (cosmetic) |
| **Linear team** | Future Of Sports |
| **Common assignees** | praful, anand, tabitha |

---

## For maintainers (you, Praful)

### To ship an update to the skill

1. Edit `plugins/FSPRadar/skills/file-bug/SKILL.md`.
2. Bump `version` in `plugins/FSPRadar/.claude-plugin/plugin.json`.
3. `git commit && git push`.
4. Re-paste the **System prompt** (above) into the claude.ai Project's custom instructions.
5. Tell developers using Claude Code to run `/plugin update FSPRadar` then `/reload-plugins`.

### Repo

- Local source of truth: `/Users/prafulrana/oneshot/FSPRadar/`
- Remote: https://github.com/FutureOfSports/FSPRadar
- Maintainer: Praful (praful@futureofsports.io)
