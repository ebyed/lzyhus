# MVP Definition – Lazy Husband Gifting App

## 1. Overview
The MVP delivers a complete end-to-end gifting cycle for one relationship (husband → spouse) with **zero effort after initial set-up**. It proves:
• Customers are willing to trust the app with automated purchases.
• Affiliate revenue and/or subscription upsell is viable.
• Logistics (reminder → gift pick → purchase → delivery) can be executed reliably.

## 2. Must-Have Features (Launch Day)
| Capability | Description |
|------------|-------------|
| Account Sign-up & Profile | Email or social login, payment method capture (card on file) |
| Key-Date Manager | User enters anniversary & spouse birthday (at minimum) |
| Lazy-Mode Activation | Toggle that grants autopurchase consent, sets budget per gift |
| Gift Selection Engine v1 | Rule-based matching using basic preferences (category + price) |
| Order Placement & Tracking | Automated checkout via affiliate link + shipping tracking |
| Date Reminder Notifications | Heads-up email/SMS 7 days & 1 day before delivery |
| Delivery Confirmation | “Gift delivered” notification & feedback request |

## 3. Nice-to-Have (Post-MVP)
| Potential Addition | Rationale |
|--------------------|-----------|
| AI-Generated Love Notes | Increases delight; not core to proof of concept |
| Expanded Recipient Types | Unlocks more users but adds complexity |
| Concierge Premium Tier | Higher ARPU; can wait until base workflow is solid |
| Experience Gifts/Donations | Broader catalog; requires more vendor deals |

## 4. Explicitly Out of Scope
- Social-login wallets (Apple Pay, Google Pay)
- Same-day delivery network
- B2B employee gifting
- Multi-currency / localization

## 5. Dependencies & Assumptions
- At least three affiliate vendors with API-based checkout (e.g., Amazon, Etsy).
- Payment processor (Stripe) supports saved cards & scheduled charges.
- Shipping carriers provide tracking webhooks.
- Initial rule-based gift catalog curated manually.

## 6. Success Metrics (MVP timeframe)
- 95% on-time delivery for scheduled gifts.
- ≥ 40% users stay in Lazy-Mode after first cycle.
- ≥ $15 affiliate commission per active user (annualized run-rate).
- NPS ≥ 50 from recipients (spouse feedback survey).

## 7. Technical Notes & Milestones
1. Week 1–2: Core data schema, date manager, payment vault.
2. Week 3–4: Gift engine v1 & vendor integrations.
3. Week 5: Notification service & cron scheduler.
4. Week 6: Closed beta (10 users) → iterate on rules.
5. Week 8: Public MVP launch.

---
*Last updated: 2025-07-20*