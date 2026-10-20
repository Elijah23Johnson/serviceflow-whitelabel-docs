# Operator setup

For platform operators only. Customer end-users do not need this.

## Private template

The generate-from-template source repo is **private**. Set AI Change Platform env:

```text
SF_TEMPLATE_REPO=<github-user-or-org>/serviceflow-whitelabel-template
```

Default used by ServiceFlow deploy scripts: `Elijah23Johnson/serviceflow-whitelabel-template`.

## GCP

- Project for ACP + white-label tenant deploys: **`streaminglive-633c1`** (not `aspen-flight-ops`).
- Mount deploy SA JSON on ACP as `WL_GCP_SA_KEY` / `GCP_SA_KEY` so provision can copy `GCP_SA_KEY` onto each `serviceflow-wl-*` repo.
- Mount `WL_GCP_PROJECT_ID` / `GCP_PROJECT_ID` (default `streaminglive-633c1`) the same way.

## Checklist

1. Mark the private template repo as a **GitHub Template repository**.
2. Push `master` and `develop` on the template (keep `deploy-dev.yml` / `deploy-prod.yml` names).
3. Install the AI Change Platform GitHub App and Cursor GitHub App with access to the template and generated `serviceflow-wl-*` repos (prefer **All repositories** so new tenants are covered).
4. Provide **`ACP_GITHUB_HOST_TOKEN`** (classic PAT with `repo` + `workflow` on the host account that owns `SF_TEMPLATE_REPO`). GitHub App installation tokens cannot clone private templates via `/generate`.
5. Ensure ACP has handoff / internal secrets shared with the main ServiceFlow API (`ACP_HANDOFF_SECRET`, `ACP_INTERNAL_SECRET`).
6. On activate in ServiceFlow, ACP generates the private repo, invites collaborators, syncs Actions secrets/vars, and begins domain connect.

## Customer activate path (expected UX)

1. Platform admin grants White-label AI Change for the business.
2. Customer (or ops) sets apex domain + collaborator emails → **Activate**.
3. Verify domain ownership (Search Console / TXT) for Cloud Run mapping.
4. Wait for first real Deploy Dev/Prod (after secrets land) before expecting custom domain mapping.
5. Open **AI Change Studio** (SSO) or **Download / Open ServiceFlow Pro IDE**.

## Sync note

GitHub templates do **not** auto-update generated repos. Re-sync curated app code from the monorepo into the template and existing `serviceflow-wl-*` clones via the platform sync script (operators). Prefer sync into tenant `develop`, not force-push to customer `master`, unless you intentionally rebaseline.

## Related ServiceFlow surfaces

| Surface | Purpose |
|---------|---------|
| Settings → Integrations → White-label AI Change | Grant, activate, DNS, Studio launch, IDE download |
| `GET /api/white-label-ai-change/ide-download` | Installer links (`IDE_DOWNLOAD_BASE_URL`) |
| `GET /api/white-label-ai-change/open-ide` | Deep link / download fallback |
| Public marketing | [serviceflow-pro.com/white-label](https://serviceflow-pro.com/white-label) |
