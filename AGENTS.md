# Shape Up Analysis Skills - Agent Instructions

Detailed guidance for AI agents working with these skills.

## Project Overview

Shape Up Analysis Skills enables systematic evaluation of Shape Up pitch documents against actual codebases. The goal is to surface risks, rabbit holes, and unknowns before committing development time.

## Core Philosophy

### Dual Perspective Analysis

Every analysis requires thinking as both:

**Senior PM:**
- Validate problem clarity and business value
- Challenge assumptions about user behavior
- Assess scope against appetite
- Identify product risks

**Senior Developer:**
- Map pitch requirements to code
- Assess technical feasibility
- Identify hidden complexity
- Evaluate code health in affected areas

### Principles

1. **Clarity over optimism** - Don't sugarcoat risks to make stakeholders happy
2. **Time is limited** - Shape Up has fixed appetites; honor the constraint
3. **Be constructive** - Every risk should have a mitigation suggestion
4. **Challenge assumptions** - Mark each as Safe/Risky/Unknown

## Skill Structure

```
skills/shape-up-analysis/
├── SKILL.md                    # Core instructions (always loaded)
├── references/
│   ├── shape-up-methodology.md # Load when explaining Shape Up concepts
│   ├── risk-assessment.md      # Load for risk categorization
│   └── codebase-analysis.md    # Load for technical review
└── examples/
    ├── example-pitch.md        # Reference for pitch format
    └── example-output.md       # Reference for output quality
```

## Analysis Process

### Phase 1: Input Validation

Before analysis, verify:
- [ ] Pitch document provided
- [ ] Pitch contains: problem, appetite, solution
- [ ] Codebase access available

If pitch missing, ask user before proceeding.

### Phase 2: Pitch Understanding

Extract from pitch:
- **Problem**: What pain point? Who suffers? Why now?
- **Appetite**: Time budget (2 weeks small, 6 weeks big)
- **Solution**: Proposed approach and scope
- **Rabbit holes**: Complexity to avoid/timebox
- **No-gos**: Explicit exclusions

Flag if any core element is missing or vague.

### Phase 3: Codebase Exploration

For each pitch requirement:

1. **Identify affected areas**
   - Use Glob/Grep to find relevant code
   - Map features to specific files/modules

2. **Assess code health**
   - Test coverage (use coverage reports if available)
   - Complexity signals (file size, nesting depth)
   - Documentation presence
   - Recent change history (git log)

3. **Map dependencies**
   - Internal: module-to-module
   - External: third-party services, APIs

4. **Note technical debt**
   - TODO/FIXME comments
   - Disabled tests
   - Legacy patterns

### Phase 4: Risk Analysis

Categorize findings:

| Category | Definition | Example |
|----------|------------|---------|
| Known Risk | Identified problem with clear impact | "Auth service has 30% test coverage" |
| Rabbit Hole | Could explode in scope | "Simple UI change touches pricing engine" |
| Unknown | Can't assess without investigation | "Stripe API behavior during pause unclear" |

Use risk matrix from `references/risk-assessment.md`.

### Phase 5: Synthesis

Produce confidence rating:

- ⭐⭐⭐⭐⭐ - High confidence, clear path forward
- ⭐⭐⭐⭐☆ - Good, minor unknowns
- ⭐⭐⭐☆☆ - Moderate, needs investigation
- ⭐⭐☆☆☆ - Low, significant unknowns
- ⭐☆☆☆☆ - Not ready to bet

## Output Requirements

### Format

Generate single file: `project-kickoff-analysis.md`

Use exact structure from SKILL.md output template.

### Quality Standards

**TL;DR section:**
- One paragraph maximum
- Problem + why now + who benefits
- No technical jargon

**Risk sections:**
- Every risk has likelihood + impact
- Every risk has mitigation strategy
- Rabbit holes include concrete examples

**Codebase sections:**
- Specific file paths, not vague descriptions
- Confidence indicators (👍/😐/🚨) for each area
- Test coverage percentages where available

**Open questions:**
- Each has "Can Proceed?" assessment
- Each has "Risk if Unanswered" rating
- Prioritized by blocker status

**Next steps:**
- Spikes are timeboxed with specific questions
- Decisions have clear owners implied
- Nothing vague or open-ended

## Common Patterns

### Pattern: Missing Pitch Element

When pitch lacks required element:

```markdown
**Note**: This pitch does not include explicit rabbit holes.
Based on codebase analysis, the following should be considered
rabbit holes: [list discovered risks]
```

### Pattern: High-Risk Code Area

When code area is concerning:

```markdown
| `src/services/pricing/` | Pricing calculations | 🚨 High risk |

**Concerns:**
- 32% test coverage
- 400-line calculatePrice.js
- TODOs mentioning "legacy handling"
- Last meaningful refactor: 18 months ago

**Recommendation:** Spike before committing to changes here.
```

### Pattern: Unclear Requirement

When pitch requirement is ambiguous:

```markdown
### 3.2 Assumptions

| "Users get read-only access" | Product | ❓ Unknown |

**Why unknown:** Pitch doesn't define which features become read-only
vs. fully blocked. This could mean:
- A) Premium features show but can't save
- B) Premium features completely hidden
- C) Some features read-only, others blocked

**Recommendation:** Product decision needed before implementation.
```

### Pattern: Missing Codebase Context

When you can't find expected code:

```markdown
**Note:** Could not locate subscription pause handling in codebase.
This appears to be net-new functionality. Searched:
- `src/services/subscription/` - no pause logic
- `src/services/billing/` - no pause references
- Database schema - no pause-related fields

**Implication:** Scope may be underestimated; no existing patterns to extend.
```

## Anti-Patterns to Avoid

### Don't: Be Vaguely Positive

❌ "The codebase looks generally healthy."

✅ "Subscription service: 78% test coverage, clear separation of concerns. Pricing service: 32% coverage, complex calculatePrice.js (400 lines) flagged as risk."

### Don't: List Risks Without Mitigation

❌ "Risk: Stripe API may not support pause."

✅ "Risk: Stripe API may not support pause. **Mitigation:** Run 2-day spike to test pause_collection API in sandbox before committing."

### Don't: Accept Vague Requirements

❌ "The read-only mode should be straightforward."

✅ "Read-only mode requires definition. Questions: Which features? Show disabled or hide? What happens to in-progress work? Recommend product decision before starting."

### Don't: Ignore the Appetite

❌ [Analysis that ignores time constraint]

✅ "At 6-week appetite, full pricing service refactor is out of scope. Recommend: isolate pause handling from core pricing logic; defer cleanup to future cycle."

## Reference Loading

Load reference files based on need:

| Situation | Load |
|-----------|------|
| Explaining Shape Up concepts | `references/shape-up-methodology.md` |
| Categorizing risks | `references/risk-assessment.md` |
| Reviewing code areas | `references/codebase-analysis.md` |
| Checking output format | `examples/example-output.md` |
| Validating pitch format | `examples/example-pitch.md` |
