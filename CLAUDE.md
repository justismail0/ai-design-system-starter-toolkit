# CLAUDE.md

Project context for Claude Code working in this design-system starter kit.
Single Figma file `[YOUR_FIGMA_FILE_KEY]` holds every token, component,
and screen.

> **Setup:** Duplicate the session Figma file to your Drafts. Copy the
> file key from the URL (the string after `/design/`). Replace
> `[YOUR_FIGMA_FILE_KEY]` everywhere in this file with your key.
> Example key: `AbCdEfGhIjKlMnOpQrStUv`

**There is no build, lint, or test command — every workflow runs
through `use_figma`.**

## 1 · Project identity

Token-driven system, three tiers (primitive → semantic → component).
Figma is the source of truth. Local files are descriptive, never
authoritative.

| | |
|---|---|
| Figma file | `[YOUR_FIGMA_FILE_KEY]` |
| Default page on open | `Utility` |
| Welcome screen (reference layout) | `1:51` |
| Primitive colors planning canvas | `2:3389` (on `Foundations`) |
| Semantic colors planning canvas | `2:3856` (on `Foundations`) |
| Icon set frame | `26:524` (on `Utility`) |
| Status Bar main component | `5:333` (on `Utility`) |

## 2 · Architecture summary

```
Primitive → Semantic → Component
```

Six variable collections — three primitive, three semantic. Components
must reference **semantic** tokens only; never primitives, never raw
values. There is currently no per-component variable collection — token
files for components are not exported locally yet (out of scope for this
demo).

## 3 · Reference tables

### Variable collections (live state)

| Collection | Modes | Var count | Role |
|---|---|---|---|
| `Primitive: Colors` | `Value` | 72 | 8 hue ramps × 50–900 + base black/white |
| `Primitive: Size` | `Value` | 21 | 4-px-grid scale (0–999) |
| `Primitive: Typography` | `Value` | 33 | `font-family/inter`, weights (STRING), sizes/line-heights (FLOAT) |
| `Semantic: Colors` | **`Light` + `Dark`** | 44 | `background/* text/* icon/* border/*` × roles × states |
| `Semantic: Size` | `Value` | 33 | spacing, border-radius, border-width, icon, component-height |
| `Semantic: Typography` | `Value` | 27 | aliases to primitive typography |

Hue groups (primitive): `yellow`, `teal`, `green`, `red`, `orange`,
`blue`, `neutral`, plus `base/{white,black}`. Note: **`neutral`, not
`gray`**. Each hue ramp includes step `50` plus `100–900`.

Both `font-family/headline` and `font-family/text` semantic tokens alias
the same primitive `font-family/inter`. Loading just Inter (Regular,
Medium, Semi Bold, Bold) satisfies every text binding in this file.

### Pages

| Page | Purpose |
|---|---|
| `Utility` | Default on open. Holds the icon set frame `26:524` and the `Status Bar` main component `5:333`. |
| `Foundations` | Planning canvases for primitives `2:3389` and semantics `2:3856` — visual specs, not the variables themselves (variables live in collections). |
| `Screens` | Reference designs. Currently only `01 — Welcome` (`1:51`, 390×844). |
| `---` | Divider page. |
| `Button` · `Jumbotron` · `Artwork` | One page per component set. |
| `Screens with components` | Compositions assembled from the components above. Currently holds the rebuilt Welcome screen. |

### Component sets (live)

| Set | Variants | Component properties | Page |
|---|---|---|---|
| `Status Bar` (`5:333`) | single `COMPONENT` | — | Utility |
| `Button` (`106:43`) | 18 — `Style` (primary/secondary/tertiary) × `Size` (sm/md/lg) × `IconOnly` (false/true) | `Label` (TEXT), `Show left icon` / `Show right icon` (BOOL), `Left icon` / `Right icon` (INSTANCE_SWAP) | Button |
| `Jumbotron` (`122:34`) | 4 — `Size` (lg/md/sm/xs) | `Title` `Subtitle` `Description` (TEXT), `Show overline` `Show subtitle` `Show description` (BOOL; subtitle defaults true, others false) | Jumbotron |
| `Artwork` (`127:45`) | 5 — `Aspect ratio` (1:1, 4:3, 3:2, 16:9, 2:3) | inner `Image` rect — fill swapped per instance | Artwork |

### Variable scopes by token area

