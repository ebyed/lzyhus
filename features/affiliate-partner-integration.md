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