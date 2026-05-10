# ultrathink

High-rigor autonomous development and strategy validation skill for Codex.

`ultrathink` turns aggressive autonomy requests into a bounded, evidence-backed delivery loop:

- inspect the real repository or artifact before changing it
- decompose work into a critical path plus useful parallel delegation
- implement decisively while preserving unrelated user work
- verify with concrete evidence
- search for loopholes and iterate until remaining risk is specific and acceptable

It intentionally treats "100% confident" as an operational standard rather than a literal guarantee.

## Install

Copy this skill into your Codex skills directory:

```sh
mkdir -p "$HOME/.codex/skills"
cp -R skills/ultrathink "$HOME/.codex/skills/ultrathink"
```

Then invoke it with:

```text
$ultrathink
```

## Files

- `SKILL.md`: main skill instructions
- `agents/openai.yaml`: Codex UI metadata and default prompt
