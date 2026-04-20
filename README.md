# Xtropy Google Ads — Claude Code Plugin

Private plugin for Xtropy campaign managers. Adds 19 slash commands, 8 methodology skills, and a navigation guide for the hosted Google Ads MCP at `https://mcp.trafficarchitect.com/mcp`.

> **This repo contains the client-side plugin only.** The server (FastMCP + Google Ads API + Drive + Gmail pipeline) is private. If you're an Xtropy campaign manager, your onboarding contact (Hamid) will give you an API key.

---

## Install

You need two things: the MCP connection (with your personal bearer token) and the plugin files (commands + skills + nav guide). Either path below works.

### Option A — Plugin install (newer Claude Code / Cowork)

```
/plugin marketplace add Alitaliani/xtropy-google-ads
/plugin install google-ads@xtropy
```

Then add the hosted MCP connection with your key:

```
claude mcp add --transport http google-ads https://mcp.trafficarchitect.com/mcp \
  --header "Authorization: Bearer YOUR_KEY"
```

### Option B — Setup wizard (works everywhere, including older Claude Code)

First, add the MCP connection:

```
claude mcp add --transport http google-ads https://mcp.trafficarchitect.com/mcp \
  --header "Authorization: Bearer YOUR_KEY"
```

Then in Claude Code / Cowork, type literally:

```
setup-xtropy-mcp
```

The MCP's `setup_xtropy_mcp` tool returns a file manifest; Claude curls this repo's `commands/`, `skills/`, and `CLAUDE.md` into `~/.claude/` automatically.

---

## What you get

- **19 slash commands** under `~/.claude/commands/google-ads/` — autocomplete with `/google-ads:…`
- **8 methodology skills** under `~/.claude/skills/` — auto-activate on strategic questions (campaign structure, optimization, reporting, scheduling, feedback, etc.)
- **`~/.claude/CLAUDE.md`** navigation guide — Claude knows the toolkit from turn 1, no discovery round-trips

### Commands (19)

**Discovery**: `list-accounts`, `list-campaigns`, `list-conversions`
**Reports**: `sem-report`, `generate-report`, `schedule-report`, `schedule-weekly-reports`, `list-weekly-reports`, `cancel-weekly-reports`
**Audits**: `audit`, `performance-review`, `landing-page-audit`, `competitive-audit`, `bidding-audit`
**Optimization**: `optimize-keywords`, `keyword-research`, `audience-analysis`
**Setup**: `setup`, `status`

### Skills (8)

`strategy/`, `campaigns/`, `optimization/`, `audiences/`, `tracking/`, `reporting/`, `scheduling/`, `feedback/`

---

## Scheduling model (end users)

When you schedule a recurring report via `/google-ads:schedule-weekly-reports`, the `.xlsx` files download straight to a folder on your computer. No Google Drive. No email. The schedule runs client-side in your own Claude Code / Cowork — if your laptop is off at the scheduled time, the run skips that cycle.

Operator (Xtropy) runs its own weekly batch via a separate local-stdio path that does upload to Drive and email the team. End-user schedules never touch that path.

---

## Reporting issues

Message Hamid (hamid@trafficarchitect.com) or open an issue on this repo. Please include:
- Your username (so he can look up your activity in logs)
- The full command you ran
- The error Claude showed you

Don't include your API key in any issue — if you think it's leaked, ask Hamid for a rotation instead.

---

## License

MIT. See `LICENSE`.
