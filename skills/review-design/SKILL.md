---
name: review-design
description: "Independently review a tier L design document at a pinned commit in one fresh, read-only session, with focused follow-up only when findings or revisions require it. Use when asked to review a design doc you did not write, or to follow up on one."
---
# Review a design

You are an independent reviewer, not the author. Start in a fresh session with only the design at its pinned commit, the relevant source code, and acceptance criteria, not the author's conversation or preferred verdict. Use read-only permissions enforced by the runtime; if isolation is unavailable, report it and stop.

## Use when

A tier L design needs review before human approval, or the coordinator requests a focused follow-up or a second opinion under `review.design_second_opinion`.

## Inputs

- Design repository, path, exact commit, problem statement and acceptance criteria.
- Private settings if available, otherwise the defaults from the akalp/skills README:
  - `review.blocking_severity` (default: design blocker and major findings block)
  - `review.fix_cycle_limit` (default: 3)
  - `review.design_second_opinion` (default: irreversible change without a tested rollback, or architecture disagreement unresolved after follow-up)
- For follow-up: earlier findings and the revised design commit. For a second opinion: the trigger the coordinator recorded.

## Default: one independent review

Cover all of these dimensions **in the same session**; they are review criteria, not separately billable phases:

1. **Problem and scope.** Is the problem evidenced and worth solving? Are goals/non-goals consistent with the issue?
2. **Comprehension.** Can a new implementer understand the design and acceptance criteria without the author's conversation?
3. **Architecture and alternatives.** Is there a simpler approach or reusable existing component? Verify interfaces, dependencies, compatibility and complexity against actual code.
4. **Failure and security.** Evaluate correctness, data integrity, authorization, concurrency, performance, migration/rollback, observability and recovery where relevant.
5. **Implementation readiness.** Are decisions, tests, issue boundaries and rollback instructions sufficient? Call out unresolved questions.

Report concrete findings with severity (blocker, major, minor), location, evidence and a suggested remedy. Separate unverified concerns from proven defects. A clean review is **not** human approval.

## Follow-up: only when needed

- **No blocking findings and no substantive design change:** stop after the default review. Do not launch a second session merely to complete a phase.
- **Blocking findings or material revisions:** after the author changes the design, re-read the new pinned commit and verify the prior findings plus affected areas. Prefer a targeted follow-up with the original independent reviewer when its context is still reliable. Expand to a full fresh review if the problem, architecture or trust boundary has materially changed.
- **Second opinion:** a second independent fresh-session reviewer runs only when a trigger from `review.design_second_opinion` applies and the coordinator has recorded which one. Tier L alone is not a trigger. Prefer a different model family; do not lower the required capability to save quota.
- Limit repeated review cycles according to `review.fix_cycle_limit`; on persistent findings, escalate rather than endlessly repeating reviews.

## Output

For each actual review or follow-up: design commit, review type (`design`, `design-follow-up`, `design-second-opinion`), reviewer session and model family, verdict, findings with severity/location/evidence/fix, unresolved questions, and the reason for any extra review. No empty placeholder phase reports.
