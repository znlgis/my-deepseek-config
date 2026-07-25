---
name: verification-before-completion
description: Enforce evidence-before-assertions discipline. Use when about to claim work is complete, before committing, or before creating PRs.
globs: "**/*"
version: 1.0.0
alwaysApply: false
---

# Verification Before Completion

**Core principle:** Evidence before claims, always. If you haven't run the verification command, you cannot claim it passes.

## The iron law

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

## The gate function

```
BEFORE claiming any status or expressing satisfaction:

1. IDENTIFY: What command proves this claim?
2. RUN: Execute the FULL command (fresh, complete)
3. READ: Full output, check exit code, count failures
4. VERIFY: Does output confirm the claim?
   - If NO: State actual status with evidence
   - If YES: State claim WITH evidence
5. ONLY THEN: Make the claim

Skip any step = not verifying
```

## What each claim requires

| Claim | Requires | Not sufficient |
| --- | --- | --- |
| Tests pass | Test command output: 0 failures | Previous run, "should pass" |
| Linter clean | Linter output: 0 errors | Partial check, extrapolation |
| Build succeeds | Build command: exit 0 | Linter passing, logs look good |
| Bug fixed | Test original symptom: passes | Code changed, assumed fixed |
| Regression test works | Red-green cycle verified | Test passes once |
| Requirements met | Line-by-line checklist | Tests passing |

## Red flags — STOP

- Using "should", "probably", "seems to"
- Expressing satisfaction before verification
- About to commit/push/PR without verification
- Relying on partial verification
- Thinking "just this once"

## Self-verification checklist (before completion)

1. Re-read every modified file from top to bottom — scan for leftover debug prints, TODOs, incomplete logic.
2. Verify the change doesn't break callers — grep for usages of modified functions/types.
3. If the project has tests, run them; if not, state that tests were not available.
4. Check for unused imports, variables, or parameters.

## When to apply

**Always before:**
- Claiming any task is complete
- Expressing satisfaction about work state
- Committing, creating PRs, moving to next task
- Any communication suggesting completion/correctness
