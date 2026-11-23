# Contribution: Relative import past the top-level package reports a bogus module ".foo" (mypy)

**Contribution Number:** [1]

**Student:** Iman

**GitHub Username:** JadeJaguar

**Issue:** https://github.com/ecmwf/WeatherGenerator/issues/1575 (first attempt)

**Issue 2:** https://github.com/python/mypy/issues/22539

**Pull Request:** https://github.com/python/mypy/pull/22540

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

**Unit 7: Installed the ship-gate on my mypy clone and wrote a four-check manifest from CONTRIBUTING.md**

My manifest has four checks from mypy's CONTRIBUTING.md: test deps, pytest on the semanal and import tests, the self check, and pre-commit on my two files. The full suite goes in the second table, since CI runs it. The hook first called `python3`, which doesn't exist on my machine, so it silently never ran until I pointed it at my Python 3.11 path (same fix as my voice guard in Unit 6). It also runs commands through cmd.exe, so I switched to backslash paths. After that it passed on the first try, and again after I edited my AI note.

**Unit 8: Ran the PR monitor on python/mypy#22540 with a playbook built from five merged mypy PRs**

Five merged PRs from outside contributors waited 0 to 6.5 days for a first maintainer reply, so my playbook nudges after 10 quiet days (14 if a review stalls). My first search was skewed because most recent PRs came from one person, so I took one PR per author. The monitor found my PR right away and drafted a wait note, not a nudge: earliest nudge is Dec 3. I didn't post anything, since a wait note is just for me.

---

## Why I Chose This Issue

My two picks came from the AI301 Issue Selection site: ecmwf/WeatherGenerator#1575 and PowerGridModel/power-grid-model-io#384. In Unit 5, my issue-scout skill ran one scan of 216 open issues, kept 58 after ranking, and accepted 2 of the top 5. Then I gave it my two picks directly, and it accepted both. After the Unit 6 scope patch, I graded them again. #1575 was still an accept, with a "straightforward" difficulty note: the traceback points at _get_group in io.py, and the closed PR #2621 shows where a test can go. #384 changed from accept to reject, because the issue asks for new error types "across the repo" with no clear end point. I also learned that someone asked to work on #384 in March, so under the course rule it was already taken. I released it and chose #1575.

#1575 fits me because it is a small, clear Python change. The code uses an assert to stop an output item from being written twice. A maintainer (grassesi) already said it should raise a ValueError instead. So the fix is one change in one function plus a unit test, which is close to my Unit 3 fix. It has the good first issue label and was opened by a project collaborator. Before I claimed it, I read the whole timeline myself. I found that an earlier PR (#2621) was closed because it skipped the "discuss in the issue first" step. I also found an open refactor task (#1526) that lists this fix, with two open PRs from grassesi that change the same file. My scout did not see any of this. So my claim names #2621 and asks whether a separate small fix is still welcome. From this issue I hope to learn how to work with maintainers who want to talk before a PR, how zarr stores output, and how to test an error path in a large ML repo.

I claimed the issue on 2026-11-11: https://github.com/ecmwf/WeatherGenerator/issues/1575#issuecomment-6790960335

### Issue 2: python/mypy#22539

WeatherGenerator doesn't take outside contributions (clessig asked me to email first), so with my instructor's OK I switched. I also wanted a faster repo so I'd get feedback before the deadline.

I tried llama_index first, but every recent bug already had a PR, usually opened by the issue author in the same minute. My `-linked:pr` filter missed these because the PRs only cross-reference the issue. Then pandas #72276: I reproduced it, but my scout rejected it because it's labeled `Needs Triage`, and pandas declines `/take` on those. After scanning about 45 Python repos, mypy #22539 was free: filed that day, no comments, no PR.

My scout graded it accept, straightforward, with a warning that mypy closes PRs from new contributors that are "mostly generated by LLMs with little human input". It fits me: a small Python bug with a clear repro, and the issue already points at `State.correct_rel_imp` in `mypy/build.py`. I want to learn how a type checker resolves imports and how mypy's `.test` files work.

How my scout did: it was right to reject pandas, which I would have claimed. But it passed lots of issues where the opener already had a PR, since it only looks for linked PRs, comments, and assignees.

I claimed #22539 on 2026-11-22: https://github.com/python/mypy/issues/22539#issuecomment-7088994013

**Toolkit**

