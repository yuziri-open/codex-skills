---
name: goal-refiner
description: Refine slash-goal and /goal requests into a user-satisfying goal before creating or updating an active goal. Use when the user invokes /goal, says goal, asks to set a goal, complains that AI-selected goals feel wrong, wants a more convincing objective, or needs Codex to infer the true need, pain, desired outcome, completion criteria, scope, and quality bar before calling create_goal or update_goal.
---

# Goal Refiner

## Core Rule

Before creating or updating a goal, turn the user's rough instruction into an objective that would make them say, "Yes, that is what I meant."

Do not copy a template, imitate prior examples, or force every goal to have the same level of detail. The right goal is the smallest objective that still captures the user's real intent, the work to be done, the important boundaries, and how completion can be recognized.

## What "Good" Means

A refined goal is good when it:

- names the desired finished state, not just the activity
- reflects the user's need, pain, urgency, or dissatisfaction when present
- identifies the concrete deliverable, decision, implementation, or state change
- includes only the constraints that materially affect execution or verification
- is detailed enough for an agent to act without drifting
- is not so detailed that it narrows the work incorrectly
- preserves the user's exact wording when their wording is intentional and already precise

## Anti-Overfitting Rule

Treat any examples, prior goals, or familiar phrasing as non-authoritative. They are not patterns to reuse.

When refining, reason from the current request and context only:

- If the task is simple, use a simple goal.
- If the task is broad, ambiguous, high-risk, or multi-step, make the goal more specific.
- If the user complains about quality, alignment, or "AI-ish" output, include the quality failure to avoid and the quality bar to hit.
- If the user asks for a tool action, infer the human outcome behind the tool action.
- If the user asks for an improvement, name what must become better and how that improvement will be verified.

## Intent Reconstruction

Before setting a goal, infer these fields mentally. Do not output the full analysis unless the user asks.

- Need: what the user is trying to make possible
- Pain: what is wrong, inefficient, risky, unclear, disappointing, or low quality
- Desired outcome: what should be true when the work is done
- Deliverable: what file, artifact, implementation, decision, publication, or state should exist
- Acceptance: what would prove the result is complete and usable
- Boundaries: what should not be changed, assumed, sent, published, deleted, or overdone
- Scope horizon: whether the goal is for this turn, the active task, or a broader project

If a field is irrelevant to the task, omit it from the final objective.

## Detail Calibration

Choose the goal's detail level based on the work, not on a fixed formula.

- Minimal detail: use for tiny, clear requests where extra wording would add noise.
- Moderate detail: use for normal implementation, writing, research, or debugging tasks where the deliverable and verification should be named.
- High detail: use for ambiguous, high-risk, multi-file, publishing, automation, external-service, or user-dissatisfaction tasks where mistakes are costly.

Increase detail only when it changes how the work should be done or checked. Do not list every inferred subtask inside the goal unless those subtasks are essential acceptance criteria.

## Refinement Workflow

1. Detect the goal moment.
   - Trigger on `/goal`, `goal`, "set a goal", Japanese goal-setting phrasing, or user dissatisfaction with a current or proposed goal.
   - If the user explicitly provides a final objective and it is already precise, preserve it closely.
   - If the user requests a better goal, improve the objective before using any goal tool.

2. Identify the user's real target.
   - Translate tool-shaped language into outcome-shaped language.
   - Translate vague improvement language into a visible finished state.
   - Preserve concrete user instructions such as repository names, file paths, deadlines, deliverables, and publication requests.

3. Decide the required specificity.
   - Ask: what detail would prevent the most likely wrong execution?
   - Ask: what detail would let us verify completion?
   - Exclude detail that merely sounds thorough but does not guide action.

4. Compose the objective.
   - Prefer one sentence.
   - Use two sentences only when a single sentence would become tangled or hide an important boundary.
   - Include the deliverable and acceptance signal when they matter.
   - Avoid generic phrases such as "improve the thing" unless the user intentionally used them as the target label.

5. Decide whether to ask a question.
   - Ask one concise question only when two plausible interpretations would lead to materially different work.
   - Otherwise, infer from context and proceed.

6. Set the goal.
   - Use `create_goal` only after the objective is refined.
   - Include a token budget only if the user explicitly requests one.

## Internal Checks Before Calling `create_goal`

Reject and rewrite the objective if any of these are true:

- It describes an activity but not the result.
- It could fit many unrelated tasks.
- It blindly reuses example-like wording.
- It adds constraints the user did not imply.
- It omits a stated deliverable, repository, file path, publication target, or verification requirement.
- It authorizes external sending, publishing, destructive edits, credential handling, or privacy-impacting actions without explicit user request.
- It is too narrow to let the agent do necessary supporting work.
- It is too broad to know when the work is done.

## Output Behavior

If helpful, briefly state the refined goal in plain language, then call the goal tool. Keep the explanation shorter than the goal-setting work itself.

Use phrasing like:

`Given that intent, I will set the goal as: "..."`

If the user likely expects immediate execution after `/goal`, set the goal and continue the task rather than stopping at meta-discussion.
