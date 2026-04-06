# Browser Extension Store Audit — Setup Guide

This folder contains `extension-store-audit.md`, a skill for Claude Code that runs a
full store readiness audit on your browser extension before you submit it to the
Chrome Web Store, Microsoft Edge Add-ons, and/or Firefox AMO.

It checks your icons, screenshots, privacy policy, manifest, and generates
ready-to-paste copy for every form field in each store's submission flow.

---

## What you need

- [Claude Code](https://claude.ai/code) installed (the CLI or desktop app)
- Your browser extension project open in Claude Code

---

## How to install the skill

You have two options:

### Option A — Global install (recommended)
Makes the skill available in **every** project on your machine.

Just tell Claude Code (in any conversation):

> "Please save the file `extension-store-audit.md` from this folder as a global Claude skill."

Claude will copy it to the right place automatically (`~/.claude/skills/`).

Or do it manually — copy `extension-store-audit.md` to:

| OS | Path |
|----|------|
| Windows | `C:\Users\YOUR_USERNAME\.claude\skills\` |
| macOS / Linux | `~/.claude/skills/` |

Create the `skills` folder if it doesn't exist yet.

---

### Option B — Project-only install
Makes the skill available **only inside this project**.

Tell Claude Code (from inside your extension project):

> "Please save the file `extension-store-audit.md` from this folder as a local Claude skill for this project."

Or manually copy it to `.claude/skills/` inside your project root (create the folders if needed).

---

## How to use the skill

Once installed, open your extension project in Claude Code and type:

```
/extension-store-audit
```

Claude will then:

1. Read your `manifest.json` automatically
2. Check all your icon and screenshot files against store requirements
3. Verify your privacy policy situation
4. Generate pre-filled copy for every form field across Chrome, Edge, and Firefox
5. Walk through a gotcha checklist for each store
6. Produce a final readiness report with any blockers flagged

You can also target specific stores:

```
/extension-store-audit — Chrome only
/extension-store-audit — skip Firefox
```

---

## What the skill covers

| Area | What it checks / generates |
|------|---------------------------|
| Icons | All required sizes per store, format, square ratio |
| Screenshots | Correct dimensions per store, min/max counts |
| Privacy policy | Whether one is needed, HTTPS check, Firefox manifest field |
| Store copy | Pre-fills name, description, search terms, testing notes for all 3 stores |
| Gotcha checklist | MV3 compliance, permission hygiene, keyword stuffing, source code submission (Firefox) |
| Website readiness | HTTPS, privacy policy linked, mobile responsive |
| Final report | ✓/✗ summary, blockers vs. warnings, submission readiness verdict |

---

## Notes

- The skill reads your actual project files — it's not a generic template. The generated
  copy is based on your real manifest, permissions, and extension behaviour.
- Nothing is submitted automatically. Everything is drafted for you to review and paste.
- If you find issues or want to improve the skill, edit the `.md` file directly — it's
  just a text file with instructions for Claude.
