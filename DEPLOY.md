# Deploy notes for ethanramseybooks.com

The site is static: plain HTML, CSS and a little JavaScript. There is no build step. Do not deploy, buy a domain, or publish until Ethan has approved the preview and the domain payment.

Before going live, do a project-wide find-and-replace of `{MAGNET_URL}` with the live MailerLite landing-page URL (the sign-up page for *The Four O’Clock Table*). Leave the `/download/` thank-you page as it is; that is the URL to use as the MailerLite confirmation redirect.

Kindle titles stay in KDP Select. This site must only ever link to Amazon for buying, and must not offer sample chapters or downloads of any KDP book. The only free download is the reader magnet on `/download/`.

## Option A — Cloudflare Pages (fits well if the domain is on Cloudflare Registrar)

1. Create a GitHub repository under **ethanjramsey-cyber** (for example `ethanramseybooks`) and push the contents of this folder to the default branch. Do not commit anything from outside this folder.
2. In the Cloudflare dashboard: **Workers & Pages → Create → Pages → Connect to Git**. Select the repository.
3. Build settings: leave the build command empty, and set the output directory to `/` (the repository root, if this folder *is* the repo). If the site lives in a subfolder of a larger repo, set the root / output directory to that folder. No environment variables are required.
4. Deploy the production branch. Cloudflare will give a `*.pages.dev` preview URL.
5. Once `ethanramseybooks.com` is in the same Cloudflare account, add a custom domain on the Pages project and follow the DNS instructions (usually an apex CNAME flattening / ALIAS to the Pages target, plus `www` if wanted). Enable HTTPS (automatic).
6. Optional: in **Custom domains**, add `www.ethanramseybooks.com` and redirect it to the apex, or the other way around. Keep the canonical URLs in the HTML (`https://ethanramseybooks.com/...`) consistent with the choice.

## Option B — GitHub Pages

1. Create a GitHub repository under **ethanjramsey-cyber**. To use the apex domain, a project site is enough (`https://ethanjramsey-cyber.github.io/REPO/`), but the HTML uses root-relative paths (`/css/styles.css`, `/books/`), which expect the site to be served at the domain root — not a subpath. Prefer either:
   - a repository named `ethanjramsey-cyber.github.io` (user site, served at the root), or
   - a project repo with GitHub Pages plus the custom domain, so the site is served at `https://ethanramseybooks.com/`.
2. Push this folder’s contents to the branch GitHub Pages is set to deploy (usually `main`). In **Settings → Pages**, set Source to GitHub Actions or “Deploy from a branch”, folder `/ (root)`.
3. Under **Settings → Pages → Custom domain**, enter `ethanramseybooks.com`. GitHub will ask for a DNS `A` record (or `ALIAS`/`ANAME`) and a `www` `CNAME` pointing at `ethanjramsey-cyber.github.io`. Wait for DNS and the TLS certificate.
4. Tick **Enforce HTTPS**.

Root-relative paths are intentional. Do not publish the site under a subpath such as `https://ethanjramsey-cyber.github.io/ethanramseybooks/` without changing those paths.

## After DNS is live

- Visit `/`, `/books/`, `/about/`, `/free-story/` and confirm covers, Amazon links and the sign-up button.
- Confirm `/download/` is reachable as the MailerLite redirect and that it is `noindex` (see `robots.txt` too).
- Check the site on a phone.
- Open Graph tags point at `https://ethanramseybooks.com/images/og-image.jpg`.

Nothing in these notes should be executed until Ethan approves the preview and the domain is paid for.
