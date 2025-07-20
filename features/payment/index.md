# Payment & Subscription – Feature Spec
_Last updated: 2025-07-20_

## 1. Problem & Value
We monetise through a **yearly subscription** that unlocks the automated gifting service. Payments must be frictionless (UPI-first), trustworthy, and compliant with RBI e-mandate rules.

## 2. Subscription Model (MVP)
| Tier | Price (₹/yr) | Key Benefits |
|------|-------------|--------------|
| Lazy Husband Pro | ₹999 | Auto gifts for one recipient, unlimited reminders, curated shortlist, free shipping on first gift |

Future roadmap: Multi-recipient add-on, monthly plan, corporate bulk.

### Pricing Rationale
• ₹999 aligns with average gifting spend and perceived convenience value.  
• 30-day trial converts at ~25% in comparable Indian SaaS products.

## 3. Billing Flows
### 3.1 UPI AutoPay (Recommended)
1. User approves e-mandate via UPI app (PhonePe/GPay).  
2. Collect standing instruction token; auto-debit yearly.

### 3.2 Card & Net Banking Backup
• Supports Visa/Mastercard with tokenisation.  
• Net banking via Razorpay Checkout.

### 3.3 Renewal & Grace
• Renewal attempt T-7 days, T-3, T-0.  
• 14-day grace period where reminders continue but gift automation pauses.

### 3.4 Refund Policy
• 100% refund within 14 days if no gift dispatched.  
• Prorated refund minus fulfilled orders beyond 14-day window.

## 4. Epics & Key Stories
### 4.1 Subscription Signup Epic
- US-SS-1: Display tier details & CTA.  
- US-SS-2: Initiate UPI AutoPay flow.

### 4.2 Payment Processing Epic
- US-PP-1: Handle payment success/failure webhooks.  
- US-PP-2: Store mandate reference IDs.

### 4.3 Renewal Management Epic
- US-RM-1: Cron job triggers renewal charge.  
- US-RM-2: Grace period logic & notification.

### 4.4 Refund & Cancellation Epic
- US-RC-1: User requests cancellation; calculate refund.  
- US-RC-2: Process Razorpay refund API.

## 5. MVP User Stories Snapshot
| ID | Story | Priority |
|----|-------|----------|
| US-SS-1 | Display subscription tier | P0 |
| US-SS-2 | UPI AutoPay mandate | P0 |
| US-PP-1 | Payment webhook handling | P0 |
| US-RM-1 | Renewal cron job | P1 |
| US-RC-1 | Cancel & refund | P2 |

## 6. Open Questions
1. Offer monthly plan at ₹99 or keep yearly only for simplicity?  
2. Partner with RazorpayX for payouts to vendors?  
3. GST invoice download needed for B2C segment?

## 7. Dependencies
• Razorpay Payment Gateway & Subscriptions API.  
• WhatsApp notifications for payment status.  
• Subscription flag check across Date-Reminder & Gift Engine.