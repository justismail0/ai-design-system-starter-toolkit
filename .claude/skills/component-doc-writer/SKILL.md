---
name: component-doc-writer
description: Generate component documentation from a live Figma component set. Pulls metadata, variables, and screenshots, then writes a structured markdown doc. Trigger when the user says "document the <component>", "write docs for <component>", "fill the doc template for <component>", or provides a Figma component-set node ID in a docs-writing request.
---

# component-doc-writer

Automates the mechanical parts of component documentation — variants, props, states, tokens, anatomy — and leaves judgment calls as `TODO:` stubs for you to fill.

## Inputs

- **`componentName`** (required) — the component name (e.g. `Button`, `Filter Chip`, `Input Field`). Used for the output filename and the H1.
- **`nodeId`** (required) — Figma node id of the COMPONENT_SET or standalone COMPONENT (e.g. `106:43`).
- **`slug`** (optional) — kebab-case filename. Defaults to `componentName.toLowerCase().replaceAll(' ', '-')`.
- **`fileKey`** (optional) — defaults to the file key in CLAUDE.md Section 1.

## Output

- `./docs/components/<slug>.md` — filled template, with `TODO:` markers on judgment sections.
- `./docs/components/<slug>/` — directory for image exports (anatomy, variant matrix, state grid).

## Execution steps

### 1. Preconditions

- Confirm `./docs/component-doc-template.md` exists. If missing, stop and tell the user.
- Confirm the component node exists via the Figma MCP's `get_metadata`. If missing, stop and tell the user the node id is wrong.
- Read the file key from CLAUDE.md Section 7 if not provided as input.

### 2. Pull live state

Run these in parallel:

- `get_metadata` on the node — returns variant enumeration, positions, sizes.
- `get_variable_defs` on the node — returns the resolved semantic tokens + their primitive values.
- `get_screenshot` on the node — hero image.

Save the screenshot as `docs/components/<slug>/hero.png`.

Optionally, for each shipped `Size x Style` cell, grab a per-variant screenshot. Cap at the top 6 cells to bound tool calls.

### 3. Derive each template section from live data

| Template section | How to fill |
|---|---|
| Header | `componentName`, nodeId as deep-link, `stable` maturity default, today's ISO date. |
| 1. When to use / Not to use | Stub. `TODO: describe when to use / when not to use.` |
| 2. Anatomy | Parse variant names from metadata. Pick any variant's inner frame names and list them. Image placeholder. |
| 3. Variants | Build a Size x Style matrix from parsed variant strings. Under it, a `<details>` block listing every variant string verbatim. |
| 4. Props / Variant API | Transform `componentPropertyDefinitions` into a 5-column table. Variant axes = join of values. Booleans = `true | false`. |
| 5. States | List every distinct `State` axis value. Image placeholders per state. |
| 6. Tokens | Parse `get_variable_defs` result. Group into: Base bindings (Surface / Label / Icon / Height / Radius / Padding-x / Gap / Label style). Per-state overrides if hover/pressed/disabled tokens differ. |
| 7. Accessibility | Stub with scaffolded defaults: touch target from Size=md height, contrast notes. `TODO: confirm.` |
| 8. Content guidelines | Stub `TODO:` if the component has a text slot; omit otherwise. |
| 9. Do / Don't | Stub with 4 empty rows. |
| 10. i18n / RTL | Stub. Skip if no text slot. |
| 11. Usage in screens | Stub. `TODO: pick 2-3 journey screens.` |
| 12. Teaching notes | Stub. `TODO: list common mistakes for this component.` |
| 13. Changelog | `### <today> — Documentation v1` |

### 4. Write the artifact

- Create directory `docs/components/<slug>/`
- Write `docs/components/<slug>.md` with the filled template.

### 5. Report

Return to the user:
- Path of the file written.
- Count of variants covered.
- List of `TODO:` sections still needing human fill.
- Any images not yet exported.

## Hard rules

- **Never fabricate tokens.** If the MCP returns no variable def for a part, mark it `(no token bound)` and flag in the report.
- **Never invent variant combinations** the metadata doesn't show.
- **Never write judgment content** (When to use, Do/Don't reasoning, Teaching notes). Those stay `TODO:`.
- **One component per invocation.** For multiple components, run the skill multiple times.
