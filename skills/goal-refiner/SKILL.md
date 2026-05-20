---
name: goal-refiner
description: Refine slash-goal and /goal requests into a user-satisfying goal before creating or updating an active goal. Use when the user invokes /goal, says goal, asks to set a goal, complains that AI-selected goals feel wrong, wants a more convincing objective, or needs Codex to infer the true need, pain, desired outcome, completion criteria, scope, and quality bar before calling create_goal or update_goal.
---

# Goal Refiner

## Core Rule

Before creating or updating a goal, transform the user's rough instruction into a goal that the user can plausibly accept as "yes, that is what I wanted."

Do not treat the first wording as final when it is vague, emotional, broad, or tool-shaped. Interpret the user's need, frustration, desired future state, and definition of done, then set a concrete objective.

## Workflow

1. Detect the goal moment.
   - Trigger on `/goal`, `goal`, "set a goal", Japanese goal-setting phrasing such as "set the goal", "make this into a goal", or dissatisfaction with an existing goal.
   - If the user explicitly provides a final objective with no ambiguity, preserve it closely.
   - If the user is asking for a better goal, improve it before using any goal tool.

2. Reconstruct intent.
   Identify these six elements from the message and current context:
   - Need: what the user is really asking to make possible.
   - Pain: what is currently unsatisfying, risky, slow, confusing, or low quality.
   - Desired outcome: what should be true at the end.
   - Deliverable: what artifact, decision, implementation, or state should exist.
   - Acceptance criteria: what would make the user feel the work is complete and usable.
   - Boundaries: what should not be changed, assumed, sent, published, or overdone.

3. Convert vague requests into a completion-shaped objective.
   Prefer goals that name the finished state, not just the activity.
   - Weak: "Improve the prompt."
   - Strong: "Create and validate a Codex skill that detects /goal-style requests and turns rough intent into a concrete, user-satisfying objective before any goal is set."

4. Add quality expectations when the user signals dissatisfaction.
   If the user says the current goal is often not convincing, shallow, too AI-led, or not aligned with human expectations, include explicit quality language:
   - "captures the user's need, pain, desired outcome, scope, and completion criteria"
   - "asks only necessary clarifying questions"
   - "keeps the user's exact wording when it is clearly intentional"
   - "produces a goal that is specific enough to guide execution and verification"

5. Decide whether to ask a question.
   Ask one concise question only when a wrong assumption would materially change the goal. Otherwise infer from context and proceed.
   Useful question types:
   - "Should this goal cover only this turn, or the broader project?"
   - "Is the main deliverable a file change, a plan, or a decision?"
   - "Should I optimize for speed, depth, or minimal disruption?"

6. Set the goal.
   Use `create_goal` only after the objective is refined.
   Keep the objective one sentence when possible.
   Include a token budget only if the user explicitly requests one.

## Goal Formula

Use this mental template, then compress it into natural language:

`Achieve [desired outcome] by producing/updating [deliverable or state], while accounting for [need/pain/context], with completion defined by [acceptance criteria and verification].`

Examples:

- User: `/goal make a skill`
  Goal: `Create and validate a local Codex skill that reliably guides future agents through the requested workflow, including clear trigger metadata, concise instructions, UI metadata, and validator-confirmed structure.`

- User: `/goal make this a more convincing goal`
  Goal: `Refine the active task into a concrete objective that reflects the user's underlying need, dissatisfaction, desired finish state, scope boundaries, and acceptance criteria before execution continues.`

- User: `/goal make this analysis good`
  Goal: `Turn the analysis into a decision-ready deliverable with clear findings, evidence, risks, unanswered questions, and next actions that match the user's intended use.`

## Output Behavior

When using this skill, briefly explain the refined goal if helpful, then call the goal tool. Avoid long meta-explanations unless the user asks how the goal was derived.

If the goal is inferred, say so plainly:

`Given that intent, I will set the goal as: "..."`

If the user seems likely to disagree with the scope, ask before setting it.

## Quality Checklist

Before setting the goal, verify:

- The objective is outcome-based, not merely activity-based.
- The deliverable or final state is visible.
- The user's pain or dissatisfaction is reflected when present.
- Completion criteria are concrete enough to verify.
- The goal does not authorize external sending, publishing, destructive edits, or credential handling unless the user explicitly requested and approved it.
- The wording is specific but not so narrow that it blocks necessary execution.
