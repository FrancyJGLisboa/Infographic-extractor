---
name: infographic-extractor
description: Scan raw material (transcripts, articles, papers, docs, manuals, slides) and produce a complete inventory of visualizable concepts mapped to eight archetype templates, each candidate scored across four quality dimensions with a ready-to-paste handoff block for downstream infographic generation. Trigger whenever the user pastes material and asks what infographics could be made from it, wants visualizable concepts identified, requests a diagram inventory, or matches phrases like 'what could be visualized from this', 'extract diagrammable concepts', 'mapeia o que da pra ilustrar', 'que infograficos da pra fazer disso', 'identifica conceitos visualizaveis', 'destrincha esse material em visuais', 'inventario de infograficos'. Trigger even when 'infographic' is not said explicitly - visualization intent plus raw material is enough. Do NOT trigger for plain summarization, translation, or generation of an infographic from an already-named concept.
---

# Infographic Extractor

Scans raw material through eight archetype lenses and produces a complete YAML inventory of visualizable concepts with quality scoring and handoff blocks for downstream infographic generation.

## Pipeline position

This skill is the extraction half of a two-step pipeline:

1. Raw material → **this skill** → inventory of candidates with archetype recommendations
2. Selected candidate + handoff block → infographic generator (see `references/generator_handoff.md`) → renderable HTML

Runs in parallel with `transcript-distill` (extracts procedural knowledge). Same input, complementary lens. Overlap is informative, not redundant — procedural extraction finds what is executable as rule; this skill finds what is explainable as visual.

## Protocol

### Etapa 1 — Classify input

Before scanning, identify and emit a 2-3 line summary at the top of the output covering:

- Material type: transcript | article | paper | documentation | manual | slides
- Language of the input
- Domain (macro area: commodities, agro, ML, finance, etc.)
- Apparent visualizable density: high | medium | low
- Areas of concentration (where in the material visualizable content clusters)

### Etapa 2 — Scan with all eight archetype lenses

Read `references/archetypes.md` for full definitions and what each lens looks for.

Apply each lens independently to the full material. Do NOT filter at this stage — even weak candidates enter the inventory.

For each candidate found under a lens, record:
- Location in the material (timestamp, section, page, paragraph)
- Trigger content (the passage that activated the lens)
- Initial confidence: high | medium | low

### Etapa 3 — Consolidate and dedupe

After all eight scans:

- **Merge exact duplicates**: same concept under same lens in multiple locations → one entry with multiple location refs
- **Identify hybrids**: same concept under 2+ lenses → mark `is_hybrid: true`, set `primary_archetype` and `secondary_archetype`, justify primary choice
- **Split composite candidates**: one big candidate better represented as 2-3 smaller ones → split

### Etapa 4 — Score

Apply the four-dimension rubric to every candidate. Each dimension 1-5. Compute `overall_score` as simple mean rounded to 1 decimal.

See `references/scoring_rubric.md` for the full rubric with 1-5 anchors per dimension.

### Etapa 5 — Archetype gaps

List separately any visualizable content that does NOT fit any of the eight archetypes. For each gap:

- Concept name and description
- Why none of the eight fits
- Closest archetype (with caveat)
- Suggested new archetype name if a pattern repeats across multiple gaps

Common gap patterns: distributions/quantiles, anatomies/component breakdowns, multi-variable timelines.

### Etapa 6 — Emit output

Two blocks in this order. Full schemas with worked example in `references/output_schema.md`.

1. Markdown human summary at the top: classification, totals, top 5 by score, archetype distribution, brief observations.
2. YAML structured inventory below: every candidate with metadata, scoring, content seed, and `handoff_to_generator` block.

## Hard rules

- **Never filter at extraction.** `LIMIAR_INVENTARIO` controls display filtering, not the underlying inventory. Always emit everything in the YAML.
- **Never force an archetype.** If the fit is bad, score low and be explicit in `missing_for_generation`. If no archetype fits at all, route to `archetype_gaps`.
- **Never invent content.** If a candidate needs facts that are not in the material, list them in `missing_for_generation` rather than guessing.
- **Always include traceable location.** Every candidate must point to where in the material it came from. No location → not in the inventory.
- **Hybrids are signal.** Concepts appearing under multiple lenses are typically richer — pick a primary, but always record the secondary.

## Variables

| Name | Description | Default |
|---|---|---|
| `MATERIAL` | Raw content to analyze | required |
| `TIPO_MATERIAL` | Material type, or `auto` for detection | `auto` |
| `AUDIENCIA` | Target audience for the eventual infographics (calibrates scoring) | `technical generalist` |
| `IDIOMA_OUTPUT` | `PT-BR` or `EN` | matches input |
| `LIMIAR_INVENTARIO` | Display filter threshold (0-5; 0 = show everything) | `0` |
| `DOMINIO` | Domain of the material | infer from input |

## Output language

Default: same as input. Override with `IDIOMA_OUTPUT`. YAML keys are always in English (schema invariant); values follow the output language.

## Downstream handoff

Each candidate in the inventory carries a `handoff_to_generator` block — paste it as-is into the infographic generator (`references/generator_handoff.md`) to produce the actual HTML infographic.

## References

- `references/archetypes.md` — full definitions of the eight archetypes, lens criteria, content structure for the generator
- `references/scoring_rubric.md` — four-dimension scoring rubric with 1-5 anchors per dimension
- `references/output_schema.md` — markdown summary and YAML inventory schemas with worked example
- `references/generator_handoff.md` — the downstream infographic generator (v2 prompt)
