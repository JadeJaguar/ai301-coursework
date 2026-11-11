# Contribution: Proper error handling missing when output zarr file exists

**Contribution Number:** [1]

**Student:** Iman

**GitHub Username:** JadeJaguar

**Issue:** https://github.com/ecmwf/WeatherGenerator/issues/1575

**Pull Request:** [GitHub PR link, once opened]

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built a search strategy for my issue scout and fixed its rubric with real scans**

I turned my fit profile into gh search queries and ranking filters, but my broad label searches returned mostly tiny personal repos, so I limited the queries to real orgs like pallets, pytest-dev, and deepset-ai. My first scan lost 2 of its top 5 to issues whose authors had already opened a fix PR, so I added -linked:pr to every query and changed my nobody-else-on-it check to count "I have a prototype" as a claim. When I graded my picks, I added beets #7070 as a test because I knew it was taken; the rubric rejected it on 3 checks and showed my AI policy check was too narrow, so I widened it. Checking 10 issues by hand also showed the rubric still misses "discuss in the issue before a PR" rules, which I logged for later.

**Unit 6: Put a voice guard in front of my GitHub posts and wrote a memory file for WeatherGenerator**

I merged my voice guide copies into one guide with new rules for claims, questions, and pings, and hooked it in front of every gh comment, but on Windows it first did nothing: python3 did not exist, ln -s made copies instead of links, and Claude Code ran commands through its PowerShell tool, which the Bash-only hook never sees, until I turned that tool off. My old "no promised timelines" rule also clashed with a claim's "when I will report back", so I changed it to allow a time for my next update but never a date for a finished fix. On the real claim, the guard stopped my first try because I ran it from the wrong folder and it could not find claim-draft.md, then passed it from the right folder two minutes later. For CLAUDE.local.md I ran one scoped orientation over io.py and validation_io.py, which spent most of its answer on an unrelated latent write path and guessed tests live in tests/, but actions.sh and the CI file show CI only runs pytest src/, so I recorded that, left the latent path out as out of scope, and kept the file out of git with .git/info/exclude.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My two picks came from the AI301 Issue Selection site: ecmwf/WeatherGenerator#1575 and PowerGridModel/power-grid-model-io#384. In Unit 5, my issue-scout skill ran one scan of 216 open issues, kept 58 after ranking, and accepted 2 of the top 5. Then I gave it my two picks directly, and it accepted both. After the Unit 6 scope patch, I graded them again. #1575 was still an accept, with a "straightforward" difficulty note: the traceback points at _get_group in io.py, and the closed PR #2621 shows where a test can go. #384 changed from accept to reject, because the issue asks for new error types "across the repo" with no clear end point. I also learned that someone asked to work on #384 in March, so under the course rule it was already taken. I released it and chose #1575.

#1575 fits me because it is a small, clear Python change. The code uses an assert to stop an output item from being written twice. A maintainer (grassesi) already said it should raise a ValueError instead. So the fix is one change in one function plus a unit test, which is close to my Unit 3 fix. It has the good first issue label and was opened by a project collaborator. Before I claimed it, I read the whole timeline myself. I found that an earlier PR (#2621) was closed because it skipped the "discuss in the issue first" step. I also found an open refactor task (#1526) that lists this fix, with two open PRs from grassesi that change the same file. My scout did not see any of this. So my claim names #2621 and asks whether a separate small fix is still welcome. From this issue I hope to learn how to work with maintainers who want to talk before a PR, how zarr stores output, and how to test an error path in a large ML repo.

I claimed the issue on 2026-11-11: https://github.com/ecmwf/WeatherGenerator/issues/1575#issuecomment-6790960335

**Toolkit**

- Tool: issue-scout, with my search strategy and rubric. I graded my two picks again after the Unit 6 scope patch.
- What it produced: accept on #1575 with a "straightforward" note pointing at _get_group in io.py and the test location from #2621. #384 flipped from accept to reject on scope-feasible, because it asks for changes "across the repo".
- Difficulties: In Unit 5, nobody-else-on-it passed #384 even though a stranger had asked to work on it, because my rule lets old claims slide. The scout also missed #1526, an open refactor that lists #1575 and has two stalled PRs on the same file. I found it only by reading the issue timeline myself.
- What changed: I replaced scope-fits-newcomer with scope-feasible plus a difficulty note. The new check was stricter on #384 but looser on open design edges. It also dropped the old "2 or more closed PRs" rule, so my scout no longer warns about that on its own. Next, I would make nobody-else-on-it fail on any claim that was never released, and add a check for linked tasks and open PRs that touch the same code.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- OS: Windows 11 (Microsoft Windows 10.0.26200.9711), using Git Bash. The issue's traceback came from a Linux cluster; I tested on Windows only.
- Project: my fork of ecmwf/WeatherGenerator, `develop` at 3508fa9858fc61db0889e8d0e68051fa1c506c23 (2026-11-09).
- Python 3.12.10, uv 0.13.7, zarr 3.1.6.

