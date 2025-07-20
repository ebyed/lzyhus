# End-to-End Zero-Effort Gifting Journey

> "Just sign up once and never miss a special occasion again."  — Product Promise

This document stitches together every major user & system interaction from signup to annual renewal, aligning feature specs, personas, and KPIs.

## 1. Journey Stage Map

| # | Stage | Primary Actor | Goal | Key Feature(s) |
|---|-------|---------------|------|----------------|
| 1 | Awareness & Signup | Lazy Husband | Understand value, pick plan, complete UPI AutoPay | Payment & Subscription |
| 2 | Setup | Lazy Husband | Save important dates & recipient tastes in <2 min | Date-Reminder, Gift Selection Engine |
| 3 | Ongoing Automations | System | Run reminders + gift matching autonomously | Date-Reminder, Gift Selection Engine |
| 4 | Order & Fulfilment | System + Vendor | Deliver gift on time, track status | Fulfilment & Shipping |
| 5 | Unboxing & Delight | Wife / Recipient | Feel surprised & appreciated | Fulfilment, Feedback Loop |
| 6 | Renewal / Churn Loop | System + Lazy Husband | Maintain subscription; recover failures | Payment & Subscription |

### Swim-lane Diagram (text)
```
Lazy Husband → System → Vendor → Wife
Awareness & Signup ────────────────────────────────▶
              Setup ───────────────────────────────▶
Ongoing Automations → Gift Selection → Vendor Order →
                               Shipping → Unboxing →
                    Renewal / Churn ───────────────▶
```

## 2. Touch-Point Matrix

| Stage | Lazy Husband | Wife / Recipient | Internal Ops |
|-------|--------------|------------------|--------------|
| Signup | Pricing page, plan confirmation | — | — |
| Reminder | WhatsApp "Heads-up" | — | — |
| Gift Picked | Push + email summary | — | Gift QA checklist |
| Shipping | Tracking link | Tracking link | Exception dashboard |
| Delivery | "Delivered" ping | Gift + QR feedback card | SLA monitor |
| Renewal | Invoice & success notice | — | Revenue report |

## 3. Feature Alignment
- **Date-Reminder** → Drives T-7 & T-1 notifications  
- **Gift Selection Engine** → Matches SKUs using persona prefs  
- **Payment & Subscription** → Gated before placing vendor order  
- **Fulfilment & Shipping** → Vendor API integration, tracking  

## 4. KPIs per Stage
| Stage | North-Star Metric | Supporting Signals |
|-------|------------------|--------------------|
| Signup | Conversion % | CAC, Session → Plan time |
| Automations | Reminder → Gift selection success rate | Algorithm CTR |
| Fulfilment | On-time delivery % | Avg SLA deviation |
| Delight | Recipient NPS | QR feedback rate |
| Renewal | Auto-renew success % | Churn reason codes |

## 5. Doc Links
- Lazy Husband Journey → `/user-journeys/lazy-husband.md`  
- Wife Journey → `/user-journeys/wife.md`  
- Feature specs in `/features/`

---
_Last updated 2025-07-20_
