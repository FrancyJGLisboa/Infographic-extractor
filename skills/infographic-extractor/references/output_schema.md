# Output Schema

Two blocks in order: markdown summary at the top (for human reading), YAML inventory below (for machine consumption).

---

## Block 1 — Markdown Summary

Template:

```markdown
## Extraction: <short title derived from material>

**Input**: <classification line from Etapa 1>

**Inventory**: <N> primary candidates, <M> hybrids, <K> archetype gaps.

**Top 5 by overall_score**:
1. <concept_name> — <archetype> — <overall_score>
2. <concept_name> — <archetype> — <overall_score>
3. <concept_name> — <archetype> — <overall_score>
4. <concept_name> — <archetype> — <overall_score>
5. <concept_name> — <archetype> — <overall_score>

**Distribution by archetype**:
- single_number_metric: <count>
- process_flow: <count>
- state_regime: <count>
- comparison_matrix: <count>
- causal_chain: <count>
- cycle_loop: <count>

**Observations**: <2-3 sentences on overall quality of the material, rich areas, gaps, recurring patterns>
```

If `LIMIAR_INVENTARIO > 0`, "Top 5" becomes "Candidates above threshold" and lists all candidates meeting the cutoff (still showing the full inventory in YAML below).

---

## Block 2 — YAML Inventory

Schema:

```yaml
extraction_metadata:
  source_type: <transcript | article | paper | documentation | manual | slides>
  source_language: <PT-BR | EN | other>
  output_language: <PT-BR | EN>
  domain: <inferred or supplied>
  scan_lenses: [single_number_metric, process_flow, state_regime, comparison_matrix, causal_chain, cycle_loop]
  audiencia: <as supplied or default>
  limiar_inventario: <0-5>

inventory:
  - id: 01
    concept_name: "<concise name>"
    primary_archetype: <one of the six>
    secondary_archetype: <one of the six, or null>
    is_hybrid: <true | false>
    one_line_essence: "<single sentence>"
    source_location: "<traceable reference>"
    quality:
      visualizability: <1-5>
      material_density: <1-5>
      standalone: <1-5>
      audience_match: <1-5>
      overall_score: <1.0-5.0>
    content_seed:
      title_proposal: "<ALL CAPS WITH PERIOD.>"
      tagline_proposal: "<<=12 words>"
      key_facts:
        - "<fact 1>"
        - "<fact 2>"
        - "<fact 3>"
      missing_for_generation:
        - "<what still needs research or external sourcing>"
    handoff_to_generator: |
      {{CONCEITO}} = <name>
      {{TAGLINE}} = <tagline>
      {{DOMINIO}} = <domain>
      {{AUDIENCIA}} = <audience>
      {{IDIOMA}} = <output language>
      {{ARQUETIPO}} = <archetype>
      {{ANALOGIA_FINAL}} = <if applicable, else null>

  - id: 02
    # ... same shape

archetype_gaps:
  - concept_name: "<name>"
    description: "<why visualizable but does not fit>"
    closest_archetype: <one of the six, with caveat>
    suggested_new_archetype: "<proposed name if pattern repeats, else null>"
    source_location: "<traceable reference>"
```

---

## Worked Example

Material: a 45-minute podcast transcript on Brazilian corn ethanol industry economics.

### Markdown summary

```markdown
## Extraction: Brazilian Corn Ethanol Industry Economics

**Input**: transcript, PT-BR, ~9,800 words, domain: biofuels/agro. Density: high. Concentration in minutes 8-32 (technical core) and 38-44 (regional comparisons).

**Inventory**: 7 primary candidates, 2 hybrids, 1 archetype gap.

**Top 5 by overall_score**:
1. Corn Ethanol Crush Margin — single_number_metric — 4.8
2. MT to Port Origination Flow — process_flow — 4.5
3. Brazil vs US Ethanol Production Models — comparison_matrix — 4.3
4. Sugar-Ethanol Switch Decision — state_regime — 4.0
5. Drought → Yield → DDGS Supply → Feed Cost — causal_chain — 3.8

**Distribution by archetype**:
- single_number_metric: 2
- process_flow: 1
- state_regime: 1
- comparison_matrix: 2
- causal_chain: 1
- cycle_loop: 0

**Observations**: Material is dense on technical concepts and well-suited to single-number and comparison archetypes. Cycle/loop is absent — the speaker treats the industry as growth phase, not cyclical. One archetype gap: the speaker spent 4 minutes on a hierarchy of integrated vs standalone plants that does not fit any of the six.
```

### YAML excerpt (first candidate only)

```yaml
extraction_metadata:
  source_type: transcript
  source_language: PT-BR
  output_language: PT-BR
  domain: biofuels and corn ethanol
  scan_lenses: [single_number_metric, process_flow, state_regime, comparison_matrix, causal_chain, cycle_loop]
  audiencia: commodity research analysts
  limiar_inventario: 0

inventory:
  - id: 01
    concept_name: "Margem de esmagamento de etanol de milho"
    primary_archetype: single_number_metric
    secondary_archetype: null
    is_hybrid: false
    one_line_essence: "Diferença entre receita combinada de etanol + DDGS e custo do milho, em R$/saca."
    source_location: "minute 12:30-15:45 and 28:10-30:00"
    quality:
      visualizability: 5
      material_density: 5
      standalone: 5
      audience_match: 4
      overall_score: 4.8
    content_seed:
      title_proposal: "CRUSH DE ETANOL DE MILHO."
      tagline_proposal: "A margem da planta brasileira de etanol em um número."
      key_facts:
        - "Inputs: milho R$/saca; outputs: etanol R$/L + DDGS R$/ton"
        - "Conversão típica: 1 ton milho → ~410 L etanol + 280 kg DDGS"
        - "Plantas FS, Inpasa, BioUrbe operam acima de R$ 8/saca de margem em 2024"
      missing_for_generation:
        - "Fórmula exata com fatores de conversão (speaker citou aproximadamente)"
        - "Variações regionais MT vs GO vs MS"
    handoff_to_generator: |
      {{CONCEITO}} = Crush de etanol de milho
      {{TAGLINE}} = A margem da planta brasileira de etanol em um número.
      {{DOMINIO}} = etanol de milho Brasil
      {{AUDIENCIA}} = commodity research analysts
      {{IDIOMA}} = PT-BR
      {{ARQUETIPO}} = single_number_metric
      {{ANALOGIA_FINAL}} = crush de etanol está para milho como crush está para soja
```

The handoff block is designed to be copy-pasted directly into the generator (see `generator_handoff.md`) with no editing.
