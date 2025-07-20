# Entity: Lazy Husband (Primary User)

## Snapshot
- **Role:** Primary account holder & payer
- **Motivation:** Maintain relationship harmony without spending time planning gifts
- **Pain Points:** Forgets dates, last-minute stress, fear of disappointing partner

## Key Attributes
| Attribute | Type | Notes |
|-----------|------|-------|
| user_id | UUID | Primary key |
| name | String | First name for personalization |
| email | String | Login & notifications |
| phone | String | SMS reminders |
| spouse_name | String | For gift note personalization |
| key_dates | List<Date> | Anniversary, birthday, etc. |
| gift_budget | Currency | Default spend per occasion |
| preferences | Enum list | e.g., Jewelry, Experiences, Flowers |
| payment_method | Tokenized | Saved card on file |
| lazy_mode_enabled | Boolean | Grants auto-purchase consent |
| signup_source | Enum | Web, iOS, Android |

## Core Behaviors
1. Completes a **one-time onboarding** flow: enters spouse details, key dates, budget.
2. Toggles **Lazy Mode** to authorize automatic gift purchases.
3. Receives **reminder notifications** and can adjust budget/preferences before cutoff.
4. Gets **delivery confirmation** and can rate the gift (feedback loop).

## Edge Cases & Validation
- Missing payment method → disable Lazy Mode until resolved.
- Key date falls within <7 days of signup → prompt for manual approval.
- Budget too low for any item → suggest increase or fallback gift.

## Success Metrics (user-level)
- Retention: % users remaining in Lazy Mode after 6 months.
- Avg. affiliate commission generated per user.
- Gift satisfaction score from spouse feedback.

---
*Last updated: 2025-07-20*