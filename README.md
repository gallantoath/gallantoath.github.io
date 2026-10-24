# gallantoath.com

The Gallant Oath studio website: plain static HTML and one CSS file, served by GitHub Pages at
**https://www.gallantoath.com** (www is canonical; GitHub redirects the bare `gallantoath.com` to it).

No build step, no frameworks, no cookies, analytics, trackers or third-party fonts/scripts. The privacy
policy says so, so keep it that way (or update the policy first).

## Structure

| Path | What |
|---|---|
| `index.html` | Landing page: studio, Lumber Man, contact, policy links |
| `privacy/index.html` | Privacy policy for Lumber Man and this site (served at `/privacy/`) |
| `delete-account/index.html` | Google Play's account/data deletion page (`/delete-account/`) |
| `404.html` | Not-found page (GitHub Pages serves it automatically) |
| `app-ads.txt` | AdMob authorised seller line, must stay at the site root |
| `CNAME` | Custom domain for GitHub Pages: `www.gallantoath.com` |
| `style.css`, `favicon.svg` | The one stylesheet and the icon |
| `.nojekyll` | Tells Pages to serve files as-is |

All links are root-relative (`/style.css`), so the 404 page works at any URL. To preview locally:

```bash
python -m http.server 8000
```

then open http://localhost:8000.

## Before publishing a change to the policy pages

- Search for owner TODOs: `grep -rn "TODO" --include=*.html .` must find nothing that's visible on a page.
- Update "Last updated" on `/privacy/` whenever the policy changes.
- The game repo's `PRIVACY_EU.md` is the source of truth for what the game collects. Change the policy when it
  changes (new service, analytics event or stored field).
- If Cloudflare's proxy (orange cloud) is ever turned on, Cloudflare starts seeing site traffic: add that to the
  "This website" section first.

## Publishing (GitHub Pages)

1. Push this repo to the Gallant Oath GitHub organization (branch `main`).
2. Repo → Settings → Pages: Source "Deploy from a branch", branch `main`, folder `/ (root)`.
   Custom domain: `www.gallantoath.com` (matches `CNAME`).
3. Organization → Settings → Pages → Add a domain: `gallantoath.com`. GitHub shows a TXT record; add it
   (below), then Verify. Verifying the apex also covers `www`. Keep the TXT record forever.
4. Once the DNS check passes and the certificate is issued (can take up to 24 h), tick **Enforce HTTPS**.

## DNS (Cloudflare, all "DNS only" / grey cloud)

Replace `<org>` with the GitHub organization name. Checked against GitHub's Pages docs on 2026-10-24.

| Type | Name | Content |
|---|---|---|
| CNAME | `www` | `<org>.github.io` (no repo name) |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |
| TXT | `_github-pages-challenge-<org>` | the value GitHub shows in org Settings → Pages |

Don't add wildcard (`*`) records, and don't touch the MX/TXT records Cloudflare Email Routing created for
support@gallantoath.com.

## Checks once live

```bash
curl -sI https://www.gallantoath.com/privacy/
```

```bash
curl -sI https://www.gallantoath.com/delete-account/
```

```bash
curl -s https://www.gallantoath.com/app-ads.txt
```

```bash
curl -sI https://gallantoath.com/
```

The first two should be `200`, the third should print the AdMob line, and the last should be a `301` to
`https://www.gallantoath.com/`. In AdMob → Apps → View all apps → app-ads.txt, confirm the line matches and the
status turns to verified (AdMob crawls the developer website listed on the Play Store page).
