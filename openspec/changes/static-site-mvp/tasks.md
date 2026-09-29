# Tasks

## 1. Repository

- [ ] 1.1 `git init` in `~/src/Minutiae`, create `TFenby/minutiae` on GitHub with `gh repo create`, push `main`; verify `gh repo view TFenby/minutiae` succeeds
- [ ] 1.2 Add `entries/` with two seed entries (2022 Sienna cargo dimensions, signal-cli on NixOS) following `entry-format`; verify filenames match `YYYY-MM-DD-slug.md` and each first line is a heading
- [ ] 1.3 Add `README.md` stating the entry contract and the three write paths (git, `gh api`, REST with a fine-grained PAT); verify the `gh api` example in it creates a file when run against a scratch branch

## 2. Build and deploy

- [ ] 2.1 Add `.github/workflows/pages.yml`: checkout, `pandoc entries/*.md --standalone --metadata title=minuti.ae -o public/index.html`, `upload-pages-artifact`, `deploy-pages`, with a `ponytail:` comment noting the single-page ceiling; verify `pandoc` runs locally or in the first Actions log without an install step, else add the apt-get line
- [ ] 2.2 Enable GitHub Pages with source "GitHub Actions" in repo settings; verify the workflow run is green and the `*.github.io/minutiae` URL serves the page
- [ ] 2.3 Verify the served page begins with `<!DOCTYPE html>`, has a `<title>`, matching `<body>`/`</body>`, both seed entries in date order, and a rendered fenced code block (`curl -s <url> | grep` for each)

## 3. Domain

- [ ] 3.1 Add `CNAME` file containing `minuti.ae`, add a CNAME record at Cloudflare for the apex pointing to `tfenby.github.io` (DNS-only until the certificate issues), set the custom domain in Pages settings; verify `curl -sI https://minuti.ae/` returns 200 with a valid certificate
- [ ] 3.2 Re-enable the Cloudflare proxy and verify `https://minuti.ae/` still returns the page

## 4. Integration check

- [ ] 4.1 From a fresh session, add a third entry using only `gh api -X PUT repos/TFenby/minutiae/contents/entries/<file>`; verify it appears at `https://minuti.ae/` after the deploy completes with no other action taken
