# Severity Classification

Framework for consistent severity assignment across all audit dimensions.

## Severity Levels

| Severity | Definition | Response Time | Blocks Release |
|----------|-----------|---------------|----------------|
| CRITICAL | Exploitable vulnerability, data loss risk, secret exposure | Immediate | Yes |
| HIGH | Security weakness, major quality issue, architectural flaw | < 1 week | Yes |
| MEDIUM | Code quality concern, moderate risk, maintainability issue | < 1 sprint | No |
| LOW | Style issue, minor improvement, cosmetic concern | Backlog | No |

## Verdict Rules

- **PASS:** Zero CRITICAL findings AND zero HIGH findings
- **FAIL:** One or more CRITICAL or HIGH findings

No conditional pass. No soft pass. Binary.

## Severity Assignment by Dimension

### Code Quality

| Finding | Severity |
|---------|----------|
| Bare `except` / `except: pass` in critical path | HIGH |
| Bare `except` in non-critical code | MEDIUM |
| Cyclomatic complexity > 20 | HIGH |
| Cyclomatic complexity 10-20 | MEDIUM |
| No tests for critical path | HIGH |
| Test coverage < 80% overall | MEDIUM |
| Duplicated logic (3+ occurrences) | MEDIUM |
| Inconsistent naming | LOW |
| Missing type annotations | LOW |

### Security

| Finding | Severity |
|---------|----------|
| SQL injection vulnerability | CRITICAL |
| Hardcoded admin credentials | CRITICAL |
| Missing authentication on endpoint | CRITICAL |
| XSS vulnerability | HIGH |
| CSRF protection missing | HIGH |
| Insecure direct object reference | HIGH |
| Missing rate limiting on auth | MEDIUM |
| Verbose error messages in production | MEDIUM |
| Missing security headers | MEDIUM |
| Debug mode enabled | MEDIUM |

### Secrets

| Finding | Severity |
|---------|----------|
| Production API key in source | CRITICAL |
| Private key or certificate in repo | CRITICAL |
| Database connection string with password | CRITICAL |
| Test/development API key in source | HIGH |
| High-entropy string (possible secret) | MEDIUM |

### Architecture

| Finding | Severity |
|---------|----------|
| Circular dependency between modules | HIGH |
| Single point of failure with no fallback | HIGH |
| Shared mutable state across modules | HIGH |
| Overly broad public API surface | MEDIUM |
| Missing abstraction layer | MEDIUM |
| Inconsistent module structure | LOW |

### Dependencies

| Finding | Severity |
|---------|----------|
| Dependency with known CRITICAL CVE | CRITICAL |
| Dependency with known HIGH CVE | HIGH |
| Copyleft license in proprietary project | HIGH |
| Dependency abandoned (> 3 years, no maintenance) | MEDIUM |
| Unpinned dependency versions | MEDIUM |
| Unnecessary transitive dependencies | LOW |

## Deduplication Rules

When multiple dimensions flag the same file:line:

1. Keep the highest severity finding
2. Merge descriptions from all flagging dimensions
3. Note which dimensions caught it (e.g., "Flagged by: Security, Code Quality")
4. Use the most specific fix recommendation

## Finding ID Format

```
[CBA-NNN] <title>
```

- CBA = Codebase Audit
- NNN = sequential number within the report
- Sort by severity, then by file path
