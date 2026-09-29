# Tasks

## 1. Guide

- [ ] 1.1 Write `AGENTS.md` with the entry contract, the ingestion procedure (extract, sanity-check, fact-check, write, commit) and the provenance line rule; verify every `entry-ingestion` requirement maps to a step in it
- [ ] 1.2 Add `CLAUDE.md` containing `@AGENTS.md` and a README pointer to `AGENTS.md`; verify `git diff --stat` shows only those three files plus the change artifacts
- [ ] 1.3 Commit locally without pushing; verify `git status` shows the branch ahead of origin
- [ ] 1.4 After review of test 2: move the contract to format 2 (marker, Human and Robot sections, signatures, closing rule, no migration of old entries) in `AGENTS.md`, the `entry-format` delta, README, proposal and design, and restructure the test entry to match; verify the entry has both sections, both signatures, the marker and the rule

## 2. Cold test

- [ ] 2.1 Set `remote.origin.pushurl` to a fresh local bare repository in the scratchpad; verify `git remote -v` shows the local path for push
- [ ] 2.2 Run a fresh subagent with the raw Sienna message verbatim and no other context; verify against the pass criteria in design.md and record the outcome
- [ ] 2.3 If the test fails, revise `AGENTS.md`, reset the test commit, and re-run 2.2 until it passes; record each revision's reason in the commit message. Run 1 (2026-09-29): agent wrote and committed first, asked about seat position after; also wrote "standard ottomans" where its source said "available with AWD". Revised: ask before writing, prefer primary sources, claim only what the source says
- [ ] 2.4 Unset the push guard; verify GitHub `main` SHA still equals the last pushed SHA, then push

## 3. Wrap-up

- [ ] 3.1 Verify the test entry appears on https://minuti.ae after the deploy completes
