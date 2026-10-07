# plan-check

A Claude Code skill from Unit 3. It grades a fix plan and its draft plan comment against the GitHub issue and the reproduction evidence, and decides if the plan is ready to post and build from. It prints a grade for each check and ends with a verdict: accept or reject.

## Files

- `SKILL.md`: when the skill runs and the output it must give.
- `scope.md`: the repo it may grade (`codepath/pathreview-ai301-fa26-s3`).
- `rubric.md`: 9 required checks, 1 preferred check, their definitions, and the verdict rule.
- `procedure.md`: the steps the skill follows to grade a plan.
- `references/evidence-guide.md`: where each kind of evidence lives in a plan package and on GitHub, with examples.
- `voice-guide.md`: my rules for how a posted comment should sound.

## Use

Install this folder at `~/.claude/skills/plan-check/`. From the folder that holds `plan.md` and `comment.md`, run:

```
claude "plan-check: grade my plan in plan.md and draft comment in comment.md for issue <issue URL>"
```

`rubric.md`, `procedure.md`, and `references/evidence-guide.md` are frozen at the version recorded in `beat-1-sandbox/unit-3/eval-run.txt` (20/20).
