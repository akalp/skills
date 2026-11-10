# akalp/skills

Portable procedures for coding agents working on GitHub repositories with multiple models and subscriptions. Install any skill independently with the [Skills CLI](https://skills.sh/) into Claude Code, Codex, OpenCode or another supported harness.

These are procedures, not an orchestrator. The active runtime (T3 Code, HERDR, a plain CLI) starts agents; GitHub holds live issue/PR state and enforces merge rules.

## Skills

| Skill | Role | Use when |
|---|---|---|
| `coordinate-work` | Coordinator | Planning a wave of issues, routing them to models, merging |
| `deliver-issue` | Issue owner | Taking one issue from implementation to a review-clean PR |
| `review-pr` | Reviewer | Reviewing an exact PR commit, including design drift for tier L |
| `design-work` | Designer | Writing a design doc for tier L work |
| `review-design` | Reviewer | One independent design review; follow-up or second opinion only when triggered |
| `tool-discovery` | Any | Learning a CLI or tool whose interface may have changed |

## Process tiers

Every issue gets a tier before work starts. Process cost scales with risk, not with habit.

| Tier | Typical work | Process |
|---|---|---|
| S | Clear, small, low risk | Implement, test, one independent review |
| M | Multi-file or partly ambiguous, normal risk | Short plan in the issue, review by a different model family |
| L | Architecture, security, data integrity, high failure cost | Design doc, one independent design review by default, human approval, PR review with drift check; extra review only per `review.design_second_opinion` |

A one-line change can be tier L if its risk is high.

## Private context contract

Skills work standalone with the defaults below. If a private operating policy is available (loaded through the harness's global instructions), its values override the defaults. Skills refer to these settings by name, never by file path.

| Setting | Default | Meaning |
|---|---|---|
| `process.tiers` | The table above | Tier definitions and required steps |
| `review.fix_cycle_limit` | 3 | Fix/re-review cycles before escalating to a stronger model or a human |
| `review.blocking_severity` | PR: critical, high. Design: blocker, major | Findings that block acceptance |
| `review.different_family` | Required for M and L, preferred for S | Reviewer must use a different model family than the implementer |
| `review.design_second_opinion` | Only for an irreversible change without a tested rollback, or an architecture disagreement still unresolved after the first review and its follow-up. Tier L alone is never a reason. | When a second independent design reviewer may be started; the coordinator records which trigger applied |
| `approval.human_required` | Tier L, destructive operations, security changes, policy changes | Work that needs an explicit human decision |
| `routing.table` | None; choose the cheapest model you judge capable and say so | Tier and role to model pool mapping |
| `quota.unknown` | Treat as unavailable | What to do when a required quota window cannot be read |

If a setting is missing from the private policy, use the default and state that you did.

## Terms

- **Wave**: a batch of issues whose dependencies are satisfied and can run in parallel.
- **Pool**: the subscription or billing entitlement a model call consumes. The harness name does not identify the pool; a Codex model inside OpenCode may use the same ChatGPT entitlement as Codex CLI.
- **Fresh context**: a new agent session that receives only the artifact under review and its sources, not the author's conversation.
- **Design drift**: a difference between the merged implementation and the approved design.

## Install

```sh
pnpx skills add akalp/skills --list
pnpx skills add akalp/skills -g -a claude-code -a codex -a opencode --skill '*'
pnpx skills ls -g
pnpx skills update -g
```

Before publishing, install from a local checkout with `pnpx skills add . ...`. Use `npx` if pnpm is not available, and check `skills --help` if flags have changed. The CLI may install a managed copy, so editing this checkout does not update installed skills until you run `skills update` or reinstall.

## Limits

Skills recommend behavior; they do not enforce it. Enforcement comes from runtime tool permissions (a read-only reviewer must be read-only at the sandbox level) and from GitHub branch protection, required checks and authorized reviews. An agent-written report is evidence to inspect, never an authenticated approval.
