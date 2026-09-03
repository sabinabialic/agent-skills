---
name: pr-review
description: Use when reviewing code on a branch or pull request to surface bugs, security issues, performance problems, and maintainability concerns with concrete, actionable suggestions.
---

# PR Review

## Overview
Review the code on this branch and provide concrete, actionable suggestions. For each
issue found, explain the reasoning so the author understands why a change matters, not
just what to change.

## When to Use
- Reviewing a branch or pull request before merge
- Auditing a diff for bugs, security issues, or performance regressions
- Giving a second opinion on code quality before requesting human review

## What to Consider
1. **Code quality and best practices** — idiomatic usage, consistency with existing
   patterns in the codebase, unnecessary complexity.
2. **Potential bugs or edge cases** — null/undefined handling, off-by-one errors, race
   conditions, error handling gaps, untested branches.
3. **Performance optimizations** — unnecessary allocations, N+1 queries, redundant
   work in loops, blocking calls that should be async.
4. **Readability and maintainability** — naming, function/module size, comments that
   clarify non-obvious logic, dead code.
5. **Security concerns** — injection risks, unsafe deserialization, secrets in code,
   missing input validation/authorization checks, unsafe dependency usage.

## Process
1. Identify the diff scope: prefer `git diff` against the base branch (e.g.
   `git diff main...HEAD`) or the PR's changed files rather than the whole repo.
2. Read each changed file with enough surrounding context to understand intent, not
   just the changed lines in isolation.
3. Work through the five considerations above for each meaningful change.
4. For each issue, cite the specific file and line, describe the problem, and propose
   a concrete fix.
5. Explain the reasoning behind each suggestion (why it matters: correctness, security
   impact, performance cost, or long-term maintainability).
6. Distinguish must-fix issues (bugs, security) from nice-to-have suggestions (style,
   minor readability).

## Output Format
Organize findings by file, then by severity within each file:
```md
### <file path>
- **[Bug]** <description> — <reasoning and suggested fix>
- **[Security]** <description> — <reasoning and suggested fix>
- **[Performance]** <description> — <reasoning and suggested fix>
- **[Style/Readability]** <description> — <reasoning and suggested fix>
```
End with a short summary noting overall risk level and whether the PR is safe to merge
as-is, needs changes, or needs discussion.

## Common Mistakes
- Restating the diff instead of evaluating it.
- Flagging style nits without explaining why they matter, or mixing them with must-fix
  issues at the same priority.
- Ignoring surrounding/unchanged code that clarifies whether a "bug" is actually
  reachable.
- Suggesting a fix without explaining the underlying reasoning.
- Reviewing the whole repository when only a branch's diff was requested.
