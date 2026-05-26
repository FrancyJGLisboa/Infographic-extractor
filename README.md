# infographic-extractor

An [Agent Skill](https://agentskills.io) that turns any block of source material — transcripts, articles, papers, slide decks, manuals — into a scored inventory of visualizable concepts mapped to eight archetype templates. The skill stops at the inventory; the user picks which candidate to render with the downstream generator.

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
mkdir -p <your-agent-skills-dir>/infographic-extractor
cp /tmp/ie/SKILL.md <your-agent-skills-dir>/infographic-extractor/
cp -r /tmp/ie/references <your-agent-skills-dir>/infographic-extractor/
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

The agent responds with a classified summary, an inventory of candidates scored on four dimensions, and one `handoff_to_generator` block per candidate. To render an infographic, paste the chosen candidate's handoff block into the downstream generator described in `references/generator_handoff.md`.

## What it does

Given any block of source material, the skill:

- Classifies the input (material type, language, domain, visualizable density).
- Scans through all eight archetype lenses independently, without filtering.
- Identifies hybrid concepts that fit under more than one lens.
- Scores every candidate on visualizability, material density, standalone clarity, and audience match.
- Surfaces visualizable content that does NOT fit any of the eight archetypes (archetype gaps) — useful signal for evolving the taxonomy.
- Emits a markdown summary plus structured YAML inventory, with one `handoff_to_generator` block per candidate.

The skill always stops at the inventory. The user picks which candidate to render.

## Pipeline position

This skill is the extraction stage of a two-step pipeline:

1. Raw material → **this skill** → inventory of scored candidates with handoff blocks.
2. Selected candidate + handoff block → infographic generator (see `references/generator_handoff.md`) → renderable HTML poster.

## Companion skills

- `transcript-distill` — extracts reusable procedural knowledge from the same kind of source material. Runs in parallel: same input, complementary lens.
- The infographic generator referenced in `references/generator_handoff.md` handles the actual HTML output.

## Repository layout

```
Infographic-extractor/
├── README.md
├── SKILL.md                       # YAML frontmatter + protocol
├── .gitignore
└── references/
    ├── archetypes.md              # eight archetype definitions
    ├── scoring_rubric.md          # 1-5 anchors per dimension
    ├── output_schema.md           # markdown + YAML output schemas
    └── generator_handoff.md       # downstream HTML generator
```

`SKILL.md` lives at the repo root so the repository itself is the skill package.

## Contributing

Issues and pull requests welcome. If you are adding a new archetype, include the lens criteria in `references/archetypes.md`, scoring anchors in `references/scoring_rubric.md`, and a worked example in the same PR.

## License

Add a license before publishing widely. MIT, Apache 2.0, and CC BY-SA are common choices for Agent Skills.
