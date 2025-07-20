# Lazy Husband Gifting App – Lovable One-Shot Prompt

## Context

* Role: “You are an expert full-stack developer working in Lovable.”
* Product vision & target users: A playful, zero-effort gifting web app for busy or forgetful husbands (25–45, India-centric) that remembers important dates, automatically selects thoughtful gifts, orders them via affiliate partners, and delivers on time with AI-generated love notes. Indirect beneficiaries are wives who receive delightful, personalised surprises.
* High-level problem: Husbands frequently forget special occasions, causing stress and disappointment. Manually choosing, buying, and shipping gifts is time-consuming and error-prone.
* Key value proposition: “Never miss an occasion again.” One-time setup → automated reminders, gift selection, ordering, tracking, and feedback—all powered by a commission-based model that keeps the service low-cost.

## Task

1. Build the first working MVP of the Lazy Husband Gifting App in this Lovable run.
2. Implement the following pages / components / features (in priority order):
   1. Sign-Up / Authentication page with Supabase email magic-link flow.
   2. Step-by-Step Onboarding Wizard (3 screens) introducing value + collecting spouse name & default budget.
   3. Important Dates Dashboard  
      • List + calendar view of upcoming / past occasions with edit & delete.  
      • Empty states & loading skeletons for first-time and fetching scenarios.
   4. Add / Edit Date Modal (focus-trapped)  
      • Validation, duplication check, recurrence toggle, budget override.
   5. Lazy Mode Settings Panel  
      • On/off switch, gift-budget slider, preference tags selection UI, persisted in DB.
   6. Gift Shortlist Card component (used later) showing image, title, ₹ price, AI note preview.
   7. Gift History Page (read-only table/cards with status & rating).
   8. Delivery Tracking Page  
      • White-label tracking view with horizontal/vertical Delivery Timeline Tracker for order → shipped → delivered.
   9. Settings & Preferences Page (profile info, notification toggles, payment method placeholder).
   10. Global Toast / Notification Context for success, error, info toasts across the app.
3. Core user flows to enable:
   • Registration & onboarding → date entry → Lazy Mode activation.  
   • CRUD important dates.  
   • Daily reminder scheduling stub (edge cron).  
   • Automated gift scheduling stub (giftScheduler) and shortlist generation stub (aiGiftPicker).  
   • Order fulfillment tracking with webhook stub and in-app timeline view.  
   • Viewing gift history and delivery details.  
   • Updating account settings and toggling Lazy Mode at any time.

## Guidelines

* Tech stack  
  - Front-end: React 18 + Vite + TypeScript (strict mode).  
  - Styling: TailwindCSS 3 with Mobile-First Responsive Grid utilities.  
  - Routing: React Router v6 with code-splitting via `lazy()` + `Suspense`.  
  - Back-end: Supabase (Postgres, Auth, Storage, Edge Functions).  
  - Date utils: date-fns.  
  - State: React Context + lightweight hook wrappers; avoid Redux.  

* UI / UX style rules  
  - Pastel rounded cards: 12-px radius, soft shadow, palette #FFEAF3, #FFF9E6, #E6F7FF, #E8FFE6.  
  - Modal overlay for forms using Headless UI Dialog with focus trap & ESC close.  
  - Delivery Timeline Tracker component for order statuses.  
  - Emoji accents 🎁 ❤️ 🗓️ used sparingly.  
  - All screens fully responsive down to 320 px; grid/flex layouts collapse vertically.  

* Code quality & commenting expectations  
  - Functional components with hooks; group logic in `/hooks`.  
  - Folder structure: `/pages`, `/components`, `/hooks`, `/lib`, `/supabase`, `/edge`.  
  - JSDoc on every exported function; ESLint + Prettier configured.  
  - Provide `tsconfig.json` with `strict: true`; include `.eslintrc` and `.prettierrc`.  
  - Include `.env.example` and add `.env` to `.gitignore` (Environment Variable Hygiene).  

* Critical data models & relationships (SQL snippets)  
  ```sql
  -- Users handled by Supabase Auth
  create table public.users_profile (
    id uuid primary key references auth.users,
    spouse_name text,
    default_budget_cents int default 150000,
    lazy_mode_enabled boolean default false,
    preferences text[] default '{}',
    created_at timestamptz default now()
  );

  create table public.important_dates (
    id uuid primary key default uuid_generate_v4(),
    user_id uuid references public.users_profile on delete cascade,
    occasion text not null,
    date date not null,
    is_recurring boolean default false,
    recurrence_interval text, -- 'yearly', 'monthly', etc.
    budget_override_cents int,
    created_at timestamptz default now()
  );

  create table public.gifts (
    id uuid primary key default uuid_generate_v4(),
    date_id uuid references public.important_dates on delete cascade,
    status text default 'planned', -- planned|ordered|delivered
    name text,
    affiliate_url text,
    price_cents int,
    rating int, -- 1-5 stars by recipient
    created_at timestamptz default now()
  );

  create table public.orders (
    id uuid primary key default uuid_generate_v4(),
    gift_id uuid references public.gifts on delete cascade,
    external_order_id text,
    awb text,
    status text default 'pending', -- pending|shipped|delivered|exception
    tracking_url text,
    created_at timestamptz default now()
  );
  ```

* Integration requirements  
  - Supabase Edge Function `giftScheduler` (daily cron) → checks upcoming dates, inserts planned gifts, triggers `aiGiftPicker`.  
  - Edge Function `aiGiftPicker` returns mock shortlist JSON; store top pick in `gifts` table.  
  - Edge Function `trackingWebhook` (stub) consumes carrier/vendor status updates and updates `orders`.  
  - WhatsApp Business API & payment gateway integrations are placeholders; expose commented sections for future keys.  
  - Schedule keep-alive ping to Edge Functions every 15 min (Edge Function Warm-Up Optimization).  

## Constraints

* Out-of-scope for this iteration  
  - Real payment processing, real affiliate checkout, multi-recipient support, advanced ML gift engine, analytics dashboards.  
* Forbidden tools / services  
  - No server-side frameworks other than Supabase Edge Functions; no heavyweight UI libraries >50 kB minified (e.g., MUI).  
* Performance, security & platform limits  
  - Gzipped JS bundle ≤ 1 MB; enforce code-splitting & lazy loading.  
  - Images served in WebP/AVIF with responsive sizes; use `<img loading="lazy">`.  
  - Never commit real Supabase service-role keys; all secrets via env vars only.  
  - Rate-limit unauthenticated endpoints (max 10 req/min/IP).  
* Lovable-specific considerations  
  - Prompt must compile in a single run; include all schema, component stubs, and function placeholders inline.  
  - Use `lovable deploy` conventions for Edge Functions (folder `/edge` with `index.ts`).  
  - Ensure sample data seeds (`dates`, `gifts`) for local preview within Lovable playground.