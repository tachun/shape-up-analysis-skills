# Example Pitch: Subscription Pause Feature

## Problem

Customers who can't afford their monthly subscription are canceling entirely. We lose them forever, along with the relationship we've built. Exit surveys show 34% of cancellations cite "temporary financial constraints" as the reason.

We're leaving money on the table by forcing an all-or-nothing choice.

## Appetite

**6 weeks** (Big batch)

## Solution

Allow customers to pause their subscription for 1-3 months instead of canceling.

### Core Flow

**Pause initiation:**
- Add "Pause subscription" option to account settings (next to Cancel)
- User selects pause duration: 1, 2, or 3 months
- Show clear resume date and what they'll lose during pause
- Confirm and pause immediately

**During pause:**
- No billing
- Account remains accessible (read-only for premium features)
- Reminder email 7 days before resume
- Easy one-click resume anytime

**Resume:**
- Auto-resume on selected date
- Billing restarts on resume
- Full access restored

### Breadboard

```
[Account Settings] → [Pause Option] → [Duration Picker] → [Confirmation]
                                                               ↓
                                                      [Pause Active State]
                                                               ↓
                                              [Resume Reminder] → [Auto Resume]
```

## Rabbit Holes

1. **Proration complexity** - Don't prorate. Pause starts on next billing date, not immediately. Simpler billing logic.

2. **Multiple pause abuse** - Limit to one pause per year. Don't build complex fraud detection.

3. **Team/family plans** - Out of scope for v1. Only individual subscriptions.

4. **Grandfathered pricing** - Paused users keep their original price when resuming. Don't rebuild pricing engine.

5. **Annual subscriptions** - Out of scope. Only monthly billing.

## No-Gos

- No partial pause (can't pause just some features)
- No pause scheduling in advance
- No refunds for current period when pausing
- No integration with payment provider's native pause (too complex)
- No admin override/manual pause capability in v1
