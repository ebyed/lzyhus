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