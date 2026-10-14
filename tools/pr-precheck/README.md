# pr-precheck

A Claude Code skill from Unit 4. It grades a pull request before it is opened: the title, description, commits, diff, and test evidence, read against the plan the PR claims to implement, the issue, and the repo's PR template and contributing guide. It prints a grade for each check and ends with a verdict: accept (ready to submit) or reject (hold).

## Files

- `SKILL.md`: what the skill reads in live mode and eval mode, when it refuses to grade, and the output it must give.
- `CONTRACT.md`: the fixed interface from the course (not edited).
- `scope.md`: the repo it may grade (`codepath/pathreview-ai301-fa26-s3`).
- `rubric.md`: 8 required checks, 2 preferred checks, their definitions, and the verdict rule.
- `procedure.md`: the steps the skill follows to grade a PR.
- `references/evidence-guide.md`: where each kind of evidence lives in a PR package and in my own working copy, with examples.
- `voice-guide.md`: my rules for how a posted comment, PR title, and PR description should sound.

## Use

Install this folder at `~/.claude/skills/pr-precheck/`. From the top folder of the fork's clone, on the PR branch, with `plan.md`, `pr_draft.md`, and `test_evidence.md` next to it, run:

```
claude "pr-precheck: grade my draft PR for issue <issue URL>: plan in plan.md, the diff on this branch, title and description in pr_draft.md, test evidence in test_evidence.md"
```

`SKILL.md`, `rubric.md`, `procedure.md`, and `references/evidence-guide.md` are frozen at the version recorded in `beat-1-sandbox/unit-4/eval-run.txt` (20/20).
