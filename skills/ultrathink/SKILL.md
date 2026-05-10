---
name: ultrathink
description: High-rigor autonomous development and strategy validation mode. Use when the user invokes ultrathink, asks for maximum-depth autonomous coding, asks Codex to coordinate subagents for research/design/implementation/testing/review, or asks to stress-test a strategy for loopholes until confidence is evidence-backed.
---

# Ultrathink

## Core Intent

Run the task as a high-confidence delivery loop: gather enough context, decompose the work, use parallel delegation when explicitly authorized and available, implement decisively, verify with concrete evidence, then search for loopholes and iterate until no material unresolved risk remains.

Treat "100% confident" as an operational standard, not a literal guarantee. Do not claim certainty beyond the evidence. The target state is: all known important assumptions are checked, all discovered material loopholes are fixed or disclosed, and the remaining risk is specific and acceptable.

## Operating Rules

- Obey all current system, developer, tool, safety, sandbox, and approval constraints.
- Do not spawn subagents unless the active environment permits it and the user's request or explicit `$ultrathink` invocation authorizes delegation.
- Use subagents for bounded parallel work, not as generic search engines. Give them concrete tasks such as codebase investigation, design review, implementation in a disjoint file area, test repair, regression analysis, or security review.
- Keep the main agent responsible for the critical path, integration, final judgment, and user communication.
- Do not run unbounded loops. Continue iterating while new material risks are found and there is a concrete next action. Stop when further work would be speculative, blocked, or disproportionate, and state the residual risk.
- Preserve user work. Never revert unrelated changes or rewrite broad areas unless the user requested it.

## Workflow

1. Clarify the objective only if local context cannot answer a risky ambiguity. Otherwise proceed.
2. Inspect the repository or artifact directly. Identify existing patterns, constraints, tests, and likely failure modes before changing files.
3. Build a concise task map:
   - immediate critical-path work for the main agent
   - independent sidecar work suitable for subagents
   - validation gates required before completion
4. If delegation is authorized and useful, spawn the maximum helpful set of non-overlapping subagents that the task can support without duplicating work. Assign ownership, expected output, and limits.
5. While subagents run, perform non-overlapping critical-path work locally.
6. Integrate returned findings or patches deliberately. Review their changes before relying on them.
7. Run the strongest practical verification available: tests, type checks, builds, linters, screenshots, browser checks, command output, or manual inspection, as appropriate.
8. Run the confidence loop below.
9. Finish with concise results, evidence, changed files, and any remaining risk.

## Delegation Pattern

Use delegation like this when available:

- Investigator: inspect a narrow subsystem and report relevant files, contracts, and risks.
- Architect: challenge the proposed design against existing patterns and edge cases.
- Worker: implement a bounded slice with a disjoint write set.
- Tester: add or run focused tests and report exact failures.
- Reviewer: review the integrated diff for regressions, missing checks, and maintainability risks.

For every subagent:

- State that it is not alone in the codebase and must not revert edits made by others.
- Specify files or modules it owns when editing.
- Ask for a final answer listing changed files, tests run, and unresolved concerns.
- After completion, either give it the next bounded task if more work remains, or close it when no longer needed.

## Confidence Loop

Before finalizing, ask internally:

"Am I fully justified in this strategy and result based on the evidence I have?"

If not, repeat:

1. List plausible loopholes: wrong requirement interpretation, stale assumptions, hidden dependencies, race conditions, edge cases, UX breakage, security/privacy issues, performance regressions, test gaps, deployment gaps, or documentation drift.
2. Rank loopholes by likelihood and impact.
3. Fix what can be fixed now.
4. Verify the fix with direct evidence.
5. Update the strategy or implementation.

Stop the loop when there are no material unresolved loopholes that can be acted on within the current task. In the final response, use evidence-backed language such as "verified by..." or "remaining risk..." instead of claiming literal perfect certainty.

## Final Response

Keep the final response short and useful:

- Say what was done.
- Mention the strongest verification performed.
- Name important files changed when relevant.
- State any residual risk or blocker plainly.
