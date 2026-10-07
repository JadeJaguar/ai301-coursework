# repro-check

A Claude Code skill from Unit 2. It grades a claim comment and a reproduction report against their GitHub issue, and decides if they are ready to post. It prints a grade for each check and ends with a verdict: accept or reject.

## Files

- `SKILL.md`: when the skill runs and the output it must give.
- `scope.md`: the repo it may grade (`codepath/pathreview-ai301-fa26-s3`).
- `rubric.md`: the 10 required checks and the verdict rule.
- `references/evidence-guide.md`: where each kind of evidence lives in a report.
- `voice-guide.md`: my rules for how a posted comment should sound.

## Use

Install this folder at `~/.claude/skills/repro-check/`. Run Claude Code from the folder that holds the drafts, and ask it to run repro-check on them.

`rubric.md` and `references/evidence-guide.md` are frozen at the version recorded in `beat-1-sandbox/unit-2/eval-run.txt`.