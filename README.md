[PizzaCut](https://github.com/fidadeve/pizzacutbyfida)

All-in-one pizza ordering and delivery application: storefront, admin dashboard, rider portal, and a local PGlite-based Postgres dev database you can run entirely on your machine.

## What's in this repo

- **Storefront** — menu, product detail, cart, checkout, order tracking, FAQs, policy pages, about/contact.
- **Admin dashboard** — orders, deliveries, products, categories, toppings, promo codes, riders, customers, content (pages & FAQs), and store settings.
- **Rider portal** — delivery lifecycle (accept → pickup → out-for-delivery → delivered), live availability toggle, issue reporting.
- **Auth** — single login for customers, riders, and admins; session cookies; change-password and profile routes.
- **Local database** — a small PGlite socket server (`scripts/local-db-server.mjs`) that runs a real Postgres wire-protocol database on your laptop, plus `npm run db:setup` (migrate + seed).

## Quick start

```bash
npm install
npm run db:setup
npm run dev
```

The app opens on `http://localhost:3000`. The first admin account is created by `npm run db:setup` from `ADMIN_EMAIL` / `ADMIN_PASSWORD` in your environment (see `.env.example`). If those are unset, a strong admin password is generated and printed once.

## Environments

| Variable | Purpose |
|---|---|
| `DATABASE_URL` | Postgres connection string used by the app everywhere. Local dev points at the PGlite socket server; production points at a managed Postgres. |
| `PG_MAX_CONNECTIONS` | `postgres.js` pool size. Set to `1` locally so the pool never contends the single PGlite socket. |
| `LOCAL_DB_MAX_CONNECTIONS` | `PGLiteSocketServer` max connections. Set to `1` locally. |
| `AUTH_SECRET` | Server token used for session cookies. |
| `APP_ORIGIN` | Public origin used for auth callbacks (e.g. `http://localhost:3000` in dev, your real domain in production). |

## Local DB note

The local PGlite socket server is single-protocol-state: it accepts one active Postgres frontend at a time. Keep both the app pool and the socket server pinned to a single connection locally (`PG_MAX_CONNECTIONS=1` and `LOCAL_DB_MAX_CONNECTIONS=1`). That is already the default in `.env.local` / `.env.example`.

## API

Admin routes live under `/api/admin/*`. Storefront-facing routes live under `/api/*` (cart, auth, orders, products, etc.). Rider routes live under `/api/rider/*`.

## Dev

```bash
npm run typecheck
npm run test
npm run build
```

## Repo

- Owner: `fidadeve`
- Repo: `pizzacutbyfida`
- Default branch: `main`
