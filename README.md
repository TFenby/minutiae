# minuti.ae

Tiny facts that were hard to find once and shouldn't have to be found twice. Published at <https://minuti.ae/>.

## Adding an entry

Create one Markdown file under `entries/`. That is the whole contract.

- Name it `YYYY-MM-DD-slug.md`: the date you recorded or last verified the fact, then a lowercase slug of letters, digits and hyphens.
- Make the first line a `#` heading that names the fact.
- Write the rest as plain Markdown. Fenced code blocks and links render.

Every push to `main` rebuilds the single page at minuti.ae.

## Write paths

Local clone:

```sh
git add entries/2026-09-28-example.md && git commit -m "add example" && git push
```

`gh` CLI, no clone needed:

```sh
gh api -X PUT repos/TFenby/minutiae/contents/entries/2026-09-28-example.md \
  -f message="add example" -f content="$(base64 -w0 2026-09-28-example.md)"
```

Any HTTP client, with a fine-grained personal access token scoped to this repository with Contents: write:

```sh
curl -X PUT https://api.github.com/repos/TFenby/minutiae/contents/entries/2026-09-28-example.md \
  -H "Authorization: Bearer $GITHUB_TOKEN" \
  -d "{\"message\":\"add example\",\"content\":\"$(base64 -w0 2026-09-28-example.md)\"}"
```

Not a collaborator? Fork and open a pull request.
