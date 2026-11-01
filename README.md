# Chief Debugger

A Claude Code custom command that makes the agent debug with evidence instead of guesses.

Most AI debugging goes: see an error, change some code, hope. Chief Debugger blocks that. The agent has to find and prove the root cause before it may touch code, and it can never declare a bug fixed on its own.

## How it works

1. **Understand** what was expected versus what happened.
2. **Trace** the full path from input to output (UI, API route, tool call, database, external API) and mark each step as deterministic code, AI-controlled or provider-controlled.
3. **Gather evidence**: logs, database rows, provider responses, stack traces, the recent diff.
4. **Find the failure point**, the exact step where expected and actual state split, and classify the bug.
5. **Prove the root cause** in a fixed Diagnosis format before writing any code.
6. **Make the smallest fix**, with a test for the failure case.
7. **Verify** with the strongest proof available, then hand back to the human.

## Guardrails

| Rule | What it stops |
|---|---|
| No code changes before a proven root cause | Random fix attempts |
| Every claim needs evidence | Confident guesses |
| No production provider actions without approval | Agent side effects on live systems |
| Only the human can say "fixed" | False "done" reports |
| Fewest files possible, no unrelated rewrites | Collateral damage |
| Must say "I could not verify this because ___" | Silent gaps in testing |
| Stop after 3 failed attempts and question the design | Fix loops |
| External calls wrapped with full error logging | Silent failures |

## Install

Copy `chief-debugger.md` into `~/.claude/commands/` (all projects) or `.claude/commands/` in a repo (one project). Then run `/chief-debugger` in Claude Code and describe the bug.

The toolkit section points to the `systematic-debugging` and related skills from [Superpowers](https://github.com/obra/superpowers). Chief Debugger still works without them.
