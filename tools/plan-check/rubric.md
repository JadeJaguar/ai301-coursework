# Rubric: is this plan ready to post and build from?

Each pass condition below is one short rule. Words in a rule that could
be read two ways are defined in the Definitions section under the
table, and the rule means exactly what the definition says. Examples
from practice packages live in `references/evidence-guide.md`, not
here.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause, read against every step and every control result in the Repro evidence block | No repro result **conflicts** with the stated cause. | required |
| fix-at-cause | The plan's main change, read against the plan's own stated cause | The main change edits the code, setting, or value that the plan's own cause names; it is not a **workaround**. | required |
| scope-bounded | Every change listed in the plan's scope, change list, approach, and files, read against the Repro evidence | Every change in the plan is **needed for this fix**. | required |
| out-of-scope-named | The plan's not-in-scope, deferred, or "not touching" statement, under any heading, or the plan comment | The plan or comment names at least one **specific nearby thing** it will not change. | required |
| executable | The plan's files or code areas and its approach | The plan names **where the change goes** and one **chosen approach**. | required |
| test-decisive | The plan's test plan, read against the Repro evidence steps | The test plan names a **repeatable run** that re-creates the repro's failing condition and the **observable result** that run must show after the fix. | required |
| comment-faithful | The Candidate plan comment, read against the Candidate plan | Every **change the comment promises** is in the plan, and the comment does not leave out the plan's main change. | required |
| thread-engaged | The Thread highlights and the Issue body, read against the Candidate plan comment | The comment **engages** every **direction comment** in the thread. | required |
| ai-disclosure | The contribution policy line in Repo facts, read against the Candidate plan comment | If the policy **requires disclosure for comments**, the comment contains a **disclosure**. If it does not, this check passes. | required |
| unknowns-stated | The plan's risks, unknowns, or open questions, under any heading | The plan names at least one risk, unknown, or open question about its own change. | preferred |

## Definitions

These definitions are part of the rules above. Apply them exactly.

**conflicts** (diagnosis-grounded). A repro result conflicts with the
stated cause when at least one of these is true:

1. The failure still happens in a run where the stated cause is absent
   or switched off.
2. The failure goes away in a run that changes only things the stated
   cause does not depend on. A run that changes the version, the
   configuration, cached or saved state, the input, the flags, or the
   environment counts here only if the stated cause gives no role to
   the thing that changed.
3. A step shows that something the cause depends on is not true. For
   example, the cause says a module is missing, and a step shows that
   module working in the same build.
4. A trace, log, debug output, timing, or intermediate value shows the
   failure happening before, or outside, the code or stage the cause
   names.
