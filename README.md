# docs.apitcg.com

Public documentation for **API TCG**, rendered with [Scalar](https://scalar.com).

It is a **static site** — no build step. The whole thing is:

| File              | Purpose                                                      |
| ----------------- | ------------------------------------------------------------ |
| `index.html`      | Loads the Scalar API Reference from the CDN (EN/ES sources). |
| `openapi.json`    | OpenAPI spec in **English** (default document).              |
| `openapi.es.json` | OpenAPI spec in **Spanish**.                                 |
| `logo.svg`        | API TCG logo (next to the title and in the footer).          |
| `intro.png`       | Banner shown in the introduction.                            |
| `favicon.ico`     | Browser tab icon.                                            |
| `.nojekyll`, `CNAME` | Only needed if hosted on GitHub Pages (unused on Vercel). |

## Editing the docs

Edit **`openapi.json`** (English) and **`openapi.es.json`** (Spanish) — keep
both in sync. Everything the site shows — endpoints, parameters, examples —
comes from those files. No Markdown, no config.

## Preview locally

Because `index.html` fetches the specs over HTTP, open it through a static
server (not `file://`):

```bash
npx serve .
# then open the printed URL, e.g. http://localhost:3000
```

## Deploy (Vercel)

The site is hosted on **Vercel**, connected to this GitHub repository: every
push to `main` deploys automatically. The `docs.apitcg.com` domain is
configured in the Vercel dashboard.

> ⚠️ The Vercel project was created for the old Docusaurus site. Since this is
> now a plain static site with **no build step**, make sure the project uses:
> **Settings → Build and Deployment → Framework Preset = "Other"**, with empty
> Build Command and Output Directory. Otherwise the deploy fails looking for
> the removed `package.json`.

> Migrated from Docusaurus. The old Docusaurus source lives in the git history
> if you ever need it (`git log`).
