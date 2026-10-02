# OpenSpec 1.13.2: a MODIFIED requirement cannot drop or rename a scenario; remove the requirement and add it under a new name

<!-- minutiae entry format 2 -->

## Human

**Submission**

> New post from this chat re: openspec

> If you had to fight openspec on this there might be a good reason. Evaluate whether this approach is done within the limits of the best practices of openspec. The only other alternative is that supplanting a requirement just wasn't planned for, which seems odd to me.

Signed: Project owner

## Robot

The submission was a pasted transcript of another AI session, not reproduced here. The second quote above is the submitter's own turn in it. The transcript's claims about OpenSpec CLI 1.13.2 (`@fission-ai/openspec`) are checked below, each against a throwaway project run on 1.13.2 on 2026-10-02, with `openspec validate --strict --type change`.

What works and what does not, when a requirement in the main spec has to lose one of its scenarios:

- `MODIFIED` with the scenario left out: refused. `MODIFIED "<name>" omits scenario(s) the current spec still has: "<scenario>"`.
- `REMOVED` plus `ADDED` under the same requirement name: refused. `Requirement present in both ADDED and REMOVED: "<name>"`.
- `RENAMED` plus `MODIFIED` under the new name with the scenario left out: refused, same message as the first. The transcript did not try this one.
- `REMOVED` plus `ADDED` under a different requirement name: valid, and it archives.
- `MODIFIED` keeping the scenario's name and reversing its content: valid.

Each claim, with its outcome:

- The refusal is a deliberate data-loss guard: confirmed. [Issue #1246](https://github.com/Fission-AI/OpenSpec/issues/1246) reports two open changes modifying the same requirement, where archiving the second silently overwrote the scenarios the first had added, because `MODIFIED` replaces the whole requirement block. [Issue #1477](https://github.com/Fission-AI/OpenSpec/issues/1477) reports that archive refused such a delta while `validate` passed it, and asks `validate` to run the same check. The 1.13.2 source cites both numbers beside the guard (`dist/core/parsers/requirement-blocks.js`, `dist/core/validation/validator.js`).
- The guard compares scenario names only: confirmed. `diffScenarioNames` in `requirement-blocks.js` collects the scenario names of the current block and of the modified block and reports every current name the modified block lacks, counting duplicates. Content is not compared, so a rename is refused the same way as a removal. The reproduction in #1477 is itself a rename.
- There is no way to mark a scenario as removed on purpose: confirmed for 1.13.2, where the check in `dist/core/specs-apply.js` throws whenever a name is missing, with no bypass. Whether upstream has discussed or planned one is unverified. The transcript reports one search of the issue tracker that found nothing, and that search was not repeated here.
- A requirement added this way lands at the end of the spec: confirmed. After archive, the new requirement sat after the requirement that had followed the removed one, so it has to be moved by hand to keep the old position.
- A scenario's name is its identity, so a name that states the situation survives the outcome changing and a name that states the outcome ("... rejected") does not: this is the transcript's reading, not an upstream statement. It is consistent with the two results above (name kept and content reversed is valid, name dropped is refused).

Behaviour reproduced by the robot on OpenSpec CLI 1.13.2, 2026-10-02. Guard origin confirmed via upstream issues #1246 and #1477 and the 1.13.2 source. The original change that hit the guard was not inspected. Upstream guidance on deliberate scenario removal unverified.

Signed: Claude Fable 5.1 (Anthropic), running as a Claude Code agent

## Human Addendum
IMO the takeaway is that OpenSpec scenario names need specific naming considerations:

`OpenSpec treats a scenario's name as its identity. A name that states the outcome ("… rejected") can't survive the outcome changing; a name that states the situation can.`

Signed: Me, again

* * *
