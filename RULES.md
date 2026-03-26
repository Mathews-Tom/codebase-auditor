# RULES.md — Codebase Auditor

Hard constraints that govern all agent behavior. Non-negotiable.

## ALWAYS

- Spawn the three review agents (code quality, security, secret detection) in parallel — never sequentially
- Include a file:line reference and a specific fix recommendation for every finding
- Deduplicate aggressively — the user must not see the same issue reported twice from different dimensions
- When merging duplicate findings, preserve the highest severity and note all dimensions that flagged it
- Report strengths alongside issues — balanced assessments build trust
- Apply a binary pass/fail verdict: any CRITICAL or HIGH finding means FAIL
- Include a machine-parseable summary line with finding counts by severity
- Communicate scope and estimated duration to the user before starting reviews
- Sort findings by severity (CRITICAL, HIGH, MEDIUM, LOW), then by file path
- Classify each finding into its dimension: Code Quality, Security, Secrets, Architecture, Dependencies

## NEVER

- Invent findings — only report what the sub-agents and skills actually detected
- Present a flat unsorted list of findings without severity ranking
- Skip deduplication — cross-reference findings across all dimensions before reporting
- Issue a "soft pass" or "conditional approval" — the verdict is binary
- Run review agents sequentially when parallel execution is available
- Omit the strengths section from the report

## SHOULD

- For repositories with more than 50 findings, group by file instead of by severity for readability
- Determine scope automatically from git status when no explicit scope is provided
- Identify project type from project files to tailor review focus
- Build a file inventory grouped by type (source, test, config, docs) during scope analysis
- Include top 5-10 highest-impact fixes as prioritized action items
- Run architecture review and dependency audit as complementary analysis after agent-based reviews
- Present the dimension summary table early in the report for quick orientation
