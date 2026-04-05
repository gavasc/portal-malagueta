# Portal Malagueta — Project Notes

## Stack
- **Framework:** Hugo (theme: `malagueta-theme`)
- **Deployment:** GitHub Pages via GitHub Actions
- **CMS:** Sveltia CMS at `/admin`
- **Domain:** portalmalagueta.com.br (purchased, DNS not yet configured)
- **Repo:** github.com/gavasc/portal-malagueta

## Content structure
- `content/posts/*.md` — blog posts
- `content/sobre.md` — about page (single file)
- `static/images/uploads/` — CMS media uploads

## Post front matter fields
title, subtitle, date, draft, author, authorBio, authorAvatar, cover, categories, tags, summary, body

---

## CMS setup (Sveltia CMS)

- Admin panel will be at: `https://portalmalagueta.com.br/admin`
- CMS files: `static/admin/index.html` and `static/admin/config.yml`
- Every post published via the admin panel triggers a GitHub Actions build and redeploys the site automatically
- Media uploads go to `static/images/uploads/`, served from `/images/uploads/`

### Pending: OAuth proxy for CMS authentication
Sveltia CMS requires an OAuth proxy to authenticate with GitHub (GitHub Pages can't run server-side code).

Steps:
1. Create a **GitHub OAuth App** (github.com → Settings → Developer Settings → OAuth Apps → New OAuth App)
   - Homepage URL: `https://portalmalagueta.com.br`
   - Authorization callback URL: `https://<your-worker>.workers.dev/callback`
2. Deploy the **Sveltia CMS Auth** Cloudflare Worker: github.com/sveltia/sveltia-cms-auth
   - Set `GITHUB_CLIENT_ID` and `GITHUB_CLIENT_SECRET` as Worker environment variables
3. Uncomment and fill in `base_url` in `static/admin/config.yml`:
   ```yaml
   base_url: https://<your-worker>.workers.dev
   ```
4. Update the GitHub OAuth App callback URL with the final Worker URL

---

## Deployment (GitHub Actions)

Workflow file: `.github/workflows/deploy.yml`
- Triggers on every push to `main`
- Builds Hugo with `--minify`
- Deploys to GitHub Pages using the official `actions/deploy-pages` action

### Pending: Enable GitHub Pages on the repo
1. Go to github.com/gavasc/portal-malagueta → Settings → Pages
2. Set Source to **GitHub Actions**
3. Set Custom domain to `portalmalagueta.com.br`
4. Once DNS propagates, check **Enforce HTTPS**

---

## DNS configuration (portalmalagueta.com.br)

Add these records at your domain registrar:

| Type  | Host  | Value                  |
|-------|-------|------------------------|
| A     | @     | 185.199.108.153        |
| A     | @     | 185.199.109.153        |
| A     | @     | 185.199.110.153        |
| A     | @     | 185.199.111.153        |
| CNAME | www   | gavasc.github.io       |

DNS propagation can take up to 48h but is usually faster.

### Pending: Add DNS records
- [ ] Add the 4 A records above
- [ ] Add the CNAME for www
- [ ] After propagation, enable Enforce HTTPS in GitHub Pages settings

---

## Checklist — remaining manual steps

- [ ] Push current code to `main` (triggers first deploy)
- [ ] Enable GitHub Pages → Source: GitHub Actions
- [ ] Add custom domain `portalmalagueta.com.br` in Pages settings
- [ ] Add DNS records at registrar
- [ ] Enable Enforce HTTPS after DNS propagates
- [ ] Create GitHub OAuth App
- [ ] Deploy Sveltia CMS Auth worker to Cloudflare
- [ ] Add `base_url` to `static/admin/config.yml`
- [ ] Update OAuth App callback URL with final worker URL
