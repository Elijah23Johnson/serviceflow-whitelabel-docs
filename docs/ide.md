# ServiceFlow Pro IDE

Optional local development path for white-label tenants. Complements **AI Change Studio** (cloud agents).

## What it is

A branded VS Code OSS build (**ServiceFlow Pro IDE**) that:

1. Signs in with device auth against AI Change Platform (`/ide/authorize`).
2. Clones the business’s private `serviceflow-wl-*` repo.
3. Writes a local `.env` from the bootstrap/env bundle.
4. Runs **API + Web** via committed `.vscode` launch/tasks.

## Customer flow

1. In ServiceFlow → **Settings → Integrations → White-label AI Change**, use **Download ServiceFlow Pro IDE** (or **Open in IDE** once installed).
2. Install for your platform (macOS Apple Silicon / Intel, Windows).
3. Sign in from the IDE sidebar; approve the device code in the browser.
4. Run **ServiceFlow: Open My Code**, then **Run and Debug → API + Web**.

Installers and `latest.json` are published to a public GCS bucket (default base `https://storage.googleapis.com/serviceflow-pro-ide-downloads`). ServiceFlow API resolves the correct download via `/api/white-label-ai-change/ide-download`.

## Operator notes

| Item | Notes |
|------|--------|
| `IDE_DOWNLOAD_BASE_URL` | Optional override on the main ServiceFlow API |
| `IDE_VERSION` | Fallback version string if `latest.json` is unreachable |
| ACP | Device codes / IDE tokens live in the AI Change Platform database |

Source for the IDE product lives in the private ServiceFlow monorepo; this public docs repo does not publish IDE source.