5. The plan calls a repro result irrelevant (for example "a red
   herring" or "a side effect") without showing why.

Only these five count as a conflict. A result that matches what the
cause predicts is not a conflict, even if the plan never mentions that
step. A part of the plan's explanation that no repro step tests is not
a conflict either. A run in another product used
only to show the expected behavior is not a conflict. A cause taken
from the issue or the thread is graded the same way as any other
cause: it passes only if no repro result conflicts with it.

**workaround** (fix-at-cause). The main change is a workaround when it
leaves the named cause as it is and only does one of these: documents
a way to avoid the bug, catches or hides the error, adds a retry, or
adds a general safety net around the failing code. A change that
stops the named cause from happening at the place the cause names
(for example, guarding a value before it goes wrong, or handling data
before it reaches the wrong place) is a fix, not a workaround, even if
some related behavior stays imperfect.

**needed for this fix** (scope-bounded). A change is needed for this
fix when it is one of these:

1. The edit that fixes the reproduced failure, including new code that
   edit needs.
2. The same edit applied to other sites with the same defect in the
   same function, file, or command builder.
3. Tests for this fix.
4. A one-line doc note about the fixed behavior.

Any other change fails this check, even if it is a good idea. That
includes: refactoring or restructuring code beyond the fix site,
upgrading or swapping a dependency, adding a new option, setting, flag,
or prop, migrating tests or tools, fixing a different symptom or a
different issue, and adding a CI job. Saying what will be checked or
filed separately is not a change.

**specific nearby thing** (out-of-scope-named). A named file, function,
behavior, related symptom, related issue, or alternative approach.
"Nothing else" or "no unrelated changes" alone does not count.

**where the change goes** (executable). At least one file path, or a
named module or component plus the function, branch, or code area
inside it. A named code area with a stated way to pin the exact
function (for example, tracing with debug logs) counts.

**chosen approach** (executable). The plan says what the change will
do. It does not count when the main step is "investigate", "profile",
"look into", or "figure out" with no change chosen, or when the plan
leaves the choice open ("whichever is easier", "upstream or vendored",
"X? Y? not sure"). A fallback with a stated trigger ("if the benchmark
shows a cost, I will move the check to two sites") is a chosen
approach.

**repeatable run** (test-decisive). A command, script, test, or
numbered manual steps that a stranger could run again. Manual steps
count. "Run the full test suite" alone does not count, because it does
not re-create the repro's failing condition.

**observable result** (test-decisive). A specific output, exit code,
value, count, log line, or visible state that the run must show, or
must not show, after the fix. "Should feel fast", "should work",
"looks much better", and "nothing else should break" are not
observable results.

**change the comment promises** (comment-faithful). An edit to the
repo that the comment says will be made. These are not new changes: an
offer to put the same work into someone else's PR instead ("if
another PR lands first, I will add my tests to it"), a promise to report back, and a
promise to file a separate issue.

**direction comment** (thread-engaged). A thread comment, or the issue
body, that does at least one of these:

1. Comes from an author tagged OWNER, MEMBER, COLLABORATOR, or
   CONTRIBUTOR, and names the cause, names the code location of the
   cause, says the behavior is intended, or proposes, chooses, or
   rejects a fix approach.
2. Comes from an author tagged OWNER, MEMBER, COLLABORATOR, or
   CONTRIBUTOR, and asks contributors for a specific action (test a
   build, report a result, follow a process).
3. Comes from anyone, and names an existing fix for this issue: an
   open pull request, a patch, or a test build.

Comments that only report symptoms, environments, or "me too" are not
direction comments. If the thread has no direction comment, this
check passes.

**engages** (thread-engaged). At least one of these is true:

1. The comment refers to the direction comment (by person, by PR
   number, or by its point) and either follows it or says why the plan
   differs.
2. The plan the comment describes clearly follows the direction
   comment's point without naming it: it uses the cause, location, or
   approach that comment named, or avoids the approach it rejected.

A thread comment that lists several options or several existing fixes
counts as one direction comment. The plan comment engages it when it
says which option it takes, or how it relates to the listed fixes; it
does not need to name every item in the list.

An existing fix that someone posts or announces in its own thread
comment ("I opened PR #N", "here is a patch build to test") is engaged
only by rule 1: the plan comment must refer to it by number, by
author, or by the approach it takes.

**requires disclosure for comments** (ai-disclosure). The policy text
says AI use must be disclosed, stated, or marked, and that rule covers
issue comments or covers "all AI usage" or "any form" of AI use. A
policy that only asks for disclosure in pull requests, only asks the
contributor to understand or review AI output, or only asks that
comments be in the contributor's own words, does not require
disclosure for comments. "No stated AI policy" does not require it.
Treat every package as AI-assisted work, so a required disclosure
cannot be skipped on the guess that no AI was used.

**disclosure** (ai-disclosure). A sentence in the comment that says AI
was used and names the tool or the extent of its use.

## Verdict rule

Accept only if every required check grades `pass`. A required check
that grades `fail` or `unclear` makes the verdict reject. `unclear`
counts as `fail`. Preferred checks are reported but never change the
verdict.
