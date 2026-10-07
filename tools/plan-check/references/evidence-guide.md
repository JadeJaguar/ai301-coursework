# Evidence guide: where evidence lives in a plan package

This guide is the map. For each family it says where to look in an
eval bundle, where to look in live mode, and what good looks like.
The rules themselves are in `rubric.md`. The examples here only show
how a rule reads on a real package; they never change a rule.

A plan can use any headings or none. calib-01 uses labels like
"Cause:", "Change:", "Out:", and "Test:" with no headings at all. Find
each part by what it says, not by what it is called.

## Diagnosis and grounding

Checks: diagnosis-grounded, fix-at-cause.

**Where it lives (eval bundle).**

- The stated cause: the plan's "Diagnosis", "Cause", "Root cause", or
  "Problem statement" part, or the first sentence that says why the
  bug happens. The plan comment often repeats it.
- The results the cause must fit: the `## Repro evidence` block. Every
  numbered step, every line marked "Control", and every trace, log,
  `--debug` output, or timing table.
- The main change: the plan's "Change", "Approach", "Proposed
  changes", or "Scope: in scope" part.

**Where it lives (live mode).** The cause is in the student's
`plan.md`. The results are in the student's own posted repro comment
on the issue, as quoted in `plan.md`. If `plan.md` quotes no repro
output, grade from what it does quote.

**What good looks like.** Every repro result is one the cause could
produce. Controls matter most, because a control is a run where one
thing was changed on purpose. If the bug goes away when the named
cause is still there, or stays when the named cause is gone, the cause
is wrong. The main change then edits that same cause.

**How it reads on a real package.**

- calib-03 (fails). The plan blames pager key bindings. Repro step 3
  runs with no pager at all and is still slow (25.8 s). That is
  conflict type 1: the failure happens with the named cause absent.
  The plan copied the cause from a thread comment, and a thread
  comment is not proof.
- calib-01 (passes). The cause is "the view is not refreshed after a
  push". Step 3 (old color while the view stays open) and step 4 (new
  color after re-entering the view) are both what that cause predicts.
- A control that shows the expected behavior in a different product
  (another terminal, another tool) is a reference point. It does not
  conflict with a cause in the product under test.

## Scope

Checks: scope-bounded, out-of-scope-named.

**Where it lives (eval bundle).** The plan's "Scope", "In scope",
"Not in scope", "Out", "Proposed changes", "Approach", and "Files"
parts. A change can hide in any of them, and in the test plan (a new
CI job is a change). Deferral lines can also be in the plan comment.

**Where it lives (live mode).** The same parts of `plan.md` and the
draft comment.

**What good looks like.** One fix for the reproduced failure, plus its
tests. The plan names something close by that it will not change, so a
reviewer can hold the diff to that line. Saying "I am deliberately
leaving X for later, because Y" is a strength, not a gap. A plan that
fixes part of the issue and says so clearly can be ready.

**How it reads.**

- A list that starts with the right fix and then adds "while I am in
  there" work (a dependency swap, a new setting, a module split, a
  test-harness migration, a CI matrix) fails scope-bounded, even when
  the first item is correct.
- Applying the same fix to a sibling call site with the same defect in
  the same function is category 2 of "needed for this fix". It passes.
- calib-01 (passes out-of-scope-named): "Out: any change to how push
  status is computed, or to other views' refresh behavior."

## Executability

Check: executable.

**Where it lives (eval bundle).** The plan's "Files", "Files and
areas", "Approach", "Steps", and "Change" parts. File paths are often
in backticks.

**Where it lives (live mode).** The "Files" and "Approach" parts of
`plan.md`.

**What good looks like.** A stranger could open the named file and
start the change today, without asking which layer, which library, or
which of two options. The plan can still have an open detail if it
says how it will settle it.

**How it reads.**

- calib-02 (fails): "poke around the editor components this weekend,
  figure out where the undo history lives". No file, and the only step
  is to look around.
- calib-01 (passes): names `pkg/gui/controllers/sync_controller.go`,
  the push completion callback, and what the callback will add.
- Naming a component and saying "exact function to be pinned with
  debug logs, which I have working" still passes: the place and the
  approach are known.

## Test plan

Check: test-decisive.

**Where it lives (eval bundle).** The plan's "Test plan", "Test",
"Testing", or "Verification" part. Some plans also list tests inside
"Changes" or "Approach"; read those too. Compare it with the steps in
the `## Repro evidence` block.

**Where it lives (live mode).** The test plan in `plan.md`, compared
with the student's posted repro steps.

**What good looks like.** The test re-runs the repro (or a test that
runs the same failing input through the real code) and states, before
any code is written, what the output must be after the fix: a value,
an exit code, a color, a count, a log line that must be gone. A manual
check is fine. The check is the result a stranger can compare with,
not whether the test is automated.

**How it reads.**

- calib-01 (passes): "repro steps above; at step 3 the color must flip
  without leaving the view."
- calib-04 (fails): "Run the full test suite and make sure nothing
  regresses." It never re-runs the failing command, so it cannot show
  the bug is fixed.
- calib-02 (fails): "undo works after toggling" names no run to repeat
  and no exact result to see.

## Honesty

Check: unknowns-stated (preferred). It also helps diagnosis-grounded,
because a plan that admits what it has not checked is easier to grade.

**Where it lives (eval bundle).** The plan's "Risk", "Risks",
"Unknowns", "Open question", or "Not tried" lines, and hedges like
"I have not yet verified".

**Where it lives (live mode).** The risks and unknowns part of
`plan.md`, and its `## Deviations` section after the build. A
deviation written there is honest work. A deviation that only shows in
the diff is not.

**What good looks like.** The plan names what could go wrong with its
own change, or what it has not checked, and what it will do about it.
"Risk: none identified" with a reason is acceptable. A long, confident
plan with no unknowns is not proof of a good plan.

## Comms

Checks: comment-faithful, thread-engaged, ai-disclosure.

**Where it lives (eval bundle).**

- The comment: the `## Candidate plan comment` block.
- Direction: the issue body and the `## Thread highlights` block. Each
  line there shows the author and an author tag in brackets, such as
  (OWNER), (MEMBER), (COLLABORATOR), (CONTRIBUTOR), or (NONE). Also
  look for PR numbers, patches, and test builds named by anyone.
- Policy: the "contribution policy" line in `## Repo facts`.

**Where it lives (live mode).** The draft comment file. The live issue
thread on GitHub, including linked pull requests in the issue's
Development panel. The repo's `CONTRIBUTING.md`, `README`, and PR
template for an AI policy.

**What good looks like.** The comment is a short version of the plan:
the same cause, the same change, the same limits, nothing extra. It
answers what the thread already said: if a maintainer named a culprit,
posted a patch, or chose an approach, the comment says how the plan
relates to it. If there are open PRs for the issue, the comment names
them and says how this work differs or how it will add to them. If the
policy requires AI disclosure for comments, the comment has one
sentence that names the tool or the extent of AI help.

**How it reads.**

- A comment that proposes its own fix while the owner has already
  named the culprit file and posted a test build, and never mentions
  either, fails thread-engaged.
- calib-03 (fails thread-engaged): a thread comment names PR #3836 as
  a fix, and the plan comment never names that PR.
- A policy that says "comments must be in your own words" or "you must
  understand AI output" does not require disclosure for comments. A
  policy that says "all AI usage in any form must be disclosed" does.
- calib-01 (passes thread-engaged): the thread is empty, so there is
  no direction comment to engage.
