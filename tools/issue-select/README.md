# issue-select

A Claude Code skill from Unit 1. It grades a GitHub issue against a written rubric, and decides if it is a good first contribution.

## Files

- `SKILL.md`: when the skill runs and the output it must give.
- `scope.md`: the repo it may grade.
- `rubric.md`: the checks and the verdict rule.
- `references/evidence-guide.md`: where each kind of evidence lives in an issue.

## Use

Install this folder at `~/.claude/skills/issue-select/`, then ask Claude Code to run issue-select on an issue URL.

`rubric.md` and `references/evidence-guide.md` are frozen at the version recorded in `beat-1-sandbox/unit-1/eval-run.txt`.