# 🚀 GoDaddy Hosting Deploy Workflow

This GitHub Actions workflow makes it simple to **build and deploy static websites** (Vite, React, Next.js static export, Lovable, WindSurf, V0, etc.) directly to **GoDaddy shared hosting** via FTP/FTPS.  

No more zipping files and uploading them in cPanel — every push to `main` (or a manual trigger) can automatically compile your project and deploy it live.  

---

## ✨ Features
- 🔧 **Zero setup**: add one YAML file to `.github/workflows/`
- 🔄 **Reusable**: call this workflow from any repo with a single `uses:` line
- 🎛 **Flexible**: configure Node.js version, build command, deploy directory, and logging level
- 🔐 **Secure**: uses GitHub Secrets for credentials (`FTP_HOST`, `FTP_USER`, `FTP_PASS`)
- 🌍 **Compatible**: works with any static site generator (Vite, React, Next.js export, Astro, etc.)

---

## 📦 Inputs

| Input         | Required | Default                   | Description                                                                 |
|---------------|----------|---------------------------|-----------------------------------------------------------------------------|
| `server_dir`  | ✅ Yes   | —                         | Target folder relative to your FTP root (`./` for `public_html`, or `./mysubdomain/`) |
| `node_version`| ❌ No    | `20`                      | Node.js version to use for the build                                        |
| `build_cmd`   | ❌ No    | `npm ci && npm run build` | Command to build your site                                                  |
| `local_dir`   | ❌ No    | `dist/`                   | Local build output folder                                                   |
| `protocol`    | ❌ No    | `ftps`                    | FTP protocol (`ftp` or `ftps`)                                              |
| `clean_slate` | ❌ No    | `true`                    | Remove old files before deploy                                              |
| `log_level`   | ❌ No    | `minimal`                 | Log detail level (`minimal`, `standard`, `verbose`)                         |

---

## 🔑 Secrets

| Secret     | Description                                  |
|------------|----------------------------------------------|
| `FTP_HOST` | Your GoDaddy FTP host (domain or IP)         |
| `FTP_USER` | Your FTP username                            |
| `FTP_PASS` | Your FTP password                            |

---

## 🖥 Setup Steps

1. In **GoDaddy cPanel**, create or confirm your FTP account.  
   - FTP root is usually `/home/<user>/public_html`  
   - In this workflow, use `./` for root or `./subfolder/` for subdomains.  

2. In **GitHub repo → Settings → Secrets and variables → Actions**, add:  
   - `FTP_HOST` (e.g. your domain or server IP)  
   - `FTP_USER`  
   - `FTP_PASS`  

3. Commit your workflow file under `.github/workflows/`.  

4. Push to `main` or trigger manually in **Actions** → “🚀 Deploy to GoDaddy”.  

---

## 🛠 Troubleshooting
- **Wrong folder?** If files show up under `public_html/home/...`, your `server_dir` was wrong. Use `./` or `./subfolder/` instead of absolute `/home/...` paths.  
- **No changes deployed?** Delete `.ftp-deploy-sync-state.json` from your server root and re-run.  
- **Client-side routing (SPA)?** Add this `.htaccess` in your deploy folder:  
  ```apache
  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /
    RewriteRule ^index\.html$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.html [L]
  </IfModule>