- Tool: issue-scout, with my search strategy and rubric. I graded my two picks again after the Unit 6 scope patch. For Issue 2, I graded pandas #72276 and mypy #22539 directly.
- What it produced: accept on #1575 with a "straightforward" note pointing at _get_group in io.py and the test location from #2621. #384 flipped from accept to reject on scope-feasible, because it asks for changes "across the repo". For Issue 2: reject on pandas (`Needs Triage`), accept on mypy (straightforward, with an LLM-policy warning).
- Difficulties: In Unit 5, nobody-else-on-it passed #384 even though a stranger had asked to work on it, because my rule lets old claims slide. The scout also missed #1526, an open refactor that lists #1575 and has two stalled PRs on the same file. I found it only by reading the issue timeline myself. For Issue 2, my search let through issues whose openers already had a PR.
- What changed: I replaced scope-fits-newcomer with scope-feasible plus a difficulty note. The new check was stricter on #384 but looser on open design edges. It also dropped the old "2 or more closed PRs" rule, so my scout no longer warns about that on its own. Next, I would make nobody-else-on-it fail on any claim that was never released, check for open PRs by the issue's author, and add a check for linked tasks and open PRs that touch the same code.

---

## Understanding the Issue

**Problem Description**

If a relative import climbs higher than the top package (like `from ..foo import bar` in `pkg/mod.py`), mypy gives the wrong error. It says it can't find a module called `".foo"`, a name that can't exist, and points you to the missing-imports docs, which won't help.

**Expected Behavior**

mypy should say `No parent module -- cannot perform relative import`, like it already does for `from .. import foo`. Python itself raises an ImportError here too.

**Current Behavior**

mypy prints `Cannot find implementation or library stub for module named ".foo"`. All three forms in the issue do this.

**Affected Components**

`State.correct_rel_imp` in `mypy/build.py`, which turns relative imports into full module names. `correct_relative_import` in `mypy/util.py` has the same code. The tests live in `test-data/unit/semanal-errors.test`.

**Toolkit**

- Tool: Claude Code in my mypy clone, with CLAUDE.local.md, to read the two functions and find the existing tests.
- What it produced: the cause in one line: the base name ends up `""`, so `"" + "." + "foo"` gives `".foo"`, which isn't empty and skips the `if not new_id` check.
- Difficulties: the reporter had already named the line, so the work was checking it, not finding it. My first repro files came out UTF-16 from PowerShell and mypy couldn't read them.
- What changed: I remade the files in Git Bash and wrote the key functions into CLAUDE.local.md.

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

### Issue 2: python/mypy#22539

**Environment Setup**

- Windows 11, Git Bash (the issue was on Linux).
- mypy `master` at 1572a1884, editable, not compiled: the same commit as the issue.
- Python 3.12.10 (issue: 3.12.3), with `--no-incremental` like the issue.

```bash
git clone https://github.com/python/mypy.git mypy
cd mypy
python -m venv .venv
.venv/Scripts/python -m pip install -e .
```

Challenge: PowerShell's `>` wrote my files as UTF-16 and mypy couldn't decode them, so I used `printf` in Git Bash.

**Steps to Reproduce**

1. Next to the clone, make the issue's layout plus two controls:

```bash
mkdir -p pkg/sub
printf 'bar = 1\n' > foo.py
: > pkg/__init__.py
: > pkg/sub/__init__.py
printf 'from ..foo import bar\n' > pkg/mod.py        # issue's main case
printf 'from .. import foo\n' > pkg/mod2.py          # control: no name after the dots
printf 'from .foo import bar\n' > a.py               # issue's second form
printf 'from ...foo import bar\n' > pkg/sub/mod.py   # issue's third form
printf 'from ..foo import bar\n' > pkg/sub/ok.py     # control: a valid relative import
```

2. Run `../mypy/.venv/Scripts/python -m mypy --no-incremental <file>` on each one.
3. Compare with Python: `python -c "import pkg.mod"`

**Reproduction Evidence**

