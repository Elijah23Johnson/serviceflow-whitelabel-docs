# ServiceFlow White-Label — Public Docs

Public documentation for **White-label Light** (White-label AI Change): private tenant repos, custom domain, AI Change Studio, and ServiceFlow Pro IDE.

The **template source code** lives in a separate **private** repository and is not published here.

## Docs

- [Overview](docs/overview.md) — how white-label + AI Change works
- [Branding](docs/branding.md) — env-driven brand hooks and tenant API bake
- [Deployment](docs/deployment.md) — `develop` / `master` deploy, four-host domains, secrets
- [ServiceFlow Pro IDE](docs/ide.md) — local code-oss path
- [Operator setup](docs/operator-setup.md) — GitHub Apps, secrets, provisioning (platform operators)

## Pages site

Static HTML mirrors these docs (GitHub Pages): `index.html`, `branding.html`, `deployment.html`, `ide.html`, `operator-setup.html`.

## Branch contract

| Branch | Role | Workflow |
|--------|------|----------|
| `develop` | Dev / staging | `deploy-dev.yml` |
| `master` | Production | `deploy-prod.yml` |

Agents open PRs to `develop`; promoting `develop → master` ships production.

## License

Documentation: public. Product / template source: private.
