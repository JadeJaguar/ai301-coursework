# Procedure: how this tool grades a PR package

These are the steps for grading a PR package. Follow them in order. Do
not skip a step, and do not add steps of your own. If a step cannot be
done because the package is missing something, write that down and
keep going; the check that needs it grades `unclear`.

## Read order

Read the parts in this order. Write short notes as you go, because
later steps use the notes, not your memory.

1. **Repo facts.** Copy every ask in the PR template and contributing
   guide as a numbered list, one ask per line, word for word. Mark each
   conditional ask with its condition. Copy the AI policy line word for
   word.
2. **Issue and thread.** Note the reported failure in one line. Note
   every comment by an OWNER, MEMBER, or COLLABORATOR: author, role,
   and its point in one line.
3. **Plan context.** Read the plan before any part of the PR. Make four
   lists:
   - **Planned changes:** every edit the plan says it will make (code,
     tests, docs, changelog), one per line, with the file or code area.
   - **Out of scope:** everything the plan calls not in scope,
     deferred, or a known limit.
   - **Failing cases:** every command or step the repro evidence shows
     failing on the unfixed code, each with its failing result. List
     controls separately and mark them "control".
   - **Test plan:** every run the test plan names, with its
     expected-after, and every named check (suite, linter, type
     checker, build).
   Also copy every deviation note word for word.
4. **Diff.** Go through it file by file and hunk by hunk. For each
   hunk, write one line: the file, what the hunk changes, and whether
   it adds or removes debris. Also list the files the diff adds or
   changes.
5. **Commits.** List them. They show what the PR did, but the diff is
   what counts.
6. **Title and description.** Copy every content claim, one per line.
   List the description's sections and any issue link.
7. **Test evidence.** For each run, write the command, which failing
   case or control it runs, and its observable result (or "judgment
   only" if it states no concrete result). For each named check, write
   the command and its stated result.

Why this order: the plan's lists and the diff's hunk list must be in
your notes before you read the description. A confident description
("implements the plan exactly", "all tests pass") can make you read the
diff its way. The description and the test evidence are claims to
check, never proof.

In live mode, before step 1, read `scope.md` and `voice-guide.md` as
`SKILL.md` says. Then follow the same order, using the places listed in
`references/evidence-guide.md` under "In live mode". In live mode the
plan is `plan.md`, including its `## Deviations` section.

## Evidence gathering

For each check, put these notes side by side before grading. Quote the
exact words you use.

1. **diff-within-plan:** the hunk list (Read order step 4) next to the
   planned changes list and the deviation notes (step 3).
2. **plan-delivered:** the planned changes list (step 3) next to the
   hunk list (step 4), the deviation notes, and the description.
3. **description-matches-diff:** the content claims (step 6) next to
   the hunk list and the list of changed files (step 4).
4. **repro-rerun:** the failing cases and the expected-after from the
   test plan (step 3) next to the test evidence runs (step 7).
5. **repo-checks-run:** the named checks from the test plan (step 3)
   and from the repo asks (step 1) next to the named checks in the
   test evidence (step 7).
6. **no-debris:** the hunk list (step 4), with the debris marks.
7. **template-asks-met:** the numbered asks (step 1) next to the
   description's sections, issue link, and claims (step 6) and the
   changed files (step 4).
8. **ai-disclosure:** the AI policy line (step 1) next to any sentence
   in the description about AI or tools.
9. **test-targets-fix:** the test hunks (step 4) next to the failing
   cases (step 3).
10. **thread-direction-engaged:** the maintainer comments (step 2) next
    to the plan and the description.

## Check execution

1. Grade the checks in the order they appear in the rubric table.
2. For each check, apply its pass condition and the Definitions for
   any word in bold. Use only the notes from Evidence gathering.
3. Grade `pass` when the condition is met. Grade `fail` when the
   evidence is present and does not meet the condition. Grade
   `unclear` only when the package has no text at all for the part the
   check reads (for example, no diff, or no test evidence and no
   testing text anywhere). "(none provided)" is text: grade it `fail`.
4. For diff-within-plan, go through the hunk list one hunk at a time.
   Mark each hunk with the number of the "covered by the plan" item it
   meets. If any hunk meets none, grade `fail` and name the file and
   what the hunk changes.
5. For plan-delivered, go through the planned changes one at a time.
   Find the hunk that makes each one. If one has no hunk, check the
   deviation notes and the description. If either one is silent about
   it, grade `fail` and name the missing change.
6. For description-matches-diff, go through the content claims one at
   a time. For each, find the hunk that makes it true (or, for a "not
   added" claim, confirm that no hunk adds it). If any claim is false,
   grade `fail` and quote the claim and the hunk or file that
   contradicts it.
7. For repro-rerun, go through the failing cases one at a time. Find
   the run in the test evidence that runs that same case on the fixed
   code. Check that its observable result is the plan's expected-after.
   If any failing case has no such run, or its run gives only a
   judgment, grade `fail` and name the case. Controls do not need a
   run.
8. For repo-checks-run, go through the named checks one at a time. If
   any has no command with a stated result, grade `fail` and name it.
9. For no-debris, go through the hunk list. If any hunk has a debris
   mark, grade `fail` and quote the line.
10. For template-asks-met, go through the numbered asks one at a time.
    Skip an ask whose condition is false. If any applicable ask is not
    met, grade `fail` and name the ask.
11. Grade each check on its own. Do not let one check's result change
    another check's grade. A PR with debris can still pass repro-rerun,
    for example.
12. For every check, write one line of evidence: the quote or fact that
    decided the grade. For a `fail`, quote both sides, for example the
    description's claim and the diff file that contradicts it.
13. Do not re-read the whole package for a check. Go back to a part
    only if your notes do not hold the words the check needs.

## Verdict assembly

1. List every required check with its grade.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail` or `unclear`, the verdict is
   `reject`. `unclear` counts as `fail`.
4. Report preferred checks, but never use them to change the verdict.
5. The deciding check is the first required check, in rubric order,
   that is `fail` or `unclear`. In the short summary before the JSON
   block, name the deciding check and quote its evidence line, then
   name any other failed required checks. If the verdict is `accept`,
   say "all required checks passed".
6. End with the JSON block exactly as `SKILL.md` describes, with one
   entry per check in rubric order. Nothing comes after it.
