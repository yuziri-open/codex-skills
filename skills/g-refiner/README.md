# g-refiner

Refine `/goal` and goal-setting requests into a concrete, user-satisfying objective before Codex creates or updates an active goal.

`g-refiner` helps Codex:

- infer the user's real need, pain, desired outcome, deliverable, boundaries, and acceptance criteria
- calibrate the right level of detail for each task instead of forcing a fixed template
- avoid overfitting to examples or familiar goal phrasing
- preserve the user's wording when the objective is already clear
- ask only necessary clarifying questions
- set goals that are specific enough to guide execution and verification

## Install

Copy this skill into your Codex skills directory:

```sh
mkdir -p "$HOME/.codex/skills"
cp -R skills/g-refiner "$HOME/.codex/skills/g-refiner"
```

Then invoke it with:

```text
$g-refiner
```

## Files

- `SKILL.md`: main skill instructions
- `agents/openai.yaml`: Codex UI metadata and default prompt
