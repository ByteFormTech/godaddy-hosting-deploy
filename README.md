# GoDaddy Hosting: Build & FTP Deploy (Composite Action)

A composite GitHub Action to **build** a static site (Vite/React/Next export/etc.) and **deploy** it to **GoDaddy shared hosting** via FTP/FTPS.

## Inputs
- `server-dir` (required): Remote path relative to your FTP root (e.g., `./` or `./subfolder/`).
- `node-version` (default: `20`)
- `build-cmd` (default: `npm ci && npm run build`)
- `local-dir` (default: `dist/`)
- `protocol` (default: `ftps`)
- `clean-slate` (default: `true`)
- `log-level` (default: `minimal`)

## Environment Variables (required)
- `FTP_HOST`: Your FTP host (domain/IP)
- `FTP_USER`: FTP username
- `FTP_PASS`: FTP password

> Tip: Add these as **GitHub Secrets** in your repository and map them via `env:` when using this action.