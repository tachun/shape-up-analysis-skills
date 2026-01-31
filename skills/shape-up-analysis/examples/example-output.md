# Project Kickoff – Pitch & Codebase Analysis

## 1. Project Summary (TL;DR)

We're building a subscription pause feature to reduce churn from customers facing temporary financial constraints. Instead of forcing an all-or-nothing cancel decision, users can pause for 1-3 months and automatically resume. This matters now because 34% of cancellations cite financial reasons—recoverable revenue we're currently losing.

---

## 2. Understanding the Problem (PM View)

### 2.1 Problem Statement
- **Real problem**: Customers with temporary financial issues have no middle ground between "keep paying" and "cancel forever"
- **Clearly defined**: Yes, supported by exit survey data (34% cite financial constraints)
- **Not a solution in disguise**: Correct—the problem is customer loss, pause is one possible solution

### 2.2 Why Now?
- **Trigger**: Exit survey data showing recoverable churn
- **Cost of not doing this**: Continued loss of customers who would otherwise return; competitors offering pause options

### 2.3 Success Criteria
- **How we know it worked**:
  - Reduction in cancellations (target: 15% of would-be cancellations choose pause instead)
  - Pause-to-resume rate >70%
- **Measurable**: Pause initiation rate, resume rate, LTV of paused vs. canceled users
- **Good enough**: Basic pause/resume flow working; polish can come later

---

## 3. Proposed Solution (Critical Review)

### 3.1 Solution Overview
- **Proposed**: Pause subscription for 1-3 months with auto-resume

**In scope:**
- Pause initiation from account settings
- Duration selection (1/2/3 months)
- Read-only access during pause
- Resume reminder emails
- Auto-resume with billing restart

**Out of scope:**
- Team/family plans
- Annual subscriptions
- Admin override
- Partial feature pause
- Advance scheduling

### 3.2 Assumptions

| Assumption | Type | Status |
|------------|------|--------|
| Payment provider supports pause without full cancel | Technical | ⚠️ Risky - Need to verify Stripe subscription pause API |
| Users understand "read-only access" | Product | ❓ Unknown - May need user testing |
| One pause per year prevents abuse | Product | ✅ Safe - Industry standard |
| Pausing starts on next billing date (no proration) | Technical | ✅ Safe - Simplifies implementation |
| Grandfathered pricing preserved on resume | Technical | ⚠️ Risky - Need to check pricing service behavior |
| Email service can handle scheduled reminders | Technical | ✅ Safe - Already used for renewal reminders |

---

## 4. Risks & Rabbit Holes

### 4.1 Known Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Stripe API doesn't support pause the way we need | Medium | High | Spike first; fallback to manual status management |
| Pricing service resets grandfathered prices | Medium | High | Test in staging with legacy-priced account |
| Read-only mode unclear to users | Medium | Medium | Clear UI states; help documentation |
| Edge case: payment fails on resume | Low | Medium | Existing failed payment flow applies |

### 4.2 Potential Rabbit Holes

1. **Stripe subscription state management** - Pitch says "no integration with payment provider's native pause" but we still need Stripe to not charge. Could be complex if Stripe's pause doesn't match our needs.

2. **"Read-only" feature gating** - What exactly is read-only? Each premium feature needs pause-state handling. Could explode if we have many gated features.

3. **Grandfathered pricing preservation** - The pricing service may not be designed to "remember" old prices through a pause. Touching this could open a can of worms.

4. **Email timing edge cases** - What if user resumes before the 7-day reminder? What if they pause again? Email logic could get complex.

---

## 5. Codebase Review (Dev View)

### 5.1 High-Level Architecture
- **Backend**: Node.js/Express API with PostgreSQL
- **Billing**: Stripe integration via `stripe-node` SDK
- **Frontend**: React SPA with Redux state management
- **Email**: SendGrid with template system
- **Background jobs**: Bull queue for scheduled tasks

### 5.2 Relevant Areas

