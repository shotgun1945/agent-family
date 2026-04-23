---
updated: 2026-04-23
---

# Plugin Registry

Official plugins for the agent-family system.  
Managed by the agent-family maintainer. To register a new plugin, open a PR to this file.

---

## How plugins are installed

The `install-plugin` skill reads this file via raw URL and lists available plugins.  
Selected plugins are cloned into `../{username}_children/{plugin-name}/`.

Raw URL: `https://raw.githubusercontent.com/shotgun1945/agent-family/main/plugins/registry.md`

---

## Available Plugins

| Name | Repo | Version | Description |
|------|------|---------|-------------|
| _(no plugins yet)_ | — | — | — |

---

## For Plugin Developers

To submit a plugin:
1. Create a public GitHub repo using `.claude/templates/plugin/` in `agent-family-core` as a starting point
2. Ensure `CLAUDE.md` has no hardcoded personal references — use relative paths and placeholders
3. Open a PR to this file adding a row to the table above
