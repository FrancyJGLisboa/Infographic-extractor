# Generator Handoff (v2)

The downstream infographic generator. Paste a candidate's `handoff_to_generator` block into this prompt to produce a renderable HTML infographic.

This file is reference-only. The extractor does NOT run the generator — it produces handoff blocks that feed into a separate generation step (a separate Claude turn, a separate skill, or a separate session).

---

## Variables (filled from the extractor's `handoff_to_generator` block)

| Variable | Description |
|---|---|
| `{{CONCEITO}}` | Concept name (1-4 words) |
| `{{TAGLINE}}` | Essence sentence, <=12 words |
| `{{DOMINIO}}` | Domain or market |
| `{{AUDIENCIA}}` | Target audience |
| `{{IDIOMA}}` | `PT-BR` or `EN` |
| `{{ARQUETIPO}}` | One of: `single_number_metric`, `process_flow`, `state_regime`, `comparison_matrix`, `causal_chain`, `cycle_loop`, `network_map`, `taxonomy_tree` |
| `{{ANALOGIA_FINAL}}` | Cross-domain analogy for the pull quote, or null |

---

## Generation protocol

### Step 1 — Confirm archetype

If `{{ARQUETIPO}}` is supplied (which is always the case from extractor handoff), skip diagnosis and proceed.

If not supplied, run the router: ask which of the eight archetype patterns the concept fits, justify briefly, propose an alternative if the concept is hybrid, request user confirmation before generation.

### Step 2 — Apply design system

The design system is invariant across all eight archetypes.

**Palette (fixed hex)**:
- Navy primary: `#1B2A47` — titles, section numbers, quote block
- Teal secondary: `#2A8B8C` — subtitles, positive indicators, accent icons
- Warning orange: `#E45A2E` — negative indicators, alerts, transitions
- Cream gray: `#F5F1EA` — card backgrounds
- White: `#FFFFFF` — main background
- Divider gray: `#D9D9D9` — separator lines

**Typography**:
- Titles: `Montserrat` 900, ALL CAPS, with period at end
- Subtitles: `Montserrat` 600, teal, all caps, letter-spacing +2%
- Body: `Inter` 400, dark navy
- Section numbers: `Montserrat` 700, white, in navy-filled square 32×32px

Import via Google Fonts: `Montserrat:wght@400;600;900` and `Inter:wght@400;600;900`.

**Layout**: vertical poster, ratio approximately 2:3, width 800px, natural height. Header (title + tagline) → upper visual block (archetype-specific metaphor) → numbered content sections in 2-column grid → footer (navy block with italic pull quote and large decorative quotation marks).

**Iconography**: flat modern, no complex gradients. Arrows in teal/orange circles for binary states. Circular flag icons (emoji or SVG) for geographic variants.

### Step 3 — Apply archetype-specific content structure

See the extractor's `archetypes.md` for the content structure of each archetype. Each archetype defines its upper visual metaphor and its 4-5 numbered sections.

### Step 4 — Generate

Produce HTML that is:
- Single file, self-contained
- No JavaScript
- CSS inline or in `<style>` block
- Only external dependency: Google Fonts
- Width fixed at 800px, height natural
- Renderable directly as a Claude artifact

---

## Content rules (transversal to all archetypes)

- Each section: <= 60 words
- Each bullet: <= 12 words
- Every magnitude with explicit units
- Concrete nouns > abstract nouns
- No filler ("It is important to note that...", "It is worth highlighting...")
- No hedging when the fact is clear
- Language: `{{IDIOMA}}`

---

## Quality checklist (run before delivering)

- [ ] Title in ALL CAPS with period at the end?
- [ ] Tagline <= 12 words?
- [ ] Upper visual metaphor coherent with the archetype?
- [ ] Section structure follows the assigned archetype template?
- [ ] Palette restricted to the six defined colors?
- [ ] All magnitudes have units?
- [ ] Pull quote has analogy or strong insight?
- [ ] Renders cleanly at 800px width?

---

## Notes on archetype-specific content structures

(Condensed reference; full versions in extractor's `archetypes.md`.)

**single_number_metric**: 5 sections — what it is / how calculated / what it tells you (binary columns) / how used in practice (action arrows) / regional variations. Upper visual: input(s) → output(s) = metric. Pull quote: cross-domain analogy.

**process_flow**: 5 sections — overview / stages detailed (full width) / bottlenecks / monitoring metrics / context variations. Upper visual: 3-7 horizontal stages with arrows. Pull quote: why understanding flow matters more than the end product.

**state_regime**: 5 sections — state definitions / transition triggers / actions per state / detection signals / historical cases. Upper visual: 2-4 state boxes with labeled transition arrows. Pull quote: cost of identifying regime late.

**comparison_matrix**: 4 sections (matrix is centerpiece) — matrix / when each makes sense / pitfalls / how to choose. Upper visual: row of alternative icons. Pull quote: the central trade-off.

**causal_chain**: 5 sections — driver / transmission mechanism / expected magnitudes / modifiers / when chain breaks. Upper visual: 3-5 chained nodes with thick arrows, first node teal, last colored by sign. Pull quote: why the effect is not linear.

**cycle_loop**: 5 sections — cycle stages / typical duration / what drives it / inflection signals / geographic variations. Upper visual: circular diagram with 4-8 stages. Pull quote: why the cycle self-perpetuates.

**network_map**: 5 sections — network overview / major nodes / principal corridors / concentration vs dispersion / structural shifts. Upper visual: world or regional map (or abstract node graph) with nodes sized by weight and edges sized by flow magnitude. Teal for primary nodes, gray for secondary, orange for emerging or contested. Pull quote: the structural reason the dominant corridor is dominant.

**taxonomy_tree**: 5 sections — root and first-level split / branches detailed / leaf attributes / where boundaries blur / why classification matters. Upper visual: tree diagram (top-down or horizontal) with navy root, teal first-level branches, smaller leaves with short attribute labels. Orange highlight on branches with overlap. Pull quote: the cost of misclassifying.
