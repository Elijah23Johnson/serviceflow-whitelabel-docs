# ServiceFlow White-Label — Public Docs

This repository contains **public documentation** for the ServiceFlow white-label product and AI Change pipeline.

The **template source code** lives in a separate **private** repository and is not published here.

## Docs

- [Overview](docs/overview.md) — how white-label + AI Change works
- [Branding](docs/branding.md) — env-driven brand hooks
- [Deployment](docs/deployment.md) — `develop` / `master` deploy contract
- [Operator setup](docs/operator-setup.md) — GitHub Apps, secrets, provisioning (for platform operators)

## Branch contract

| Branch | Role | Workflow |
|--------|------|----------|
| `develop` | Dev / staging | `deploy-dev.yml` |
| `master` | Production | `deploy-prod.yml` |

Agents open PRs to `develop`; promoting `develop → master` ships production.

## License

Documentation: public. Product / template source: private.
