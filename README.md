# FSPRadar

Future Of Sports product radar — internal Claude Code plugin that catches bugs, feature feedback, and feature requests the moment a teammate notices them.

## Install (teammates — run these once)

In a Claude Code session, type:

```
/plugin marketplace add FutureOfSports/FSPRadar
/plugin install FSPRadar@FSPRadar
```

That's it. The Linear MCP server is bundled, so the only extra step is authenticating to Linear the first time you use it (Claude will walk you through it).

## What it does

Paste a screenshot of something broken, vent about a glitch, or share a Slack complaint at your Claude — FSPRadar auto-detects the intent (no magic phrases needed) and:

1. Extracts everything it can from your input — version, steps, error logs from screenshots, etc.
2. Asks warmly for whatever's missing, **one or two questions at a time**, conversationally.
3. Files the issue in the `Future Of Sports` Linear team with the right labels, priority, assignee, and a structured description.
4. Replies with the issue URL and flags any field it had to guess so you can correct.

You never need to learn the schema. Your Claude knows it.

### Priority scale

- **P0** — show-stopper, no workaround
- **P1** — show-stopper but you can work around it
- **P2** — trivial but visible to ~20% of users
- **P3** — minor / cosmetic

### Type labels

- **Bug** — something is broken
- **Feature Feedback** — feature works but needs polish
- **New Feature** — net-new thing we should build

## Updates

```
/plugin update FSPRadar
```

(Or your Claude Code may auto-update depending on your settings.)

## For maintainers

To ship an update:

1. Edit `plugins/FSPRadar/skills/file-bug/SKILL.md`.
2. Bump `version` in `plugins/FSPRadar/.claude-plugin/plugin.json` if behavior changes.
3. `git commit && git push`.
4. Teammates pull via `/plugin update FSPRadar`.
