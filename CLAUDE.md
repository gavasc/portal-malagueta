# Portal Malagueta — Project Notes

## Stack
- **Framework:** Hugo (theme: `malagueta-theme`)
- **Deployment:** GitHub Pages via GitHub Actions
- **CMS:** Sveltia CMS at `/admin`
- **Domain:** portalmalagueta.com.br (purchased, DNS added, TLS certificate being provisioned)
- **Repo:** github.com/gavasc/portal-malagueta

## Content structure
- `content/posts/*.md` — blog posts
- `content/sobre.md` — about page (single file)
- `static/images/uploads/` — CMS media uploads

## Post front matter fields
title, subtitle, date, draft, author, authorBio, authorAvatar, cover, categories, tags, summary, body

---

## CMS setup (Sveltia CMS)

- Admin panel: `https://portalmalagueta.com.br/admin`
- CMS files: `static/admin/index.html` and `static/admin/config.yml`
- Every post published via the admin panel triggers a GitHub Actions build and redeploys the site automatically
- Media uploads go to `static/images/uploads/`, served from `/images/uploads/`

### OAuth proxy (Sveltia CMS Auth)
- **Worker URL:** `https://sveltia-cms-auth.gavasc.workers.dev`
- **Worker variables set:** `GITHUB_CLIENT_ID` (text) and `GITHUB_CLIENT_SECRET` (secret)
- `base_url` is configured in `static/admin/config.yml`
- GitHub OAuth App created with callback URL pointing to the Worker

---

## Deployment (GitHub Actions)

Workflow file: `.github/workflows/deploy.yml`
- Triggers on every push to `main`
- Builds Hugo with `--minify`
- Deploys to GitHub Pages using the official `actions/deploy-pages` action

---

## DNS configuration (portalmalagueta.com.br)

Records added at registrar:

| Type  | Host  | Value                  |
|-------|-------|------------------------|
| A     | @     | 185.199.108.153        |
| A     | @     | 185.199.109.153        |
| A     | @     | 185.199.110.153        |
| A     | @     | 185.199.111.153        |
| CNAME | www   | gavasc.github.io       |

---

## Checklist

- [x] Push current code to `main` (triggers first deploy)
- [x] Enable GitHub Pages → Source: GitHub Actions
- [x] Add custom domain `portalmalagueta.com.br` in Pages settings
- [x] Add DNS records at registrar
- [ ] Enable Enforce HTTPS after TLS certificate is provisioned
- [x] Create GitHub OAuth App
- [x] Deploy Sveltia CMS Auth worker to Cloudflare
- [x] Add `base_url` to `static/admin/config.yml`
- [x] Update OAuth App callback URL with final Worker URL

---

## Open issue: second user cannot access /admin

A collaborator was added to the repo (accepted invite, status shows as Collaborator with Write access). When she tries to log in via `/admin`, she gets "you don't have access to portal-malagueta repository" from Sveltia CMS.

- OAuth App has Device Flow enabled
- Invite was accepted (confirmed via github.com/gavasc/portal-malagueta/invitations)
- Next step to investigate: check browser console/network tab when the error appears to identify where Sveltia is rejecting her
