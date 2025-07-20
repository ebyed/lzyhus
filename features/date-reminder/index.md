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