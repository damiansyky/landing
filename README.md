# DsxID landing page

Static product page for **https://dsxid.org/**, built with the same visual
language as the desktop app: light Windows surfaces, Segoe UI, thin borders,
blue accent, and compact cards. No build step, fonts, CDNs, or external assets.

The site is deployed from this folder by GitHub Actions. Only `landing/` is
published, so the Go source and desktop build are not exposed on the web.

## Preview

Serve the landing folder itself so the root-relative paths and download link
resolve exactly like production:

```powershell
npx.cmd --yes serve landing
```

Open **http://localhost:3000/**.

## GitHub Pages deployment

The workflow is [`.github/workflows/pages.yml`](../.github/workflows/pages.yml).
It runs on every push to `main`, uploads `landing/` as the Pages artifact, and
deploys it to the custom domain.

One-time setup:

1. Push this repository to GitHub with `main` as the default branch.
2. Open **Settings → Pages → Build and deployment** and set **Source** to
   **GitHub Actions**.
3. Set **Custom domain** to `dsxid.org`. The `CNAME` file in this folder keeps
   the same value in the artifact.
4. At the domain registrar, point the apex domain at GitHub Pages:

   ```
   A     @    185.199.108.153
   A     @    185.199.109.153
   A     @    185.199.110.153
   A     @    185.199.111.153

   AAAA  @    2606:50c0:8000::153
   AAAA  @    2606:50c0:8001::153
   AAAA  @    2606:50c0:8002::153
   AAAA  @    2606:50c0:8003::153

   CNAME www  <your-github-user>.github.io.
   ```

5. Wait for the DNS check, then enable **Enforce HTTPS**.

The first deploy creates the site. Later pushes to `main` redeploy it. If the
default branch is not `main`, update the branch name in the workflow.

## Download file

The three download buttons use the relative link `download/DsxID.exe`, so they
work on `https://dsxid.org/` and in local preview:

```html
<a href="download/DsxID.exe" download>Download DsxID</a>
```

`landing/download/DsxID.exe` is a tracked copy of the portable Windows build.
`scripts/build.ps1` refreshes it after every build. GitHub Pages allows files up
to 100 MB and sites up to 1 GB, so the 14 MB executable fits comfortably. For
high-traffic releases, attach the same file to a GitHub Release and point these
buttons at `https://github.com/<owner>/<repo>/releases/latest/download/DsxID.exe`
instead.

## Hosted SDK

`https://dsxid.org/sdk/dsxid.js` and `/sdk/dsxid.css` are served from this folder.
After changing the SDK, rebuild it; `web-sdk/build.ps1` copies the result into
`landing/sdk/` automatically:

```powershell
powershell -ExecutionPolicy Bypass -File web-sdk/build.ps1
```

The developer section computes the SDK URL from the current page address and
offers a copy button.

## Files

- `index.html` — landing page content and inline SVG icons,
- `docs.html` — SDK integration documentation,
- `404.html` — GitHub Pages not-found page,
- `styles.css` — layout and app-style design tokens,
- `app.js` — mobile navigation, SDK URL copy, scroll reveal,
- `CNAME` — `dsxid.org`,
- `.nojekyll` — serve files as-is if branch deployment is used,
- `robots.txt`, `sitemap.xml` — search engine metadata,
- `assets/app-home.png` — app overview screenshot,
- `assets/app-profile.png` — e-shop profile screenshot,
- `assets/logo.png` — real DsxID logo, page mark and favicon,
- `assets/og.png` — 1200×630 social preview card,
- `download/DsxID.exe` — published Windows build,
- `sdk/dsxid.js` — hosted browser SDK,
- `sdk/dsxid.css` — optional hosted SDK button styles,
- `sdk/README.md` — SDK hosting instructions.

The page respects `prefers-reduced-motion` and is responsive down to phone
widths. The desktop build is currently Windows; the JavaScript SDK is
platform-independent.
