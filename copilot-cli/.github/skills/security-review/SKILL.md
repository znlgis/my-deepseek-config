---
name: security-review
description: Audit code changes for security vulnerabilities before merging. Reports findings; never auto-fixes silently.
---

# Security Review

A focused checklist for catching real vulnerabilities in a change set. Apply to the **diff** first, then widen only if a finding points elsewhere.

## Threat checklist

- **Injection** — untrusted input flowing into SQL, shell, OS commands, eval, template engines. Look for string concatenation instead of parameterized queries/argument arrays.
- **XSS** — user data rendered into HTML/JS/DOM without escaping. Check `innerHTML`, `dangerouslySetInnerHTML`, unescaped template interpolation.
- **AuthN / AuthZ** — missing authentication, missing ownership/permission checks, IDOR, privilege escalation, trusting client-supplied roles.
- **Secrets** — hardcoded API keys, tokens, passwords, private keys, connection strings; secrets logged or committed. Should come from env/secret store.
- **Path traversal / file access** — user-controlled paths joined without normalization/allow-listing.
- **SSRF** — server-side requests to user-supplied URL/host without allow-listing.
- **Deserialization / parsing** — untrusted data fed to unsafe deserializers.
- **Crypto** — weak algorithms (MD5/SHA1 for passwords), hardcoded IVs, ECB mode, missing TLS verification, predictable randomness.
- **Sensitive data exposure** — PII/secrets in logs, error messages, or API responses; verbose stack traces leaked to clients.
- **Dependencies** — newly added packages: reputable, pinned, free of known CVEs.
- **Resource & DoS** — unbounded loops/request sizes, missing timeouts, ReDoS.
- **Race conditions / TOCTOU** — check-then-act without locking.

## Method

1. Identify the **trust boundary**: where does untrusted input enter, and where does it reach a sink (DB, shell, filesystem, network, HTML)?
2. Trace each tainted value from source to sink; a vuln exists when it reaches a dangerous sink without validation/encoding/parameterization.
3. Prefer **allow-lists** over deny-lists; prefer library-provided escaping over manual sanitization.
4. Only flag issues with a concrete exploit path — no speculative noise.

## Report format

For each finding:

```
[severity: high|medium|low] <title>
location: path/to/file.ext:LINE
issue: <what is wrong and the input that triggers it>
impact: <what an attacker gains>
fix: <the minimal concrete remediation>
```

Order findings by severity. If you reviewed and found nothing actionable, say so explicitly.
