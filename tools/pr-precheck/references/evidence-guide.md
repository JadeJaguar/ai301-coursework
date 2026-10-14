# Evidence guide: where evidence lives in a PR package

This is the map for every check in `rubric.md`. Each family below says
where the evidence lives in an eval bundle, where it lives in live mode
(my own PR), and what good looks like. Examples from the practice
packages are here, not in the rubric.

## Plan fidelity (harness category: silent-drift)

Checks: diff-within-plan, plan-delivered, description-matches-diff.

**In an eval bundle:**

- The plan: the "Plan context" section. The planned changes are in the
  sentences after "Scope", the files after "Files:", and the approach.
  "Not in scope", "deferred", and "known limit" sentences are the
  boundary. Any deviation note is in the plan text too ("deviation
  note", "recorded in the plan").
- What changed: the "Diff" section, read hunk by hunk. Each
  `--- a/` / `+++ b/` pair starts a file, and each `@@` line starts a
  hunk. The "Commits" list hints at extra work (for example a
  `refactor:` or `feat:` commit in a bug fix), but only the diff counts.
- What the PR claims: the "Title", the "Description" (including any
  checklist), and the sentences in "Test evidence" that say what was
  added.

**In live mode:**

- The plan: `plan.md` in the fork folder, including `## Scope`,
  `## Files`, `## Approach`, and `## Deviations`.
- What changed: `git diff main...HEAD` (three dots), and
  `git diff main...HEAD --stat` for the file list. Commits:
  `git log --oneline main..HEAD`.
- What the PR claims: `pr_draft.md`. The first line is the title; the
  rest is the description.

**What good looks like:** every hunk serves a change the plan names, or
a deviation note names it. Every planned change has a hunk, or a
deviation note and the description both say it was left out. Every
"adds X" or "no other changes" sentence in the description is true of
the diff.

**Examples from the practice packages:**

- Drift, more than the plan: calib-03 also rewrites `printPropertyKey`
  in `src/language-js/print/property.js`, a file the plan never names,
  while the description says "with no changes beyond it". pkg-03 adds
  a `truncate()` rewrite and a new `resolve_symlinks` config option
  inside the planned file, while the plan scopes out any config change.
  pkg-09 adds a `--time-format` flag and rewords unrelated help text in
  the planned file `src/cli.rs`. A hunk inside a planned file can still
  be drift.
- Drift, less than the plan: pkg-17's plan names a dev warning AND a
  docs update. The diff has only the warning, but the description says
  the docs "now document the limitation".
- A false "added" claim: pkg-12 says "CHANGELOG entry included" and
  pkg-09 says "CHANGELOG entry added", but neither diff touches
  `CHANGELOG.md`.
- Honest shortfall, still ready: pkg-13 does not handle `%VAR%`
  expansion. The plan states that limit, and the description restates
  it as a deviation note. pkg-16 defers the start-path behavior in the
  plan and says so in the description. Both are accepts.

## Test evidence (harness category: not-tested)

Checks: repro-rerun, repo-checks-run, and the preferred
test-targets-fix.

**In an eval bundle:**

- The failing cases: the "Repro evidence" paragraph at the top of
  "Plan context". A step that shows the bug is a failing case; a step
  that works on the unfixed code is a control.
- The expected-after and the named checks: the "Test plan" sentence at
  the end of the plan (for example "expect `?997;1n`", "`cargo test`").
- What was run: the "Test evidence" section, plus any before/after in
  the "Description".
- The repo's stated test asks: the "pull requests" and "contribution
  policy" lines in "Repo facts".

**In live mode:**

- The failing cases: the "Repro evidence" and "Test plan" sections of
  `plan.md`, which quote my posted repro.
- What was run: `test_evidence.md` in the fork folder, and the Testing
  section of `pr_draft.md`.
- The named checks: the plan's test plan, and the Testing checkboxes
  in `.github/PULL_REQUEST_TEMPLATE.md` (`make test-unit`,
  `make test-integration`, `make lint`, `make typecheck`).

**What good looks like:** each failing case from the repro is run
again on the fixed code, and the evidence states the concrete result
(printed output, an exit code, an error text, a value). That result is
the plan's expected-after. Each named check shows its command and its
result. A check that did not pass is shown with its output and a
reason.

**Examples from the practice packages:**

- Decisive: pkg-19 prints the before and after `yq . source.yml`
  output and a `diff ... && echo IDENTICAL` line. pkg-02 prints
  `''` before and `'IMPORTANT OUTPUT\n'` after. pkg-20 states
  `# before: ESC[?997;2n (light)` and `# after: ESC[?997;1n (dark)`.
  A note that gives the exact value counts.
- Judgment only: pkg-04 says "colors work now". calib-03 says
  "# after: matches the issue's expected output byte for byte". Neither
  shows the output.
- Wrong run: pkg-07 runs only the single-file control, never the
  two-file repro. pkg-14 runs a GET with header casing, not the
  single-header POST from the issue. pkg-10 reports a day of syncing,
  not the plan's delayed-connect repro.
- Missing a failing case: calib-04's repro has two failing commands
  (`-j 9223372036854775807 .` and `-j 200000 -x echo .`). The evidence
  re-runs only the first.
- Repo checks: "`cargo test` passes (612 tests)" (pkg-09) and
  "`go test ./pkg/minikube/machine/... ./cmd/...` passes" (pkg-16) name
  the command. "All tests pass." (pkg-14) names nothing. calib-01's
  plan and repo name no suite, so it needs none.

## Diff quality (harness category: unreviewable)

Checks: no-debris.

**In an eval bundle:** the "Diff" section, every added (`+`) line and
every hunk. The "Commits" list can point to debris ("wip", "misc
cleanups while debugging", "fmt pass"), but the diff is what counts.

**In live mode:** `git diff main...HEAD`, every added line.

**What good looks like:** every added line is the fix, its test, or a
comment that explains the fix. No debug output, no commented-out code,
no dead code, no TODOs, and no hunk that only reformats or reorders.

**Examples from the practice packages:**

- pkg-12: `// eprintln!("DBG buffer_size = {}", buffer_size);`, a
  commented-out first attempt, a dead `_unused_buffer_probe` function,
  `// TODO: ring buffer?` on an import, and a hunk that re-prints the
  read loop with the same content.
- pkg-15: `eprintln!("DEBUG accept err = {:?}", err);`, commented-out
  fd-counting lines, a dead `count_open_fds` behind
  `#[allow(dead_code)]`, and a hunk that only rewraps
  `ClientAttributes { size, style }`.
- pkg-18: a commented-out `eprintln!` and a hunk of `println!` lines
  removed and re-added with the same content.
- Not debris: pkg-02's diff shows `html = self.export_html(` as removed
  and re-added, but the same hunk changes `clear=clear` to
  `clear=False`. That is a real change, not churn.

## Standards and comms (harness category: standards-wall)

Checks: template-asks-met, ai-disclosure, and the preferred
thread-direction-engaged.

**In an eval bundle:**

- The asks: the "pull requests" line in "Repo facts" (template
  sections, checklist, issue-link form, changelog or whatsnew entry)
  and the "contribution policy" line (including any AI policy).
- Where the PR meets them: the "Title", the "Description", and, for a
  required file entry, the "Diff".
- Maintainer direction: comments in "Thread highlights" by an OWNER,
  MEMBER, or COLLABORATOR, and the issue author's role.

**In live mode:**

- The asks: `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with
  `Closes #`, Changes, Testing checkboxes, Screenshots / Demo, Notes
  for Reviewers) and `docs/CONTRIBUTING.md` (branch name, Conventional
  Commits, CI green).
- AI policy: Path Review states none, but the course requires an
  AI-use disclosure on every PR, so in live mode the disclosure is
  always required. It goes in Notes for Reviewers.
- Maintainer direction: the issue thread on GitHub, read with each
  commenter's role.

**What good looks like:** every section the template asks for has real
content for this PR ("N/A" with a reason is fine). The issue link uses
the template's form. Any required file entry is in the diff. Where a
policy requires it, the description says which AI tool was used and
what it did. Every maintainer direction is answered.

**Examples from the practice packages:**

- pkg-01: the template asks for `closes #xxxx`, a checklist, and a
  `doc/source/whatsnew/vX.X.X.rst` entry. The description has none of
  them, and the diff has no whatsnew file. The fix itself is fine.
- pkg-20: the policy says "All AI usage in any form must be disclosed,
  stating the tool used and the extent of the assistance". The
  description has no disclosure.
- Met: pkg-11's policy welcomes AI with a human in the loop, and the
  description says "this change was AI-assisted with me reviewing and
  understanding every line, and this text is in my own words". pkg-16
  meets "a fixes #<issue number> line" with `fixes #23471`.
- Maintainer direction answered: pkg-05 answers fdncred's uniqueness
  question and kronberger-droid's caveat by name.
