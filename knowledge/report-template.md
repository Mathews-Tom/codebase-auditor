# Report Template

Standard output format for unified audit reports.

## Report Structure

```markdown
# Codebase Audit Report

**Scope:** <files/directories audited>
**Date:** <YYYY-MM-DD>
**Verdict:** PASS | FAIL

**Summary:** X CRITICAL, Y HIGH, Z MEDIUM, W LOW across N files

## Dimension Summary

| Dimension | Critical | High | Medium | Low | Score |
|-----------|----------|------|--------|-----|-------|
| Code Quality | - | - | - | - | A-F |
| Security | - | - | - | - | A-F |
| Secrets | - | - | - | - | A-F |
| Architecture | - | - | - | - | A-F |
| Dependencies | - | - | - | - | A-F |

## CRITICAL

### [CBA-001] <title>
- **Dimension:** <dimension name>
- **File:** `path/to/file.ext:line`
- **Issue:** <description of what is wrong>
- **Fix:** <specific recommendation with code example if applicable>
- **Flagged by:** <list of dimensions that caught this>

## HIGH
<same format>

## MEDIUM
<same format>

## LOW
<same format, or summarized if > 20 findings>

## Strengths
- <positive observations from each dimension>

## Priority Action Items
1. <highest impact fix with file reference>
2. <second highest>
3. <third>
4. <fourth>
5. <fifth>
```

## Dimension Scoring

| Grade | Criteria |
|-------|----------|
| A | Zero findings in this dimension |
| B | Only LOW findings |
| C | MEDIUM findings, no HIGH or CRITICAL |
| D | HIGH findings present |
| F | CRITICAL findings present |

## Report Assembly Checklist

| Step | Action |
|------|--------|
| 1 | Collect all findings from agents and skills |
| 2 | Deduplicate by file:line |
| 3 | Assign severity using classification framework |
| 4 | Sort: CRITICAL > HIGH > MEDIUM > LOW, then by file path |
| 5 | Generate dimension summary table |
| 6 | Calculate dimension grades |
| 7 | Determine verdict (PASS/FAIL) |
| 8 | Identify top 5 action items by impact |
| 9 | List strengths from each dimension |
| 10 | Generate machine-parseable summary line |

## Large Codebase Handling

When total findings > 50:
- Group findings by file instead of by severity
- Add a per-file summary section
- Collapse LOW findings into a count-only summary
- Maintain full detail for CRITICAL and HIGH

## Machine-Parseable Summary

Include at the top of every report for downstream consumption:

```
**Findings:** X CRITICAL, Y HIGH, Z MEDIUM, W LOW
**Verdict:** PASS | FAIL
**Scope:** <N files, M lines>
**Dimensions:** Code Quality: <grade>, Security: <grade>, Secrets: <grade>, Architecture: <grade>, Dependencies: <grade>
```

## Handoff Format

When returning to a spawning agent:
- Full report as markdown text
- Summary line parseable by regex: `\*\*Verdict:\*\* (PASS|FAIL)`
- Finding count parseable by regex: `\*\*Findings:\*\* (\d+) CRITICAL, (\d+) HIGH`
- Top 3 action items as a standalone section
