# Risk Assessment Framework

Structured approach to identifying and categorizing risks in Shape Up pitches.

## Risk Categories

### 1. Technical Risks

**Integration Risks**
- Third-party API dependencies
- External service reliability
- Authentication/authorization touchpoints
- Data sync requirements

**Architecture Risks**
- Cross-cutting concerns (logging, auth, caching)
- Database schema changes
- Migration requirements
- Performance at scale

**Legacy Code Risks**
- Undocumented behavior
- Missing tests
- Tight coupling
- Technical debt accumulation

### 2. Product Risks

**Scope Risks**
- Ambiguous requirements
- Hidden complexity in "simple" features
- Undefined edge cases
- Implicit expectations

**User Risks**
- Assumptions about user behavior
- Missing user research
- Accessibility requirements
- Localization needs

**Business Risks**
- Compliance requirements
- Legal constraints
- Stakeholder alignment
- Market timing

### 3. Process Risks

**Team Risks**
- Knowledge gaps
- Dependencies on specific people
- Context switching
- Onboarding needs

**Timeline Risks**
- External dependencies
- Review/approval bottlenecks
- Testing requirements
- Deployment windows

## Risk Assessment Matrix

Rate each risk:

| Likelihood | Impact | Priority |
|------------|--------|----------|
| High | High | 🔴 Critical - Must address before starting |
| High | Medium | 🟠 High - Needs mitigation plan |
| High | Low | 🟡 Medium - Monitor closely |
| Medium | High | 🟠 High - Needs mitigation plan |
| Medium | Medium | 🟡 Medium - Plan mitigation |
| Medium | Low | 🟢 Low - Accept risk |
| Low | High | 🟡 Medium - Have contingency |
| Low | Medium | 🟢 Low - Accept risk |
| Low | Low | 🟢 Low - Accept risk |

## Rabbit Hole Identification

### Common Rabbit Holes

**The "Simple" UI Change**
- Looks like CSS tweak
- Actually requires state management redesign
- **Signal**: "Just move this button" touching complex components

**The Data Migration**
- Looks like schema change
- Actually requires backfill, validation, rollback plan
- **Signal**: Any database structure change

**The "Quick" Integration**
- Looks like API call
- Actually requires auth, error handling, retry logic, monitoring
- **Signal**: New external dependency

**The Edge Case Explosion**
- Looks like single feature
- Actually N × M combinations
- **Signal**: "For each X, do Y" requirements

**The Legacy Entanglement**
- Looks like isolated change
- Actually wired into deprecated systems
- **Signal**: Touching old code without tests

### Rabbit Hole Questions

Ask these to surface hidden complexity:

1. What happens when this fails?
2. What's the rollback plan?
3. How does this work for existing users/data?
4. What's the migration path?
5. How does this interact with [adjacent feature]?
6. What happens at scale (10x, 100x current)?
7. What about [edge case user type]?

## Unknown Classification

### Known Knowns
- Clear requirements, understood implementation
- Can estimate with confidence
- **Action**: Execute

### Known Unknowns
- Questions we know to ask
- Can investigate with timeboxed spike
- **Action**: Spike before committing

### Unknown Unknowns
- Surprises during implementation
- Can't fully prevent, only mitigate
- **Action**: Build in buffer, plan for discovery

## Spike Framework

When unknowns need investigation:

### Spike Definition
```markdown
**Question**: What specific question does this answer?
**Timebox**: Maximum time allowed (hours/days)
**Success criteria**: What output indicates resolution?
**Go/No-go**: What decision does this inform?
```

### Spike Types

| Type | Purpose | Output |
|------|---------|--------|
| Technical | Validate approach works | Proof of concept |
| Design | Validate solution fits | Prototype |
| Data | Understand data shape | Analysis document |
| Integration | Validate external system | Working connection |

## Mitigation Strategies

### For High-Impact Risks

1. **Eliminate**: Remove risky requirement
2. **Reduce**: Simplify approach
3. **Transfer**: Use proven solution/library
4. **Accept**: Acknowledge and monitor

### For Rabbit Holes

1. **Timebox**: Set hard limit, cut if exceeded
2. **Defer**: Move to separate cycle
3. **Simplify**: 80/20 the solution
4. **Spike First**: Investigate before committing

## Confidence Scoring

### Bet Confidence Levels

| Score | Meaning | Guidance |
|-------|---------|----------|
| ⭐⭐⭐⭐⭐ | High confidence | Green light, execute |
| ⭐⭐⭐⭐☆ | Good confidence | Proceed with monitoring |
| ⭐⭐⭐☆☆ | Moderate confidence | Address key risks first |
| ⭐⭐☆☆☆ | Low confidence | Needs more shaping or spikes |
| ⭐☆☆☆☆ | Very low confidence | Not ready to bet |

### Confidence Factors

**Increases confidence:**
- Clear problem definition
- Proven technical approach
- Good test coverage in affected areas
- Team has domain knowledge
- Similar past successes

**Decreases confidence:**
- Vague requirements
- Novel technical territory
- Legacy code involvement
- External dependencies
- Tight timeline + large scope
