# Overview

**White-label Light** (White-label AI Change) gives a business a private ServiceFlow instance on its own domain, with an AI-assisted change pipeline — without transferring the ServiceFlow brand or the hosted multi-tenant product.

When a business activates **White-label AI Change** in ServiceFlow (Settings → Integrations):

1. AI Change Platform generates a **private** tenant repo from the curated white-label template (`serviceflow-wl-{slug}`).
2. Collaborator emails are invited under the platform GitHub account.
3. Domain setup maps **four** hosts and configures the tenant API for CORS / web origin:
   - `app.{apex}` / `dev.{apex}` → web
   - `api.{apex}` / `dev-api.{apex}` → API
4. `develop` deploys to **dev**; `master` deploys to **prod**.
5. Changes ship via **AI Change Studio** (chat → agent → PR to `develop` → promote `develop` → `master`), or locally with **ServiceFlow Pro IDE**.

## Ways to change the product

| Path | What it is |
|------|------------|
| AI Change Studio | Browser SSO into ACP; agents open PRs to `develop`, then promote to production |
| ServiceFlow Pro IDE | Branded VS Code OSS: device-auth, clone the private repo, write `.env`, run API + Web locally |

## Agent guidance (for operators / agents)

This is a **white-label ServiceFlow instance**, not the multi-tenant SaaS monorepo.

- Prefer brand/theme/config, copy, and customer-facing UX changes.
- Do **not** hardcode secrets, platform API keys, or other tenants’ data.
- Keep the `develop` / `master` branch contract and deploy workflow **file names** intact.
- Treat as sensitive: `.env*`, secrets, `.github/**`, billing/auth isolation code.
- Do **not** point the SPA at `https://api.serviceflow-pro.com` — tenant web must bake `VITE_API_URL` to `api.{apex}` / `dev-api.{apex}`.

## Branch contract

| Branch | Role | Workflow |
|--------|------|----------|
| `develop` | Dev / staging | `deploy-dev.yml` |
| `master` | Production | `deploy-prod.yml` |

Agents open PRs to `develop`; promoting `develop → master` ships production.

## What is public vs private

| Surface | Visibility |
|---------|------------|
| This documentation | Public |
| White-label template source | Private |
| Per-customer `serviceflow-wl-*` repos | Private |
| Hosted serviceflow-pro.com product | Separate SaaS (not transferred by white-label) |

## Full source ownership (separate offer)

Organizations that need a one-time owned copy of the application source and infrastructure (not this Light path) should see the public [White label](https://serviceflow-pro.com/white-label) page on ServiceFlow Pro. That offer is a signed commercial agreement, not GitHub template access.
