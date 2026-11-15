---
name: deliver-issue
description: "Own one GitHub issue end to end in an isolated worktree: implement, test, get an independent read-only review, fix findings and hand a merge-ready PR to the coordinator. Use when assigned an issue as its owner."
---
# Deliver an issue

Your job ends when an independently reviewed PR is ready to merge, not when the PR is opened. The coordinator merges.

## Use when

You have been assigned an issue as its owner, or you are working on one issue alone.

## Inputs

- Assignment: issue, tier, base commit, worktree, scope, acceptance criteria, design link (tier L), reviewer constraints.
- Private settings if available: `review.fix_cycle_limit` (default 3), `review.blocking_severity`, `review.different_family`.
- Project instructions in the repository (AGENTS.md, CLAUDE.md, contributing docs).

## Procedure

1. **Confirm the assignment.** Read what `memory.start_read` names for this repository. Check you are the only writer in this worktree, the base is current and dependencies are merged. For tier M, write a short plan as an issue comment before coding. For tier L, read the approved design at its pinned commit.
2. **Implement** the agreed scope with the smallest robust change. Do not drop requirements to make it smaller, and do not add unrequested changes.
3. **Verify** using the project's own commands (find them in package config or docs): format, lint, type check, tests. Add tests for new behavior. Report only commands you actually ran and their real results.
4. **Open the PR** with: issue link, design link if any, acceptance checklist, commands run and results, risks, rollback notes, head SHA.
5. **Get review without racing quota.** **Do not start a reviewer yourself.** When the PR is ready (last push done, draft cleared), post the PR number and head SHA and ask the coordinator for a review; the coordinator chooses the reviewer model, pool and slot and starts it with `review-pr`, so the verdict reaches the coordinator. The reviewer must use a different model family when `review.different_family` requires it. If capacity is not granted, leave the PR queued for review. Fix the findings the coordinator relays and ask for a re-review of the new head. Keep responsibility for fixing findings.
6. **Fix and re-review.** Fix verified blocking findings, re-run tests, request re-review of the changed code. After `review.fix_cycle_limit` cycles on the same root cause, stop and escalate.
7. **Hand off** to the coordinator.

If interrupted (quota, crash, harness switch), write a handoff in private memory, or as a PR comment if private memory is unavailable: branch, worktree, remote and local head SHA, unpushed or polluted state, what is done with test results, exact next step, open findings, and the few files the next owner must read, so nobody re-scans the codebase.

## Output

Issue and PR links, head SHA, summary, tests run with results, reviewer session and model family, review verdict for that SHA, unresolved findings by severity, drift result for tier L, risks, suggested next action. Propose any durable lesson separately; do not edit shared memory or policy.
