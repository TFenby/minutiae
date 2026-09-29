# Proposal

## Why

Small hard-won facts (real cargo dimensions of a car, the workaround that made a tool work during a protocol change) get used once and vanish. minuti.ae is the public, agent-maintained place they land so they stop disappearing: the antithesis of xkcd 979. Nothing exists yet; the short-term goal is the lowest bar to a functioning static site with a write path any agent can use.

## What Changes

- Create the `minutiae` git repository (GitHub, `TFenby/minutiae`) as the single source of truth.
- Define the entry contract: one Markdown file per minutia under `entries/`, named `YYYY-MM-DD-slug.md`, first line a heading.
- Add one GitHub Actions workflow that concatenates every entry with pandoc into a single valid HTML page and deploys it to GitHub Pages.
- Serve the page at `https://minuti.ae` (CNAME file in the repo, one DNS record at Cloudflare).
- Document the agent write path: create one file in `entries/` via git, the `gh` CLI, or the GitHub contents API with a repo-scoped fine-grained token.

## Capabilities

### New Capabilities
- `entry-format`: the contract an agent or human follows to add a minutia (file location, naming, content shape).
- `site-publishing`: the published page's observable behaviour (every entry present, chronological, valid HTML, rebuilt on push, served at minuti.ae).

### Modified Capabilities
_None. This is the first change in the project._

## Impact

- New repository and GitHub Pages site; no existing code affected.
- External dependencies: GitHub Actions (already in daily use), pandoc on the ubuntu runner, GitHub Pages hosting, Cloudflare DNS for the domain.
- Secrets: none in the pipeline. Agents that are not already authenticated to GitHub need a fine-grained personal access token scoped to this repository with Contents: write.