| Token area | Scopes |
|---|---|
| `background/*` | `FRAME_FILL, SHAPE_FILL` |
| `text/*` | `TEXT_FILL` |
| `icon/*` | `SHAPE_FILL, TEXT_FILL` |
| `border/*` | `STROKE_COLOR` |
| Primitive colors | `ALL_FILLS, STROKE_COLOR, EFFECT_COLOR` |
| Spacing dimensions | `GAP, WIDTH_HEIGHT` |
| Radius | `CORNER_RADIUS` |
| Border width | `STROKE_FLOAT` |
| Typography | `FONT_SIZE, LINE_HEIGHT, FONT_FAMILY, FONT_STYLE` |

## 4 · Conventions

### Variable names

```
Primitive    {group}/{step}                  → yellow/500, neutral/100, base/white
Semantic     {category}/{role}/{state}       → background/brand/solid, text/primary/disabled
             {property}/{style}              → font-size/headline-md
             {type}/{size}                   → spacing/lg, border-radius/full
```

Composite `{role}-{state}` keys split on the **first hyphen only**:
`brand-default-hover` → `brand/default-hover`, not `brand/default/hover`.
Keeps `default-hover` and `solid-hover` legible as one state unit.

### Components

| | |
|---|---|
| Component-set name | Title Case with spaces — `Button`, `Status Bar` |
| Variant axis keys | PascalCase — `Style`, `Size`, `IconOnly`, `Aspect ratio` |
| Variant values | lowercase — `primary`, `lg`, `false`. Punctuation like `:` is permitted (`1:1`, `16:9`). Reserved: only `=` and `,` |
| Inner layer names | PascalCase nouns — `Label`, `LeftIcon`, `Title`, `Image` |
| Component property name | Sentence case — `Show left icon`, `Aspect ratio` |

## 5 · Critical rules

1. **Semantic only on components.** Every fill, stroke, font-*, spacing,
   radius, and dimension on a component variant binds to a `Semantic: *`
   variable. Primitives are referenced only by semantics.
2. **Variable scopes are always explicit.** The `ALL_SCOPES` default
   pollutes pickers — set `variable.scopes` per the table in §3.
3. **Tier reference rule.** Semantic variables alias primitives.
   Components reference semantics. Primitives never reference anything.
4. **Page-per-component.** Each new component set lives on its own
   Figma page named after the component.
5. **State model.** Default state is built into variants. `hover` and
   `disabled` are *documented* (in the component-set description) using
   semantic `*-focus` / `*-disabled` tokens but not built as separate
   variants in this demo. **`active` has no semantic token** — record
   the gap explicitly; the stopgap is CSS `filter: brightness(0.95)`.
6. **Figma is the source.** Any "tokens.json" you see referenced in
   downstream tooling is regenerated from Figma — never hand-edit
   exported files; round-trip changes through Figma.

## 6 · Workflow patterns

- **Required preflight.** Load the `figma:figma-use` skill before any
  `use_figma` call. Pass `skillNames: "figma-use"` on every invocation.
  For multi-screen or library work also load `figma:figma-generate-design`.
