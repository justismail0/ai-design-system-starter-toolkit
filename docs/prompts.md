# Prompts

Working prompts for each pillar — copy, paste, adapt. The `[BRACKETED]` parts are placeholders — replace them with your project's specifics before sending.

**Style note:** these are one-shot prompts for post-session readers who already understand the propose-then-execute discipline. The masterclass teaches that discipline through verbose multi-step prompts; here, you get the cleaner working version. If the agent's first output is wrong, redirect with a follow-up — see *Redirecting the agent* below.

---

## Pillar 1 — Tokens (visual context)

### Create a primitive color collection from a palette

```
Create a `Primitive: Colors` Variables collection from the color palette
in this Figma section [PALETTE_NODE_ID]. Group by hue. Use the naming
pattern `color/{hue}/{shade}`. Preserve shade numbering from the palette.
```

### Create a semantic layer that aliases primitives

```
Create a `Semantic: Colors` collection following the canvas at
[SEMANTIC_CANVAS_NODE_ID]. Trace every mapping from the canvas, alias
every value to Primitive: Colors (no hardcoded hex), use naming
{property}/{role}/{state}. Preserve the exact labels from the canvas —
do not normalize.
```

### Add a state across all roles

```
Add the `[STATE]` state to Semantic: Colors across all roles (text,
background, icon, border). Pairing logic must be coherent — explain
the pairing matrix in your response, then write the tokens.
```

### Add a mode (e.g., dark mode)

```
Add a Dark mode to Semantic: Colors. Cover every existing Light-mode
token. Pairing rules to follow: which grays anchor dark mode, whether
brand shifts, how subtle vs solid backgrounds invert, whether
focus/disabled states need different pairings in dark.
```

### Redirecting the agent (when output doesn't match intent)

```
You bound [SURFACE] to `[TOKEN_NAME]`. We want [DESIGN INTENT]. Re-examine —
is the token missing, or am I wrong about the intent?
```

---

## Pillar 2 — Specs (textual context)

### Bootstrap a CLAUDE.md from blank

```
/init
```

> When the agent asks what to document, answer with one sentence:
> *"Project anchor for [SYSTEM NAME] — [WHAT IT DOES]. Use the 8-section
> structure: identity, architecture, reference, conventions, critical
> rules, workflow patterns, file references, discovered rules."*

### Build a component using only your tokens

```
Build a [COMPONENT_NAME] component in Figma. Reference
tokens/design-tokens.json for available bindings. Variants:
[size × state × type or your matrix]. Bind every fill/border/text/icon
to a Semantic token. Build as a component set, hug-content sizing.
```

### Create a documentation skill

```
/skill-creator

Create a skill `component-documentation` that documents a component
from `components/{tier}/{name}/`. Read the tokens.json and CLAUDE.md.
Generate {name}.md with sections: metadata, overview, anatomy,
tokens used, variants/properties, states, code example,
cross-references. Stub judgment sections (when-to-use, do/don't)
with TODO. Do not invent tokens — flag missing as TODO.
```

### Invoke the skill

```
Document component [COMPONENT_NAME].
```

---

## Pillar 3 — Audit

### Build an audit script

```
Build a Node.js script at scripts/audit-tokens.js that audits this
project for token discipline. Rules:
- Raw hex / rgb / oklch in HTML or tokens.json (should reference variables)
- Raw px values where a semantic spacing token exists
- tokens.json references that don't resolve to a token in tokens/design-tokens.json
- Missing required fields in component frontmatter

Output format: markdown table with file, line, rule, severity. Exit 0
if clean, exit 1 if any errors. Run it against [TARGET_PATH] and show
me the output.
```

> See [`audit-tokens.starter.js`](audit-tokens.starter.js) for a working starting point.

---

## Pillar 4 — Automation

### Schedule the audit

```
Schedule the audit script at scripts/audit-tokens.js to run [CADENCE —
e.g., every Monday at 9 AM]. Output goes to audits/{date}.md. If the
script exits non-zero, [DESTINATION — e.g., open a GitHub issue tagged
`design-system/drift`]. Use the `schedule` skill.
```

### Extend with conditional alerts

```
Extend the audit's output handling. Currently it writes to
audits/{date}.md. Add: if violations exist, also [DESTINATION — e.g.,
open a GitHub issue with the report body, labeled
`design-system/drift`, assigned to me]. If no violations, do nothing
extra. Update the scheduled trigger to use this new flow.
```

---

## Anti-patterns: prompts to avoid

### ❌ Vague generation
```
Build me a button component
```
Why it fails: no addressable references, no constraints. The agent has to invent the variants, states, naming, bindings. You'll get a generic button that ignores your system.

### ❌ Mixing context with execution
```
We've been working on tokens this week and the brand is yellow and we want
a button now please make one with hover states
```
Why it fails: too much narrative, no addressable references. The agent guesses at "yellow" and "hover states" instead of binding to actual tokens.

### ❌ Implicit constraints
```
Add dark mode
```
Why it fails: no rules for pairing logic, no mention of preserving aliasing, no scope on which tokens. The agent invents conventions.

---

## Adapting these prompts

The pattern across every prompt:
1. **Address a real file or location** — `[NODE_ID]`, `[FILE_PATH]`, `[COMPONENT_NAME]`
2. **State the goal in one sentence** — what should exist after this runs
3. **Specify constraints** — naming patterns, scopes, what to preserve, what NOT to invent
4. **Name the format** — output destination, file structure, exit codes

If your prompt is missing any of these, the agent will hallucinate the gap. Redirect with the *Redirecting the agent* prompt above when output drifts from intent — that's how the discipline shows up in pack-style usage.
