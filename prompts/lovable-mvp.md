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
