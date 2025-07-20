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