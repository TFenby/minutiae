# minuti.ae: agent guide

This repository publishes tiny hard-won facts at <https://minuti.ae>: one Markdown file per fact under `entries/`, and every push to `main` rebuilds the single page. Your job here is to turn a raw message into one of those files.

## The entry contract

- Path `entries/YYYY-MM-DD-slug.md`. The date is today, the day the fact is recorded or last verified. The slug is lowercase letters, digits and hyphens, and names the subject so a searcher finds it.
- Line 1: a `#` heading naming the fact (make, model, year, the thing measured or the thing that works).
- Line 3: the marker `<!-- minutiae entry format 2 -->`. It is invisible on the page and says which format the file follows.
- `## Human`: only what people actually said, verbatim. The original submission as a blockquote under **Submission**. Each question you asked as a **Robot asked:** line. Each answer as a blockquote under **Follow-up**. No summaries, no commentary, no note about what went unanswered: if nothing more was said, nothing more appears. Ends with `Signed: <submitter>`; the project owner signs as "Project owner".
- `## Robot`: your analysis. Numbers first with their conditions and conversions, then each checked claim with its outcome and source, then the provenance line (as-of date, what was measured by whom, what was confirmed against which source, what is unverified). Ends with `Signed: <AI make and model>, running as <runtime>`.
- Last line: `* * *`, a horizontal rule that marks the file boundary on the page. Not `---`, which pandoc can read as a metadata block.
- Nothing else. No front matter. Touch no other file.

When the format changes, the marker's number changes and the guide changes. Existing entries are never rewritten to the new format; the page shows the boundary between eras.

## Ingesting a raw message

Submissions are terse, mix units, and carry hunches ("irremovable?", "I think the trim matters"). This is a conversation, not a one-shot. Do this, in order:

1. **Extract every claim.** Keep the submitter's measurements exactly as given. You may add a conversion beside a value (4 ft, 48 in) but never replace a submitted number with one from the web. The submitter's numbers are the point of the entry.
2. **Ask before you write.** A number means nothing without its conditions: where a seat was positioned, which version, which settings, what the two endpoints were. If the message leaves a condition unstated and that condition changes what the number means, ask the submitter and wait for the answer. Do the same for contradictions and for units you cannot resolve. Only if nobody can answer (you are running unattended) do you write anyway, and then the entry says the condition was not stated. Asking is not a failure; a confident entry over a silent assumption is.
3. **Sanity-check.** Units consistent? Nested measurements ordered sensibly, so a longer path carries a larger number? Never silently fix a contradiction; ask, or record it as a conflict.
4. **Fact-check the checkable parts.** Every hunch or question mark gets checked. Prefer primary sources: the manufacturer's site or owner's manual, official documentation, the project's own changelog. A reseller's blog or a forum thread is a last resort, and the entry says so. Claim only what the source actually says, and give every number that is not the submitter's a source. State the outcome for each claim: confirmed, corrected, or unverified. Do not check the submitter's own measurements against the web; a differing published spec may be noted as a separate sourced fact.
5. **Write the entry.** Heading, marker, the Human section with the words exactly as said, the Robot section with numbers first, checked facts, provenance and your signature, then the rule. Keep the Robot section short. Include nothing that is not from the message, the submitter's answers, or a source you name. Exclude names, addresses, plates, account identifiers, and anything the submitter marked private.
6. **Commit.** One entry per commit, containing only that file: `git add entries/<file> && git commit -m "add <file>"`, then `git push`. Without a checkout, use the `gh api` write path in the README.

If the message cannot become an entry (nothing checkable, private, or contradictory beyond repair), say so and stop rather than writing a bad one.

## Shape of a finished entry

```markdown
# <Subject>: <what was measured or what works>

<!-- minutiae entry format 2 -->

## Human

**Submission**

> <the message, verbatim>

**Robot asked:** <the question, in one line>

**Follow-up**

> <the answer, verbatim>

Signed: Project owner

## Robot

<the submitted numbers or steps, with their conditions and conversions beside them>

<each checked claim, with its outcome and source>

Measured by the submitter, <date>. <Claim> confirmed via <source>. <Claim> unverified.

Signed: <AI make and model>, running as <runtime>

* * *
```
