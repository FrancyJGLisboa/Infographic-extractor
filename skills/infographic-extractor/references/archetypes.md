# Archetypes

Six archetypes. Each has a lens (what to look for in raw material) and a content structure (what the downstream generator will produce).

Apply each lens independently to the full material during Etapa 2. Mark hybrids when a concept fires under multiple lenses.

---

## A1 — Single-Number Metric

**When to use**: concept with an explicit formula combining inputs into a single output metric, with actionable binary states (high/low → different actions).

**Lens — what to look for**:
- Explicit formulas with units
- Metrics: ratios, spreads, margins, indices
- Concepts described as "the difference between X and Y", "X divided by Y", "X minus Y"
- Mentions of conversion factors, weights, or proportions
- Language like "above X means…", "below Y triggers…", "tight crush", "wide spread", "elevated ratio"

**Visual metaphor**: `input(s) → output(s) = metric` horizontal equation, plus binary indicators (positive ↑ / negative ↓) to the right.

**Content structure (5 sections)**:
1. What it is — operational definition (2-3 sentences)
2. How it is calculated — explicit formula with units and conversion factors
3. What it tells you — 2 columns (positive state vs negative state), 3 bullets each, plus a one-line cyclical/self-regulating principle
4. How it is used in practice — 3 specific actions (with colored arrows) plus caveats
5. Regional/contextual variations — 3-row table

**Pull quote**: cross-domain analogy (e.g., "crush is to soy what crack is to crude").

---

## A2 — Process Flow

**When to use**: ordered sequence of stages that transforms something (supply chain, ETL pipeline, originate-to-port logistics, trade lifecycle, processing operation).

**Lens — what to look for**:
- Sequences with explicit ordering ("first…then…then…")
- Stages with distinct operators, inputs, outputs
- Transformation language ("the bean leaves the farm, gets trucked to…, then…")
- Workflows, pipelines, processes
- Time-ordered descriptions of how something happens end-to-end

**Visual metaphor**: 3-7 horizontal stages connected by arrows. Each stage carries icon + label + sub-label (input → action → output).

**Content structure (5 sections)**:
1. Overview — what enters at one end, what exits at the other (2-3 sentences)
2. Stages detailed — for each stage: what happens, who operates, key metric (full-width section)
3. Typical bottlenecks — 3 bullets on where the flow stalls plus warning signs
4. Monitoring metrics — KPIs per stage in a compact table
5. Variations by context — how the flow changes across 2-3 contexts (geography, scale, segment)

**Pull quote**: why understanding the flow matters more than the end product.

---

## A3 — State Regime

**When to use**: discrete mutually-exclusive states with transition triggers and distinct recommended actions (ENSO phases, contango/backwardation, COT regimes, monetary policy stance, crop cycle phase).

**Lens — what to look for**:
- Discrete states named explicitly ("El Niño", "La Niña", "Neutral"; "contango", "backwardation")
- Transition language ("when SST exceeds X, regime shifts to…", "above 0.5°C anomaly triggers…")
- Distinct actions associated with each state ("during La Niña, hedge X; during El Niño, hedge Y")
- Threshold-based classification rules
- Historical case lists of when each state occurred

**Visual metaphor**: 2-4 state boxes (in line or triangle layout) connected by arrows labeled with transition triggers. Each state gets a distinct color (teal / orange / gray for neutral).

**Content structure (5 sections)**:
1. What defines each state — classification criterion (1 line per state)
2. Transition triggers — what moves A→B, B→C (with thresholds when applicable)
3. What to do in each state — table: state × recommended action
4. How to detect the change — leading and confirming signals
5. Historical cases — 2-3 dated examples with duration and outcome

**Pull quote**: the cost of identifying the regime too late.

---

## A4 — Comparison Matrix

**When to use**: N alternatives compared across the same dimensions (hedging instruments, contract types, data methodologies, vendors, frameworks).

**Lens — what to look for**:
- Lists of alternatives presented in parallel ("futures vs options vs swaps", "FOB vs CFR vs CIF")
- Repeated attributes evaluated across alternatives ("cost", "liquidity", "flexibility")
- Comparative tables or implicit comparisons in prose
- Phrases like "the trade-off between…", "the main difference is…", "X is better when Y but worse when Z"

