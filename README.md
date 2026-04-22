# agent-family

> Build your own AI family system — a personal AI with identity, memory, and extensible plugins.

---

## What is this?

**agent-family** is an open template framework for building a *family-structured AI system*:

- A **Family Core** — your AI agent's identity, memory, and behavior, defined entirely in Markdown
- **Plugins** — domain-specific child projects that extend the core (finance, messaging, etc.)

No code in the core. No database. Files are memory.

---

## How it works

```
{username}_family/
├── {username}/                  # Family Core — this agent's "brain"
│   ├── CLAUDE.md                # Governance constitution
│   ├── data/persona/            # Who the user is, who the AI is
│   └── .claude/skills/          # Reusable agent behaviors
└── {username}_children/
    ├── financial-planner/       # Core Plugin
    └── telegram-bot/            # Connector Plugin
```

The agent reads `CLAUDE.md` first, then persona files, then acts according to skills. No runtime required — just Claude Code (or Cursor).

---

## Templates

| Template | Description | Use for |
|----------|-------------|---------|
| [family-core](templates/family-core/) | Family Core template | Your AI agent's home base |
| [family-plugin](templates/family-plugin/) | Plugin template | Each domain-specific extension |

---

## Quick Start

### 1. Create the Family Core

Use the [family-core](templates/family-core/) template to create your core repo.  
Name it `{username}` (e.g. `alice`).

### 2. Run the onboarding prompt

Open the repo in Claude Code and paste:

```
Set up my agent-family-core.

Replace the placeholders with the values below,
then ask me questions to fill in the persona files.

- username: [your name]
- AI name: [agent name]
- Backlog prefix: [2–3 uppercase letters, e.g. MY]
- Project purpose: [one-line description]
- Language: [your preferred language, e.g. English / 한국어 / 日本語]
```

> **Language is required.** The agent uses this as its default language for all responses and documents.

The agent will replace placeholders and fill in your persona files interactively.

### 3. Add plugins (optional)

Use the [family-plugin](templates/family-plugin/) template for each new domain.  
Place it at `{username}_children/{plugin-name}/`.

---

## Design Principles

| Principle | How |
|-----------|-----|
| Zero code in core | Core is `.md` only. Code lives in plugins. |
| Files = memory | Markdown files are the agent's persistent state. No DB. |
| Skills = behavior | One `SKILL.md` = one reusable workflow. |
| Plug-and-play | Add or remove plugins without touching the core. |
| Single source of truth | `CLAUDE.md` is the only governance document. |

---

## Repository Structure

```
agent-family/
├── templates/
│   ├── family-core/             # Distributable Family Core template
│   └── family-plugin/           # Distributable Plugin template
└── docs/
    ├── 10_planning/             # Onboarding prompt design
    └── 20_spec/                 # Architecture specs
```

---

## License

MIT
