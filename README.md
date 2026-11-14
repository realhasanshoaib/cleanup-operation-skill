# cleanup-operation-skill

`cleanup-operation` is an agent skill that runs a large, deadline-bound cleanup of a codebase. It treats the codebase as a garden: the agent pulls every weed it can find before the deadline and adds checks so weeds don't grow back.

A run untangles the code, prunes tests, drives dead code, complexity and duplicates toward zero, splits oversized files, trims docs, and models risky state machines to find bugs. It ships every change as a small reviewed PR and lands the ready ones before the deadline. At the end it reports the measurable change with an honest verdict on whether it was worth it.

## Install

```bash
npx skills add realhasanshoaib/cleanup-operation-skill --skill cleanup-operation
```

Or copy `skills/cleanup-operation` into your agent's skills folder (for example `~/.claude/skills/` or `~/.agents/skills/`).

## Use

Tell your agent to run it and give it four facts:

> Run a cleanup operation on `my-app` and `my-api`. Deadline 17:00, 40% usage left, skip mobile.

The agent asks for any of the four it is missing. Starting the skill counts as permission to spend the usage left on parallel agents, so start it when you want that usage burned.

## What's inside

- `skills/cleanup-operation/SKILL.md`: the skill.
- `skills/cleanup-operation/references/untangle-codebase.md`: [caldoai's `untangle-codebase`](https://github.com/caldoai/skills/tree/main/skills/untangle-codebase), copied at commit `f3cf477` under its MIT license (`untangle-codebase.LICENSE`).
- `skills/cleanup-operation/agents/openai.yaml`: metadata for Codex.

## License

MIT
