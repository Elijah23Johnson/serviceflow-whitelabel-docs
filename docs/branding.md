# Branding

White-label branding is driven by environment variables (set per Cloud Run service / Secret Manager).

| Key | Used by | Notes |
|-----|---------|-------|
| `APP_NAME` | Web shell title, emails | Customer product name |
| `PRIMARY_COLOR` | CSS `--brand-primary` | Hex |
| `LOGO_URL` | Nav / login | HTTPS URL |
| `CUSTOM_DOMAIN` | Prod redirects, cookies | Apex or www |
| `DEV_CUSTOM_DOMAIN` | Dev environment | Usually `dev.{CUSTOM_DOMAIN}` |
| `API_BASE_URL` | Public API origin | |
| `WEB_BASE_URL` | Public web origin | |

Do not commit customer logos or secrets into source. Agents should update CSS tokens and copy, not bake secrets into the repo.
