# GitHub Actions: `ubuntu-latest` does not ship pandoc

A step that calls `pandoc` on the `ubuntu-latest` runner (the 24.04 image as of September 2026) fails with `pandoc: command not found`. It is not in the image's preinstalled tool list any more. Install it as its own step first:

```yaml
- run: sudo apt-get install -y -qq pandoc
```

Verified 2026-09-28.
