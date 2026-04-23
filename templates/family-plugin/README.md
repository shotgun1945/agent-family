# agent-family-plugin

> A **plugin** for the agent-family system — a distributable child project.

> [!NOTE]
> **Do not fork or edit this repo directly.**
> Copy the onboarding prompt below and paste it to your AI agent — it will set everything up for you.

[![Use this template](https://img.shields.io/badge/Use%20this%20template-2ea44f?style=for-the-badge)](../../generate)

---

## What is this?

A plugin is a child project designed to be distributed publicly — built to extend Family Core for anyone in the agent-family ecosystem.

**Child vs Plugin:**
- A **child** is a personal project created from within Family Core, used privately.
- A **plugin** is a child that has been promoted for public distribution, managed through agent-family.

Plugins can start as children and be promoted via the `promote-to-plugin` skill, or be built as plugins from the start.

---

## Quick Start

### 1. Create your repo

Click **"Use this template"** → name it after the plugin's role (e.g. `tele-agent`, `notion-sync`).

### 2. Place it next to your core

```
{username}_family/
├── {username}/                    # Family Core (parent)
└── {username}_children/
    └── {plugin-name}/             # This repo (plugin)
```

### 3. Open Claude Code

```bash
cd {username}_children/{plugin-name}
claude
```

### 4. Paste the onboarding prompt

```
Set up my agent-family-plugin.

Ask me each of the following values one by one, then replace all placeholders and initialize the project.

Required:
- Plugin name
- Backlog prefix (2–3 uppercase letters, e.g. TA)
- Purpose (one-line description)
- Core path (relative path to the Family Core repo, e.g. ../../{username})
```

---

## Folder Structure

```
{plugin-name}/
├── CLAUDE.md                      # Plugin governance
├── .claude/
│   ├── settings.json
│   └── skills/
│       └── lets-commit/SKILL.md
├── docs/
│   ├── README.md
│   ├── 00_backlog/
│   │   ├── backlog.md
│   │   └── backlog_done.md
│   └── 90_logs/
│       └── CHANGELOG.md           # Release notes
└── README.md                      # Public-facing usage guide
```

---

## Staying in Sync with the Parent

| Direction | Skill | When to use |
|-----------|-------|-------------|
| Parent → Plugin | `sync-to-children` | After updating skills or rules in the parent |
| Plugin → Parent | `sync-to-core` | When a plugin-level change should be promoted to the parent |

---

## Distribution

This plugin is managed through [agent-family](https://github.com/{owner}/agent-family).  
To register or update this plugin, open a PR to `plugins/registry.md` in the agent-family repo.

---

## License

MIT
