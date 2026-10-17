# Overview

When a business activates **White-label AI Change** in ServiceFlow:

1. AI Change Platform generates a **private** tenant repo from the white-label template (`serviceflow-wl-{slug}`).
2. Collaborator emails are invited under the platform GitHub account.
3. `develop` deploys to the customer’s **dev** URL; `master` deploys to **prod** (their domain).
4. Cursor cloud agents make changes on feature branches → PR to `develop` → promote `develop` → `master`.

## Agent guidance (for operators / agents)

This is a **white-label ServiceFlow instance**, not the multi-tenant SaaS monorepo.

- Prefer brand/theme/config, copy, and customer-facing UX changes.
- Do **not** hardcode secrets, platform API keys, or other tenants’ data.
- Keep the `develop` / `master` branch contract and deploy workflow **file names** intact.
- Treat as sensitive: `.env*`, secrets, `.github/**`, billing/auth isolation code.

## What is public vs private

| Surface | Visibility |
|---------|------------|
| This documentation | Public |
| White-label template source | Private |
| Per-customer `serviceflow-wl-*` repos | Private |
