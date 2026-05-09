# AI Design System Starter

> From the **AI Design System: The Know-How** masterclass

Working project + skill + prompts from the session. Clone it, connect your Figma file, and keep building where we left off.

---

## Prerequisites

- [Claude Code](https://claude.ai/claude-code) installed
- Figma account with Edit access
- [Figma MCP](https://github.com/anthropics/claude-code) connected in Claude Code

## Setup (5 minutes)

### 1. Create your copy

Click **"Use this template"** on GitHub to create your own repo, or download the ZIP.

### 2. Get the Figma file

Open the session Figma file (link provided in the session). Click **Duplicate to your drafts**. Copy the file key from the URL — it's the alphanumeric string after `/design/`:

```
https://www.figma.com/design/AbCdEfGhIjKlMnOpQrStUv/...
                               ^^^^^^^^^^^^^^^^^^^^^^
                               this is the file key
```

### 3. Configure your project

Open `CLAUDE.md` and replace every `[YOUR_FIGMA_FILE_KEY]` with your key.

### 4. Verify the connection

Open Claude Code in this directory and ask:

```
What pages exist in my Figma file?
```

You should see: Utility, Foundations, Screens, Button, Jumbotron, Artwork, Screens with components.

---

## Picking up where the session left off

### Part 1 exercises — Tokens

The `foundations/` and `tokens/` folders contain the token system we built. Try these prompts from `docs/prompts.md`:

- "Add the `hover` state to Semantic: Colors across all roles"
- "Add a Dark mode to Semantic: Colors"

### Part 2 exercises — Specs & Skills

The `components/atoms/button/` folder has the Button we documented. Try these:

- **"Document the Button component"** — triggers the `component-doc-writer` skill
- **"Build an Input Field component with Size (sm/md/lg) and Type (default/outlined). Bind everything to semantic tokens."**

### Going further — Your own project

When you're ready to apply this to your own design system:

1. Copy `docs/CLAUDE.md.template` to your project root as `CLAUDE.md`
2. Fill in the 8 sections (start with identity, architecture, and 3 critical rules)
3. Copy `.claude/skills/` to your project
4. Read `docs/next-steps.md` for the 90-day adoption playbook

---

## What's in this repo

| Folder | Purpose |
|---|---|
| `CLAUDE.md` | Project context — the 8-section persistent memory file |
| `.claude/skills/` | Component documentation skill |
| `foundations/` | Primitive token definitions (colors, size, typography) |
| `tokens/` | Semantic token aliases (Light + Dark modes) |
| `components/` | Component implementations (Button example + empty tiers) |
| `docs/` | Prompts, mental models, templates, adoption playbook |
| `source/` | Consolidated DTCG token export (reference) |
| `guide/` | PDF starter guide |

## Skill included

| Skill | What it does | Trigger |
|---|---|---|
| `component-doc-writer` | Generates component documentation from live Figma data | "Document the Button component" |

## The five artifacts

These are what you built during the session — all included here as working examples:

1. **Primitive tokens** (`foundations/`) — raw color, size, and typography values
2. **Semantic tokens** (`tokens/`) — purpose-based aliases with Light and Dark modes
3. **Components** (`components/atoms/button/`) — Button with 18 variants, fully token-bound
4. **CLAUDE.md** — persistent project context (8 sections)
5. **Documentation skill** (`.claude/skills/component-doc-writer/`) — reusable spec generator

## The four-pillar model

Read `docs/four-pillars.md` — the mental model behind everything in the session:

```
TOKENS  →  SPECS  →  AUDIT  →  AUTOMATION
```

Each pillar earns the next. Skip any one and the rest collapse.

> **Discipline first. Automation second.**

---

## Links

- Starter guide PDF: `guide/starter-guide.pdf`
- All prompts: `docs/prompts.md`
- Adoption playbook: `docs/next-steps.md`
