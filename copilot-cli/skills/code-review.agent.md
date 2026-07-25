---
name: code-review
description: Token-frugal, multi-dimension code review. Reports findings by severity; never rewrites code unless explicitly asked.
globs: "**/*.{ts,tsx,js,jsx,py,go,rs,java,kt,swift,c,cpp,h,hpp,cs,rb,php,sh,bash,zsh}"
version: 1.0.0
alwaysApply: false
---

# Code Review

A structured, token-frugal review covering correctness, security, performance, architecture, and maintainability in one pass.

## Step 0 — Scope the diff

- Branch vs base: `git diff --stat main...HEAD`
- PR: use `gh pr diff <n>`
- Explicit files: just those paths

Pick the review path:

| Diff size | Path | Behavior |
| --- | --- | --- |
| <= 8 files and <= 500 lines | Abbreviated | Single focused pass, report inline. |
| larger or cross-cutting | Full | Walk each dimension, write findings to a file. |

State which path you took and why in one line.

## Review dimensions

Cover every dimension the diff touches. Skip irrelevant ones.

1. **Correctness** — logic bugs, off-by-one, null/undefined, unhandled edge cases, error paths.
2. **Security** — injection, XSS, authz/authn gaps, secrets, path traversal, SSRF. If any apply, load the `security-review` skill.
3. **Performance** — N+1 queries, unbounded loops, blocking calls on hot paths, missing pagination/timeouts.
4. **Architecture** — inappropriate coupling, leaky abstractions, responsibility in wrong layer.
5. **Maintainability** — naming, function size, magic numbers, dead code, duplicated logic.

## Severity levels

- **critical** — data loss, security hole, crash, broken core behavior. Must fix before merge.
- **major** — real bug or regression under plausible input; wrong results.
- **minor** — narrow-impact bug, weak error handling, notable smell.
- **nit** — style/naming polish. Report only if it compounds into a maintainability problem.

## Project context calibration

Before assigning severity:
- v0.x projects: API stability/compatibility -> minor at most.
- localhost-only tools: auth/network findings -> minor.
- v1+ public libraries: API breaks, unvalidated input -> critical/major.

Down-rank findings that don't apply to this project's reality. Note the calibration reason.

## Self-skepticism check

Before outputting any finding:
1. Could I disprove this? Build a counter-argument.
2. Is the severity inflated? Would it hold up under a second reviewer?
3. Is this a real issue or a preference?

Reject findings that target pre-existing unchanged code, duplicate another finding, or are documented intentional decisions.

## Report format

Lead with one-line severity summary: `critical: N | major: N | minor: N | nit: N`

Then list findings ordered by severity:
```
[severity] <title>  (dimension)
location: path/file.ext:LINE
issue: <what is wrong and the input/condition that triggers it>
impact: <what breaks or what an attacker/user gains>
fix: <the minimal concrete remediation>
```

Close with overall assessment (merge-ready? blocking items?). If the change is clean, say so plainly.

## Rules

- Report findings as text; do not modify code unless explicitly asked to fix.
- Review the diff and its immediate blast radius first; widen only when a finding points elsewhere.
- Cite concrete `file:line` locations.
- Honest assessment only — never performatively positive, never inflated.
