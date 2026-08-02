---
name: conventional-commits
description: Write commit messages and PR titles following the Conventional Commits standard.
---

# Conventional Commits

Produce commit messages and PR titles in the Conventional Commits format.
Spec: https://www.conventionalcommits.org

## Format

```
<type>(<optional scope>): <imperative subject>

<optional body — what changed and why, wrapped at ~72 cols>

<optional footer(s) — BREAKING CHANGE:, Closes #123, Co-authored-by:>
```

## Types

| Type | Use for | SemVer |
| --- | --- | --- |
| `feat` | new user-facing feature | minor |
| `fix` | bug fix | patch |
| `docs` | documentation only | -- |
| `style` | formatting/whitespace, no behavior change | -- |
| `refactor` | code change, neither fix nor feat | -- |
| `perf` | performance improvement | patch |
| `test` | adding or fixing tests | -- |
| `build` | build system or dependencies | -- |
| `ci` | CI configuration and scripts | -- |
| `chore` | maintenance not touching src or tests | -- |
| `revert` | reverts a previous commit | -- |

## Rules

1. Subject is imperative mood: "add", not "added"/"adds".
2. No trailing period; keep subject <= ~50 chars when possible.
3. Scope is optional and names the affected area: `feat(parser): ...`.
4. Breaking changes: mark with `!` after type/scope and/or `BREAKING CHANGE:` footer.
5. One logical change per commit.
6. Reference issues in footer (`Closes #42`), not the subject.

## Examples

```
feat(auth): add OAuth2 device-code login flow

fix(api): guard against null user before serializing

docs: clarify skill auto-discovery paths in README

refactor(orchestrator)!: drop legacy routing table

BREAKING CHANGE: the old route is removed; callers must migrate.
```

## When NOT to over-engineer

For a tiny one-file tweak, a single-line `type: subject` is enough. Only add body when the *why* isn't obvious from the diff. Match the repository's existing commit style if it follows a different but consistent convention.

## Checking existing conventions

Before proposing a format: `git log --oneline -20` to see actual commit style in use. If the repo uses a different convention, follow the existing style.
