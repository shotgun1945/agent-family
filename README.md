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
│   ├── data/persona/              # Who the user is, who the AI is
│   └── .claude/
│       ├── skills/                # Skills managed here, propagated to children
│       └── templates/             # Templates used to create child projects
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

Do the following steps in order:

1. Ask me for a family name (this becomes the folder and repo name).
2. Create the folder structure:
   - {family-name}_family/
   - {family-name}_family/{family-name}/        ← Family Core lives here
   - {family-name}_family/{family-name}_children/
3. Clone the Family Core template into {family-name}_family/{family-name}/:
   git clone https://github.com/shotgun1945/agent-family-core.git {family-name}_family/{family-name}
4. Ask me the following values one by one, then replace all placeholders:
   - AI name
   - Backlog prefix (2–3 uppercase letters, e.g. MY)
   - Language (e.g. English / 한국어 / 日本語)
5. Fill in the persona files interactively.
6. Fetch the plugin registry and ask which plugins I want to install:
   https://raw.githubusercontent.com/shotgun1945/agent-family/main/plugins/registry.md
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
git clone https://github.com/shotgun1945/agent-family-core.git {family-name}_family/{family-name}
```

**3. Open the core in your AI agent**

```bash
cd {family-name}_family/{family-name}
claude   # or open in Cursor / your preferred agent
```

**4. Paste the setup prompt**

```
Set up my Family Core.

Ask me the following values one by one, then replace all placeholders and fill in the persona files interactively.

Required:
- Family name
- AI name
- Backlog prefix (2–3 uppercase letters, e.g. MY)
- Language (e.g. English / 한국어 / 日本語)
```

---

## Skills

| Skill | Direction | Purpose |
|-------|-----------|---------|
| `create-child` | Parent → new child | Scaffold a new personal child project |
| `install-plugin` | Registry → children | Install an official plugin from the registry |
| `promote-to-plugin` | Child → community | Promote a child to a distributable plugin |
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