- **Inspect first, write second.** Always run a read-only `use_figma`
  (list collections, dump a node's structure, resolve variable IDs)
  before creating anything. Existing names are easy to mis-guess
  (`neutral` not `gray`; `font-weight/medium` is a STRING).
- **Small atomic steps.** ≤10 logical operations per call. Build
  pattern: inspect → primitives → semantics → components → validate.
  Each step returns IDs the next step uses as string literals.
- **Return IDs from every call.**
  `return { createdNodeIds: [...], mutatedNodeIds: [...] }`.
  `console.log` is invisible across `use_figma`.
- **Page context resets between calls.** Always
  `await figma.setCurrentPageAsync(targetPage)` at the top of any script
  that targets a non-default page. The sync setter
  `figma.currentPage = page` throws.
- **Validation rhythm.** After mutating a component or composition,
  audit `boundVariables` on every changed node and screenshot the
  affected variant. Spot-check resolved values via
  `variable.resolveForConsumer(node).value`.

## 7 · File references

### Local repo

```
ai-design-system-starter/
├── CLAUDE.md              ← this file
├── foundations/            ← primitive token definitions (colors, size, typography)
├── tokens/                ← semantic token aliases (colors, size, typography)
├── components/            ← component implementations (atoms / molecules / organisms / patterns)
│   └── atoms/button/      ← Button interactive spec + token bindings
├── docs/                  ← prompts, mental models, templates, adoption playbook
├── source/                ← consolidated DTCG token export (reference)
├── guide/                 ← PDF starter guide
└── .claude/skills/        ← component-doc-writer skill
```

There is no `package.json`, build script, or test runner — workflow is
entirely Figma-driven via `use_figma`.

### Figma node IDs (jump points)

| Use | ID |
|---|---|
| File key | `[YOUR_FIGMA_FILE_KEY]` |
| Welcome screen | `1:51` |
| Primitive colors planning canvas | `2:3389` |
| Semantic colors planning canvas | `2:3856` |
| Icons frame | `26:524` |
| Status Bar main | `5:333` |
| Button set | `106:43` |
| Jumbotron set | `122:34` |
| Artwork set | `127:45` |

### External skills

- `figma:figma-use` — required for every `use_figma` invocation
- `figma:figma-generate-design` — for assembling whole screens from components

## 8 · Discovered rules (living log)

**Append new rules here the moment they're learned**, in the section
that matches the failure mode. Each rule should stand alone with enough
context that a fresh session can apply it.

### Fonts

- **Inter style names contain spaces** — `"Semi Bold"`, `"Extra Bold"`.
  `"SemiBold"` fails `loadFontAsync`.
- **SF Pro uses one word** — `"Semibold"`. Don't paste between Inter
  and SF Pro projects without checking.
- When `loadFontAsync` throws, call `figma.listAvailableFontsAsync()`
  for the exact style strings rather than guessing.

### Variable bindings

- **Bound-paint render-fallback bug.** `setBoundVariableForPaint(paint,
  'color', variable)` returns a paint whose original `color` field acts
  as the renderer's fallback. In some contexts (notably component
  variants), Figma renders the **fallback** instead of the resolved
  variable — silently producing white/black where you expect a tokenized
  value. **Fix:** resolve first
  (`variable.resolveForConsumer(node).value`) and pass that as the input
  paint's `color` so the fallback matches. Apply on every fill/stroke
  binding.
- **Font weight: STRING binds to `fontStyle`, not `fontWeight`.** When
  the weight variable holds a style string like `"Medium"` / `"Bold"`
  (which is how `Semantic: Typography > font-weight/*` is set up here),
  bind via `node.setBoundVariable('fontStyle', weightVar)`. The
  `'fontWeight'` field only accepts FLOAT-typed numeric weights.
- **`setBoundVariableForPaint` returns a new paint.** Capture and
  reassign — don't assume in-place mutation.
- **Text node `boundVariables` is array-keyed.**
  `boundVariables.fontFamily`, `.fontStyle`, `.fontSize`, `.lineHeight`,
  `.fills` are **arrays** (one entry per text run), not objects. Read
  `bv.fontSize[0].id`, not `bv.fontSize.id`.

### Component sets and instances

- **Boolean property bound to `visible` is shared globally across
  variants.** Setting `visible = true` on one variant flips every other
  variant's value too — last write wins. **Fix:** for variants that
  need a per-variant default, **detach** the visible reference (assign
  `componentPropertyReferences = { mainComponent: '...' }` without the
  `visible` key) and hardcode `node.visible` directly. Per-variant text
  and instance-swap defaults work fine — only boolean visibility is
  affected.
- **`addComponentProperty(... 'INSTANCE_SWAP', defaultValue)`** —
  `defaultValue` must be a main component **node ID** (e.g. `"26:424"`).
  Passing an instance ID, or the published `component.key`, throws
  `"Property value is incompatible with component property type"`.
- **Variant axis values support most punctuation** — including `:`
  (`Aspect ratio=1:1`, `16:9`). Reserved: only `=` (axis-value separator)
  and `,` (axis separator).

### Plugin API behavior

- `figma.notify()` throws `"not implemented"` — never use it. Return
  output via `return`.
- `figma.currentPage = page` throws. Use
  `await figma.setCurrentPageAsync(page)`.
- `getPluginData` / `setPluginData` not supported. Use
  `getSharedPluginData` / `setSharedPluginData` with a stable namespace
  (3+ chars).
- `hiddenFromPublishing` on variables/collections throws "Node not
  found". Toggle manually in the Local Variables panel.
- **Set `layoutSizingHorizontal` / `Vertical` to `'FILL'` only after
  `parent.appendChild(child)`.** Setting before throws.
- **Page context resets between `use_figma` calls.** Re-set the current
  page at the start of every script that targets a non-default page.
- **Scripts are atomic** — on error, no partial state is written; safe
  to retry after a fix.

### Audit / inspection helpers

- **`cornerRadius` does not exist on TEXT nodes.** A generic
  `summarize(node)` that always reads `n.cornerRadius` will throw on
  text. Guard with `'cornerRadius' in n` or branch on `n.type`.
