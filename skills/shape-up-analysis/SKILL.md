---
name: shape-up-analysis
description: Analyze a Shape Up pitch document alongside a codebase to identify risks, rabbit holes, and unknowns. Produces comprehensive kickoff analysis with bet confidence rating. Use when asked to "analyze this pitch", "review Shape Up pitch", "kickoff analysis", "identify risks in this pitch", "what are the rabbit holes", "assess this bet", "prepare project kickoff", "evaluate this pitch", or when starting a new Shape Up cycle and need to understand scope vs codebase reality.
---

# Shape Up Pitch & Codebase Analysis

Analyze Shape Up pitches against actual codebase to produce comprehensive kickoff documents with risk assessment and confidence rating.

## References

Load as needed for detailed guidance:
- `references/shape-up-methodology.md` - Shape Up concepts, pitch anatomy, betting criteria
- `references/risk-assessment.md` - Risk categorization, rabbit hole identification, confidence scoring
- `references/codebase-analysis.md` - Systematic code review approach, health signals, impact mapping

## Examples

See for expected quality:
- `examples/example-pitch.md` - Well-formed Shape Up pitch
- `examples/example-output.md` - Complete analysis output demonstrating format and depth

## Required Inputs

1. **Pitch document** - Shape Up format (problem, appetite, solution, rabbit holes, no-gos)
2. **Codebase access** - Available in current repository

If pitch not provided, ask user before proceeding.

## Dual Perspective Analysis

Analyze as both **Senior PM** and **Senior Developer**:

**PM lens:**
- Is problem clearly defined or solution disguised as problem?
- Who benefits and why now?
- What does "good enough" look like?
- What's measurable?

**Dev lens:**
- What does affected code actually look like?
- What complexity hides in "simple" requirements?
- What technical debt will we encounter?
- What's missing from pitch that code reveals?

**Principles:**
- Clarity over optimism—don't sugarcoat
- Time is limited—honor the appetite
- Be constructive—risks need mitigations
- Challenge assumptions—mark as ✅ Safe / ⚠️ Risky / ❓ Unknown

## Process

### Phase 1: Understand Pitch

1. Read pitch document carefully
2. Extract: problem, appetite, solution, rabbit holes, no-gos
3. Identify explicit scope boundaries
4. Note assumptions (stated and implied)
5. Flag missing elements

### Phase 2: Codebase Exploration

1. Identify affected code areas using Glob/Grep
2. For each area:
   - Read relevant files
   - Assess health (tests, complexity, docs)
   - Note dependencies and integrations
   - Flag legacy or fragile code
3. Map pitch requirements to code paths
4. Find hidden complexity pitch doesn't mention

### Phase 3: Risk Analysis

1. Cross-reference pitch assumptions with codebase reality
2. Categorize:
   - **Known risks**: Identified problems with clear impact
   - **Rabbit holes**: Could explode in scope
   - **Unknowns**: Need spikes to assess
3. Rate each: likelihood (High/Med/Low) × impact (High/Med/Low)
4. Propose mitigations

### Phase 4: Synthesis

1. Rate bet confidence (1-5 stars)
2. List blocking questions
3. Suggest spikes, experiments, decisions
4. Note accepted trade-offs

## Output Format

Generate `project-kickoff-analysis.md`:

```markdown
# Project Kickoff – Pitch & Codebase Analysis

## 1. Project Summary (TL;DR)
One paragraph: problem, why now, who benefits. No jargon.

---

## 2. Understanding the Problem (PM View)

### 2.1 Problem Statement
- Real user/business problem?
- Clearly defined or solution disguised as problem?

### 2.2 Why Now?
- What triggered this pitch?
- Cost of not doing this?

### 2.3 Success Criteria
- How we know it worked
- What's measurable
- What's "good enough"

---

## 3. Proposed Solution (Critical Review)

### 3.1 Solution Overview
- Proposed solution summary
- Explicitly in scope
- Explicitly out of scope

### 3.2 Assumptions
| Assumption | Type | Status |
|------------|------|--------|
| Description | Technical/Product/User | ✅/⚠️/❓ |

---

## 4. Risks & Rabbit Holes

### 4.1 Known Risks
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Description | H/M/L | H/M/L | Strategy |

### 4.2 Potential Rabbit Holes
List with concrete examples:
- What looks simple but isn't
- What could explode in scope
- Hidden dependencies

---

## 5. Codebase Review (Dev View)

### 5.1 High-Level Architecture
Brief tech stack overview relevant to this pitch.

### 5.2 Relevant Areas
| Area | Why It Matters | Confidence |
|------|---------------|------------|
| `path/to/module` | Reason | 👍/😐/🚨 |

**Legend:** 👍 Well understood, 😐 Needs exploration, 🚨 High risk

### 5.3 Code Health Signals
- Test coverage by area
- Complexity hotspots
- Technical debt markers
- Documentation gaps

---

## 6. Constraints & Boundaries

### 6.1 Time Box
- Fixed appetite: X weeks
- Must fit vs. can drop

### 6.2 Technical Constraints
- Required/forbidden tech
- Performance/security limits

---

## 7. Open Questions

| Question | Type | Can Proceed? | Risk if Unanswered |
|----------|------|--------------|-------------------|
| Specific question | Product/Tech/Data | Yes/No | Low/Med/High |

---

## 8. Bet Confidence

**Overall: ⭐⭐⭐☆☆** (X/5)

**Positive factors:**
- List

**Negative factors:**
- List

**What would increase confidence:**
- List

**What would kill the bet:**
- List

---

## 9. Suggested Next Steps

### Spikes to Run
- [ ] Spike name (timebox): Specific question to answer

### Experiments to Validate
- [ ] Experiment: What to test

### Decisions Needed Before Building
- [ ] Decision: What needs resolving

---

## 10. Final Notes
- Uncomfortable truths
- Trade-offs we're accepting
```

## Quality Checklist

Before finalizing output:
- [ ] Every risk has mitigation strategy
- [ ] Code areas have specific paths, not vague descriptions
- [ ] Confidence ratings backed by evidence
- [ ] Open questions prioritized by blocker status
- [ ] Spikes are timeboxed with specific questions
- [ ] No vague or open-ended next steps
