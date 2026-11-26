# DsxID landing page

Static product page for **https://dsxid.org/**, built with the same visual
language as the desktop app: light desktop surfaces, Segoe UI, thin borders,
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

## Download files

The header and hero **Download** buttons, plus the docs download button, open
the latest GitHub Release:

```
https://github.com/damiansyky/dsxid/releases/latest
```

The final download card offers two options:

- **Download for Windows** — the direct latest release asset,
  `https://github.com/damiansyky/dsxid/releases/latest/download/DsxID.exe`,
- **Linux packages** — the latest release page, where the Linux `.tar.gz`
  artifacts are attached.

`landing/download/DsxID.exe` remains a tracked copy of the Windows build and
`scripts/build.ps1` still refreshes it, but the page no longer links to it. Keep
it only if you want a direct `https://dsxid.org/download/DsxID.exe` fallback.

For the Windows button to work, upload the new `DsxID.exe` to the latest release.
The header button always lands on the release page, so it works for both
platforms even before the direct asset is replaced.

Linux packages are built by `.github/workflows/build-linux.yml` and contain the
binary, `.desktop` entry, icon, README, and `install.sh`. Attach the generated
`DsxID-linux-amd64-webkit40-0.1.0.tar.gz` and
`DsxID-linux-amd64-webkit41-0.1.0.tar.gz` to the same release.

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
- `download/DsxID.exe` — local fallback copy of the Windows build; the landing
  buttons themselves point to GitHub Releases.
- `sdk/dsxid.js` — hosted browser SDK,
- `sdk/dsxid.css` — optional hosted SDK button styles,
- `sdk/README.md` — SDK hosting instructions.

The page respects `prefers-reduced-motion` and is responsive down to phone
widths. Desktop builds are available for Windows and Linux; the JavaScript SDK is
platform-independent. Linux closing behaviour and packaging are documented in
the root [README](../README.md).
