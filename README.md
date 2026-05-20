# Paris Weekly Events Newsletter 🗼

An automated weekly newsletter that curates events happening in Paris, delivered every Monday at 7 AM Paris time as a Slack DM, a Gmail draft, and a live filterable dashboard.

**Live dashboard:** https://rasalghulsslayer.github.io/paris-weekly-planner/

---

## How it works

A [Claude Code routine](https://claude.ai/code/routines) runs every Monday at `0 5 * * 1` UTC (= 7 AM Paris). On each run, the remote agent:

1. **Discovers events** via `WebSearch` + `WebFetch` across 10 categories: Pop-ups, Sports, Live Music, Food/Dining, Cinema, Theater, Comedy, Dance, Exhibitions, Galleries.
2. **Curates** ~10–30 events with strict anti-hallucination rules: only real, sourced events. If no events are found, the routine reports 0 honestly rather than inventing content.
3. **Generates** `docs/index.html` — a static dashboard with color-coded cards, category filter buttons, chronological sort, and mobile-responsive layout.
4. **Commits** to `main`. GitHub Pages serves `/docs` automatically (~30 s build time).
5. **Notifies**:
   - Slack DM (self-DM) with top 3 picks + dashboard link
   - Gmail draft to `akadosthomas@gmail.com` with the full summary

## Architecture

```
┌─ Monday 5 AM UTC ──────────────────────────────────────────┐
│                                                            │
│  Claude Code routine (claude-sonnet-4-6)                   │
│  ├─ WebSearch / WebFetch  → discover real events           │
│  ├─ Write docs/index.html → dashboard                      │
│  ├─ git push origin main  → via Claude GitHub App          │
│  ├─ Slack DM via MCP      → top 3 picks                    │
│  └─ Gmail draft via MCP   → full summary                   │
│                                                            │
└────────────────────────────────────────────────────────────┘
                          │
                          ▼
        GitHub Pages (main/docs) rebuilds in ~30 s
                          │
                          ▼
        https://rasalghulsslayer.github.io/paris-weekly-planner/
```

## Categories & colors

| Category       | Color     |
| -------------- | --------- |
| 🎪 Pop-ups     | `#FF6B6B` |
| ⚽ Sports      | `#4ECDC4` |
| 🎵 Live Music  | `#FFE66D` |
| 🍽️ Food/Dining | `#FF9FF3` |
| 🎬 Cinema      | `#54A0FF` |
| 🎭 Theater     | `#A29BFE` |
| 😂 Comedy      | `#6C5CE7` |
| 💃 Dance       | `#00B894` |
| 🖼️ Exhibitions | `#FDCB6E` |
| 🎨 Galleries   | `#E17055` |

## Setup (one-time)

Already done — kept here as a runbook in case the routine needs to be rebuilt.

1. **Install the Claude GitHub App** at https://github.com/apps/claude on this repo with **Contents: Read and write**. Without this, the routine's `git push` fails with `HTTP 403 "Resource not accessible by integration"`.
2. **Enable GitHub Pages**: source = `main`, path = `/docs`.
3. **Create the routine** at https://claude.ai/code/routines with cron `0 5 * * 1`, repo source set to this repo, MCP connections to Slack + Gmail, and `allowed_tools` including `Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch`.
4. **Routine ID:** `trig_018z5SMyYf2SG5fzVqE1kAZh` (manage at https://claude.ai/code/routines/trig_018z5SMyYf2SG5fzVqE1kAZh).

## Manual run

To trigger a run outside the Monday cron, open the routine in https://claude.ai/code/routines and click **Run now**.

## Troubleshooting

**Dashboard shows last week's events**
- Check that the routine actually ran: routine page → "Last fired at".
- Check `git log origin/main` — if the most recent commit isn't from "Paris Weekly Routine", the push failed.
- Most common cause: Claude GitHub App lost write access. Reinstall/reauthorize it at https://github.com/settings/installations.

**Slack DM didn't arrive**
- Messages go to your **self-DM**, not a channel. Open Slack → Direct messages → yourself.
- Check the routine's run log for Slack errors.

**Gmail email "didn't send"**
- The Gmail MCP integration only supports **draft creation**, not send. Open Gmail → Drafts. To auto-send, you'd need a separate mechanism (e.g. a Google Apps Script that sends matching drafts on a schedule).

**Hallucinated events**
- Prior runs occasionally invented events when all sources failed. The current prompt has an explicit anti-hallucination guardrail. If you see fake events again, check the routine prompt at https://claude.ai/code/routines/trig_018z5SMyYf2SG5fzVqE1kAZh and confirm the "⚠️ CRITICAL ANTI-HALLUCINATION RULE" section is still present.

## Files

- `docs/index.html` — the live dashboard. Overwritten by the routine each Monday.
- `README.md` — this file.

## Contact

Issues or feedback: thomas.dos@nomadic-labs.com
