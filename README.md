# agent-family

> Build your own AI family system — a personal AI with identity, memory, and extensible children.

---

## What is this?

**agent-family** is an open template framework for building a family-structured AI system.

The system has two layers:

- **Family Core** — the parent. Defines the agent's identity, memory, skills, and rules. Manages and propagates everything to children.
- **Children** — independent child projects that connect to the parent. Each has its own purpose and backlog, but inherits skills and rules from the parent.

---

## Parent–Child Structure

```
{family-name}_family/
├── {family-name}/                 # Family Core (parent)
│   ├── CLAUDE.md                  # Governance constitution
│   ├── AGENTS.md                  # Agent compatibility entrypoint
│   ├── data/
│   │   ├── persona/               # Who the user is, who the AI is
│   │   └── templates/             # Templates used to create child projects
│   ├── .claude/skills/            # Skills managed here, propagated to children
│   └── .agents/skills/            # Thin wrappers for broad agent compatibility
└── {family-name}_children/
    ├── financial-planner/         # Child project (personal)
    └── telegram-bot/              # Plugin (installed from registry)
```

Family Core is the single source of truth for skills and rules.  
Children start from the parent's template and stay in sync through propagation skills.

---

## Child vs Plugin

A **child** is a personal project created locally from within Family Core using the `create-child` skill.

A **plugin** is an official project installed from the [plugin registry](plugins/registry.md) via the `install-plugin` skill. Plugins are independent Git repositories maintained by the community.

All plugins are children, but not all children are plugins.

---

## Installation

### Option A — Automatic (recommended)

Open any empty folder in your AI agent (Claude Code, Cursor, etc.) and paste this prompt:

```
Set up my agent-family system.

Step 1: Ask me which language to use (e.g. English / 한국어 / 日本語). Use that language for all output from this point on.

Step 2: Once I answer, show me this form and wait for me to fill it in and send it back:

---
Family name (becomes the folder and repo name):
AI name:
Backlog prefix (2–3 uppercase letters, e.g. MY):
---

Step 3: After I submit the form, do the following:
1. Create the folder structure:
   - {family-name}_family/
   - {family-name}_family/{family-name}/        ← Family Core lives here
   - {family-name}_family/{family-name}_children/
2. Download the Family Core template (no git history attached):
   git clone --depth 1 https://github.com/shotgun1945/agent-family-core.git {family-name}_family/{family-name}
   rm -rf {family-name}_family/{family-name}/.git
3. Replace all placeholders in the cloned files. Use today's date for {SETUP_DATE} (YYYY-MM-DD).
   Files to update:
   - README.md → {family-name}
   - CLAUDE.md → {SETUP_DATE}, {username}, {LANGUAGE}, {AI_NAME}, {BACKLOG_PREFIX}
   - data/persona/assistant_persona.md → {SETUP_DATE}, {AI_NAME}, {username}
   - data/persona/profile.md → {SETUP_DATE}
   - data/persona/preferences.md → {SETUP_DATE}
   - data/persona/personality.md → {SETUP_DATE}
   - data/persona/update_rules.md → {SETUP_DATE}
   - data/children_manifest.md → {SETUP_DATE}, {username}
   - data/children_registry.md → {SETUP_DATE}, {username}
   - docs/README.md → {SETUP_DATE}
   - docs/00_backlog/backlog.md → {SETUP_DATE}, {BACKLOG_PREFIX}
   - docs/00_backlog/backlog_done.md → {SETUP_DATE}
   Note: {username} and {family-name} = family name from the form.
4. Fetch the plugin registry and ask which plugins I want to install:
   https://raw.githubusercontent.com/shotgun1945/agent-family/main/plugins/registry.md
5. Tell me to open {family-name}_family/{family-name}/ in my AI agent and start fresh from there.
```

---

### Option B — Manual

**1. Create the folder structure**

```bash
mkdir -p {family-name}_family/{family-name}
mkdir -p {family-name}_family/{family-name}_children
```

**2. Clone Family Core into it**