**Visual metaphor**: row of icons of the alternatives (3-5) side by side at top with label underneath.

**Content structure (4 sections — matrix is the centerpiece)**:
1. Comparison matrix — table alternatives × dimensions (5-7 dimensions: cost, liquidity, flexibility, custody, etc.) with short cells
2. When each makes sense — 1-2 sentences per alternative
3. Pitfalls and hidden costs — 3-5 bullets on common traps
4. How to choose — simple decision tree (3-4 questions) or rule of thumb

**Pull quote**: the central trade-off nobody wants to admit.

---

## A5 — Causal Chain

**When to use**: driver → transmission mechanism → outcome chain (weather shock → yield → price; rate hike → DXY → commodities).

**Lens — what to look for**:
- "X causes Y" or "X leads to Y which leads to Z" patterns
- Transmission mechanism explanations
- Macroeconomic, biological, or physical chains of causation
- Sensitivity language ("for every 1% decline in X, Y moves Z%")
- Discussions of when the chain breaks or weakens

**Visual metaphor**: horizontal chain with 3-5 nodes connected by thick arrows. First node teal (driver), intermediate nodes gray (mechanisms), final node colored by sign (teal positive / orange negative).

**Content structure (5 sections)**:
1. Driver — starting point and how to measure it
2. Transmission mechanism — step-by-step propagation
3. Expected magnitudes — how much each link moves (with units and ranges)
4. Modifiers — what amplifies or attenuates the effect (3-5 factors)
5. When the chain breaks — conditions under which historical effect fails to confirm

**Pull quote**: why the effect is rarely linear.

---

## A6 — Cycle / Loop

**When to use**: recurring stages that return to the starting point (crop calendar, hog cycle, sugar cycle, business cycle).

**Lens — what to look for**:
- "After X, comes Y, then Z, then back to X"
- Cyclical language: "cycle", "loop", "seasonality", "recurrence"
- Self-perpetuating dynamics ("high prices → planting expansion → oversupply → low prices → planting contraction → tight supply → high prices…")
- Typical duration estimates ("4-year cycle", "18-month boom-bust")
- Geographic or sectoral variation in cycle behavior

**Visual metaphor**: circular diagram with 4-8 stages around a circle, arrows indicating direction. Stages colored by intensity (light → dark → light back to start).

**Content structure (5 sections)**:
1. Cycle stages — short description of each (1 line per stage)
2. Typical duration — average time in each stage plus total cycle length
3. What drives the cycle — fundamental drivers (supply, demand, biology, policy)
4. Inflection signals — how to identify transitions between stages
5. Geographic variations — how the cycle differs across 2-3 regions

**Pull quote**: why the cycle self-perpetuates even when everyone knows it exists.

---

## Hybrid handling

When a concept fires under multiple lenses, mark `is_hybrid: true` and record both:

- `primary_archetype`: the lens that produces the richest infographic with the available material
- `secondary_archetype`: the alternative lens with brief justification

Common hybrids worth noting:
- ENSO can read as `state_regime` (3 discrete states) or `cycle_loop` (recurrent oscillation). Primary depends on whether the material emphasizes states+thresholds or recurrence+drivers.
- Carry trade can read as `causal_chain` (rate diff → flows → FX) or `state_regime` (risk-on / risk-off). Primary depends on framing.
- Hog cycle is canonical `cycle_loop` but the breeding-pricing transmission is also `causal_chain`. Primary: cycle (richer for the topic).

---

## Outside-archetype patterns (route to `archetype_gaps`)

Visualizable but not covered by the six:

- **Taxonomy / hierarchy** — tree of categories and subcategories
- **Distribution / quantiles** — tails vs body of a probability distribution
- **Anatomy / component breakdown** — labeled parts of a thing
- **Network / map** — nodes and connections without sequence
- **Multi-variable timeline** — multiple series over time with annotated events

If a pattern recurs across multiple `archetype_gaps` entries, surface it as a candidate for a future seventh archetype.
