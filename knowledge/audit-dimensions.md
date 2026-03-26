# Audit Dimensions

Reference for the five review dimensions assessed during a codebase audit.

## Dimension Overview

| Dimension | Reviewer | Focus Areas |
|-----------|----------|-------------|
| Code Quality | code-reviewer agent | Naming, complexity, DRY, error handling, test coverage |
| Security | security-reviewer agent | OWASP Top 10, injection, XSS, auth vulnerabilities |
| Secrets | secret-scanner agent | Hardcoded API keys, tokens, passwords, private keys |
| Architecture | architecture-reviewer skill | Module boundaries, coupling, dependency direction |
| Dependencies | dependency-audit skill | CVEs, license compliance, maintenance health |

## Code Quality Checks

| Check | What to Look For |
|-------|-----------------|
| Naming conventions | Consistent casing, descriptive names, no abbreviations |
| Cyclomatic complexity | Functions with complexity > 10 |
| DRY violations | Logic duplicated across 2+ locations |
| Error handling | Bare except, swallowed errors, missing error paths |
| Test coverage | Untested critical paths, missing edge cases |
| Dead code | Unused imports, unreachable branches, commented-out code |
| Type safety | Missing type annotations, `any` usage, unsafe casts |
| Function length | Functions > 50 lines |
| File length | Files > 300 lines |

## Security Checks (OWASP Top 10)

| Category | Detection Pattern |
|----------|------------------|
| Injection (SQL, NoSQL, OS) | String concatenation in queries, unsanitized shell commands |
| Broken Authentication | Weak token generation, missing expiry, no rate limiting |
| Sensitive Data Exposure | Unencrypted PII, secrets in logs, missing HTTPS |
| XML External Entities | XML parsing without disabling external entities |
| Broken Access Control | Missing auth checks, IDOR vulnerabilities, privilege escalation |
| Security Misconfiguration | Debug mode, default credentials, verbose errors in production |
| Cross-Site Scripting | Unescaped user input in HTML, missing CSP headers |
| Insecure Deserialization | Deserializing untrusted data without validation |
| Known Vulnerabilities | Outdated dependencies with published CVEs |
| Insufficient Logging | No audit trail for auth events, no tamper detection |

## Secret Detection Patterns

| Provider | Pattern |
|----------|---------|
| AWS | `AKIA[0-9A-Z]{16}`, `aws_secret_access_key` |
| GitHub | `ghp_[A-Za-z0-9]{36}`, `gho_`, `ghu_`, `ghs_` |
| Slack | `xoxb-`, `xoxp-`, `xapp-` |
| Stripe | `sk_live_`, `rk_live_` |
| Google | `AIza[0-9A-Za-z-_]{35}` |
| Generic | High-entropy strings (> 4.5 Shannon entropy) |

## Architecture Checks

| Check | What to Look For |
|-------|-----------------|
| Module boundaries | Clear separation of concerns, no circular imports |
| Dependency direction | Dependencies flow inward (infra → domain, not reverse) |
| Coupling metrics | Number of cross-module imports, shared mutable state |
| Scalability | Single points of failure, bottleneck services |
| API surface | Overly broad public interfaces, leaking internals |

## Dependency Health Indicators

| Indicator | Healthy | Unhealthy |
|-----------|---------|-----------|
| Last release | < 6 months | > 2 years |
| Open issues trend | Stable or declining | Growing, no maintainer response |
| CVE count | Zero known | Unpatched critical/high CVEs |
| License | MIT, Apache 2.0, BSD | GPL (if proprietary), AGPL, unknown |
| Download trend | Stable or growing | Declining sharply |
