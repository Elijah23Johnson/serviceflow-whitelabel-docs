# Deployment

## Workflows

- **Dev:** `deploy-dev.yml` runs on push to `develop`.
- **Prod:** `deploy-prod.yml` runs on push to `master`.

Both deploy `web` and `api` images to Cloud Run. Service names are derived from the tenant repo name:

- `{REPO_NAME}-web-dev` / `{REPO_NAME}-api-dev`
- `{REPO_NAME}-web` / `{REPO_NAME}-api`

## Domain mapping

After the first successful deploy:

1. Point `CUSTOM_DOMAIN` CNAME/A to Cloud Run domain mapping for the prod web service.
2. Point `DEV_CUSTOM_DOMAIN` to the dev web service.
3. Store the live URLs in AI Change Platform so the studio can show them after promote.

## Required GitHub Actions secrets (operators)

| Secret | Description |
|--------|-------------|
| `GCP_SA_KEY` | Deploy service account JSON |
| `GCP_PROJECT_ID` | GCP project |
| `DATABASE_URL_DEV` / `DATABASE_URL_PROD` | Tenant databases |
| `SESSION_SECRET` | Session signing |

## AI Change Platform contract

The platform watches `deploy-dev.yml` after merge to `develop`, then `deploy-prod.yml` after promote `develop` → `master`. Keep those workflow **file names** unchanged.
