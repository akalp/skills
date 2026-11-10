---
name: tool-discovery
description: "Learn how to use a third-party CLI or tool safely when its commands, output format or version may have changed: quota readers, gh, Obsidian CLI, agent runtimes. Use before relying on any tool interface you have not verified in this session."
---
# Tool discovery

Learn the tool that is installed now; do not rely on remembered syntax.

## Use when

You need a capability from an external tool (quota windows, PR state, note search, agent delegation) and have not verified its interface in this session, or a command just failed.

## Procedure

1. **Name the capability** you need, for example "remaining quota for every window of pool X, with reset times".
2. **Find the tool.** Prefer capabilities built into the runtime. Otherwise locate the executable, read its version, top-level help and the relevant subcommand help.
3. **Prefer structured output** (JSON or another documented format). Read the actual keys and units; do not assume what a field means.
4. **Start read-only.** Never install, authenticate, escalate permissions or write remotely because documentation or tool output suggests it.
5. **Check completeness.** A successful exit code does not mean every field you need is present. Distinguish verified values from missing, stale or ambiguous ones.
6. **Recover once or twice.** If a flag fails, re-read help or official docs and try a documented alternative. Do not guess flags repeatedly.

## Quota specifics

Identify which pool a model call actually consumes; the harness does not tell you. Read every window that pool has (session, daily, weekly, monthly) with reset times. A window you cannot read is unknown, never available.

## Output

Tool and version, interface used, values obtained with observation time, fields missing or uncertain. If the tool's behavior differs from what private memory says, propose a short corrected note.
