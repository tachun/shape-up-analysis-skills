# Shape Up Analysis Skills

Agent skills for analyzing Shape Up pitch documents alongside codebases to identify risks, rabbit holes, and unknowns before project kickoff.

## What This Does

This skill transforms Claude into a specialized analyst that reviews Shape Up pitches with dual PM and Developer perspectives:

- **PM View**: Validates problem clarity, success criteria, and business value
- **Dev View**: Maps pitch requirements to actual code, identifies hidden complexity
- **Risk Analysis**: Categorizes known risks, rabbit holes, and unknowns with mitigations
- **Actionable Output**: Produces comprehensive kickoff document with confidence rating

## Available Skills

| Skill | Description |
|-------|-------------|
| `shape-up-analysis` | Comprehensive pitch & codebase analysis for Shape Up projects |

## Installation

Install via Claude Code CLI:

```bash
npx @anthropic-ai/claude-code skills add tachunwu/shape-up-analysis-skills
```

Or manually copy `skills/shape-up-analysis/` to your `.claude/skills/` directory.

## Usage

Once installed, trigger the skill by asking Claude to:

- "Analyze this Shape Up pitch"
- "Review this pitch against the codebase"
- "Identify risks and rabbit holes in this pitch"
- "Prepare a kickoff analysis"
- "Assess this bet"
- "Evaluate this pitch for the betting table"

Provide your pitch document and Claude will analyze it against the current codebase.

## Output

The skill generates a `project-kickoff-analysis.md` with:

1. **Project Summary** - TL;DR of problem, timing, and beneficiaries
2. **Problem Understanding** - PM perspective on problem clarity and success criteria
3. **Solution Critical Review** - Scope boundaries and assumption assessment
4. **Risks & Rabbit Holes** - Categorized risks with likelihood, impact, and mitigations
5. **Codebase Review** - Affected code areas with confidence levels and health signals
6. **Constraints & Boundaries** - Time box and technical constraints
7. **Open Questions** - Blocking questions with proceed/risk assessment
8. **Bet Confidence** - 1-5 star rating with supporting factors
9. **Suggested Next Steps** - Timeboxed spikes, experiments, and decisions needed
10. **Final Notes** - Uncomfortable truths and accepted trade-offs

## Repository Structure

```
shape-up-analysis-skills/
├── CLAUDE.md                 # Repository guide
├── AGENTS.md                 # Detailed agent instructions
├── README.md                 # This file
├── LICENSE                   # MIT license
└── skills/
    └── shape-up-analysis/
        ├── SKILL.md          # Main skill definition
        ├── references/
        │   ├── shape-up-methodology.md   # Shape Up concepts & terminology
        │   ├── risk-assessment.md        # Risk frameworks & scoring
        │   └── codebase-analysis.md      # Code review approach
        └── examples/
            ├── example-pitch.md          # Sample pitch document
            └── example-output.md         # Sample analysis output
```

## Confidence Rating Scale

| Rating | Meaning | Guidance |
|--------|---------|----------|
| ⭐⭐⭐⭐⭐ | High confidence | Green light, execute |
| ⭐⭐⭐⭐☆ | Good confidence | Proceed with monitoring |
| ⭐⭐⭐☆☆ | Moderate confidence | Address key risks first |
| ⭐⭐☆☆☆ | Low confidence | Needs more shaping or spikes |
| ⭐☆☆☆☆ | Very low confidence | Not ready to bet |

## About Shape Up

[Shape Up](https://basecamp.com/shapeup) is Basecamp's product development methodology featuring:

- **Fixed time, variable scope** - Appetite sets the budget, scope flexes to fit
- **Pitches** - Shaped proposals with problem, solution, rabbit holes, and no-gos
- **Betting table** - Leadership decides which pitches get cycles
- **Six-week cycles** - Big batches of focused work
- **Circuit breaker** - Projects that don't converge get cut, not extended

This skill helps teams validate pitches before committing development time, reducing wasted effort and improving bet quality.

## Contributing

1. Fork the repository
2. Create skill under `skills/{skill-name}/SKILL.md`
3. Keep SKILL.md under 500 lines; use `references/` for details
4. Include examples demonstrating expected quality
5. Submit pull request

## License

MIT
