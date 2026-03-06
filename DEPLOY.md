# Band Checker Deployment Guide

Two identical copies of the band checker are served from different infrastructure for redundancy.

## 1. Railway (keitextc.com/compatibility)

This is automatic. The files live in `website/compatibility/` and are served by the FastAPI routes added to `src/api/server.py`. When you push to your main repo and Railway redeploys, the pages go live at:

- https://keitextc.com/compatibility
- https://keitextc.com/compatibility/canada.html

No extra steps needed.

## 2. GitHub Pages (tools.keitextc.com)

### Push the repo

```powershell
cd keitex-band-checker
git init
git add .
git commit -m "Carrier band compatibility checker"
gh repo create keitextc/keitex-band-checker --public --source=. --push
```

### Enable GitHub Pages

1. Go to https://github.com/keitextc/keitex-band-checker/settings/pages
2. Source: Deploy from a branch
3. Branch: main, folder: / (root)
4. Save

### Add Cloudflare DNS record

1. Log into Cloudflare dashboard for keitextc.com
2. Go to DNS > Records
3. Add a new CNAME record:
   - Type: CNAME
   - Name: tools
   - Target: keitextc.github.io
   - Proxy status: DNS only (grey cloud) — required for GitHub Pages SSL
4. Save

### Verify custom domain in GitHub

1. Back in GitHub repo settings > Pages > Custom domain
2. Enter: tools.keitextc.com
3. Click Save
4. Wait for DNS check to pass (usually 1-2 minutes with Cloudflare)
5. Check "Enforce HTTPS" once the certificate is provisioned

Once complete, the pages are live at:

- https://tools.keitextc.com (main US/Canada checker)
- https://tools.keitextc.com/canada.html (Canada deep dive)

## Keeping files in sync

The source of truth is `keitex-band-checker/`. When updating:

1. Edit files in `keitex-band-checker/`
2. Copy to `website/compatibility/`:
   ```powershell
   Copy-Item keitex-band-checker\index.html website\compatibility\index.html
   Copy-Item keitex-band-checker\canada.html website\compatibility\canada.html
   ```
3. Commit and push both repos

## Email contact

Both pages show admin@keitextc.com in the top nav bar. If your business email is different, update the `topnav` div near the top of both HTML files.
