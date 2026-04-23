# agent-family

> Build your own AI family system — a personal AI with identity, memory, and extensible children.

---

## What is this?

**agent-family** is an open template framework for building a family-structured AI system.

The system has two layers:

- **Family Core** — the parent. Defines the agent's identity, memory, skills, and rules. Manages and propagates everything to children.
- **Children** — independent child projects that connect to the parent. Each has its own purpose and backlog, but inherits skills and rules from the parent.

No code in the parent. No database. Files are memory.

---

## Parent–Child Structure

```
{username}_family/
├── {username}/                    # Family Core (parent)
│   ├── CLAUDE.md                  # Governance constitution
│   ├── data/persona/              # Who the user is, who the AI is
│   └── .claude/
│       ├── skills/                # Skills managed here, propagated to children
│       └── templates/             # Templates used to create child projects
└── {username}_children/
    ├── financial-planner/         # Child project
    └── telegram-bot/              # Child project
```

Family Core is the single source of truth for skills and rules.  
Children start from the parent's template and stay in sync through propagation skills.

---

## Child vs Plugin

A **child** is a personal project created from within Family Core using the `create-child` skill. Freely added and removed by the user.

A **plugin** is a child that has been promoted for public distribution — managed and deployed through agent-family. Plugins can start as children and be promoted via the `promote-to-plugin` skill, or be built as plugins from the start.

All plugins are children, but not all children are plugins.

---

## How Children Are Created

Children are created directly from the parent using the `create-child` skill — no manual setup needed.

The parent holds the templates used to scaffold each new child (under `.claude/templates/`).  
Modifying these lets you customize the default structure of all future children.

When creating a child, the agent asks whether you intend to distribute it publicly.  
If yes, it scaffolds a plugin-ready structure from the start.

```
# In Family Core, just ask:
Create a new child project.
```

---

## Propagation

Skills and rules flow between parent and children through two built-in skills.

| Skill | Direction | When to use |
|-------|-----------|-------------|
| `create-child` | Parent → new child | Create a new child project |
| `promote-to-plugin` | Child → agent-family | Promote a child to a distributable plugin |
| `sync-to-children` | Parent → children | Push updated skills or rules to children |
| `sync-to-core` | Child → Parent | Promote a child-level change back to the parent |

---

## Templates

| Template | Description |
|----------|-------------|
| [family-core](templates/family-core/) | Family Core — the parent project |
| [family-plugin](templates/family-plugin/) | Plugin — distributable child project |

---

## Quick Start

### 1. Create the Family Core

Use the [family-core](templates/family-core/) template.  
Name the repo `{username}` (e.g. `alice`).

### 2. Run the onboarding prompt

Open Claude Code in the repo and paste:

```
Set up my agent-family-core.

Ask me each of the following values one by one, then replace all placeholders and fill in the persona files interactively.

Required:
- username
- AI name
- Backlog prefix (2–3 uppercase letters, e.g. MY)
- Project purpose (one-line description)
- Language (default language for all responses and documents, e.g. English / 한국어 / 日本語)
```

> **Language is required.** The agent uses this as its default language for all responses and documents.

### 3. Create children from the parent

Once Family Core is set up, ask the agent to create child projects:

```
Create a new child project.
```

---

## Design Principles

| Principle | How |
|-----------|-----|
| Zero code in parent | Family Core is `.md` only. Code lives in children. |
| Files = memory | Markdown files are the agent's persistent state. No DB. |
| Skills = behavior | One `SKILL.md` = one reusable workflow. |
| Parent manages, children execute | Skills and rules flow down from parent to children. |
| Single source of truth | `CLAUDE.md` is the only governance document. |

---

## Auto-deployment

This repo uses GitHub Actions to automatically sync template folders to their respective GitHub Template repositories on every push to `main`.

```
push to agent-family (main)
  ├── templates/family-core/**   →  agent-family-core repo
  └── templates/family-plugin/** →  agent-family-plugin repo
```

### Setup

**1. Create two GitHub repos** (empty, no README)
- `agent-family-core`
- `agent-family-plugin`

Enable **Template repository** in each repo's Settings → General.

**2. Generate a Fine-grained PAT**

GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens

| Field | Value |
|-------|-------|
| Repository access | `agent-family-core`, `agent-family-plugin` |
| Contents permission | Read and write |

**3. Add secret to this repo**

`agent-family` → Settings → Secrets and variables → Actions → New repository secret

- Name: `DEPLOY_TOKEN`
- Value: the PAT from step 2

After this, any push that modifies `templates/` will automatically update the template repos.

---

## Repository Structure

```
agent-family/
├── .github/workflows/
│   └── deploy-templates.yml     # Auto-deployment to template repos
├── templates/
│   ├── family-core/             # Source for agent-family-core repo
│   └── family-plugin/           # Source for agent-family-plugin repo
└── docs/
    ├── 10_planning/             # Onboarding prompt design
    └── 20_spec/                 # Architecture specs
```

---

## License

MIT
