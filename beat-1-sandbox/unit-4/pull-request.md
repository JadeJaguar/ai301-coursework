# Unit 4: Test and Submit

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/235

**Branch**

`fix/61-health-check-sql-text`

## Eval iterations

**Run history**

1. Partial run, 6 packages (`--only pkg-01,pkg-02,pkg-11,pkg-13,pkg-20,calib-03 --include-calibration`): 5/5 scored items agreed, and calib-03 (never scored) was rejected, matching its label. I picked the packages most likely to show a rule that was too strict or too loose: pkg-02 (unchanged lines shown as removed and re-added), pkg-11 (before/after given as notes), pkg-13 (an honest shortfall), and both standards-wall packages. I read every failed check with `--out` before going on: each reject failed for its gold reason, and the three accepts had no failed check at all.
2. Full run: 20/20, `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`, `agreement: 20/20 scored items  (bar: 18/20: PASS)`. This is the run in `eval-run.txt`. I froze `SKILL.md`, `rubric.md`, `procedure.md`, and `references/evidence-guide.md` after it.

**Package analysis**

pkg-01 (pandas-dev/pandas#66657, category standards-wall). Gold label: reject. My rubric: reject, in both runs.

The gold note says: "solid C-parser fix with decisive ASAN evidence, but pandas' required PR checklist is visibly ignored: no closes #xxxx, no checklist, and the stated whatsnew entry is absent from the diff".

My rubric read it the same way. In the full run, the deciding failure was `template-asks-met`, with the grader's evidence: "No 'closes #66657' link, no checklist, and no doc/source/whatsnew/vX.X.X.rst entry in the diff". The repo-facts line it read says the template "is a required checklist: closes #xxxx with the issue number, tests added and passed, all code checks passed (pre-commit), type annotations added to new arguments, and an entry in the latest doc/source/whatsnew/vX.X.X.rst when fixing a bug". My definition of **applicable ask** covers all three misses: "an issue link in the form the template gives", "a checklist", and "a file entry such as a changelog or whatsnew line", and it says "A required file entry is met only if the diff adds it". The whatsnew ask starts with a condition ("when fixing a bug"), and this PR fixes a bug, so the ask applies.

Every other check that tests the fix itself passed: the diff stays inside the plan's `tokenizer.c` change and its test, and `repro-rerun` passed because the evidence prints `99999` before and `100000` after. That is the point of this category: a correct, well-tested fix still fails if it ignores the repo's stated asks.

pkg-01 also failed one more required check, `repo-checks-run`: "pytest pandas/tests/io/parser passes (2841 passed), but template-required 'all code checks passed (pre-commit)' has no command or result". That is a fair reading of my **named check** definition ("a test suite, linter, type checker, or build command that ... the repo's stated asks require to pass"), but it means one template ask fails two checks. I watched for that rule hurting pkg-08, a clear accept whose template asks for "code formatted" with no command shown. pkg-08 was accepted with no failed check, so I did not change the rule.

**Check rationale**

The check, as it reads in `tools/pr-precheck/rubric.md`:

```
| repro-rerun | The test evidence (and the description's testing text), read against the **failing cases** in the plan's repro evidence and the expected-after in the plan's test plan | For every failing case, the evidence shows a run of that case on the fixed code and its **observable result**, and that result is the plan's expected-after. | required |
```

and the definition it depends on most:

```
**observable result** (repro-rerun). The concrete thing the run
produced: the printed text, a value, an exit code, an error message,
table rows, or a named log line. It may be printed output or a note
that states it exactly (for example `# after: exit 0, prints "parsed
OK"`). The absence of a named error or line counts when the evidence
names what is absent (for example "no `dangling '\'` error"). A
judgment is not an observable result: "works", "fixed", "colors work
now", "matches the expected output", "verified", "no problems for a
day". A run of a different input, of a control only, or of a path the
fix does not change is not a run of the failing case.
```

It reads this way for three reasons.

First, I rejected the class worksheet's stricter rule. The worksheet said "a yes needs a command and its printed output. A note after # saying what it printed doesn’t count." But three gold accepts give their before/after as notes: pkg-20 (`# before: ESC[?997;2n (light)`), pkg-11 (`# after: 'foo ' not listed (ignored),`), and pkg-13 (`-- after:  { code = 0, stderr = "" }`). What makes calib-03's note fail is not that it is a note. It is that `# after: matches the issue's expected output byte for byte` is a judgment with no value in it. So the line is drawn between an exact value and a judgment, not between output and a note.

Second, **failing case** means a step that "the plan's repro evidence shows failing on the unfixed code", and "If the repro evidence shows two different failing commands, there are two failing cases". That is what catches calib-04, which re-runs only the first of its two failing commands. It also keeps the check from failing pkg-05, whose test plan adds a new same-key case that was never a failing repro step. Controls do not need a re-run, because pkg-13 and pkg-19 state "control unchanged" in a sentence and are gold accepts.

Third, the last sentence names the three not-tested patterns I found in the set: a different input (pkg-14 runs a GET with header casing, not the single-header POST), a control only (pkg-07 runs only the one-file case), and a judgment (pkg-04 "colors work now", pkg-10 "a full day of WebDAV syncs").

**Trade-offs**

The cost of accepting a note as evidence is that the tool cannot tell a note someone copied from real output from a note someone typed. A PR that writes `# after: exit 0` without running anything passes `repro-rerun`. I accept that miss, because the gold accepts write evidence this way, and a rule that demanded raw printed output would have rejected pkg-11, pkg-13, and pkg-20 (pkg-20 would still be rejected, but for the wrong reason).

The lenient rules could have flipped other packages, so I checked them before spending a full run. My first partial run used pkg-02, pkg-11, and pkg-13 as accept canaries, and pkg-01 and pkg-20 as canaries for the 2-package standards-wall category. In the full run all 7 clear accepts came back with zero failed checks, so nothing that agreed was paid for by the lenient rules.

Live mode on my own PR showed a case the eval set does not test. My first live run rejected my draft on `repo-checks-run`, because `docs/CONTRIBUTING.md` says "All five jobs must pass" and gives `cd frontend && npm ci && npm test -- --run`, which I had not run. The rubric's definition caught it, but my evidence guide's live-mode list names only the four `make` checks from the PR template, and the run reported that as a gap. I had already frozen the files after the 20/20 run, so I did not edit the guide. I ran the frontend check instead, reported its local timeout honestly with the reason, and the second live run accepted. The run also reported a procedure gap: nothing says how to grade "CI is green on this PR" before the PR exists.
