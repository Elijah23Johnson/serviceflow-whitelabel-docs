# Deployment

## App source

The private template ships curated ServiceFlow `apps/web` + `apps/api` (plus required packages). Platform operators re-sync from the ServiceFlow monorepo and push to the template + existing `serviceflow-wl-*` clones. GitHub “Use this template” only copies at create time — tenants do **not** auto-update.

## Workflows

- **Dev:** `deploy-dev.yml` runs on push to `develop`.
- **Prod:** `deploy-prod.yml` runs on push to `master`.

Both build via root `Dockerfile.web` / `Dockerfile.api` and deploy to Cloud Run. Service names are derived from the repo name:

- `{REPO_NAME}-web-dev` / `{REPO_NAME}-api-dev`
- `{REPO_NAME}-web` / `{REPO_NAME}-api`

### Web must call the tenant API

The SPA bakes `VITE_API_URL` from GitHub Actions **variables** at image build time:

| Environment | Variable | Example |
|-------------|----------|---------|
| Prod web | `PROD_API_BASE_URL` | `https://api.yourdomain.com` |
| Dev web | `DEV_API_BASE_URL` | `https://dev-api.yourdomain.com` |

If these are missing or still point at `https://api.serviceflow-pro.com`, login hits the **shared** ServiceFlow API and CORS fails for the tenant origin.

Provision / Studio setup sets these vars automatically. To fix an existing tenant: confirm Actions variables, then re-run **Deploy Prod** / **Deploy Dev**.

## Domain mapping (web + API)

ServiceFlow automates Cloud Run domain mappings when the tenant activates White-label AI Change (or refreshes DNS in Integrations). Expect **four** hosts:

| Host | Cloud Run service |
|------|-------------------|
| `app.{apex}` (`CUSTOM_DOMAIN`) | `{REPO}-web` |
| `dev.{apex}` (`DEV_CUSTOM_DOMAIN`) | `{REPO}-web-dev` |
| `api.{apex}` (`PROD_API_BASE_URL`) | `{REPO}-api` |
| `dev-api.{apex}` (`DEV_API_BASE_URL`) | `{REPO}-api-dev` |

During setup, the tenant **API** Cloud Run service is updated with:

- `WEB_APP_URL` / related → `https://app.{apex}` (or `https://dev.{apex}`)
- `CUSTOM_DOMAIN` → app host
- `API_URL` / `API_BASE_URL` → `https://api.{apex}` (or Cloud Run URL until cert is ready)
- `CORS_EXTRA_ORIGINS` → the matching web origin

That allows browser login from the tenant SPA without manual CORS edits.

**Ownership:** Google Cloud Run domain mapping requires the apex (or parent) verified for the mapping identity (Search Console / DNS TXT). Temporary `*.run.app` URLs work before custom domain/certs are ready.

## GCP project

ServiceFlow white-label tenant deploys use GCP project **`streaminglive-633c1`**. Do **not** deploy SF white-label services into `aspen-flight-ops`.

Set Actions secret `GCP_PROJECT_ID` to `streaminglive-633c1`.

## Required GitHub Actions secrets

| Secret | Description |
|--------|-------------|
| `GCP_SA_KEY` | Deploy service account JSON (plain JSON starting with `{`). **Required** — empty value makes `google-github-actions/auth` fail. |
| `GCP_PROJECT_ID` | GCP project (`streaminglive-633c1`) |

AI Change Platform provision / host-var sync copies these from ACP env onto new tenant repos. If the first deploy failed before that was configured, add them under **Settings → Secrets and variables → Actions**, then re-run **Deploy Dev**.

**First-push race:** Template create can trigger Deploy Dev/Prod before secrets exist. Workflows **skip green** when `GCP_SA_KEY` is empty; ACP writes secrets then dispatches a real deploy. You should not see a hard failure on those first gated runs once the current template is in use.

Cloud Run `--set-secrets` references **GCP Secret Manager** names (not GitHub secrets), e.g. tenant `DATABASE_URL_*` and shared platform secrets such as `SESSION_SECRET` / `JWT_SECRET`.

## Required GitHub Actions variables

| Variable | Description |
|----------|-------------|
| `APP_NAME` | Product name in the UI |
| `PRIMARY_COLOR` | Brand accent |
| `CUSTOM_DOMAIN` | Prod web host (`app.{apex}`) |
| `DEV_CUSTOM_DOMAIN` | Dev web host (`dev.{apex}`) |
| `PROD_WEB_BASE_URL` | `https://app.{apex}` |
| `DEV_WEB_BASE_URL` | `https://dev.{apex}` |
| `PROD_API_BASE_URL` | `https://api.{apex}` — baked into prod web as `VITE_API_URL` |
| `DEV_API_BASE_URL` | `https://dev-api.{apex}` — baked into dev web as `VITE_API_URL` |

## AI Change Platform contract

ACP watches `deploy-dev.yml` after merge to `develop`, then `deploy-prod.yml` after promote `develop` → `master`. Keep workflow **file names** unchanged. Success detection considers whether a workflow run actually deployed (not a secrets-gate skip).
