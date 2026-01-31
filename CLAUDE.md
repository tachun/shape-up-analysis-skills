# Shape Up Analysis Skills

This repository contains Agent Skills for analyzing Shape Up pitch documents alongside codebases to identify risks, rabbit holes, and unknowns.

## Repository Structure

```
shape-up-analysis-skills/
├── CLAUDE.md           # This file
├── AGENTS.md           # Detailed agent instructions
├── README.md           # User documentation
├── LICENSE             # MIT license
└── skills/
    └── shape-up-analysis/
        ├── SKILL.md              # Main skill definition
        ├── references/           # Supporting documentation
        │   ├── shape-up-methodology.md
        │   ├── risk-assessment.md
        │   └── codebase-analysis.md
        └── examples/             # Example inputs and outputs
            ├── example-pitch.md
            └── example-output.md
```

## Available Skills

| Skill | Trigger Phrases |
|-------|-----------------|
| `shape-up-analysis` | "analyze this pitch", "review Shape Up pitch", "kickoff analysis", "identify risks", "assess this bet", "prepare project kickoff" |

## Skill Behavior

When triggered, the skill:

1. **Validates inputs** - Ensures pitch document is provided
2. **Analyzes pitch** - Extracts problem, appetite, solution, rabbit holes, no-gos
3. **Explores codebase** - Maps requirements to actual code
4. **Assesses risks** - Identifies known risks, rabbit holes, unknowns
5. **Produces output** - Generates `project-kickoff-analysis.md`

## Key Outputs

The analysis produces:
- Bet confidence rating (1-5 stars)
- Risk matrix with mitigation strategies
- Code area assessment with confidence levels
- Open questions blocking commitment
- Suggested spikes and next steps

## Reference Files

Load these as needed for detailed guidance:

- `references/shape-up-methodology.md` - Shape Up concepts and terminology
- `references/risk-assessment.md` - Risk categorization and mitigation frameworks
- `references/codebase-analysis.md` - Systematic codebase review approach

## Examples

See `examples/` for:
- `example-pitch.md` - Well-formed Shape Up pitch
- `example-output.md` - Complete analysis output

## Development Standards

- Skills use YAML frontmatter with `name` and `description`
- SKILL.md should remain under 500 lines
- Reference files for detailed content
- Examples demonstrate expected quality
