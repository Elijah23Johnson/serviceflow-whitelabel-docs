# Operator setup

For platform operators only. Customer end-users do not need this.

## Private template

The generate-from-template source repo is **private**. Set AI Change Platform env:

```text
SF_TEMPLATE_REPO=<your-github-user-or-org>/serviceflow-whitelabel-template
```

## Checklist

1. Mark the private template repo as a **GitHub Template repository**.
2. Push `master` and `develop` on the template.
3. Install the AI Change Platform GitHub App and Cursor GitHub App with access to the template and generated `serviceflow-wl-*` repos.
4. Prefer **org-level** Actions secrets so generated repos inherit deploy credentials.
5. On activate in ServiceFlow, ACP calls generate-from-template (private repos, all branches).

## Sync note

GitHub templates do **not** auto-update generated repos. Platform updates should be proposed via sync PRs into tenant `develop` (optional automation), not force-pushed to customer `master`.