I used a small setup with only the `common` package, because that is where the code for this bug lives:

```bash
uv venv -p 3.12 .venv-repro
uv pip install -p .venv-repro -e packages/common
```

Challenges: the repo's full setup (`./scripts/actions.sh sync`) pulls in torch and GPU packages, and another contributor hit download and disk space problems with it on #2621. This bug does not need any of that, so I skipped it. The `common` package needs Python 3.12 exactly (>=3.12,<3.13), so I asked uv for 3.12 by name. I also added `.venv-repro` and my local `CLAUDE.local.md` to `.git/info/exclude` so they never end up in a commit. uv was not installed at first (`uv: command not found` in Git Bash), so I installed it in PowerShell with `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"` and opened a new Git Bash window so it was on my PATH. After that, the install ran with no errors.

**Steps to Reproduce**

1. On a branch of my fork, add `repro/repro_1575.py`. It makes a new local zarr store in a temp folder, makes one output item (`ItemKey(sample=0, forecast_step=1, stream="ERA5")`, no datasets), and calls `write_zarr(item)` twice on the same store.
2. Run it normally: `.venv-repro/Scripts/python repro/repro_1575.py`
3. Run it again with asserts removed: `.venv-repro/Scripts/python -O repro/repro_1575.py`
4. Observed result: the first write works both times. The second write raises `AssertionError: Group already exists, stop overwriting` in the normal run, and zarr's `ContainsGroupError` in the `-O` run. Neither run raises a `ValueError`.

The script:

```python
"""Repro for ecmwf/WeatherGenerator#1575: write the same output item twice."""
import pathlib
import tempfile

from weathergen.common.io import ItemKey, OutputItem, zarrio_writer

store = pathlib.Path(tempfile.mkdtemp()) / "out.zarr"
key = ItemKey(sample=0, forecast_step=1, stream="ERA5")
item = OutputItem(key=key, forecast_offset=None)

with zarrio_writer(store) as zio:
    zio.write_zarr(item)
    print("first write ok:", key.path)
    try:
        zio.write_zarr(item)
        print("second write: no error")
    except Exception as e:
        print(f"second write raised {type(e).__name__}: {e}")
print("python -O active:", not __debug__)
```

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/JadeJaguar/WeatherGenerator/commit/1ee396bba6a35e7fc6f50a57f7f8091e4a6dfbd2 (branch `repro/1575-zarr-overwrite`)
- **Screenshots/logs:** output of both runs. In the `-O` output I replaced my Windows username and the temp folder name with `<user>` and `<tmp>`, as CONTRIBUTING.md asks.

  Normal run:

  ```text
  first write ok: 0/ERA5/1
  second write raised AssertionError: Group already exists, stop overwriting
  python -O active: False
  ```

  With `python -O`:

  ```text
  first write ok: 0/ERA5/1
  second write raised ContainsGroupError: A group exists in store LocalStore('file://C:/Users/<user>/AppData/Local/Temp/<tmp>/out.zarr') at path '0/ERA5/1'.
  python -O active: True
  ```

- **My findings:** The bug is still real on `develop`: writing the same output item twice never raises the `ValueError` that grassesi asked for. The code changed after the issue was filed. #1450 added an `assert` in `_get_group` (packages/common/src/weathergen/common/io.py, line 430), so a normal run now stops with an `AssertionError` instead of the issue's error. Under `python -O` the assert is removed and the write fails inside zarr with `ContainsGroupError`, the same error as the original report. So the remaining fix is to replace the assert with a `ValueError` that names the item path, as my claim says, and to add a unit test. One thing for the next unit: CI only runs `pytest src/`, so I need to ask the maintainers where an io.py test should live.

**Toolkit**

- Tool: repro-check, which graded my claim comment and my repro report together.
- What it produced: accept on all 10 checks. It fetched my public repro commit to confirm the script matched my steps, and checked the assert at io.py:430 on the same commit.
- Difficulties: it caught three things I had missed. My `-O` output showed my Windows username, which CONTRIBUTING.md asks people not to share. My claim and my report described the fix differently ("the item path" against "the store and the item path"). And I tested on Windows while the issue came from a Linux cluster, which env-matches-issue let pass because the issue names no platform.
- What changed: I redacted the path, made the report match the claim, pasted the 18-line script into the report, and added a line about the platform. Next I would change env-matches-issue to ask for the platform whenever it differs from the issue's, even when the issue does not name one.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
