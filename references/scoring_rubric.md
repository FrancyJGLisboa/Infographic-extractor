# Scoring Rubric

Four dimensions, each scored 1-5. `overall_score` = simple mean rounded to 1 decimal.

Apply scoring AFTER consolidation (Etapa 3), so hybrids are scored once under the primary archetype, not duplicated.

---

## Visualizability

**How naturally the candidate fits its assigned archetype, without forcing the template.**

- **5**: Fits the archetype perfectly. Every element of the content structure is present in the material or trivially inferable. No forcing required.
- **4**: Fits well. 1-2 minor elements need to be inferred from context but the fit is natural.
- **3**: Fits partially. Requires moderate interpretation. Some sections of the archetype template will be thin.
- **2**: Barely fits. Forcing the template will produce an awkward infographic. Consider hybrid or downgrade.
- **1**: Does not fit naturally. Should probably be routed to `archetype_gaps` instead.

---

## Material Density

**How much the source material covers the candidate concept.**

- **5**: Deep coverage. At least 3 distinct angles discussed in the material (e.g., for a process flow: stages described, bottlenecks discussed, monitoring metrics mentioned).
- **4**: Solid coverage. 2 angles developed in detail.
- **3**: Medium coverage. 1 angle developed with some depth; others mentioned briefly.
- **2**: Quick mention with little development. Most content will need to be inferred or researched externally.
- **1**: Surface mention only. The material barely touches the concept; producing a useful infographic requires extensive external research.

---

## Standalone-ness

**Whether the candidate can be understood without additional context from the broader material.**

- **5**: Fully interpretable on its own. The infographic would make sense to a reader who has not seen the source material.
- **4**: Needs 1-2 sentences of setup context. Easy to provide.
- **3**: Needs a paragraph of setup. Manageable but adds friction.
- **2**: Depends heavily on other concepts discussed in the material. Hard to extract cleanly.
- **1**: Incomprehensible outside the original material. Probably not worth turning into a standalone infographic.

---

## Audience Match

**Calibration of the candidate against `AUDIENCIA`.**

- **5**: Ideal level. Informs without being too basic or too advanced. The infographic would land with no adaptation.
- **4**: Well-calibrated. Minor tweaks to vocabulary or depth.
- **3**: Usable after adaptation. Needs translation of jargon, or expansion/compression of detail.
- **2**: Significantly off-level. Either much too basic (audience already knows this) or much too advanced (audience cannot follow without prerequisites).
- **1**: Completely misaligned. Probably not worth producing for this audience.

---

## Overall score

Mean of the four dimensions, rounded to 1 decimal:

```
overall_score = round((visualizability + material_density + standalone + audience_match) / 4, 1)
```

No weighting. All four dimensions count equally. If the user wants a different weighting, they apply it downstream on the YAML output.

---

## Interpretation guide

- **4.5-5.0**: Top-tier candidate. Ready to generate with minimal extra work.
- **3.5-4.4**: Solid candidate. Will produce a useful infographic with light additional research.
- **2.5-3.4**: Marginal candidate. Worth considering but requires substantial work to make landable.
- **1.5-2.4**: Weak candidate. Probably skip unless the topic is strategically important.
- **<1.5**: Should likely be in `archetype_gaps` or excluded entirely.

The extractor includes all of these in the YAML regardless of score — `LIMIAR_INVENTARIO` only affects what is highlighted in the markdown summary at the top.
