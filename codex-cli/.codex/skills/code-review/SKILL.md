---
name: code-review
description: Token-frugal, multi-dimension code review for a diff/branch/PR. Use when reviewing changes, checking a PR, running a review-fix loop, or the task mentions "code review", "review my changes", "review this PR", "审查代码". Scales review depth to diff size, reports findings by dimension and severity, and calibrates against the project's threat model to avoid severity inflation. Reports findings; never rewrites code unless explicitly asked.
tools: ["Bash", "Read", "Grep", "Glob"]
---

# Code Review

A structured, token-frugal review discipline that covers all dimensions in one
pass, scales depth to diff size, and communicates through files for large
reviews. Pair with the `security-review` skill when the diff touches a trust
boundary.

## Step 0 — Scope the diff first

Establish the change set and its size before reading code:

- Branch vs base: `git diff --stat main...HEAD` (or the stated base)
- PR: use `gh pr diff <n> --patch`, `gh pr view <n>`
- Explicit files: just those paths

Count changed files and net changed lines. Pick the path:

| Diff size | Path | Behavior |
| --- | --- | --- |
| <= 8 files and <= 500 lines | Abbreviated (default) | Single focused pass over the diff and its immediate callers |
| larger, or cross-cutting | Full | Walk each dimension deliberately; write findings to a file |

## Review dimensions

Cover every dimension the diff actually touches. Skip dimensions with no relevant
changes.

1. **Correctness** — logic bugs, off-by-one, null/undefined, unhandled edge
   cases, error paths, incorrect assumptions about callers.
2. **Security** — injection, XSS, authz/authn gaps, secrets, path traversal,
   SSRF, unsafe deserialization. If any apply, load the `security-review` skill
   for the full checklist.
3. **Performance** — N+1 queries, unbounded loops/allocations, blocking calls on
   hot paths, missing pagination/timeouts, leaks.
4. **Architecture** — inappropriate coupling, leaky abstractions, responsibility
   in the wrong layer, needless complexity.
5. **Maintainability** — naming, function size, magic numbers, dead code,
   duplicated logic, convention drift from the surrounding codebase.
6. **Docs & comments** — AI-boilerplate comments that restate code, commented-out
   code, comments explaining WHAT not WHY, stale doc/README claims.
7. **Compatibility** — breaking API/signature changes, altered public contracts,
   changed defaults, DB/schema migrations, callers left unupdated.

## Severity levels

- **critical** — data loss, security hole, crash, or broken core behavior. Must
  fix before merge.
- **major** — real bug or regression under plausible input; wrong results.
- **minor** — narrow-impact bug, weak error handling, notable smell.
- **nit** — style/naming/comment polish. Report only if it compounds.

## Severity calibration

Judge impact in context, not by pattern-matching a rule:

- Check `AGENTS.md` for stated context, threat model, and conventions.
- v0.x projects: API stability findings downgrade to minor at most.
- Localhost-only tools: auth/network findings downgrade to minor.
- v1+ public libraries: API breaks, unvalidated input escalate to critical/major.
- Prefer one accurate high-severity finding over ten inflated ones.

## Self-skepticism check

Before writing any finding:

1. Could I disprove this? Build a counter-argument.
2. Is the severity inflated? Would it hold under a second reviewer's scrutiny?
3. Is this a real issue or a preference?

Only surface findings that survive all three questions.

## Report format

Lead with a one-line severity summary:
`critical: N | major: N | minor: N | nit: N` and the path taken.

Then list findings, ordered by severity:

```
[severity] <title>  (dimension)
location: path/to/file.ext:LINE
issue: <what is wrong and the input/condition that triggers it>
impact: <what breaks, or what an attacker/user gains>
fix: <the minimal concrete remediation>
```

Close with a short overall assessment. If the change is genuinely clean, say so
plainly.

## Large reviews — communicate through files

On the Full path, write the findings to a file (e.g.
`.codex/review-<short-ref>.md`) and return only the severity summary plus the
file path.

## Review -> fix loop

When asked to review AND fix:

1. Review the current diff.
2. If no findings above `nit`, stop.
3. Apply minimal fixes for `critical`/`major` findings.
4. Verify: run the project's format/lint/test commands.
5. Re-review only the changed region. Repeat.

Stop conditions: clean (no findings above nit), max 5 iterations, or convergence.

## Rules

- Report findings as text; do not modify code unless the task explicitly asks to fix.
- Review the diff and its immediate blast radius first; widen only when a finding points elsewhere.
- Follow AGENTS.md — Quality Bar and Comment Discipline in particular.
- Cite concrete `file:line` locations.
- Honest assessment only — never performatively positive, never inflated.
