# Design

## Context

Empty directory, no repository yet. Domain `minuti.ae` is owned and its DNS lives at Cloudflare. GitHub Actions is already the CI used across the owner's projects; `gh` is authenticated locally as TFenby. See proposal.md for motivation. Ponytail ultra applies: fewest files, no dependencies beyond what the runner ships.

## Goals / Non-Goals

**Goals:**
- A writer of any kind (local Claude session, remote agent, cron script, human) publishes by creating one file.
- Zero secrets in the build and deploy path.
- One page, valid HTML, searchable with find-in-page.

**Non-Goals:**
- Per-entry pages, tags, search, RSS, theming. Earned later if ever.
- A write API or front door. GitHub's own auth and PR model is the front door.
- Front matter or any schema beyond filename and first-line heading.

## Decisions

**File per entry, Markdown.** Chosen over a single hand-appended HTML file. A create-only write never conflicts and needs no prior read; Markdown is the most durable plain-text format and is what agents produce natively. Alternatives: one `index.html` appended in place (read-modify-write with blob SHAs, 409s under concurrency); `.txt` in `<pre>` (loses links and code fences for no simplicity gain).

**pandoc as the whole build.** `pandoc entries/*.md --standalone --metadata title=minuti.ae -o public/index.html` concatenates all inputs and emits a complete HTML document, so no head/foot wrapper files exist. Alternatives: `cat` with wrapper files (three files, and entries would have to be HTML); a static site generator (a dependency and a config for a one-page site); Jekyll on GitHub Pages (Liquid mangles `{{ }}` inside code blocks, which minutiae are full of).

**GitHub Actions to GitHub Pages, Cloudflare for DNS only.** The workflow is checkout, pandoc, `upload-pages-artifact`, `deploy-pages`. The automatic workflow token is sufficient, so no secret is stored anywhere. Alternatives: Cloudflare Pages git build (no pandoc in its image, so wrapper files return); Actions pushing to Cloudflare Pages via wrangler (adds a Cloudflare API token to GitHub secrets for no gain).

**Write path is GitHub, never the host.** Local sessions use `gh api -X PUT repos/TFenby/minutiae/contents/entries/<file>`. Anything else uses the same REST call with a fine-grained PAT scoped to this repo, Contents: write, one token per agent so revocation is granular. Strangers fork and open a PR.

**Trust boundary is the token.** Whoever can write to the repo can put arbitrary HTML on the domain. Acceptable while every writer is owner-controlled. Upgrade path when that changes: outsiders go through PRs; no code needed.

## Risks / Trade-offs

- [pandoc missing from the runner image] → `sudo apt-get install -y pandoc` is one workflow line; check the first run's log.
- [GitHub Pages certificate issuance fails behind the Cloudflare proxy] → set the CNAME record to DNS-only during the first verification, re-enable proxy after.
- [Single page grows unwieldy] → `--toc` is one pandoc flag; splitting into per-year pages is a later change. Note the ceiling in the workflow with a `ponytail:` comment.
- [Two agents write the same filename] → the second PUT fails with 422 because the file exists; the slug is distinct by construction, and the failure is loud.
- [Heading levels collide with pandoc's title] → entries use a level-one heading; if pandoc's page title renders as another `<h1>`, add `--shift-heading-level-by=1` rather than changing the entry contract.

## Migration Plan

Greenfield. Rollback is deleting the DNS record or disabling Pages; the repo remains the record either way.

## Open Questions

- Whether the papercuts log gains a clause to promote entries here, or promotion stays manual. Does not affect this change.
