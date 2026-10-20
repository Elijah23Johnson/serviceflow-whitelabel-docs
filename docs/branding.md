# Branding & hosts

White-label branding and hostnames are driven by **GitHub Actions variables** (and matching Cloud Run / Secret Manager env). Do not commit customer logos or secrets into source.

| Variable | Used for | Example |
|----------|----------|---------|
| `APP_NAME` | Product name in chrome / emails | Customer brand name |
| `PRIMARY_COLOR` | Accent / CSS brand token | `#2563eb` |
| `LOGO_URL` | Nav / login | HTTPS URL |
| `CUSTOM_DOMAIN` | Prod web host / cookies | `app.{apex}` |
| `DEV_CUSTOM_DOMAIN` | Dev web host | `dev.{apex}` |
| `PROD_WEB_BASE_URL` | Absolute prod web URL | `https://app.{apex}` |
| `DEV_WEB_BASE_URL` | Absolute dev web URL | `https://dev.{apex}` |
| `PROD_API_BASE_URL` | Tenant API (baked into prod web as `VITE_API_URL`) | `https://api.{apex}` |
| `DEV_API_BASE_URL` | Tenant API (dev web bake) | `https://dev-api.{apex}` |

## Critical: tenant API, not shared API

Do **not** point the SPA at `https://api.serviceflow-pro.com` for a white-label tenant. That is the shared ServiceFlow API; tenant login must use `api.{apex}` (with CORS configured on the tenant API during domain setup).

If `PROD_API_BASE_URL` / `DEV_API_BASE_URL` are missing or still point at the shared API, browser login will fail with CORS errors for the tenant origin. Fix the Actions variables, then re-run **Deploy Prod** / **Deploy Dev** so web rebuilds with the correct `VITE_API_URL`.

Agents should update CSS tokens and copy, not bake secrets into the repo.