| Area | Why It Matters | Confidence |
|------|---------------|------------|
| `src/services/billing/` | Stripe integration, subscription lifecycle | 😐 Needs exploration |
| `src/services/subscription/` | Subscription state management | 👍 Well understood |
| `src/models/User.js` | User model, subscription status field | 👍 Well understood |
| `src/services/pricing/` | Pricing logic, grandfathered price handling | 🚨 High risk |
| `src/middleware/featureGate.js` | Premium feature access control | 😐 Needs exploration |
| `src/jobs/` | Background job scheduling | 👍 Well understood |
| `src/services/email/` | Email templates and scheduling | 👍 Well understood |
| `frontend/src/pages/AccountSettings/` | Account settings UI | 👍 Well understood |

### 5.3 Code Health Signals

**Test coverage:**
- `billing/`: 45% - Medium risk
- `subscription/`: 78% - Good
- `pricing/`: 32% - High risk ⚠️
- `featureGate.js`: 85% - Good

**Complexity hotspots:**
- `src/services/pricing/calculatePrice.js` - 400 lines, high cyclomatic complexity
- `src/services/billing/stripeWebhooks.js` - 500+ lines, many edge cases

**Technical debt:**
- `pricing/` has TODO comments about "legacy price handling"
- `billing/` has inconsistent error handling patterns
- No integration tests for Stripe webhook flows

---

## 6. Constraints & Boundaries

### 6.1 Time Box
- **Fixed appetite**: 6 weeks
- **Must fit**: Core pause/resume flow, email reminders, read-only state
- **Can drop**: Polish UI animations, detailed analytics dashboard, edge case email scenarios

### 6.2 Technical Constraints
- Must use existing Stripe integration
- Must preserve existing pricing logic
- Email templates follow existing design system
- No new infrastructure (use existing Bull queues)

---

## 7. Open Questions

| Question | Type | Can Proceed? | Risk if Unanswered |
|----------|------|--------------|-------------------|
| Does Stripe `pause_collection` preserve subscription state correctly? | Tech | No | High - Core functionality |
| How does pricing service handle grandfathered prices on subscription changes? | Tech | No | High - Could reset prices |
| What exactly becomes "read-only" during pause? (Feature list) | Product | Yes | Medium - Can define incrementally |
| Should paused users count toward team seat limits? | Product | Yes | Low - Edge case for later |
| What analytics events do we need to track? | Product | Yes | Low - Can add post-launch |

---

## 8. Bet Confidence

**Overall: ⭐⭐⭐☆☆** (3/5 - Moderate confidence)

### Why This Rating

**Positive factors:**
- Clear problem with data backing (34% exit survey)
- Well-scoped pitch with explicit no-gos
- Most affected code areas well understood
- Existing infrastructure (jobs, email) supports requirements

**Negative factors:**
- Pricing service is high-risk, low-test area
- Stripe integration details unknown—could force architecture changes
- "Read-only mode" not fully specified
- Two critical unknowns blocking confident commitment

### What Would Increase Confidence
- Spike on Stripe pause_collection API behavior
- Pricing service investigation (grandfathered price handling)
- Clear definition of read-only feature list

### What Would Kill the Bet
- Stripe doesn't support pause without cancel+resubscribe
- Pricing service requires major refactor to preserve prices
- Scope creep into team plans or annual billing

---

## 9. Suggested Next Steps

### Spikes to Run
- [ ] **Stripe Pause Spike (2 days)**: Test `pause_collection` API in sandbox. Verify subscription preserves price, status, and metadata through pause/resume cycle.
- [ ] **Pricing Service Audit (1 day)**: Trace grandfathered price logic. Test in staging: modify subscription → verify price preserved.

### Experiments to Validate
- [ ] **Read-only mode prototype**: Build quick prototype showing paused state in 2-3 key features to validate UX approach.

### Decisions Needed Before Building
- [ ] **Feature list for read-only**: Product to define exactly which features become read-only vs. fully blocked.
- [ ] **Pause limit policy**: Confirm one pause per year, or define alternative abuse prevention.
- [ ] **Communication plan**: What emails/in-app messaging during pause period beyond 7-day reminder?

---

## 10. Final Notes

**Uncomfortable truths:**
- The pricing service is a known problem area. This project will likely surface tech debt that's been ignored.
- "Read-only mode" is doing a lot of work in this pitch—implementation may be more complex than it appears.

**Trade-offs we're accepting:**
- No admin tools in v1 means support can't manually pause/resume for customers
- No partial pause means we lose flexibility for future product tiers
- Starting on next billing date means customers pay for the full current period even if they want to pause today
