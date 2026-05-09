<!--
  Component documentation template v1.
  Instructions:
  · Replace every <placeholder> with concrete content.
  · Do NOT remove the HTML comment blocks — they carry the rules. Keep them in
    the final doc so future editors see the contract.
  · Sections marked REQUIRED must ship. OPTIONAL sections can be omitted if
    truly not applicable, but prefer to keep them with an explicit "N/A — reason".
  · Tone: imperative voice, short sentences, no marketing language ("simply",
    "just", "easily" are banned). Present tense for behavior, past tense only
    in Changelog.
-->

# <Component Name>

<!-- Header block: one-liner, Figma source, maturity. REQUIRED. -->
> <one-sentence description of what the component does and its primary use case>

| | |
|---|---|
| **Figma node** | [`<node-id>`](<deep-link>) |
| **Maturity** | `experimental` · `beta` · `stable` · `deprecated` |
| **Last updated** | YYYY-MM-DD |
| **Owner** | <design-system team / instructor> |

---

## 1. When to use · When NOT to use <!-- REQUIRED -->

<!-- RULE: Decision framing first. Before anything visual, the reader should
     know whether they're looking at the right component. Mirrors Polaris's
     "Buttons vs Links" section — the single most-cited pattern across
     benchmarks. -->

**Use `<Component>` when:**
- <specific trigger 1>
- <specific trigger 2>

**Do NOT use it when:**
- <use `<alternative-component>` for ...>
- <use `<other>` for ...>

---

## 2. Anatomy <!-- REQUIRED -->

<!-- RULE: Annotated image pointing at every named slot in the Figma component.
     Slot names must match Figma layer names exactly — students and AI-driven
     builds reference these directly. -->

![Anatomy of <Component>](./<component>/anatomy.png)

| # | Slot | Description |
|---|---|---|
| 1 | `<LayerName>` | <purpose> |
| 2 | `<LayerName>` | <purpose> |

---

## 3. Variants <!-- REQUIRED -->

<!-- RULE: Show every shippable combination. For components with many variants,
     start with a hero matrix (Size × Hierarchy or similar), then a collapsible
     "All variants" list. Every cell labelled with its literal variant string. -->

### Primary matrix

|  | `Size=xs` | `Size=sm` | `Size=md` | `Size=lg` | `Size=xl` |
|---|---|---|---|---|---|
| **Primary** | ![](./<component>/<variant-image>.png) | … | … | … | … |
| **Secondary** | … | … | … | … | … |

### All variants

<details>
<summary>Show the full variant list (<total>)</summary>

- `Size=xs, Hierarchy=Primary, …`
- …

</details>

---

## 4. Props / Variant API <!-- REQUIRED -->

<!-- RULE: 5-column table exactly. Merge of Polaris (Property/Type/Description)
     and Radix (Default) because readers routinely ask "what's the default?"
     The Required column is a single `yes` / `no` — not an asterisk. -->

| Name | Type | Default | Required | Description |
|---|---|---|---|---|
| `Size` | `xs \| sm \| md \| lg \| xl` | `sm` | yes | Controls height and horizontal padding. |
| … | … | … | … | … |

---

## 5. States <!-- REQUIRED -->

<!-- RULE: Show every interactive state visually. Same size + hierarchy across
     the row so readers compare like-for-like. -->

| Default | Hover | Pressed | Focus | Disabled |
|---|---|---|---|---|
| ![](./<component>/state-default.png) | ![](./<component>/state-hover.png) | ![](./<component>/state-pressed.png) | ![](./<component>/state-focus.png) | ![](./<component>/state-disabled.png) |

---

## 6. Tokens <!-- REQUIRED -->

<!-- RULE: Every color / size / typography reference is a semantic token name,
     deep-linked to its row on the relevant foundations page. NEVER cite a raw
     hex in prose. Primitives are reference-only.
     Follow the 3-tier chain: Component → Semantic → Primitive.
     Per-state tokens go in the sub-tables below. -->

### Base bindings

