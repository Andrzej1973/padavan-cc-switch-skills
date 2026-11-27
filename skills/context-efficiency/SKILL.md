---
name: context-efficiency
description: Reduce unnecessary context and repeated work in long Codex sessions while preserving correctness; use stable instructions and evidence-based handoffs without claiming control over prompt caching.
---

# Context Efficiency

## Purpose

Help Codex work efficiently in large, long-running repositories by avoiding redundant reading, repeated explanations, oversized prompts, and unnecessary tool output. Preserve technical accuracy, safety checks, and project-specific requirements.

This skill is an operating procedure, not a token-metering tool.

## Important limits

- Prompt caching is managed by the model-serving platform. This skill cannot enable, disable, inspect, or guarantee a cache hit.
- Do not claim that a cached token is free, excluded from a subscription limit, or always cheaper. Billing and usage treatment depend on the product, model, and current plan.
- Reusing a prior conclusion is safe only when its source and freshness are established.
- Never omit security review, tests, relevant context, or user requirements merely to reduce token usage.

## Workflow

1. **Establish the task boundary**
   - Identify the repository, branch, HEAD, working-tree state, target platform, and exact task.
   - Read applicable `AGENTS.md`, project handoff documents, and the relevant skill instructions once per session or when they change.
   - Keep unrelated projects and workstreams separate.

2. **Use a stable instruction prefix**
   - Keep durable project rules in repository instruction files or dedicated Skills.
   - Keep recurring instructions concise and stable; put task-specific details after them.
   - Do not repeatedly paste a large project prompt when a short reference to a maintained project document is sufficient.
   - Do not reorder or rewrite stable instructions without a reason. Stable prefixes may improve the chance of prompt-cache reuse, but do not guarantee it.

3. **Read only what the task needs**
   - Start with status, diffs, file paths, symbols, and targeted searches.
   - Read complete files when control flow, configuration semantics, or safety requires it.
   - Avoid dumping entire repositories, build logs, or generated files into context.
   - For large outputs, filter to relevant sections and retain the exact command, exit code, and evidence needed for the conclusion.

4. **Avoid redundant work**
   - Before rereading or rerunning something, check whether the file, commit, configuration, or test result has changed.
   - Reuse prior findings only when they identify the inspected commit/file version and remain applicable.
   - Re-run checks after relevant changes; do not reuse stale PASS results.
   - Do not repeat full reports when a concise delta and current status are enough.

5. **Maintain a compact, reliable handoff**
   - Record current branch and HEAD, task objective, files inspected/changed, tests run and results, blockers, and next safe action.
   - Distinguish facts, assumptions, and unknowns.
   - Keep separate handoffs for independent boards or workstreams.
   - Link to canonical project documents instead of copying their full contents into every task.

6. **Control tool output**
   - Prefer targeted file reads, focused searches, and concise summaries.
   - Do not request verbose output unless needed to diagnose a failure.
   - Preserve raw evidence for critical findings, CI failures, security issues, and release decisions.
   - Never hide errors or truncate evidence in a way that changes the conclusion.

## Required response

For long-running work, report:
- Current repository/branch/commit
- What was reused from prior analysis and why it is still valid
- New files or evidence inspected
- Checks newly run and their results
- Remaining unknowns and next action

## Acceptance criteria

The workflow reduces avoidable repetition without reducing verification quality. No cache hit, token saving, or subscription-limit benefit may be claimed unless the product exposes evidence for that specific claim.
