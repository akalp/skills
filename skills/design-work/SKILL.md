---
name: design-work
description: "Write an implementation-ready design document in the project repository for tier L work (architecture, security, data integrity or other high-risk changes), get it reviewed with review-design, and record human approval before issues are created. Use before implementing high-risk or architecture-level changes."
---
# Design work

## Use when

The work is tier L. Do not write a design for tier S or M; a plan in the issue is enough there.

## Inputs

- Goal, constraints and acceptance criteria from the human or issue.
- Relevant source code and tests.
- Private settings if available: `approval.human_required`.

## Procedure

1. **Understand.** Read the relevant code and any earlier decisions. Separate facts you verified from assumptions.
2. **Challenge the problem.** Is it real and correctly scoped? Could existing code be reused? What are the simpler alternatives?
3. **Write** `docs/designs/<slug>.md` in the project repository with: problem and evidence, goals and non-goals, alternatives considered, design (interfaces, data, components), security and failure handling, migration and rollback, test strategy, issue breakdown, open questions. Put a single line `Status: Draft` under the title. Commit it.
4. **Review.** Have the coordinator start one independent `review-design` session on the pinned design commit. Address blocking findings with changes or evidence-based rebuttals; obtain a targeted follow-up only for substantive revisions or unresolved findings. A second independent opinion needs a documented exceptional-risk reason, not tier L alone.
5. **Approval.** Ask the human for an explicit decision. Record where it happened (PR review, issue comment). Change the status line to `Status: Approved (<link to the decision>)` and commit. Only a human can approve; never write the approval yourself.
6. **Create issues** from the breakdown, each linking the design as `<commit>:docs/designs/<slug>.md` using the approval commit.

## Output

Design path and approved commit, review summary, approval link, created issues.