- **Commit showing reproduction:** none (the failing tests are the first commit on my branch: https://github.com/JadeJaguar/mypy/tree/fix/22539-relative-import-past-top)
- **Screenshots/logs:**

  ```text
  $ mypy --no-incremental pkg/mod.py    # from ..foo import bar
  pkg\mod.py:1: error: Cannot find implementation or library stub for module named ".foo"  [import-not-found]

  $ mypy --no-incremental pkg/mod2.py    # from .. import foo
  pkg\mod2.py:1: error: No parent module -- cannot perform relative import

  $ mypy --no-incremental a.py    # from .foo import bar
  a.py:1: error: Cannot find implementation or library stub for module named ".foo"  [import-not-found]

  $ mypy --no-incremental pkg/sub/mod.py    # from ...foo import bar
  pkg\sub\mod.py:1: error: Cannot find implementation or library stub for module named ".foo"  [import-not-found]

  $ mypy --no-incremental pkg/sub/ok.py    # from ..foo import bar (valid)
  pkg\sub\ok.py:1: error: Cannot find implementation or library stub for module named "pkg.foo"  [import-not-found]

  $ python -c "import pkg.mod"
  ImportError: attempted relative import beyond top-level package
  ```

- **My findings:** The bug is real on `master`: all three forms say `".foo"`. The no-name control already gets the right error, and the valid import still resolves to `pkg.foo`, so normal imports aren't affected.

**Toolkit**

- Tool: repro-check, which graded my claim comment and my repro report together. I did the same for my mypy claim and repro.
- What it produced: accept on all 10 checks. It fetched my public repro commit to confirm the script matched my steps, and checked the assert at io.py:430 on the same commit. For mypy: accept on all 10.
- Difficulties: it caught three things I had missed. My `-O` output showed my Windows username, which CONTRIBUTING.md asks people not to share. My claim and my report described the fix differently ("the item path" against "the store and the item path"). And I tested on Windows while the issue came from a Linux cluster, which env-matches-issue let pass because the issue names no platform. For mypy, it wouldn't grade until my scout had, and my voice guard still stopped the claim three times: "reproduced" with no output, a cause with no evidence, and no report-back time.
- What changed: I redacted the path, made the report match the claim, pasted the 18-line script into the report, and added a line about the platform. Next I would change env-matches-issue to ask for the platform whenever it differs from the issue's, even when the issue does not name one. For mypy, I added the output, the line of code, and "report back here within a few days".

---

## Solution Approach

**Analysis**

In `State.correct_rel_imp` (`mypy/build.py`), mypy strips parts off the current module name to find the base package. If the import climbs past the top, the base is `""`, and line 1188 builds:

```python
new_id = file_id + "." + imp.id if imp.id else file_id
```

That gives `".foo"`. It isn't empty, so the `if not new_id:` check right below (which reports "No parent module") never fires. `correct_relative_import` in `mypy/util.py` has the same join.

**Proposed Solution**

Only add the name when the base isn't empty:

```python
new_id = file_id + "." + imp.id if imp.id and file_id else file_id
```

Then `new_id` stays `""` and the existing check reports the right error. Valid imports don't change. I'm not touching `util.py` (I asked the maintainers about it) or the error wording.

**Implementation Plan**

1. Add three cases to `semanal-errors.test` (top-level `from .m`, top-level `from ..m`, and `from ..m` in `pkg/mod.py`), each expecting "No parent module".
2. Run them before the fix and save the failures.
3. Make the one-line change in `mypy/build.py`.
4. Run the nearby test suites, the self check, and pre-commit.
5. Rerun the issue's repro and save before and after output.

Deviations: none.

**Toolkit**

- Tool: plan-check, grading plan.md and plan-comment.md against the issue and my repro.
- What it produced: accept, all 9 required checks passing.
- Difficulties: it wouldn't grade until my claim was posted. My procedure.md also expects a posted repro comment, but in the wild the repro stays off the thread.
- What changed: I posted the claim first. Next I'd update procedure.md to use the claim's output block and my repro report instead.

---

## Testing Strategy

Two checks prove the fix:

1. **The issue's repro, before and after.** Before: `module named ".foo"`. After: `No parent module -- cannot perform relative import`, for all three forms. The controls didn't change, so nothing else moved.
2. **New tests that fail first.** My three cases failed before the fix (`3 failed`, showing `".m"`) and pass after (`4 passed` with `-k RelativeImportFrom`; the fourth is an existing case).

Then mypy's own checks after the fix:

```text
$ .venv\Scripts\python -m pytest -q -n 4 mypy/test/testsemanal.py mypy/test/testcheck.py::TypeCheckSuite::check-modules.test
829 passed in 49.92s

$ .venv/Scripts/python -m pytest -q -n 4 mypy/test/testsemanal.py mypy/test/testcheck.py::TypeCheckSuite::check-modules.test mypy/test/testcheck.py::TypeCheckSuite::check-incremental.test mypy/test/testfinegrained.py mypy/test/testgraph.py
1954 passed, 32 skipped in 160.85s

$ .venv\Scripts\python -m mypy --config-file mypy_self_check.ini -p mypy
Success: no issues found in 196 source files

$ .venv\Scripts\pre-commit run --files mypy/build.py test-data/unit/semanal-errors.test
black, ruff check, codespell, whitespace: Passed
```

I didn't run the full suite locally; CI runs it.

**Toolkit**

- Tool: pytest with mypy's `.test` files, plus my repro folder.
- What it produced: the before and after output above (saved in test_evidence.md) and three tests that fail on `master` and pass on my branch.
- Difficulties: one test also printed a second error from `util.py` before the fix. And since the ship-gate uses cmd.exe, I had to rerun the checks that way so my evidence matched.
- What changed: I kept each test's expected output to the one "No parent module" line, and ran every check through `cmd /c` before saving the evidence.

---

## Implementation Notes

**2026-11-18**

llama_index #24315 got a PR 24 minutes after it was filed, while I was writing my claim. Noted llama_index as fast moving.

**2026-11-22, afternoon**

Left WeatherGenerator. Every recent llama_index bug already had a PR from its author. Reproduced pandas #72276, but my scout rejected it (`Needs Triage`), so I dropped it.

**2026-11-22, evening**

Found mypy #22539 after scanning about 45 repos and reproduced it. The voice guard stopped my claim three times; posted it at 7:14 PM. Fixed the ship-gate hook (`python3` again). Wrote three failing tests, then the one-line fix. Thought about also changing `util.py`, but asked the maintainers instead. All nearby tests, the self check, and lint passed. Ship-gate passed twice, and I opened PR #22540 at 7:36 PM. The monitor says wait until Dec 3 before nudging.

**Toolkit**

- Tool: Claude Code, working in my clone of my mypy fork and reading notes for that repo.
- What it produced: drafts for the repro, the three tests and the one-line fix (I read every line and ran them), test_evidence.md, and a two-commit branch.
- Difficulties: my commits only have subject lines, so `Fixes #22539` is only in the PR. mypy may close PRs from new contributors that read as mostly AI-written.
- What changed: I put the issue link and an honest AI note in the PR.

---

## Pull Request

**PR Link:** https://github.com/python/mypy/pull/22540

**PR Description:** "Fixes #22539". Explains the `".foo"` bug and the one-line fix in `mypy/build.py`, lists the three new tests, says I left `util.py` alone and asked about it, and shows before and after output plus the test results.

**Maintainer Feedback:**

- 2026-11-22: none yet.

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate, run by hand on my branch and again by its hook when I opened the PR.
- What it produced: `ship-gate: PASS`, with all four manifest checks and every pr-precheck check passing.
- Difficulties: the hook failed with `python3: command not found` at first, so it wasn't guarding anything. pr-precheck also said my title doesn't name the place, and mentioned "Sonnet 5.5" because its grader runs on a different model, which confused me.
- What changed: I fixed the hook path, kept the title, and reran the gate the pass matched what I posted.

---

## Learnings and Reflections

**Technical Skills Gained**

How mypy turns relative imports into module names, and why `".foo"` slipped past `if not new_id` (it only catches empty strings). How to write mypy's `.test` cases (`[case]`, `[file]`, `[out]`) and run them with `pytest -k`. And how to run checks the way a fresh terminal sees them: on Windows that means cmd.exe and backslash paths.

**Challenges Overcome**

Finding a free issue was the hardest part. llama_index bugs were all taken by their own authors, and pandas #72276 was blocked by `Needs Triage`. I found mypy #22539 after scanning about 45 repos. My voice guard then stopped my claim three times, fairly each time. And Windows broke two hooks the same way (`python3`).

**What I'd Do Differently Next Time**

Teach my scout to spot PRs from the issue's author and triage labels.

---

## Resources Used

- mypy CONTRIBUTING.md (https://github.com/python/mypy/blob/master/CONTRIBUTING.md): setup, test commands, and the LLM policy.
- The issue, python/mypy#22539: the reporter pointed at line 1188, which I checked.
- The existing `testRelativeImportAtTopLevelModule` cases in `semanal-errors.test`, which showed me the test format.
- pandas' contributing guide: its `Needs Triage` rule is why I skipped #72276.
- Five merged mypy PRs (#22430, #22481, #21989, #22455, #22499) for my playbook's wait times.
