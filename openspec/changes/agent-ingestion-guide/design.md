# Design

## Context

Repo has a README with the file contract, three entries, and no agent guidance. Claude Code loads `CLAUDE.md` automatically but not `AGENTS.md`; most other agent tooling reads `AGENTS.md`. See proposal.md for motivation.

## Goals / Non-Goals

**Goals:**
- One short procedure any agent can follow cold.
- The procedure is proven by a cold test, not by inspection.

**Non-Goals:**
- Automating submission intake (webhooks, inboxes, forms).
- Reviewing or editing existing entries.
- A style guide beyond what the procedure needs.

## Decisions

**`AGENTS.md` is the guide; `CLAUDE.md` is one import line.** Vendor-neutral file holds the content; the Claude-specific file is `@AGENTS.md` so Claude Code sessions and subagents load it without duplication. Alternative: put everything in `CLAUDE.md` (other agents would not see it).

**Human and Robot sections, format marker, no migration.** Reviewing test 2 showed the reader needs to see what the person said, untouched, apart from what the agent concluded. The Human section is verbatim quotes plus the questions that prompted follow-ups, signed; the Robot section is analysis, signed with the AI make and model. An HTML comment names the format version so future migrations can grep for it, and a `* * *` rule closes each file so the page shows boundaries. Old entries are never rewritten when the format changes; the trail of formats is part of the record. Alternative: migrate old entries on every schema change (busywork that also erases what the process looked like at the time).

**Provenance goes in a final line, not front matter.** Keeps the entry contract at "heading plus Markdown". Existing entries already end with "Verified <date>." and comply. Alternative: YAML front matter (pandoc would render it as metadata and the contract would grow a schema).

**Cold test with a push guard.** The test agent is a fresh subagent given the raw message verbatim and nothing else, working in the real checkout so it sees `CLAUDE.md`. Before the test, `remote.origin.pushurl` is pointed at a local bare repository in the session scratchpad so a `git push` succeeds without reaching GitHub; it is unset after. Pass criteria: entry file under `entries/` with correct name and heading, submitted numbers preserved, the uncertain seat claim checked against a source and stated correctly, provenance line present, one commit naming the file, nothing private, nothing invented, and GitHub's `main` unchanged. Alternative: a worktree (would hide `CLAUDE.md` import behaviour differences and still allow a branch push).

**Push only after the test passes.** The guide and the test entry reach GitHub together, after the test.

## Risks / Trade-offs

- [Subagent does not load `CLAUDE.md`] → the test shows it immediately; fallback is passing the file path in the prompt, which would be recorded as a limitation, not hidden.
- [Agent uses the README's `gh api` path during the test and writes to GitHub directly] → the guide says a checkout uses git; after the test, compare GitHub's `main` SHA with the last pushed SHA to detect a leak.
- [Fact-check finds nothing] → the spec allows "unverified"; the test still passes if the claim is marked, not asserted.

## Open Questions

- Whether submissions should ever be batched into one entry. Not needed for the test message.
