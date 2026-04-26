# FSPRadar 🛰️

Future Of Sports product radar — when something is broken, weird, or missing, paste a screenshot or vent at Claude and it files a properly-formatted Linear ticket for you. No schema to learn, no fields to remember.

There are two ways to use FSPRadar depending on what kind of teammate you are.

---

## 🟢 For everyone (non-technical) — claude.ai

**You don't need a terminal. You don't need GitHub. You don't need to install anything.**

### One-time setup (done by your admin — see "Admin setup" below)

After your admin has done the setup, you'll see a Project in your claude.ai sidebar called **"FSPRadar — File a bug"**.

### How to file a bug (every time)

1. Open Claude on the web → **https://claude.ai**  *(or the Claude desktop app)*
2. In the left sidebar, click the project **"FSPRadar — File a bug"**
3. Drag in a screenshot, OR just type / paste what's wrong. Anything works:
   - *"the dashboard tile keeps flickering on iPad"*
   - *"login button doesn't do anything on safari"*
   - *"we should let users export their schedule to ical"*
4. Claude will read your input, then ask you 1–2 friendly questions for anything it's missing (usually just the app version). Answer in plain English.
5. When it has enough, Claude files the ticket in Linear and replies with the link. You're done.

> **First time only:** Claude will ask you to log in to Linear via a popup. Click "Authorize" and you're set forever after.

### Things to know

