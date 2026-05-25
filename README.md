# infographic-extractor

An [Agent Skill](https://agentskills.io) that turns any block of source material — transcripts, articles, papers, slide decks, manuals — into a scored inventory of visualizable concepts mapped to six archetype templates. Auto-renders the top candidate as an HTML infographic when generation intent is detected; otherwise stops at the inventory and lets the user pick.

Works across Claude Code, GitHub Copilot, Cursor, Codex, Gemini CLI, Antigravity, and any other agent that supports the [SKILL.md format](https://agentskills.io).

## Install

Requires [GitHub CLI](https://cli.github.com) v2.90 or later. Pick your agent:

| Agent | Install (user scope) |
|---|---|
| Claude Code | `gh skill install FrancyJGLisboa/Infographic-extractor infographic-extractor --agent claude-code --scope user` |
| GitHub Copilot | `gh skill install FrancyJGLisboa/Infographic-extractor infographic-extractor --agent github-copilot --scope user` |
| Cursor | `gh skill install FrancyJGLisboa/Infographic-extractor infographic-extractor --agent cursor --scope user` |
| Codex | `gh skill install FrancyJGLisboa/Infographic-extractor infographic-extractor --agent codex --scope user` |
| Gemini CLI | `gh skill install FrancyJGLisboa/Infographic-extractor infographic-extractor --agent gemini --scope user` |
| Antigravity | `gh skill install FrancyJGLisboa/Infographic-extractor infographic-extractor --agent antigravity --scope user` |

Drop `--scope user` to install at project scope only (inside the current git repository). Antigravity, Codex, Cursor, and Gemini CLI all share the same `.agents/skills/` directory at project scope, so one project-scope install covers all of them.

For agents not covered by `gh skill` — Amp, OpenCode, Junie, Goose, Windsurf — clone manually:

```bash
git clone https://github.com/FrancyJGLisboa/Infographic-extractor.git /tmp/ie
cp -r /tmp/ie/skills/infographic-extractor <your-agent-skills-dir>/
```

### Where the skill lands on disk

| Agent | Project scope | User scope |
|---|---|---|
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |
| GitHub Copilot | `.agents/skills/` | `~/.copilot/skills/` |
| Cursor | `.agents/skills/` | `~/.cursor/skills/` |
| Codex | `.agents/skills/` | `~/.codex/skills/` |
| Gemini CLI | `.agents/skills/` | `~/.gemini/skills/` |
| Antigravity | `.agents/skills/` | `~/.gemini/antigravity/skills/` |

## Verify

Start a new agent session and paste:

```
What could be visualized from this material?

[paste any transcript, article, or doc here]
```

Or in Portuguese:

```
Mapeia o que dá pra ilustrar desse material:

[cola qualquer transcript, artigo ou doc aqui]
```

The agent responds with a classified summary, an inventory of candidates scored on four dimensions, and one `handoff_to_generator` block per candidate.

To go all the way to a rendered infographic in a single turn, use generation intent:

```
Gera um infográfico desse material:

[paste material]
```

The skill auto-picks the top-scoring candidate and runs the downstream generator inline.

## What it does

Given any block of source material, the skill:

- Classifies the input (material type, language, domain, visualizable density).
- Scans through all six archetype lenses — single-number metric, process flow, state regime, comparison matrix, causal chain, cycle/loop — independently and without filtering.
- Identifies hybrid concepts that fit under more than one lens.
- Scores every candidate on visualizability, material density, standalone clarity, and audience match.
- Surfaces visualizable content that does NOT fit any of the six archetypes (archetype gaps) — useful signal for evolving the taxonomy.
- Emits a markdown summary plus structured YAML inventory, with one `handoff_to_generator` block per candidate.

When the prompt expresses generation intent ("make an infographic from this material" / "gera um infográfico desse material"), the top-scoring candidate is auto-rendered in the same turn. Otherwise the skill stops at the inventory and the user picks.

## Pipeline position

This skill is the extraction stage of a two-step pipeline:

1. Raw material → **this skill** → inventory of scored candidates with handoff blocks.
2. Selected candidate + handoff block → infographic generator (see `skills/infographic-extractor/references/generator_handoff.md`) → renderable HTML poster.

Step 2 runs automatically on generation intent. On exploration intent, the user replies with a number to pick which candidate to render.

## Companion skills

- `transcript-distill` — extracts reusable procedural knowledge from the same kind of source material. Runs in parallel: same input, complementary lens.
- The infographic generator referenced in `skills/infographic-extractor/references/generator_handoff.md` handles the actual HTML output.

## Repository layout

```
Infographic-extractor/
├── README.md
├── .gitignore
└── skills/
    └── infographic-extractor/
        ├── SKILL.md                          # YAML frontmatter + protocol
        └── references/
            ├── archetypes.md                 # six archetype definitions
            ├── scoring_rubric.md             # 1-5 anchors per dimension
            ├── output_schema.md              # markdown + YAML output schemas
            └── generator_handoff.md          # downstream HTML generator
```

The `skills/<name>/SKILL.md` layout follows the convention used by `gh skill install`, so this repository can host additional skills under `skills/` in the future without restructuring.

## Contributing

Issues and pull requests welcome. If you are adding a new archetype, include the lens criteria in `references/archetypes.md`, scoring anchors in `references/scoring_rubric.md`, and a worked example in the same PR.

## License

Add a license before publishing widely. MIT, Apache 2.0, and CC BY-SA are common choices for Agent Skills.
