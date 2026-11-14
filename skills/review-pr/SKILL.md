---
name: review-pr
description: "Independently review a GitHub PR at an exact commit in read-only mode: acceptance criteria, regressions, security, tests and quality, plus design drift when the PR implements an approved design. Use when asked to review a PR you did not write."
---
# Review a PR

You are not the author. You work in a separate session with read-only permissions enforced by the runtime, not just by instruction. If the runtime cannot enforce read-only, continue only when your brief says the owner approved the instruction-only fallback (`policies/process.md`, Reviewer isolation); otherwise report that and stop. Do not write to the repository; your only GitHub write is the review comment on the PR, and you also send the findings to the owner.

## Use when

Asked to review or re-review a PR, or to check a tier L PR against its design.

## Inputs

- PR URL, linked issue and acceptance criteria, design link and pinned commit for tier L.
- Private settings if available: `review.blocking_severity` (default critical and high block).

## Procedure

1. **Pin the revision.** Read the PR's current head SHA from GitHub and review exactly that. If the head moves during review, start over.
2. **Acceptance criteria.** Check each criterion is met. Note missing requirements and unrequested changes.
3. **Correctness and risk.** Look at edge cases, error paths, concurrency, data integrity, authorization, performance, compatibility and test coverage. Every finding needs a file and line and concrete evidence. Mark speculation as speculation, never as a blocker.
4. **Tests.** State which checks you saw pass on this SHA (CI or your own read-only run) and which you could not verify.
5. **Design drift (tier L only).** Read the approved design with `git show <commit>:<path>`. Compare interfaces, data changes, migrations, error handling, security and non-goals with the code. Classify each difference as approved, harmless (with reason) or significant. Significant drift blocks until the design is updated and approved again.
6. **Re-review.** On a new head SHA, check every new commit and every earlier finding. Widen the review if contracts, security or scope changed.

## Output

Verdict `approved`, `changes_requested` or `blocked`; repository, PR, head SHA; your session and model family; implementer family if known; checks verified; findings as severity, location, evidence, suggested fix; drift result for tier L. Your report is evidence, not a GitHub approval.
