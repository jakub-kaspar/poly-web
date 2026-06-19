# Polymont CMS — Decap Setup Guide

Decap CMS is a **Git-based CMS** — edits are committed directly to the GitHub repository.  
No server needed. The admin panel lives at `/admin/` on the website.

---

## What the CMS manages

| Section | File edited |
|---|---|
| Reference gallery (photos + categories) | `_data/references.json` |
| Contact info, address, hours | `_data/settings.json` |

> **Note:** The website reads `_data/references.json` via JavaScript at runtime.  
> `_data/settings.json` is reserved for future dynamic loading — currently contact info is hardcoded in the HTML. Run a find-and-replace when you update it.

---

## 1. Deploy the site to a host

Decap CMS authenticates via GitHub OAuth, which requires HTTPS.  
Recommended: **Netlify** (free tier is fine).

1. Push the `Polymont web` folder to a GitHub repository
2. Connect the repo to Netlify → it deploys automatically
3. Note your site URL (e.g. `https://polymont.netlify.app`)

---

## 2. Update `admin/config.yml`

Open `admin/config.yml` and replace the placeholder:

```yaml
backend:
  name: github
  repo: YOUR_GITHUB_USERNAME/YOUR_REPO_NAME   # ← replace this
  branch: main
```

Example:
```yaml
backend:
  name: github
  repo: polymont/polymont-web
  branch: main
```

---

## 3. Set up GitHub OAuth on Netlify

Decap needs GitHub OAuth to let editors log in.

1. Go to **GitHub → Settings → Developer settings → OAuth Apps → New OAuth App**
2. Fill in:
   - **Application name:** Polymont CMS
   - **Homepage URL:** `https://your-site.netlify.app`
   - **Authorization callback URL:** `https://api.netlify.com/auth/done`
3. Copy the **Client ID** and **Client Secret**
4. In Netlify → **Site settings → Access control → OAuth**
   - Add provider: **GitHub**
   - Paste Client ID and Client Secret
5. Save

---

## 4. Access the admin panel

Go to `https://your-site.netlify.app/admin/`  
Log in with your GitHub account → you'll see the CMS dashboard.

---

## 5. Local development (no GitHub needed)

To run the CMS locally against your filesystem:

```bash
# In the project root:
npx decap-server
```

Then open `http://localhost:8080/admin/` in your browser.  
Changes are written directly to `_data/*.json` — no Git commits needed locally.

> The `local_backend: true` line in `config.yml` enables this mode automatically.

---

## 6. Adding / removing gallery photos

1. In the CMS, go to **Reference (galerie) → Seznam fotek**
2. Click **+** to add a photo, or click an existing entry to edit/delete
3. Upload the image or pick from the media library
4. Set the category: **Halové vestavby** or **Vrata a dveře**
5. Click **Save** — the change is committed to GitHub and auto-deployed

---

## Folder structure

```
Polymont web/
├── admin/
│   ├── index.html          ← Decap CMS admin UI
│   └── config.yml          ← CMS configuration
├── _data/
│   ├── references.json     ← gallery photos (edited via CMS)
│   └── settings.json       ← contact info (edited via CMS)
├── main.js                 ← loads references.json at runtime
├── index.html
├── kontakt.html
└── ...
```
