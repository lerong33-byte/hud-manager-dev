# HUD Manager — AI Chat Session Context

> Paste the contents of this file at the start of any AI chat session to give the assistant full context about this project.

---

## Project Overview

**Name:** HUD Manager (formerly SC ShipForge / scstarforge)
**Domain:** [hud-manager.com](https://hud-manager.com)
**Description:** A Star Citizen ship loadout builder and HUD management tool. It allows players to browse, configure, and compare ship loadouts including weapons, missiles, shields, and other components. The app displays detailed stats and values (in aUEC) for each loadout item.

---

## Repositories

| Repo | Purpose | URL |
|------|---------|-----|
| `hud-manager` | Production (live site) | https://github.com/lerong33-byte/hud-manager |
| `hud-manager-dev` | Development / staging | https://github.com/lerong33-byte/hud-manager-dev |

**Previous repo (legacy):** `lerong33-byte/scstarforge` — do not push new features here.

---

## Tech Stack

- **Frontend:** Single-page HTML app (`index.html`) — vanilla HTML, CSS, JavaScript. No frameworks, no build tools.
- **Hosting:** GitHub Pages (production) via Cloudflare DNS (`hud-manager.com`)
- **CDN / Proxy:** Cloudflare (DNS only mode for GitHub Pages HTTPS)
- **Functions:** Cloudflare Pages Functions (in `functions/netlify/functions/` folder) used as proxy
- **Domain registrar:** Cloudflare (`hud-manager.com`, registered May 2026)

---

## Repository Structure

```
hud-manager/
├── index.html          # Main app — entire frontend (HTML + CSS + JS in one file)
├── index.html.bak      # Backup of previous stable version
├── _headers            # Cloudflare Pages HTTP security headers
├── _redirects          # SPA fallback: /* /index.html 200
├── sitemap.xml         # SEO sitemap
├── deploy-prod.sh      # Script to strip -DEV tag and push to production
├── CNAME               # GitHub Pages custom domain file
├── CONTEXT.md          # This file — AI chat session context
└── functions/          # Cloudflare Pages serverless functions (proxy)
```

---

## Versioning Convention

- Dev builds are tagged with `-DEV` suffix in the version string inside `index.html` (e.g., `v8.52-DEV`)
- Production builds strip `-DEV` using `deploy-prod.sh` (e.g., `v8.52`)
- Version is embedded directly in `index.html` — search for `v[0-9]` to find it
- Commit messages follow: `PROD: vX.XX — description` for production, `vX.XX-DEV: description` for dev

---

## Deployment Workflow

1. **Development:** Work in `hud-manager-dev` repo. Version string includes `-DEV`.
2. **Production deploy:** Run `bash deploy-prod.sh` from the `hud-manager` repo.
   - Strips `-DEV` from version in `index.html`
      - Commits and pushes to `main` on the prod remote
         - Restores the dev state after pushing
         3. **GitHub Pages** automatically deploys on push to `main` branch.
         4. **DNS:** Cloudflare points `hud-manager.com` → GitHub Pages IPs (A records, DNS only).

         ---

         ## DNS Configuration (Cloudflare)

         | Type | Name | Content | Proxy |
         |------|------|---------|-------|
         | A | hud-manager.com | 185.199.108.153 | DNS only |
         | A | hud-manager.com | 185.199.109.153 | DNS only |
         | A | hud-manager.com | 185.199.110.153 | DNS only |
         | A | hud-manager.com | 185.199.111.153 | DNS only |
         | CNAME | www | lerong33-byte.github.io | DNS only |

         ---

         ## Key Features of the App

         - **Ship loadout display:** Shows all equipped components per hardpoint (weapons, missiles, shields, power plants, coolers, etc.)
         - **Missile rack tree:** Hierarchical view — rack row (with model, size, capacity) expands to show individual missiles. Chevron toggles expand/collapse.
         - **Component values:** Each item displays its value in aUEC
         - **Enemy loadout toggle:** Ability to toggle/change/reset enemy loadout
         - **Meta URLs:** SEO-friendly URLs for sharing specific loadouts
         - **Proxy function:** CF Pages function used to proxy external API calls

         ---

         ## HTTP Headers (`_headers`)

         ```
         /*
           X-Frame-Options: SAMEORIGIN
             X-Content-Type-Options: nosniff
               Referrer-Policy: strict-origin-when-cross-origin
                 Permissions-Policy: camera=(), microphone=(), geolocation=()

                 /index.html
                   Cache-Control: no-cache, no-store, must-revalidate

                   /sitemap.xml
                     Cache-Control: public, max-age=86400
                     ```

                     ---

                     ## Coding Conventions

                     - All code lives in a **single `index.html`** file (inline CSS + JS)
                     - No external dependencies / npm / build step
                     - Version string format: `vX.XX` or `vX.XX-DEV`
                     - JavaScript functions are globally scoped
                     - CSS uses class-based selectors; component rows use consistent naming patterns
                     - The deploy script uses `sed` to strip `-DEV` — avoid embedding `-DEV` in strings that should not be stripped

                     ---

                     ## Owner / Account

                     - **GitHub:** `lerong33-byte`
                     - **Email:** lerong33@gmail.com
                     - **Cloudflare account:** Lerong33@gmail.com

                     ---

                     ## Notes for AI Assistant

                     - This is a **solo developer project** — no team, no PRs needed unless requested
                     - Always work in `hud-manager-dev` for new features; `hud-manager` is production
                     - The app is a **single HTML file** — keep it that way unless explicitly asked to split
                     - When suggesting changes, provide the full modified section, not just diffs, unless the change is small
                     - Version bumps should increment the minor version (e.g., `v8.52` → `v8.53-DEV`)
                     - Do not run `deploy-prod.sh` without explicit user approval
