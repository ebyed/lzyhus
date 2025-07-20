# Lazy Husband Gifting App - Compilation Knowledge Base

Generated on: 2025-07-20

## Table of Contents

- [/concept-note](#concept-note-md) (1.1k chars)
  - [lazy-husband](#entities-lazy-husband-md) (1.7k chars)
  - [vendor](#entities-vendor-md) (5k chars)
  - [wife](#entities-wife-md) (3.3k chars)
- [/feature-catalog](#feature-catalog-md) (2.2k chars)
  - [affiliate-partner-integration](#features-affiliate-partner-integration-md) (2.9k chars)
    - [index](#features-date-reminder-index-md) (2.7k chars)
    - [index](#features-fulfillment-index-md) (3.2k chars)
    - [index](#features-gift-selection-index-md) (3.7k chars)
  - [important-dates-input](#features-important-dates-input-md) (909 chars)
  - [index](#features-index-md) (2.7k chars)
    - [index](#features-payment-index-md) (2.5k chars)
  - [user-registration](#features-user-registration-md) (433 chars)
- [/mvp-scope](#mvp-scope-md) (500 chars)
  - [lazy-husband](#personas-lazy-husband-md) (465 chars)
  - [wife](#personas-wife-md) (461 chars)
- [/plan](#plan-md) (1.7k chars)
  - [lazy-husband-lovable](#prompts-lazy-husband-lovable-md) (2.1k chars)
  - [lovable-mvp](#prompts-lovable-mvp-md) (3.4k chars)
  - [delivery-tracking-notifications](#user-flows-delivery-tracking-notifications-md) (584 chars)
  - [feedback-rating](#user-flows-feedback-rating-md) (550 chars)
  - [gift-selection-ordering](#user-flows-gift-selection-ordering-md) (653 chars)
  - [lazy-mode-activation](#user-flows-lazy-mode-activation-md) (684 chars)
  - [lazy-mode-selection](#user-flows-lazy-mode-selection-md) (329 chars)
  - [notification-gifting](#user-flows-notification-gifting-md) (264 chars)
  - [overview](#user-flows-overview-md) (498 chars)
  - [sign-up-date-entry](#user-flows-sign-up-date-entry-md) (262 chars)
  - [index](#user-journeys-index-md) (2.8k chars)
  - [lazy-husband](#user-journeys-lazy-husband-md) (1.8k chars)
  - [wife](#user-journeys-wife-md) (1.4k chars)
- [/vision-mvp](#vision-mvp-md) (2.5k chars)
- [/vision](#vision-md) (3.2k chars)


**Summary:**
- Documents: 32
- Total characters: 56.6k chars

---

## Documents

### /concept-note.md

**Path:** `/concept-note.md`  
**Characters:** 1.1k chars  

# Concept Note

## Problem Statement
Forgetful husbands often miss key gifting occasions such as anniversaries and birthdays, leading to disappointment and relationship stress.

## Solution Approach
A done-for-you gifting service where the husband enters important dates once and selects a "lazy mode" option. The system then automatically picks, orders, and delivers perfect gifts on special occasions, requiring zero ongoing effort.

## Revenue Model
The platform earns money through affiliate partnerships with major e-commerce stores (e.g., Amazon Associates, Etsy, specialty boutiques).
- Every automated gift purchase is placed using the product’s referral links, generating commission on each order.
- The app can surface curated gift options only from merchants that honor referral payouts.
- Future upsell opportunities include premium subscription tiers for expedited shipping or concierge-level curation.

## Success Metrics
- Number of user sign-ups
- Gift delivery success rate
- User satisfaction ratings and feedback
- Affiliate commission revenue (total & ARPU)
- Repeat purchase / retention rate

---

### /entities/lazy-husband.md

**Path:** `/entities/lazy-husband.md`  
**Characters:** 1.7k chars  

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

---

### /entities/vendor.md

**Path:** `/entities/vendor.md`  
**Characters:** 5k chars  

# Gift Vendor Entity (India Focus)

## 1. Overview
Gift Vendors are third-party merchants that supply the physical or experiential gifts ultimately delivered to recipients. In the Indian context, vendors range from premium chocolatiers in Mumbai to handicraft collectives in Jaipur, as well as pan-India e-commerce sellers. A robust vendor network ensures variety, reliability, and competitive pricing while maintaining the zero-effort promise for our Lazy Husband users.

## 2. Key Attributes
| Attribute | Description | Example |
|-----------|-------------|---------|
| `vendor_id` | Unique internal identifier | `VEND_2025_0452` |
| `name` | Brand / Storefront name | "The Goodie Basket" |
| `catalogue_url` | Endpoint returning JSON catalogue | `https://api.vendor.com/v1/catalogue` |
| `category` | Primary gift category | Gourmet, Flowers, Jewellery |
| `gstin` | 15-digit GST number for compliance | `27ABCDE1234F2Z5` |
| `base_city` | Fulfilment origin | Bengaluru |
| `ship_regions` | Supported PIN codes or zones | PAN-India (38k PINs) |
| `sla_tier` | Agreed SLA level (`premium`, `standard`) | Premium |
| `rating` | Dynamic quality score (0-5) | 4.6 |
| `status` | `active`, `suspended`, `pending` | Active |

## 3. Catalogue & API Integration
- **Catalogue Endpoint (GET)**: `GET /v1/catalogue?updated_since=<ts>` returns array of SKUs with price (INR), inventory, images, tags, max_ship_time (hours).
- **Pricing Sync (Webhooks)**: `POST /v1/price-update` informs delta pricing to avoid stale rupee amounts.
- **Order Placement (POST)**: `POST /v1/orders` with payload containing `sku_id`, `recipient_address`, `gift_wrap`, `message_card`.
- **Status Webhook**: `POST /v1/order-status` within 15 min of any state change (`accepted`, `shipped`, `delivered`, `exception`).
- **Security**: HMAC-SHA256 header using shared secret; request timestamp window ±5 mins.

## 4. Pricing & Commission Model (INR)
| Model | How It Works | Example |
|-------|--------------|---------|
| **Commission %** | Platform retains `10%` of pre-tax item price. | Gift priced at ₹1,000 → Vendor receives ₹900. |
| **Subscription Add-On** | For subscription tiers (Gold & above), platform subsidises shipping up to ₹50 per order. |
| **Bulk / Festival Deals** | Special pricing slabs during Diwali, Raksha Bandhan, Valentine’s Day negotiated ahead of season. |

## 5. Fulfilment & Logistics
- **Packaging**: All gifts must include discreet outer carton + branded inner wrap; no price tags.
- **Courier Options**: Vendor may self-ship or use platform’s logistics partner via generated shipping label (Shiprocket / Delhivery APIs).
- **Same-Day Capability**: Metro-city vendors tagged `same_day=true` must ship within 3 hrs of order acceptance.
- **Returns / Replacements**: Initiate pickup within 48 hrs of issue ticket.

## 6. Service-Level Agreements (SLA)
| Metric | Target | Premium Tier | Standard Tier |
|--------|--------|--------------|---------------|
| Order Acceptance | ≤ 15 min | 95% | 85% |
| On-Time Dispatch | As per `max_ship_time` | 98% | 90% |
| Delivery Success Rate | First attempt | 97% | 92% |
| Damage / Defect Rate | % orders | <0.5% | <1% |
| Support Response | Ticket acknowledgement | 30 min | 4 hrs |

Penalties: 2% commission claw-back for each KPI month where targets not met.

## 7. Onboarding Workflow
1. **Application Form** – collect brand, GSTIN, catalogue sample.
2. **KYC & GST Verification** – automated via govt APIs.
3. **Quality Audit** – test order placed; packaging & SLA observed.
4. **Contract & Rate Card** – digital signature (HelloSign).
5. **API Credential Issuance** – client_id, secret.
6. **Pilot Phase** – 30 orders; must hit 90% SLA.
7. **Full Launch** – listed in customer-facing catalogue.

## 8. Risk, Legal & Compliance
- Must comply with **Legal Metrology (Packaged Commodities) Rules 2011**.
- For food items, **FSSAI licence** mandatory.
- Handle customer data under **IT Act 2000** & **DPDP Bill** guidelines.
- Periodic GST filing reconciliation to avoid mismatch penalties.

## 9. Edge Cases & Failure Handling
| Scenario | Handling |
|----------|----------|
| Out-of-stock after order | Vendor must auto-suggest substitute within 30 min or platform cancels & re-routes. |
| Festival Surge (e.g., Diwali) | Temporary SLA relaxation negotiated but communicated to users. |
| Remote PIN-code delay | Proactive SMS & WhatsApp updates to recipient. |
| Damaged Item | Free replacement shipped within 24 hrs; prepaid return pickup. |

## 10. Success Metrics
- **GMV via Vendor** ≥ ₹10 L / month within 90 days.
- **On-Time Delivery Rate** ≥ 95%.
- **Average Recipient Rating** ≥ 4.5/5.
- **Dispute Rate** < 1%.

## 11. Alignment with Product Principles
- **Simplicity**: Vendor API abstracts complexity, one-click catalogue ingest.
- **Personalisation**: Rich tags (occasion, persona, style) enable Gift-Selection Engine matching.
- **Reliability**: Strict SLAs ensure zero-effort promise is kept.

---
*Last updated: 2025-07-20*

---

### /entities/wife.md

**Path:** `/entities/wife.md`  
**Characters:** 3.3k chars  

# Persona – "Ananya" (Wife / Gift Recipient – India)

| Key Facts | Details |
|-----------|---------|
| **Age range** | 25–45 (sweet-spot 28–38) |
| **Life stage** | Married 2–12 yrs, 0–2 children |
| **Occupation** | Mix of busy professionals (product manager in Bengaluru, chartered accountant in Mumbai) and at-home caregivers |
| **Location** | Tier-1 & emerging Tier-2 Indian metros (Bengaluru, Mumbai, Delhi-NCR, Pune, Hyderabad) |
| **Tech stack** | Android flagship or iPhone, **WhatsApp**, **Instagram**, Amazon/Flipkart/Nykaa power-user, UPI savvy (GPay/PhonePe) |
| **Disposable income** | ₹10-35 lakh annual household |

---

## 🎯 Goals & Motivations
- Feel **genuinely remembered** on birthdays, anniversaries & festivals without dropping hints
- Receive gifts that **reflect her taste & values** (hand-crafted, eco-friendly, supports local artisans)
- Build a stronger emotional bond with spouse through thoughtful surprises & shared experiences

## 😖 Pain Points
1. Partner forgets occasions or buys **last-minute generic gifts** (bouquet + chocolate routine)
2. Friction of returns / exchanges when size, colour or style is off
3. Emotional disappointment of feeling **taken for granted**
4. Logistics hiccups (late deliveries around Diwali / Valentine’s Day)

## 🏃‍♀️ Behaviours & Tech Habits
- Online shopping **3–4× / month**; happily experiments with D2C brands via Instagram ads
- Maintains wish-lists on Amazon & Nykaa; saves Pinterest boards for home décor & fashion
- Active on **WhatsApp** family groups; checks push notifications but hates spam
- Uses **UPI** & credit cards; expects COD option for some gifts being shipped to parents/in-laws

## 🎁 Gift Preferences & Constraints
| Likes | Constraints |
|-------|-------------|
| Gold-plated or silver jewellery, artisanal scented candles (natural oils), luxury spa vouchers, gourmet mithai hampers, personalised photo frames, weekend getaway experiences | Avoid strong artificial fragrances, prefers cruelty-free & eco-friendly packaging, mostly vegetarian family, avoids alcohol based gifts |

## 📈 Success Metrics (Product-Side)
- **Surprise-delight score ≥ 8/10** on post-gift survey
- **Recipient NPS ≥ 60** within 3 months
- **Repeat-gift satisfaction ≥ 90 %** across 3 occasions

## 💬 Representative Quotes
> “Mujhe mehenga gift nahin chahiye, bas dil se soch-samajh kar diya ho.”  
> “I love it when he remembers Karva Chauth before I do!”

## ✨ Journey Touchpoints
1. *Gift arrival* – attractive, recyclable packaging; on-time even during festival rush
2. *Unboxing* – personalised note in spouse’s words + a QR code to a voice message
3. *Thank-you moment* – easy path to capture her reaction & share gratitude
4. *Feedback* – 2-click survey to fine-tune taste model & verify delivery quality

## 🔍 Implications for Product & UX
- **Local festival calendar** integration (Diwali, Karva Chauth, Raksha Bandhan anniversaries)
- Highlight **“Why this gift”** explanation tied to her saved preferences & wish-lists
- Offer **UPI one-tap payments** for husband & seamless address management
- Build regional shipping SLAs & peak-season buffers to avoid late deliveries
- Prioritise sustainable packaging cues (FSC, cloth potli bags) to align with eco-values

---
*Persona updated 2025-07-20 to reflect Indian user base and currency.*

---

### /feature-catalog.md

**Path:** `/feature-catalog.md`  
**Characters:** 2.2k chars  

# Feature Catalog

| Feature                         | Description                                                                                                  | Priority |
|---------------------------------|--------------------------------------------------------------------------------------------------------------|----------|
| User Registration               | Account creation and profile setup                                                                           | High     |
| Important Dates Input           | Input and edit key dates (anniversary, birthdays, others)                                                    | High     |
| Lazy Mode Selection             | Choose between AI decision or broad theme                                                                    | High     |
| AI Gift Selection               | Automated gift picking based on user input                                                                   | High     |
| Gift Order Automation           | Automatic order placement and tracking                                                                       | High     |
| Notification System             | Notify users before gift delivery                                                                            | High     |
| AI-generated Love Notes         | Mandatory AI-generated personalized notes for each gift                                                      | High     |
| Affiliate Partner Integration   | Connect to Amazon Associates & other networks, append referral codes to orders, and track commission revenue | High     |
| Custom Message Editing          | Optional user edits to AI notes                                                                              | Medium   |
| Gift History & Tracking         | View past gifts and order statuses                                                                           | Medium   |
| Payment Integration             | Secure payment processing                                                                                    | High     |
| User Settings & Preferences     | Manage account info and preferences                                                                          | Medium   |

---

### /features/affiliate-partner-integration.md

**Path:** `/features/affiliate-partner-integration.md`  
**Characters:** 2.9k chars  

# Feature Specification: Affiliate Partner Integration

## 1. Goal & Rationale
Enable the app to earn commission on every gift purchased by integrating with affiliate programs (Amazon Associates, Etsy, specialty boutiques). All automated gift orders will use referral links, attributing sales back to us and generating passive revenue without additional user effort.

## 2. User Stories
| ID | As a … | I want … | So that … |
|----|---------|----------|-----------|
| AP-1 | Product Owner | automated gift purchases to carry my affiliate code | we generate commission on each order |
| AP-2 | Lazy Husband | gifts sourced from reputable online stores | I trust the product quality |
| AP-3 | Finance Analyst | a dashboard of commission earnings | I can track revenue performance |
| AP-4 | Support Agent | visibility into failed affiliate attributions | I can resolve revenue loss issues quickly |

## 3. Partner APIs & Accounts
- **Amazon Associates:** REST endpoints for link generation, order reporting (24-48 h delay)
- **Etsy Affiliate:** Share-a-Sale deep links, CSV order feeds
- **Future partners:** Modular adapter pattern to plug in new programs easily

Authentication handled via stored API keys in encrypted vault (AWS Secrets Manager).

## 4. Key Flows
1. **Link Generation**
   - Gift selection engine requests a product link → Adapter appends our affiliate tag → returns final URL.
2. **Order Attribution**
   - User is redirected through the affiliate link and completes purchase (automated checkout via Amazon gift API where available).
3. **Commission Tracking**
   - Nightly cron polls partner reporting APIs → stores transactions in `affiliate_commissions` table.
4. **Revenue Reporting**
   - Dashboard aggregates total commission, ARPU, by partner & time period.

## 5. Data Schema (simplified)
```sql
Table: affiliate_links
- id (PK)
- partner (enum)
- product_sku
- raw_url
- affiliate_url
- created_at

Table: affiliate_commissions
- id (PK)
- partner (enum)
- order_id
- amount_usd
- status (pending | confirmed | rejected)
- event_date
- created_at
```

## 6. Edge Cases & Error Handling
- Partner API downtime → queue link requests and retry with exponential backoff.
- Order returns/refunds → detect negative commissions and update metrics.
- Missing attribution due to ad-blockers → fallback to backup partner or notify finance.
- Country restrictions (e.g., Amazon tag invalid in some regions) → use regional programs.

## 7. Acceptance Criteria
- 100 % of outgoing gift orders use valid affiliate URLs.
- Commission data syncs daily with <1 % discrepancy vs partner dashboard.
- Revenue dashboard shows real-time totals updated at least hourly.
- Graceful degradation: if affiliate flow fails, gift purchase still completes for user.
- Unit & integration tests cover adapters for each partner with 90 % coverage.

---
*Last updated: 2025-06-22*

---

### /features/date-reminder/index.md

**Path:** `/features/date-reminder/index.md`  
**Characters:** 2.7k chars  

# Date-Reminder & Notification Feature Spec
_Last updated: 2025-07-20_

## 1. Problem & Value
Forgetful partners often miss special dates, causing disappointment and friction. An iron-clad reminder system is the foundation of the zero-effort promise—if we fail here, nothing else matters.

## 2. Objectives
• **Never miss** a registered occasion.  
• Support **multiple channels** (push, WhatsApp, email) starting with WhatsApp + push.  
• Allow user-defined **lead times** (e.g., 7-day, 1-day alerts).  
• Be **timezone-aware** and **scalable** for millions of reminders.

## 3. Scope
| Item | MVP | Stretch |
|------|-----|---------|
| Add/Import Dates | ✅ Manual entry & CSV upload | Google Calendar sync |
| Notification Channels | ✅ WhatsApp Business API, FCM Push | Email, SMS |
| Reminder Lead Times | ✅ Fixed presets (7d, 3d, 1d) | Custom picker |
| Smart “Just-in-Time” Nudges | — | AI-predicted best time |
| Recipient Birthday Auto-Detect from Contacts | — | Phonebook permission + ML |

## 4. Epics & Stories
### 4.1 Date Management Epic
- US-DM-1: As a user, I can add an occasion (name, date, relationship) so it’s tracked.
- US-DM-2: As a user, I can bulk-import a CSV of dates so setup is quick.

### 4.2 Notification Scheduling Epic
- US-NS-1: System runs a daily cron to queue reminders based on lead times.
- US-NS-2: Queued reminders trigger WhatsApp template messages via API.

### 4.3 Preferences Epic
- US-PR-1: User selects preferred channels per occasion.
- US-PR-2: User toggles reminders on/off per date.

### 4.4 Channels Integration Epic
- WhatsApp Business API integration with template IDs, fallback to push.

### 4.5 Edge Cases Epic
- Leap year birthdays (Feb 29) → auto-shift to Feb 28 on non-leap years.
- Multi-recipient same day → batch reminders to avoid spam.

### 4.6 Analytics & Monitoring Epic
- Track sent vs delivered vs read for each channel (Firebase, Twilio webhook).

## 5. User Stories (MVP Extract)
| ID | Story | Priority |
|----|-------|----------|
| US-DM-1 | Add single occasion | P0 |
| US-DM-2 | Bulk CSV import | P1 |
| US-NS-1 | Daily scheduler queues reminders | P0 |
| US-NS-2 | WhatsApp message dispatch | P0 |
| US-PR-1 | Select channels per occasion | P1 |
| US-PR-2 | Toggle reminder | P2 |

## 6. Open Questions
1. WhatsApp template approval timeline—risk to launch?  
2. Should partner receive reminder nudge (“Time to thank her!”) separate from gift flow?  
3. Do we localize reminder content by festival (e.g., Karva Chauth)?

## 7. Dependencies
• Vendor integration for WhatsApp Business API  
• Payment—reminders only active for paid subscribers  
• Analytics stack (Firebase + BigQuery)

---

### /features/fulfillment/index.md

**Path:** `/features/fulfillment/index.md`  
**Characters:** 3.2k chars  

# Order Fulfillment & Shipping – Feature Spec
_Last updated: 2025-07-20_

## 1. Problem & Value
A perfectly chosen gift means nothing if it arrives late or damaged. A rock-solid fulfillment pipeline preserves trust and ensures the “effortless” promise all the way to unboxing.

## 2. Objectives
• **On-time delivery** ≥ 95% for metro areas, ≥ 90% for non-metro.  
• Real-time tracking link for buyers and internal ops.  
• Automated exception handling (RTO, damaged, address issues) with <30-min human escalation.

## 3. Scope
| Item | MVP | Stretch |
|------|-----|---------|
| Vendor Order API | ✅ Place order + receive AWB | Vendor stock sync |
| Domestic Shipping (India) | ✅ all 29 states | Intl shipping |
| Tracking Webhooks | ✅ Delhivery & Bluedart | Multi-carrier aggregator |
| Gift Wrap & Message Card | ✅ basic | Custom prints |
| NPS Survey Post-Delivery | — | 🌊 Stretch |

## 4. Workflow Diagram
```
Gift Selection  ──► Place Vendor Order ──► Generate AWB ──► Pickup ──► In-Transit ──► Delivered
                           │                     │                     │
                           ▼                     ▼                     ▼
                  Vendor Webhook          Carrier Webhook       NPS Trigger
```

## 5. Vendor Integration
• **Order Endpoint**: `/api/orders` (POST) → returns `order_id`, `awb`, `eta`.  
• **Status Webhook**: Vendor pushes `order_id` state changes: `confirmed`, `shipped`, `delivered`, `failed`.

## 6. Shipping & Tracking
• Partner carriers: Delhivery (primary), Bluedart (fallback).  
• AWB used to generate a **white-label tracking page** with live status + expected delivery date.  
• WhatsApp proactive updates on `shipped` and `out-for-delivery`.

## 7. SLAs & Monitoring
| Stage | SLA | Alert Threshold |
|-------|-----|-----------------|
| Vendor confirm | 2 hrs | Pager if >4 hrs |
| Pickup from vendor | 24 hrs | Pager if 36 hrs |
| Last-mile delivery | 3-5 days metro | Pager if EDD+2 |

## 8. Epics & Key Stories
### 8.1 Order Service Epic
- US-OS-1: Place vendor order and store `order_id`, `awb`.  
- US-OS-2: Retry on 5xx with exponential backoff.

### 8.2 Tracking & Notifications Epic
- US-TN-1: Subscribe to carrier webhooks.  
- US-TN-2: Push WhatsApp updates to buyer.  
- US-TN-3: Expose tracking page URL.

### 8.3 Exception Handling Epic
- US-EH-1: Auto-detect `RTO` or `failed` → notify ops Slack.  
- US-EH-2: Partial refund logic if SLA breach.

### 8.4 Analytics Epic
- US-AN-1: Track on-time %, RTO rate, damaged rate.  
- US-AN-2: Weekly vendor scorecard.

## 9. Edge Cases & Failure Handling
• **Address Not Found**: escalate to buyer via WhatsApp; pause SLA timer.  
• **Damaged in Transit**: auto-order replacement if within budget buffer.  
• **Festival Surge** (Diwali): dynamic ETA adjustments.

## 10. Open Questions
1. Do we pre-reserve inventory at vendor to avoid stock-outs?  
2. Should we partner with 3PL aggregator for single API vs direct carrier integrations?  
3. How to localize tracking page languages (Hindi, Tamil)?

## 11. Dependencies
• Gift Selection Engine output (SKU, vendor).  
• Payment confirmation webhook.  
• WhatsApp Business API for proactive notifications.  
• Ops dashboard (analytics pipeline).

---

### /features/gift-selection/index.md

**Path:** `/features/gift-selection/index.md`  
**Characters:** 3.7k chars  

# Gift Selection Engine – Feature Spec
_Last updated: 2025-07-20_

## 1. Problem & Value
Choosing the “perfect” gift is both **time-consuming** and **stressful**. Our promise of *zero-effort gifting* hinges on a matching engine that feels magically personal while remaining operationally predictable. A strong first release keeps churn low and drives upsell to higher-priced plans.

## 2. Objectives
• Deliver a gift shortlist that recipients rate ≥ 8/10 on relevance.  
• Average selection latency under 1 sec for 90% of requests.  
• ≥ 75% of orders auto-approved without human curation by month-3.  
• Support India-specific gifting norms (Diwali hampers, personalised mugs, etc.).

## 3. Scope
| Item | MVP | Stretch |
|------|-----|---------|
| Rule-based Scoring Engine | ✅ | — |
| Machine-Learning Taste Model | — | 🌊 Stretch |
| Manual Curator Override | ✅ | ✅ |
| Multi-Recipient Profiles | — | 🌊 Stretch |
| Live Pricing / Stock Check | ✅ | ✅ |
| Festival-Themed Bundles | — | 🌊 Stretch |

## 4. System Overview
```
Occasion & Budget ─┐                ┌─ Vendor API (inventory & ₹)
User Preferences ─┼─► Matching Core ┼─► Gift Shortlist (top 3)
Trend Signals  ───┘                └─ Admin Override Panel
```

## 5. Matching Algorithm (MVP)
### 5.1 Inputs
1. **Occasion Type**: Birthday, Anniversary, Diwali, Valentine’s Day…  
2. **Budget Band**: ₹1000–₹2500 (default), configurable.  
3. **Recipient Persona Tags**: likes *jewellery*, *green tea*, *sustainable brands*.  
4. **Inventory Feed**: Product SKU, category, vendor rating, margin.  
5. **Trend Score** (optional): Social buzz, seasonal popularity.

### 5.2 Scoring Formula
`Score = (CategoryMatch × 0.4) + (BudgetFit × 0.2) + (VendorRating × 0.1) + (TrendScore × 0.1) + (MarginBoost × 0.2)`

• **CategoryMatch**: binary 1/0 if product matches at least one persona tag.  
• **MarginBoost**: +0.05 bonus if margin ≥ 25%.

Top-3 SKUs above threshold → returned as shortlist.

### 5.3 Fallback Rules / Edge Cases
• If no SKU above threshold, widen budget ±₹500.  
• Inventory outage → escalate to manual curator queue (<5 min SLA).  
• Religious constraints: auto-exclude leather for Jain recipients if flagged.

## 6. Epics & Key Stories
### 6.1 Data Ingestion Epic
- US-DI-1: Pull vendor catalogue every 6 hrs via webhooks/API.  
- US-DI-2: Normalize categories to internal taxonomy.

### 6.2 Preference Weighting Epic
- US-PW-1: Store recipient tags and weightings in DB.  
- US-PW-2: Admin can adjust global weight multipliers.

### 6.3 Rule-Engine Epic
- US-RE-1: Compute shortlist given inputs under 1 sec (Lambda/Go).  
- US-RE-2: Return JSON with ranked SKUs, scores, and fallback flag.

### 6.4 Admin Tuning Panel Epic
- US-AT-1: Curator overrides shortlist via dashboard.  
- US-AT-2: Changes are logged and fed back as training data.

### 6.5 Analytics Epic
- US-AN-1: Track acceptance rate and NPS per SKU.  
- US-AN-2: Generate weekly “dud SKU” report for Merch team.

## 7. MVP User Stories Snapshot
| ID | Story | Priority |
|----|-------|----------|
| US-DI-1 | Vendor catalogue ingestion | P0 |
| US-PW-1 | Store recipient tags | P0 |
| US-RE-1 | Compute shortlist | P0 |
| US-RE-2 | Return ranked JSON | P0 |
| US-AT-1 | Curator override | P1 |

## 8. Open Questions
1. Cold-start users with zero prefs—do we default to best-selling SKUs or ask 3 quick taste questions?  
2. Should margin be capped in scoring to avoid poor customer experience?  
3. Is personalised note/surprise add-on part of this engine or separate?

## 9. Dependencies
• Vendor catalogue API (SLA 99.5%).  
• Subscription check – engine runs only for paid users.  
• Analytics warehouse (BigQuery) for feedback loop.

---

### /features/important-dates-input.md

**Path:** `/features/important-dates-input.md`  
**Characters:** 909 chars  

# Important Dates Input Feature Design

## Overview
Allows users to add and manage important gifting dates and set up recurring gift schedules.

## Features
- User may add any important dates with no mandatory fields.
- Option to add special occasions like Valentine’s Day, Diwali, New Year.
- Option to enable periodic gifting on custom intervals:
  - User chooses interval (e.g., monthly, quarterly, every two months).
  - Gifts sent on random dates within each interval for surprise.
  - Ability to enable/disable this recurring gifting.
- Input validation for date formats and duplication checks.
- Easy add, edit, delete options.
- Visual calendar or list view of entered dates and schedules for clarity.

## User Experience
- Simple intuitive interface for managing dates and schedules.
- Clear confirmation of recurring gift schedules when enabled.

## Notes
- Emphasize flexibility and zero friction.

---

### /features/index.md

**Path:** `/features/index.md`  
**Characters:** 2.7k chars  

# Features Catalogue & MVP Waterline
_Last updated: 2025-07-20_

This catalogue provides a **single source of truth** for all major capabilities planned for the **Lazy Husband Gifting App**, clearly distinguishing what makes the initial go-to-market (MVP) and what is deferred to future releases.

## Reading the Waterline
• **✅ MVP** – Must exist for day-1 launch.  
• **🌊 Stretch** – Nice-to-have enhancements that can ship right after MVP if capacity allows.  
• **🚀 Future** – Longer-term bets (≥6 months out).

---

| # | Capability | What It Enables | Waterline |
|---|-------------|-----------------|-----------|
| 1 | Date-Reminder & Notification Core | Tracks important dates, schedules push / WhatsApp alerts to the service | **✅ MVP** |
| 2 | Basic Gift Selection Engine | Rule-based matching (occasion × persona preferences × budget) | **✅ MVP** |
| 3 | Vendor Catalogue Integration | Pull curated SKUs & live inventory from partner vendors | **✅ MVP** |
| 4 | Order Fulfillment & Domestic Shipping | Place order, send shipping instructions, track delivery within India | **✅ MVP** |
| 5 | Payment & Subscription (Single Tier) | Recurring subscription in ₹ via UPI / Cards | **✅ MVP** |
| 6 | Account Management & Preferences | Profile, gift history, recipient taste questionnaire | **✅ MVP** |
| 7 | Analytics & Internal Dashboards | Ops dashboard for failed deliveries, churn alerts | 🌊 Stretch |
| 8 | Advanced AI Gift Engine | ML-driven taste modeling, continuous learning | 🌊 Stretch |
| 9 | Multi-Recipient Support | Add kids, parents, friends with independent date sets | 🌊 Stretch |
|10 | Festival Bundles & Themed Boxes | Pre-packed Diwali / Rakhi / Valentine’s combos | 🌊 Stretch |
|11 | Corporate / Employee Gifting Portal | Bulk scheduling, GST invoicing | 🚀 Future |
|12 | Global Shipping & Duty Handling | Gifts delivered outside India with landed-cost calc | 🚀 Future |
|13 | AR / Virtual Try-On for Jewelry | Immersive preview before purchase | 🚀 Future |
|14 | Social “Brag” Moments | Auto-generate shareable stories post-unboxing | 🚀 Future |

---

## Epics Mapping
A detailed specification for each MVP capability will live under `/features/<capability>/`:

1. `/features/date-reminder/`  
2. `/features/gift-selection/`  
3. `/features/vendor-integration/`  
4. `/features/fulfillment/`  
5. `/features/payment/`  
6. `/features/account-management/`

> Stretch and Future epics will be documented once MVP tickets near completion.

## Alignment with Vision & Concept Note
These capabilities directly uphold the **core value prop** defined in the vision document—"zero-effort, never-miss gifting"—and map to the revenue streams (subscription + commission) laid out in the concept note.

---

### /features/payment/index.md

**Path:** `/features/payment/index.md`  
**Characters:** 2.5k chars  

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

---

### /features/user-registration.md

**Path:** `/features/user-registration.md`  
**Characters:** 433 chars  

# User Registration Feature Design

## Overview
Enables user account creation and secure access to the app.

## User Inputs
- Email
- Password
- Basic profile information

## Optional
- Social login options: Google, Facebook

## Verification
- Email confirmation process

## Onboarding
- Introductory flow highlighting app benefits

## Privacy
- Privacy policy and data usage notice

## Notes
- Emphasize simplicity and quick sign-up

---

### /mvp-scope.md

**Path:** `/mvp-scope.md`  
**Characters:** 500 chars  

# MVP Scope

## Core Features
- User sign-up and profile setup
- Entry of important dates (anniversaries, birthdays)
- Lazy mode gift automation option
- Gift selection and ordering
- Delivery tracking and notifications

## Exclusions for MVP
- Multiple recipient handling
- Extensive gift catalog customization
- Social sharing features

## Success Criteria
- Seamless user onboarding
- Successful automated gift delivery on key dates
- Positive initial user feedback

---
*Last updated: 2025-06-10*

---

### /personas/lazy-husband.md

**Path:** `/personas/lazy-husband.md`  
**Characters:** 465 chars  

# Lazy Husband Persona

## Demographics
- Age range: 25-45
- Occupation: Various, typically busy professionals
- Location: Urban and suburban areas

## Behavior
- Often forgets important dates
- Prefers automation and simplicity in task management
- Needs to send a personal touch message with every gift

## Goals
- Avoid gifting stress
- Maintain good relationship with wife effortlessly

## Pain Points
- Forgetfulness
- Lack of time
- Indecision on gift choice

---

### /personas/wife.md

**Path:** `/personas/wife.md`  
**Characters:** 461 chars  

# Wife Persona

## Demographics
- Age range: 25-45
- Interests: Wide range, depends on personality
- Personality Traits: Appreciates thoughtful gestures and sentiment

## Expectations
- Appreciates thoughtful gifts
- Values sentiment

## Motivations
- Feeling loved and remembered

## Pain Points
- Disappointment from missed occasions
- Generic or impersonal gifts

## App Awareness
- Unaware that husband uses the Lazy Husband app; gifting is a secret service

---

### /plan.md

**Path:** `/plan.md`  
**Characters:** 1.7k chars  

## 🗺️ Roadmap

### Phase 1: Foundation *(current)*
- [x] Draft concept note *(completed: 2025-06-10)*
- [x] Define vision *(completed: 2025-06-10)*
- [x] Define MVP scope *(completed: 2025-06-10)*
- [x] Add revenue model to concept note *(completed: 2025-06-22)*

### Phase 2: Feature Development
- [x] Create feature catalog *(completed: 2025-06-10)*
- [x] Add Affiliate Partner Integration feature *(completed: 2025-06-22)*
- [ ] Define user personas *(in progress)*
- [ ] Map user journeys and flows *(in progress)*
- [ ] Design priority features *(next up)*

### Phase 3: Validation
- [ ] Review completeness
- [ ] Stakeholder feedback
- [ ] Prepare dev-ready docs

## 📋 Detailed Tasks

### Concept Note `/concept-note.md`
- [x] Problem statement *(completed: 2025-06-10)*
- [x] Solution approach *(completed: 2025-06-10)*
- [x] Success metrics *(completed: 2025-06-10)*
- [x] Revenue model *(completed: 2025-06-22)*

### Feature Catalog `/feature-catalog.md`
- [x] Initial catalog created *(completed: 2025-06-10)*
- [x] Added Affiliate Partner Integration *(completed: 2025-06-22)*

### Personas `/personas/`
- [ ] Lazy husband persona (`/personas/lazy-husband.md`)
- [ ] Wife persona (`/personas/wife.md`)

### User Flows `/user-flows/`
- [x] Sign up and date entry flow *(completed)*
- [x] Lazy mode selection flow *(completed)*
- [x] Delivery tracking notifications *(completed)*
- [x] Feedback rating flow *(completed)*

### Features Specs `/features/`
- [ ] Affiliate Partner Integration spec *(next up)*

## 🚧 Blockers & Notes
- None

## 📝 Next Actions
1. Draft Affiliate Partner Integration spec
2. Refine personas if needed
3. Prepare dev-ready priority feature designs

---
*Last updated: 2025-06-22*

---

### /prompts/lazy-husband-lovable.md

**Path:** `/prompts/lazy-husband-lovable.md`  
**Characters:** 2.1k chars  

# Lovable Prompt – Lazy Husband Gifting App

## 1. Product Overview
A zero-effort gifting service that remembers important dates, automatically selects thoughtful gifts, orders them via affiliate partners (Amazon, Etsy, etc.), and delivers on time—complete with AI-generated love notes.

## 2. Target Audience
Primary  : Forgetful/ busy husbands (25-45) who value their relationship but struggle with time & memory.
Secondary: Wives who receive the gifts (indirect beneficiaries) and need to feel loved and appreciated.

## 3. Emotional Hook
"Never miss a chance to make her smile."
Relieves guilt & stress for husbands; creates delightful surprise and appreciation for wives.

## 4. Core Benefits (speak to both logic & emotion)
- Always on-time gifts—set once, forget forever.
- Perfectly curated presents that feel personal.
- Saves hours of searching & worrying.
- Relationship brownie points—every occasion.
- Commission-powered model keeps the service low-cost for users.

## 5. Key Features to Highlight
1. One-minute sign-up & date entry.
2. "Lazy Mode" automation toggle.
3. AI Gift Selection with affiliate-backed catalogs.
4. Delivery tracking notifications.
5. Personalized love notes (editable).
6. Gift history & upcoming dates dashboard.

## 6. Voice & Tone Guidelines
- Warm, playful, slightly cheeky.
- Encouraging and reassuring—never judgmental.
- Use simple, upbeat language; sprinkle light humor.
- Avoid excessive tech jargon.

## 7. Deliverables Requested from Lovable AI
Generate:
1. **Tagline options** (3-5)
2. **App-store short description** (<80 chars)
3. **Landing-page hero copy** (headline + sub-headline)
4. **Feature block blurbs** (6 features above)
5. **Onboarding screen micro-copy** (3 screens)
6. **Email sequences**
   - Welcome email
   - Upcoming occasion reminder
   - Gift delivered confirmation
7. **Push notification snippets**
   - Date entry prompt
   - Gift shipped
   - Anniversary tomorrow

> Ensure all copy aligns with the emotional hook and tone guidelines. Add emojis sparingly (🎁 ❤️ 🗓️) where they enhance warmth and readability.

---
*Target Tool*: lovable
*Last updated*: 2025-06-22

---

### /prompts/lovable-mvp.md

**Path:** `/prompts/lovable-mvp.md`  
**Characters:** 3.4k chars  

<!-- Target Tool: lovable -->
# Prompt for lovable

# Lazy Husband Gifting App – Lovable One-Shot Prompt

Context:

* Role: “You are an expert full-stack developer working in Lovable.”
* Product vision: A playful, zero-effort gifting web app that remembers important dates for busy/forgetful husbands, automatically selects and orders thoughtful gifts via affiliate partners, and sends them on time with AI-generated love notes. Wives receive delightful, personalized surprises; husbands maintain relationship points with no stress.
* High-level problem the app solves: Prevents missed occasions and gifting anxiety by fully automating the remembering, choosing, and ordering of gifts.

Task:

1. Build the first working MVP web application in this Lovable run.
2. Implement the following pages/components/features:
   • Sign-Up / Authentication (email + Supabase magic link).
   • Important Dates Dashboard (list upcoming & past occasions).
   • Add / Edit Date Modal or Page with validation & recurrence toggle.
   • “Lazy Mode” Settings panel (on/off, gift budget slider).
   • Gift History Page (read-only list of past gifts).
   • Supabase DB schema migration script.
   • Supabase edge functions stubs: `giftScheduler`, `aiGiftPicker` (return mock data for now).
   • Global Notification context (toast).

Guidelines:

* Tech stack: Vite + React + TypeScript, Supabase (Postgres, Auth, Edge Functions), TailwindCSS.
* UI/UX style rules:
  - Playful pastel palette (#FFEAF3, #FFF9E6, #E6F7FF, #E8FFE6).
  - Rounded 12-px radius cards, soft shadows, emoji accents 🎁 ❤️ 🗓️.
  - Mobile-first responsive layout; use Tailwind utility classes.
* Code quality / commenting expectations:
  - Use functional components and hooks.
  - Organize folders: `/pages`, `/components`, `/hooks`, `/supabase`.
  - Inline JSDoc comments for every exported function.
  - Provide `.env.example` with Supabase keys placeholders.
* Data models (SQL):
```sql
-- users handled by Supabase Auth
CREATE TABLE important_dates (
  id uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id uuid REFERENCES auth.users NOT NULL,
  occasion text NOT NULL,
  date date NOT NULL,
  is_recurring boolean DEFAULT false,
  recurrence_interval text,   -- e.g. 'monthly','yearly'
  created_at timestamptz DEFAULT now()
);

CREATE TABLE gifts (
  id uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id uuid NOT NULL,
  date_id uuid REFERENCES important_dates ON DELETE CASCADE,
  name text,
  affiliate_url text,
  price_cents int,
  status text DEFAULT 'planned', -- planned|ordered|delivered
  created_at timestamptz DEFAULT now()
);

CREATE TABLE orders (
  id uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  gift_id uuid REFERENCES gifts ON DELETE CASCADE,
  external_order_id text,
  status text DEFAULT 'pending', -- pending|shipped|delivered
  tracking_url text,
  created_at timestamptz DEFAULT now()
);
```

Constraints:

* Out-of-scope for this run:
  - Real affiliate checkout or payment processing (stub only).
  - Multi-recipient support.
  - Custom gift catalog browsing.
* Forbidden tools/services: No server-side frameworks besides Supabase Edge Functions; no proprietary UI libraries that exceed size limit.
* Performance limits: Final JS bundle (gzipped) must stay under 1 MB; leverage code-splitting and lazy import heavy libs.
* Security: Never commit real Supabase service role keys; use environment variables and `.gitignore` for `.env`.

---

### /user-flows/delivery-tracking-notifications.md

**Path:** `/user-flows/delivery-tracking-notifications.md`  
**Characters:** 584 chars  

# Delivery Tracking and Notifications Flow

## Description
This flow outlines how users are kept informed about the status of gift delivery.

## Steps
1. System monitors order status from vendors.
2. User receives updates on gift dispatch and estimated delivery.
3. Notifications issued for delivery delays or issues.
4. Confirmation sent when gift is delivered.
5. Option provided for user feedback or rescheduling.

## Edge Cases
- Vendor delay: system proactively notifies user with options.
- Delivery failure: user alerted to reschedule or cancel.

---
*Last updated: 2025-06-10*

---

### /user-flows/feedback-rating.md

**Path:** `/user-flows/feedback-rating.md`  
**Characters:** 550 chars  

# Feedback and Gift Rating Flow

## Description
This flow describes the process of collecting user feedback after gift delivery.

## Steps
1. User receives prompt to rate the gift after delivery confirmation.
2. User submits feedback on gift selection, delivery, and satisfaction.
3. System stores feedback for future improvements.
4. User can submit feedback manually at any time.

## Edge Cases
- User ignores prompt: system sends one reminder.
- Negative feedback: system flags for review and possible intervention.

---
*Last updated: 2025-06-10*

---

### /user-flows/gift-selection-ordering.md

**Path:** `/user-flows/gift-selection-ordering.md`  
**Characters:** 653 chars  

# Automated Gift Selection and Ordering Flow

## Description
This flow covers the process where the system automatically selects and orders gifts based on user settings.

## Steps
1. System receives scheduled gift task from Lazy Mode.
2. System selects a gift based on user preferences and recipient profile.
3. Gift order is placed with preferred vendors or partners.
4. System sends confirmation and tracking information to user.
5. Handles order errors or unavailability with fallback options.

## Edge Cases
- Vendor out of stock: system selects alternate gift.
- Payment failure: user alerted to update payment info.

---
*Last updated: 2025-06-10*

---

### /user-flows/lazy-mode-activation.md

**Path:** `/user-flows/lazy-mode-activation.md`  
**Characters:** 684 chars  

# Lazy Mode Activation and Settings Flow

## Description
This flow describes how users enable and configure the Lazy Mode feature for automated gifting.

## Steps
1. After sign-up, user is introduced to the Lazy Mode feature.
2. User enables Lazy Mode to activate automatic gift handling.
3. User customizes preferences such as gift types, budget, and frequency.
4. System confirms settings and schedules gifting automation.
5. User can modify or disable Lazy Mode anytime from settings.

## Edge Cases
- User declines Lazy Mode activation: manual gifting reminders continue.
- User sets invalid budget or preferences: validation and feedback provided.

---
*Last updated: 2025-06-10*

---

### /user-flows/lazy-mode-selection.md

**Path:** `/user-flows/lazy-mode-selection.md`  
**Characters:** 329 chars  

# Lazy Mode Selection Flow

1. User chooses between two lazy modes:
   - **You Decide Everything**: AI selects gifts based on a 5-question quiz or guesses from age + city.
   - **Ill Choose Once**: User selects a broad gift theme (e.g., jewelry, books, experiences).
2. The app saves mode selection for future automated gifting.

---

### /user-flows/notification-gifting.md

**Path:** `/user-flows/notification-gifting.md`  
**Characters:** 264 chars  

# Notification & Gifting Flow

1. The app schedules gifts ahead of each important date.
2. Sends notification: "Your gift is on the way."
3. Prompts mandatory 1-click AI-generated love note inclusion with every gift.
4. Orders are automatically placed and tracked.

---

### /user-flows/overview.md

**Path:** `/user-flows/overview.md`  
**Characters:** 498 chars  

# User Flows Overview

This document outlines the key user journeys and flows for the gifting service.

## Included Flows

1. Sign-up and important date entry
2. Lazy mode activation and settings
3. Automated gift selection and ordering
4. Delivery tracking and notifications
5. Feedback and gift rating

## Flow Template

Each flow includes:
- Flow description and goals
- Step-by-step user actions
- System responses and automations
- Edge cases and error handling

---
*Last updated: 2025-06-10*

---

### /user-flows/sign-up-date-entry.md

**Path:** `/user-flows/sign-up-date-entry.md`  
**Characters:** 262 chars  

# Sign-up & Date Entry Flow

1. Husband registers via app or website.
2. Enters mandatory key dates: anniversary, wifes birthday.
3. Optionally adds other important dates: Valentines Day, Diwali, New Year, etc.
4. Saves profile information for automated usage.

---

### /user-journeys/index.md

**Path:** `/user-journeys/index.md`  
**Characters:** 2.8k chars  

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

---

### /user-journeys/lazy-husband.md

**Path:** `/user-journeys/lazy-husband.md`  
**Characters:** 1.8k chars  

# Lazy Husband Journey (Indian Context)

Persona reference: `/entities/lazy-husband.md`

| Stage | Desired Emotion | Pain Point / Fear | Product Interaction | "Wow" Moment |
|-------|-----------------|-------------------|---------------------|---------------|
| 1. Awareness | Relief | Missing anniversaries leads to fights | Instagram reel ➜ Landing page | “Zero effort? I’m in.” |
| 2. Signup | Confidence | Payment friction | 3-step UPI AutoPay flow | ₹499/yr feels cheaper than one forgotten gift |
| 3. Setup | Speed | Long forms | 3-field wizard + Google Contacts import | Finished in <90 sec |
| 4. Pre-Event Reminder | Control | Fear gift may mis-match | WhatsApp T-7 heads-up + quick override | Able to tweak budget with one tap |
| 5. Gift Picked | Pride | Doubt about fit | Push + email summary w/ image & message preview | “That’s PERFECT for her.” |
| 6. Shipping | Anticipation | Tracking anxiety | Live courier map (Delhivery/BlueDart) | ETA updates auto-pushed |
| 7. Delivery | Satisfaction | Need to coordinate thank-you | Auto-generated heartfelt SMS | Wife’s happy selfie in family WhatsApp |
| 8. Renewal | Assurance | Price creep | Week-before notice + invoice | “Totally worth it.” |

## Emotions Over Time
```
Relief → Confidence → Pride → Anticipation → Satisfaction → Assurance
```

## Success Metrics
- Time-to-Activate < 2 min
- Reminder ➜ Gift-Approval CTR ≥ 80 %
- Annual Renewal Rate ≥ 70 %

## Narrative Snapshot
Rahul (32, Bengaluru software engineer) is perpetually late at gift-shopping. After one heated anniversary quarrel he signs up during Flipkart’s Big Billion Days, links UPI AutoPay, and sets a default ₹2,500 budget. From then on he only interacts via occasional WhatsApp thumbs-up emojis while gifts for birthdays, Karwa Chauth, and anniversary land exactly when needed.

---
_Last updated: 2025-07-20_

---

### /user-journeys/wife.md

**Path:** `/user-journeys/wife.md`  
**Characters:** 1.4k chars  

# Wife / Recipient Journey (Indian Context)

Persona reference: `/entities/wife.md`

| Stage | Expected Emotion | Potential Pain Point | System Touch | Delight Factor |
|-------|------------------|----------------------|--------------|----------------|
| 1. Pre-Event (Unaware) | Neutral | None | — | — |
| 2. Surprise Gift Arrival | Joy | Delivery mishap | Gift package (premium wrap) | Hidden note referencing shared memory |
| 3. Unboxing | Curiosity → Delight | Wrong size/flavor | QR card for feedback/thank-you | Item matches exact taste |
| 4. Appreciation | Gratitude | Remembering to thank | Auto-suggested WhatsApp message to husband | Saves time, looks heartfelt |
| 5. Post-Event Reflection | Feeling Valued | Gift fatigue (too generic) | Optional mini-survey | “How was the surprise?” |

## Emotional Curve
```
Neutral → Joy → Delight → Gratitude → Feeling Valued
```

## Success Metrics (Recipient View)
- Surprise Delight Score ≥ 8 / 10
- Gift Return / Exchange Rate ≤ 5 %
- Feedback Response Rate ≥ 40 %

## Narrative Snapshot
Priya (29, Delhi-based marketing manager) receives a beautifully wrapped necklace on her birthday, exactly her style. The card cites their Goa trip—a personal touch triggering nostalgia. She snaps a photo, taps the QR code, leaves a quick “Loved it!” rating, and sends the pre-filled thank-you WhatsApp to Rahul.

---
_Last updated: 2025-07-20_

---

### /vision-mvp.md

**Path:** `/vision-mvp.md`  
**Characters:** 2.5k chars  

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

---

### /vision.md

**Path:** `/vision.md`  
**Characters:** 3.2k chars  

# Product Vision: Lazy Husband Gifting App

## 1. Inspirational Narrative
Gifts are more than objects—they are moments of appreciation that strengthen relationships. Yet many partners let busy schedules and forgetfulness turn special occasions into stress. Our app is the invisible side-kick that quietly handles gifting end-to-end, so love gets celebrated—even when life gets hectic.

## 2. Purpose
Provide a **zero-effort** gifting service that guarantees thoughtful, on-time gifts for every important occasion, removing mental load and preserving relationship harmony.

## 3. Guiding Principles
1. **Simplicity** – one set-up, then we handle the rest.
2. **Personalization** – gifts match the recipient’s taste and the occasion.
3. **Reliability** – reminders, ordering, and delivery never miss a date.
4. **Delight** – every touch-point feels like a concierge experience.

## 4. Market Opportunity & Positioning
| Segment | TAM (US) | Gap We Address |
|---------|----------|----------------|
| Occasion-based gifting (anniversary, birthday) | ~$40B / year | Existing services rely on the giver remembering & choosing each time |
| Subscription / curated boxes | ~$5B / year | Mostly generic, no date-specific automation |
| Reminder apps & calendars | N/A (utility) | No integrated purchase & fulfillment |

**Positioning statement:** *The first gifting platform that combines automated reminders, AI curation, and turnkey fulfillment—so you never have to “remember to remember.”*

## 5. Long-Term Product Future (3–5 Years)
- **AI Taste Graph** that learns recipient style and evolves recommendations.
- **Expanded Relationships**: parents, friends, corporate gifting.
- **Experience Bundles**: spa days, travel vouchers, charity donations.
- **B2B Employee Recognition** SaaS.
- **Global Fulfillment Network** with local vendors for same-day delivery.

## 6. Product Differentiators
- One-time *lazy-mode* activation → zero ongoing user effort.
- Curated marketplace limited to merchants offering affiliate commissions (built-in revenue stream).
- Tiered subscriptions (Standard, Premium Concierge, VIP).
- Intelligent fallback logic (backup gifts & shipping contingencies).

## 7. Vision KPIs
| North-Star | Supporting Signals |
|------------|-------------------|
| % of key dates where a gift is delivered on time | Retention rate, NPS, Avg. commission per user, Gift satisfaction score |

## 8. Target Users & Personas
- **Primary:** "Lazy Husband" – forgetful partner who values relationship harmony but dislikes planning.
- **Recipient Persona:** Spouse/partner who appreciates thoughtful surprises.
- **Future:** Busy professionals, frequent travelers, HR managers (for employee gifting).

## 9. Goals
- Eliminate gifting stress for 1 M users within 3 years.
- Achieve 95 % on-time delivery rate.
- Reach $20 ARPU in annual affiliate & subscription revenue.

## 10. Alignment with Concept Note
This vision underpins the Concept Note’s problem statement and revenue model. All strategic pillars—affiliate commissions, subscription upsells, and effortless experience—map directly to the long-term roadmap above.

---
*Last updated: 2025-07-20*
