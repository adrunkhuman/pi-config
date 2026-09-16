---
name: review
description: Review code and related tests, documentation, and comments for material defects and useful improvements. Use when asked to review a diff, PR, branch, commit, or codebase; not automatically for every implementation task.
---

# Review

Find material problems, not reasons to fill a report. Follow the requested scope and scale rigor to the project's stakes. Inspect without editing unless fixes are also requested. For a branch or PR, review the complete diff against its merge base and enough surrounding code to understand affected behavior.

Consider correctness, security, privacy, data loss, races, reachable failure paths, and material performance or cost regressions. Flag maintainability only when it creates concrete defect risk; ignore tool-owned formatting, personal preferences, speculative abstractions, and unrelated legacy issues.

Check whether tests protect changed behavior and realistic boundaries without freezing implementation details. Look for missing or stale public contracts and operational guidance. Comments should preserve rationale, constraints, and gotchas rather than narrate syntax; do not discard uncertain domain knowledge without checking it.

Verify suspected problems against code, primary documentation, or cheap, safe local checks. Lead with actionable findings ordered by impact: cite files and lines, explain the concrete failure or gap and supporting evidence, and suggest the smallest useful correction. Distinguish uncertainty from confirmed defects. If there are no material findings, say so and mention only genuine residual risks or validation gaps.

This skill does not make self-review independent. Use a fresh worker when independent judgment is the goal.
