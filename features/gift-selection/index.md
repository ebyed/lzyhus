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