| Part | Property | Semantic token | Resolves to |
|---|---|---|---|
| Surface | `fills` | [`background/<role>-<state>`](<deep-link>) | `color/<hue>/<shade>` |
| Label | `fills` | [`text/<role>-<state>`](<deep-link>) | `color/<hue>/<shade>` |
| Border | `strokes` | [`border/<role>-<state>`](<deep-link>) | `color/<hue>/<shade>` |
| Height | `height` | [`component-height/<size>`](<deep-link>) | `<N>` |
| Radius | `cornerRadius` | [`border-radius/<size>`](<deep-link>) | `<N>` |
| Padding-x | `paddingLeft/Right` | [`spacing/<size>`](<deep-link>) | `<N>` |
| Label style | text style | `text/<size>/<weight>` | SF Pro Text `<N>px` |

### Per-state overrides

<!-- Only list the properties that CHANGE per state. -->

| State | Surface | Label | Border |
|---|---|---|---|
| Hover | `background/<role>-hover` | `text/<role>-hover` | — |
| Pressed | … | … | … |
| Disabled | `background/<role>-disabled` | `text/<role>-disabled` | … |

---

## 7. Accessibility <!-- REQUIRED -->

<!-- RULE: Placed BEFORE examples/code because constraints shape the solution.
     Cover: target size, contrast, keyboard, screen reader, ARIA, icon-only
     labeling. Cite the testing bar (WCAG level + tooling if any). -->

- **Target size:** ≥ 44×44 (`component-height/md`).
- **Contrast:** text ≥ 4.5:1 AA · non-text ≥ 3:1 AA.
- **Keyboard:** `Tab` moves focus; `Enter` and `Space` activate. Focus ring visible, 2px offset.
- **Screen reader:** announces `role="button"` + accessible name. Icon-only variants require explicit label (no inferred name from the glyph).
- **ARIA:** minimal — use native semantics. If the component is rendered as a `<div>`, add `role="button"` + `tabindex="0"` + `aria-disabled` when disabled.
- **Tested against:** <VoiceOver / NVDA / JAWS — versions dated>.

---

## 8. Content guidelines <!-- OPTIONAL — keep if the component shows text -->

<!-- RULE: Concrete writing rules for the label. One sentence per rule. -->

- Lead with a verb: "Save", "Delete", not "Click here".
- Sentence case.
- ≤ 3 words where possible, ≤ 24 characters hard cap.
- No punctuation inside the label.
- Localization: label expands up to 1.6× in German / Arabic — verify at the widest size.

---

## 9. Do / Don't <!-- REQUIRED -->

<!-- RULE: 4–6 paired visuals. One pair per common mistake. One sentence of
     reasoning per pair. Green check left, red X right. -->

| ✅ Do | ❌ Don't |
|---|---|
| ![](./<component>/do-1.png) **<short rule>** — <1-line reason>. | ![](./<component>/dont-1.png) **<short counter-rule>** — <1-line reason>. |
| … | … |

---

## 10. Internationalization / RTL <!-- OPTIONAL -->

- **RTL:** icon-leading / icon-trailing swap positions; text mirrors.
- **Expansion:** plan for up to 1.6× in longer languages — verify `xs` and `sm` sizes don't truncate.
- **Numerals:** respect locale (Eastern Arabic `٠١٢٣…`).

---

## 11. Usage in screens <!-- OPTIONAL — high value for bootcamp -->

<!-- RULE: 2–3 screenshots pulled from the Journeys page showing the component
     in real context. Caption each with the journey + screen name. -->

| Screen | Screenshot | Notes |
|---|---|---|
| Journey 2 · Login | ![](./<component>/usage-login.png) | Primary CTA on empty state. |
| Journey 6 · Checkout · Payment | ![](./<component>/usage-payment.png) | Paired with Secondary for back-out. |

---

## 12. Teaching notes <!-- OPTIONAL — bootcamp-specific -->

<!-- RULE: Common student mistakes observed in the bootcamp. Each entry is a
     short prescription, not just a complaint. Cite the relevant Critical Rule
     from CLAUDE.md when applicable. -->

- **Don't use `text/primary-inverse` on `brand-solid`.** Yellow is too bright for white. See Rule 9b.
- **Don't rebind the instance height.** Only rebind width (`instance.resize(w, instance.height)`). See Rule 15.
- **Don't assume one variant axis carries over.** Re-apply every variant dimension on `setProperties`. See Rule 15.

---

## 13. Changelog <!-- REQUIRED -->

<!-- RULE: Newest first. Dated, one line per entry. Past-tense verbs. -->

### 2026-04-18 — initial documentation
Adopted the v1 template.

---

<!-- END of template.
     Drop this file under docs/components/<slug>.md, rename the H1, fill in
     every placeholder, and commit.
-->