```bash
git clone --depth 1 https://github.com/shotgun1945/agent-family-core.git {family-name}_family/{family-name}
rm -rf {family-name}_family/{family-name}/.git
```

**3. Open the core in your AI agent**

```bash
cd {family-name}_family/{family-name}
claude   # or open in Cursor / your preferred agent
```

**4. Paste the setup prompt**

```
Set up my Family Core.

Step 1: Ask me which language to use (e.g. English / 한국어 / 日本語). Use that language for all output from this point on.

Step 2: Once I answer, show me this form and wait for me to fill it in and send it back:

---
Family name (becomes {username} in all files):
AI name:
Backlog prefix (2–3 uppercase letters, e.g. MY):
---

Step 3: After I submit the form, replace all placeholders. Use today's date for {SETUP_DATE} (YYYY-MM-DD).
Files to update:
- README.md → {family-name}
- CLAUDE.md → {SETUP_DATE}, {username}, {LANGUAGE}, {AI_NAME}, {BACKLOG_PREFIX}
- data/persona/assistant_persona.md → {SETUP_DATE}, {AI_NAME}, {username}
- data/persona/profile.md → {SETUP_DATE}
- data/persona/preferences.md → {SETUP_DATE}
- data/persona/personality.md → {SETUP_DATE}
- data/persona/update_rules.md → {SETUP_DATE}
- data/children_manifest.md → {SETUP_DATE}, {username}
- data/children_registry.md → {SETUP_DATE}, {username}
- docs/README.md → {SETUP_DATE}
- docs/00_backlog/backlog.md → {SETUP_DATE}, {BACKLOG_PREFIX}
- docs/00_backlog/backlog_done.md → {SETUP_DATE}
Note: {username} and {family-name} = family name from the form.

After setup, optionally ask if I want to set up my persona (profile, preferences, personality).
```

---

## Skills

| Skill | Direction | Purpose |
|-------|-----------|---------|
| `create-child` | Parent → new child | Scaffold a new child project |
| `sync-to-children` | Parent → children | Push updated skills or rules to children |
| `sync-to-core` | Child → Parent | Promote a child-level change back to the parent |

---

## Plugin Registry

Available plugins are listed in [plugins/registry.md](plugins/registry.md).

To install a plugin, ask the agent:
```
Install a plugin.
```

---

## For Plugin Developers

Want to build a plugin for the agent-family ecosystem?

1. Use `.claude/templates/plugin/` inside `agent-family-core` as your starting point
2. Build your plugin as a public GitHub repo
3. Submit a PR to [plugins/registry.md](plugins/registry.md) to register it

---

## Design Principles

| Principle | How |
|-----------|-----|
| Files = memory | Markdown files are the agent's persistent state. No DB. |
| Skills = behavior | One `SKILL.md` = one reusable workflow. |
| Parent manages, children execute | Skills and rules flow down from parent to children. |
| Single source of truth | `CLAUDE.md` is the only governance document. |

---

## Auto-deployment

This repo uses GitHub Actions to automatically sync `templates/family-core/` to [agent-family-core](https://github.com/shotgun1945/agent-family-core) on every push to `main`.

### Setup

**1. Create a GitHub repo** (empty, no README)
- `agent-family-core`

Enable **Template repository** in the repo's Settings → General.

**2. Generate a Fine-grained PAT**

GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens

| Field | Value |
|-------|-------|
| Repository access | `agent-family-core` |
| Contents permission | Read and write |

**3. Add secret to this repo**

`agent-family` → Settings → Secrets and variables → Actions → New repository secret

- Name: `DEPLOY_TOKEN`
- Value: the PAT from step 2

After this, any push that modifies `templates/family-core/` will automatically update the template repo.

---

## Repository Structure

```
agent-family/
├── .github/workflows/
│   └── deploy-templates.yml     # Auto-deployment to agent-family-core
├── templates/
│   └── family-core/             # Source for agent-family-core repo
├── plugins/
│   └── registry.md              # Official plugin list
└── docs/
    ├── 10_planning/             # Onboarding prompt design
    └── 20_spec/                 # Architecture specs
```

---

## License

MIT
