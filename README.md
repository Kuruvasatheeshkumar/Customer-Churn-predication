# ChurnIQ — Customer Churn Prediction Platform

A premium SaaS-style customer churn prediction app: authentication, an analytics
dashboard, customer management, real-time churn scoring, reports, and an admin panel.

> **Stack note:** This project runs on the Lovable stack — **React 19 + TanStack
> Start + Tailwind v4 + Lovable Cloud** (managed Postgres, auth, storage). The
> machine-learning churn model is implemented as a transparent, weighted
> logistic-regression scorer in TypeScript (`src/lib/churn.ts`) so predictions run
> live in this serverless environment. The model-comparison dashboard reflects
> representative metrics for Logistic Regression / Random Forest / XGBoost.

## Features
- **Auth** — Email/password, sign-up with validation, forgot/reset password,
  Google sign-in, JWT sessions, protected routes (`/_authenticated/*`).
- **Dashboard** — Total/active/churned customers, retention & churn rate, monthly
  growth, segmentation, and recent prediction activity (Recharts).
- **Customers** — Add / edit / delete, search & filter, pagination, profile view.
- **Predict** — Beautiful form with validation, churn probability gauge,
  confidence score, and key contributing factors.
- **ML Models** — Pipeline overview + Accuracy / Precision / Recall / F1 comparison.
- **Reports** — CSV (Excel) and PDF export, prediction history, segmentation.
- **Admin** — Role-gated panel for users, predictions, and activity logs.
- **UX** — Glassmorphism, gradients, dark/light toggle, responsive, animations.

## Database schema (Lovable Cloud / Postgres)
- `profiles` — user profile (id → auth user, email, full_name, avatar_url).
- `user_roles` — `app_role` enum (`admin`/`user`); checked via `has_role()`.
- `customers` — tenure, charges, contract, payment method, tickets, status, churn_risk.
- `predictions` — inputs, churn_probability, will_churn, confidence, factors, model.
- `activity_logs` — user actions.

All tables use Row Level Security: users access only their own rows; admins can read
all. Roles live in a separate table to prevent privilege escalation.

## Data access ("API")
App-internal data access uses the Supabase client with RLS (`@/integrations/supabase/client`)
and TanStack Query (`src/lib/queries.ts`). Server endpoints (sitemap) use TanStack
server routes. There is no separate REST server — RLS-protected queries are the API.

## Project structure
- `src/routes/` — file-based routes (`index`, `auth`, `reset-password`, `_authenticated/*`).
- `src/components/` — app shell, dialogs, stat cards, UI primitives.
- `src/lib/churn.ts` — churn scoring model + model metrics.
- `src/lib/queries.ts`, `src/lib/export.ts` — data queries and CSV/PDF export.

## Deployment
Click **Publish** in Lovable to deploy. Backend (DB/auth) is provisioned
automatically by Lovable Cloud. Custom domains are available after publishing.
