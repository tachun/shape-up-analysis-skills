# Codebase Analysis Guide

Systematic approach to analyzing a codebase against pitch requirements.

## Initial Reconnaissance

### 1. Architecture Overview

**Questions to answer:**
- What's the tech stack? (languages, frameworks, databases)
- What's the project structure? (monorepo, microservices, monolith)
- How is code organized? (by feature, by layer, mixed)
- What are the main entry points?

**Key files to check:**
- `package.json`, `requirements.txt`, `Gemfile`, `go.mod` - dependencies
- `docker-compose.yml`, `Dockerfile` - infrastructure
- `README.md`, `docs/` - documentation
- Config files - environment setup

### 2. Dependency Map

**Internal dependencies:**
- How do modules/packages depend on each other?
- Are there circular dependencies?
- What's shared vs isolated?

**External dependencies:**
- Third-party libraries and versions
- API integrations
- External services

## Code Health Assessment

### Test Coverage Analysis

| Coverage Level | Risk Level | Notes |
|----------------|------------|-------|
| >80% | Low | Safe to modify |
| 50-80% | Medium | Proceed with caution |
| 20-50% | High | Needs test investment |
| <20% | Critical | High regression risk |

**What to check:**
- Unit test presence and quality
- Integration tests for critical paths
- E2E tests for user flows
- Test data and fixtures

### Complexity Indicators

**High complexity signals:**
- Files >500 lines
- Functions >50 lines
- Deep nesting (>4 levels)
- High cyclomatic complexity
- Many dependencies per file

**Technical debt markers:**
- `TODO`, `FIXME`, `HACK` comments
- Disabled tests (`skip`, `pending`)
- Commented-out code blocks
- Duplicated code patterns
- Inconsistent patterns across codebase

### Documentation Status

| Level | Description |
|-------|-------------|
| Well documented | README, inline docs, API docs, architecture docs |
| Partially documented | Some README, sparse comments |
| Poorly documented | Minimal or outdated docs |
| Undocumented | No meaningful documentation |

## Affected Area Analysis

### Mapping Pitch to Code

For each pitch requirement:

1. **Identify entry points** - Where does this feature begin?
2. **Trace data flow** - How does data move through the system?
3. **Find exit points** - Where does this feature end?
4. **List touched files** - What needs modification?
5. **Identify dependencies** - What else could break?

### Impact Radius Assessment

```
                    Direct Impact
                         ↓
    [Core files to modify]
              |
              ↓
         Integration Points
              |
              ↓
    [Adjacent systems affected]
              |
              ↓
        Downstream Effects
              |
              ↓
    [Consumers, tests, docs]
```

### Confidence Levels for Code Areas

| Symbol | Meaning | Action |
|--------|---------|--------|
| 👍 | Well understood | Proceed confidently |
| 😐 | Needs exploration | Budget investigation time |
| 🚨 | High risk/unclear | Spike before committing |

**Factors affecting confidence:**
- Test coverage
- Documentation quality
- Code clarity
- Team familiarity
- Recent change history

## Technical Feasibility Check

### Can We Build It?

**Compatibility questions:**
- Does our stack support this?
- Do we have required dependencies?
- Are there version conflicts?
- Does it work in our environments?

**Performance questions:**
- Will this scale with our data?
- What's the expected load?
- Are there latency requirements?
- Database query implications?

**Security questions:**
- Authentication requirements?
- Authorization changes?
- Data privacy concerns?
- New attack surfaces?

### Integration Points Checklist

For each external touchpoint:

- [ ] API contract defined
- [ ] Authentication method clear
- [ ] Error handling approach
- [ ] Rate limits understood
- [ ] Fallback behavior defined
- [ ] Monitoring/logging planned

## Code Patterns to Flag

### Red Flags

**Architecture smells:**
- God classes/files doing too much
- Circular dependencies
- Business logic in controllers
- Hardcoded configurations
- Missing abstraction layers

**Data smells:**
- Raw SQL strings in business logic
- Missing input validation
- Unhandled null cases
- Inconsistent data formats

**Process smells:**
- No CI/CD pipeline
- Manual deployment steps
- Missing environment parity
- No database migrations

### Yellow Flags

**Maintainability concerns:**
- Inconsistent naming conventions
- Mixed programming paradigms
- Outdated dependencies
- Incomplete error handling

**Scalability concerns:**
- N+1 query patterns
- Synchronous operations that could be async
- Missing caching opportunities
- Unbounded data operations

## Output Template

### Per-Area Assessment

```markdown
## Area: `path/to/module`

**Purpose**: What this code does

**Relevance**: Why it's involved in this pitch

**Health signals**:
- Test coverage: X%
- Documentation: [Well/Partial/Poor/None]
- Complexity: [Low/Medium/High]
- Last modified: [Date]

**Key files**:
- `file1.ts` - Description
- `file2.ts` - Description

**Dependencies**:
- Internal: [List]
- External: [List]

**Risks identified**:
- Risk 1
- Risk 2

**Confidence**: 👍/😐/🚨

**Notes**: Additional observations
```

## Questions to Ask the Code

1. **What breaks if we change this?**
2. **What's the blast radius of a bug here?**
3. **How do we know this works today?**
4. **What's the deployment story?**
5. **Who knows this code best?**
6. **When was this last touched and why?**
7. **What's the testing story?**
8. **What's not tested that should be?**
