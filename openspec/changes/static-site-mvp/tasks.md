# Tasks

## 1. Repository

- [x] 1.1 `git init` in `~/src/Minutiae`, create `TFenby/minutiae` on GitHub with `gh repo create`, push `main`; verify `gh repo view TFenby/minutiae` succeeds
- [x] 1.2 Add `entries/` with two seed entries following `entry-format` (seeded from verified papercuts facts: SQLite double quotes, Navidrome password API; the Sienna and signal-cli entries wait on the owner's verified facts); verify filenames match `YYYY-MM-DD-slug.md` and each first line is a heading
- [x] 1.3 Add `README.md` stating the entry contract and the three write paths (git, `gh api`, REST with a fine-grained PAT); verify the `gh api` example in it creates a file when run against a scratch branch

## 2. Build and deploy

- [x] 2.1 Add `.github/workflows/pages.yml`: checkout, `pandoc entries/*.md --standalone --metadata title=minuti.ae -o public/index.html`, `upload-pages-artifact`, `deploy-pages`, with a `ponytail:` comment noting the single-page ceiling; verify `pandoc` runs locally or in the first Actions log without an install step, else add the apt-get line
- [x] 2.2 Enable GitHub Pages with source "GitHub Actions" (done via `gh api -X POST .../pages -f build_type=workflow`); verify the workflow run is green and the `*.github.io/minutiae` URL serves the page
- [x] 2.3 Verify the served page begins with `<!DOCTYPE html>`, has a `<title>`, matching `<body>`/`</body>`, both seed entries in date order, and a rendered fenced code block (`curl -s <url> | grep` for each)

## 3. Domain

- [x] 3.1 (no `CNAME` file: Pages ignores it for Actions-built sites, so the domain was set with `gh api -X PUT .../pages -f cname=minuti.ae` and `https_enforced=true`) add a CNAME record at Cloudflare for the apex pointing to `tfenby.github.io` (DNS-only until the certificate issues), set the custom domain in Pages settings; verify `curl -sI https://minuti.ae/` returns 200 with a valid certificate
- [ ] 3.2 Re-enable the Cloudflare proxy and verify `https://minuti.ae/` still returns the page

## 4. Integration check

- [x] 4.1 From a fresh session, add a third entry using only `gh api -X PUT repos/TFenby/minutiae/contents/entries/<file>`; verify it appears at `https://minuti.ae/` after the deploy completes with no other action taken