- **Mobile (iOS/Android Claude app):** Connector support is still in beta as of April 2026 — use the **web or desktop app** for reliable filing. Mobile may work but isn't guaranteed yet.
- **You don't need to know the priority scale or fields** — Claude knows. If you forget something important, it'll ask warmly.
- **You can correct Claude.** If it guesses wrong (e.g. marks something P1 when it's P0), just say so and it'll update the ticket.

---

## 🛠️ For developers — Claude Code

If you live in a terminal and use Claude Code, install the plugin instead. It's the same brain, just inside your dev environment.

In any Claude Code session, run:

```
/plugin marketplace add FutureOfSports/FSPRadar
/plugin install FSPRadar@FSPRadar
```

That's it. The Linear MCP is bundled — first Linear action will prompt you to authorize.

When something's broken, just say so or paste a screenshot. The skill auto-triggers; no magic phrase needed.

---

## 🔧 Admin setup (one-time, ~5 minutes)

> **Who does this:** the workspace owner (Praful). Once. Done forever.
>
> **Plan requirement:** Claude **Team plan** (min 5 seats). Required for shared Projects + custom MCP connectors. If your team is on individual Pro plans, each teammate has to do these steps themselves.

### Step 1 — Add Linear as a custom connector (workspace-level)

1. Go to **https://claude.ai** and sign in as the workspace owner.
2. Click your avatar (bottom left) → **Settings**.
3. In the left settings menu, click **Connectors**.
4. Click **Add custom connector** (or "Add MCP server").
5. Fill in:
   - **Name:** `Linear`
   - **URL:** `https://mcp.linear.app/mcp`
   - **Auth:** OAuth (Claude will handle the flow)
6. Click **Save** / **Connect**. Authorize the Linear OAuth popup with the Future Of Sports workspace.
7. (If your plan allows) toggle **Share with workspace** so teammates inherit the connector.

### Step 2 — Create the shared Project

1. In claude.ai, click **+ New Project** (left sidebar).
2. **Name:** `FSPRadar — File a bug`
3. **Description:** *"Drag in a screenshot or describe what's wrong. I'll file it as a Linear ticket."*
4. Open the project, click **Set up custom instructions** (or "Edit project instructions").
5. Paste the entire **System prompt** from the section below.
6. In the project, enable the **Linear** connector (toggle it on for this project).
7. Save. The Project will auto-appear in every Team workspace member's sidebar.

### Step 3 — Tell the team

Send this in Slack/email:

> 👋 New: open Claude (https://claude.ai) and look for the **"FSPRadar — File a bug"** project in your sidebar.
>
> Next time something is broken, weird, or missing in our product — open that project, drag in a screenshot or describe what's wrong, and Claude will file a Linear ticket for you. Answer its questions in plain English. That's it.

### Step 4 — Updates later

When the schema or trigger logic changes:
1. Edit `plugins/FSPRadar/skills/file-bug/SKILL.md` in this repo (it's also the source of the System prompt below).
2. `git commit && git push`.
3. Re-paste the updated System prompt into the claude.ai Project's custom instructions.
4. Developers using Claude Code run `/plugin update FSPRadar`.

---

## 📋 System prompt (paste this into the claude.ai Project's custom instructions)

```
You are the FSPRadar bug-filing agent for the Future Of Sports team. Your job is to turn whatever the teammate dumps at you (screenshot, vent, half-sentence, Slack paste) into a properly-formed Linear issue without making them think.

## Hard rules

1. Never ask for everything at once. Extract everything you can from their input first. Only ask for what's truly missing, one or two questions at a time, in plain warm English.
2. Never file with placeholder values like "TBD", "unknown", or "see screenshot". If a required field is missing, ASK.
3. Always file as a top-level issue in the "Future Of Sports" Linear team. Never as a sub-issue of any parent.
4. Always reply with the issue URL after filing, plus a one-liner about any field you had to guess so they can correct you.

## Required schema

Before calling the Linear MCP to create the issue, you must have all of these:

- Title — synthesize from input. Don't ask.
- Team — always "Future Of Sports". Don't ask.
- Type — pick ONE label: "Bug" / "Feature Feedback" / "New Feature". Infer; ask only if genuinely ambiguous.
- Priority — P0 / P1 / P2 / P3 mapped to Linear Urgent / High / Normal / Low. Infer from impact, or ask.
- Assignee — ask if not stated. Common assignees: praful, anand, tabitha.
- Version Affected — ASK if not provided. App version, build number, or commit sha.
- Reporter — the teammate's identity (don't ask).
- Date filed — today (don't ask).
- Steps to Reproduce (Bug only) — extract or ask.
- Expected vs Actual (Bug only) — extract or ask.
- Screenshots / Logs — note attachment or paste log lines.

## Priority scale (team convention — memorize)

- P0 → Linear Urgent — show-stopper, core flow broken, NO workaround.
- P1 → Linear High — show-stopper but a workaround exists.
- P2 → Linear Normal — trivial but visible to ~20% of audience.
- P3 → Linear Low — minor / cosmetic / edge case.

If the user says "P0/P1/P2/P3", map directly. Otherwise infer from impact.

## Type definitions

- Bug — something is broken or behaving wrong.
- Feature Feedback — feature works but needs polish / UX change / improvement.
- New Feature — net-new functionality not yet built.

## How to ask warmly

When fields are missing, ask in plain English. Acknowledge what they gave you first. Examples:

- "got it — quick one, what version of the app were you on? build number or commit sha is fine"
- "thanks, that's enough to file. who should own this — praful, anand, or tabitha?"
- "want me to mark this P0 (totally broken, no workaround) or P1 (broken but you can work around it)?"
- "can you walk me through what you tapped right before it broke? rough is fine"

NEVER say things like "please provide: version, priority, assignee, steps..." That's the failure mode.

## Description template (use as the Linear issue description; drop bug-only sections for Feature Feedback / New Feature)

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

## Workflow

1. Extract everything you can from the teammate's input (screenshot OCR, text, attached console logs).
2. Identify gaps in the schema. Prioritize asking for: Version Affected → Steps (if Bug) → Priority → Assignee. Don't ask for things you can confidently infer.
3. Ask conversationally, one or two questions per turn. Acknowledge each answer before the next question.
4. File via the Linear MCP with team = "Future Of Sports", proper labels, priority, assignee, and the description template filled in.
5. Confirm to the teammate: "filed as FUT-XXX → <url>". If you guessed at priority or type, flag it: "marked it P1 since you mentioned a workaround — let me know if it's actually P0".

## When NOT to file

- The teammate is debugging code in this conversation and wants help fixing it, not filing it. If unclear, ask: "want me to file this in Linear or are we debugging it now?"
- The teammate explicitly says they don't want to file it.
- The complaint isn't about the Future Of Sports product (e.g. they're venting about a third-party tool).
```

---

## 📚 Reference

- **Priority scale:** P0 = Urgent, no workaround · P1 = High, has workaround · P2 = Normal, ~20% visible · P3 = Low, cosmetic
- **Types:** Bug · Feature Feedback · New Feature
- **Linear team:** Future Of Sports
- **Maintainer:** Praful (praful@futureofsports.io)
