# I opened a one-line fix for a wrong import error in mypy, with my own tools checking each step

## Overview

mypy is the main type checker for Python. In issue [python/mypy#22539](https://github.com/python/mypy/issues/22539), a relative import that climbs past the top package (like `from ..foo import bar` inside `pkg/mod.py`) gave a confusing error about a missing module named `".foo"` instead of the correct "No parent module" error. I reproduced the bug, claimed the issue, and opened [PR #22540](https://github.com/python/mypy/pull/22540) with a one-line fix and three tests. Claude Code wrote the tests. I read every line, ran them, and said so in the PR. I also ran tools I built in this course, with parts I wrote myself: the issue-scout rubric and search strategy, the voice guide behind my voice guard, the ship-gate manifest, and the PR monitor playbook. The PR is still open and waiting for review.

## What happened

### 1. Finding an issue nobody had taken

My first issue (ecmwf/WeatherGenerator#1575) needed email approval before outside work. On Nov 22, every recent llama_index bug already had a PR from its own author, and my scout's `-linked:pr` filter missed them because those PRs only cross-referenced the issue. I reproduced pandas #72276, but my scout rejected it, and it was right. My toolkit notes record its verdict:

> reject on pandas (`Needs Triage`), accept on mypy (straightforward, with an LLM-policy warning)

pandas declines `/take` on `Needs Triage` issues. After scanning about 45 Python repos, I found mypy #22539: filed that day, no comments, no PR.

### 2. Reproducing the bug

On mypy master (commit 1572a1884):

```text
$ mypy --no-incremental pkg/mod.py    # from ..foo import bar
pkg\mod.py:1: error: Cannot find implementation or library stub for module named ".foo"  [import-not-found]

$ mypy --no-incremental pkg/mod2.py    # from .. import foo
pkg\mod2.py:1: error: No parent module -- cannot perform relative import
```

Same mistake, two different errors. The reporter pointed at line 1188 of `mypy/build.py`, and I checked it:

```python
new_id = file_id + "." + imp.id if imp.id else file_id
```

When the import climbs past the top, `file_id` becomes `""`, so this builds `".foo"`. The next line, `if not new_id`, only catches an empty string, so the right error never fires.

### 3. The voice guard stopped my claim three times (change made)

My voice guard checks what I post on GitHub against my voice guide. It stopped my claim three times. From `guard-log.md`:

```text
19:05 [hook] stop: rule broken: "I only say I reproduced something after I have the output to show. Before that, I say what I will try."
19:10 [manual] stop: rule broken: "A root cause I have not shown with evidence"
19:11 [manual] stop: rule broken: "My claim says I want the issue, the first thing I will do, and when I will post my next update..."
19:14 [hook] pass | target: gh issue comment https://github.com/python/mypy/issues/22539
```

Each stop was fair. My second version added the mypy output, the code line, and "report back here within a few days". It passed, and became the [claim I posted](https://github.com/python/mypy/issues/22539#issuecomment-7088994013) at 7:14 PM.

### 4. Tests first, then the fix

Claude Code wrote three test cases in `semanal-errors.test`. Before the fix, they failed the way they should:

```text
Expected:
  main:1: error: No parent module -- cannot perform relative import (diff)
Actual:
  main:1: error: Cannot find implementation or library stub for module named ".m" (diff)
3 failed, 1 passed, 576 deselected in 1.29s
```

The fix only joins the name when `file_id` is not empty:

```python
new_id = file_id + "." + imp.id if imp.id and file_id else file_id
```

After the fix, all three passed, plus 1,954 nearby tests, the self check, and pre-commit. I left a similar line in `mypy/util.py` alone and asked the maintainers about it.

### 5. The ship-gate (a second change)

My ship-gate hook runs my manifest and pr-precheck before `gh pr create`. At first it failed with `python3: command not found` (exit 127), so it was guarding nothing. I pointed it at my Python path, and then it passed:

```text
- verdict: PASS
  - description-matches-diff [pass]: "Two files" and "(one line)" match the stat
  - ai-disclosure [pass]: "I used Claude Code (claude opus 5.5) for this PR..."
```

I opened PR #22540 at 7:36 PM.

### 6. What landed, and what has not

The PR is open with all 25 CI checks passing, and mypy's own bot reported:

> this change doesn't affect type check results on a corpus of open source code. ✅

No maintainer has reviewed it yet, so nothing is merged. My PR monitor drafted a wait note instead of a nudge:

> A nudge would become appropriate on or after 2026-12-03 (opened 2026-11-23T01:36Z, plus 10 days)

My playbook set 10 days after I checked five merged mypy PRs from outside contributors, which waited 0 to 6.5 days for a first response.

## Limitations and what's next

The biggest limit is that I did not write the tests myself. mypy's contributing guide discourages LLM use by new contributors and says PRs from them that are "mostly generated by LLMs with little human input will be closed." My PR says plainly that Claude Code wrote the code. I did real work around it (the repro, the checks, the `util.py` question), but a maintainer could still close it, and that would be fair. Next time I will write the change myself first and use Claude Code to check it, not the other way around.

My scout also has a gap: it passed issues whose openers already had a PR, because it only checks linked PRs, comments, and assignees. Next I will add a rubric check that searches for open PRs by the issue's author that mention the issue number.

For this PR: no nudge before Dec 3, and if a maintainer answers my `util.py` question, I will add that change.
