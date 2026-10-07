# Procedure: how this skill grades a plan package

These are the steps for grading a plan someone else wrote. Follow them
in order. Do not skip a step, and do not add steps of your own. If a
step cannot be done because the package is missing something, write
that down and keep going; the check that needs it grades `unclear`.

## Read order

Read the parts in this order. Write short notes as you go, because
later steps use the notes, not your memory.

1. **Repo facts.** Note the contribution policy line word for word,
   and whether it has any rule about AI.
2. **Issue.** Note the reported failure in one line, and the author
   tag of the issue opener (OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR,
   or NONE).
3. **Thread highlights.** Note every comment that may be a direction
   comment (see the rubric's Definitions): author, author tag, date,
   and its point in one line. Note every PR number, patch, or test
   build that anyone names.
4. **Repro evidence.** Make a numbered list: one line per step and one
   line per control, each with its result. Mark any trace, log,
   timing, or intermediate value that shows where the failure happens.
5. **Candidate plan.** Read it only after steps 1 to 4. Note its
   stated cause, every change it lists, its files or code areas, its
   approach, its test plan, its not-in-scope statements, and its risks.
   The plan may use any headings or none; find each part by what it
   says, not by its heading.
6. **Candidate plan comment.** Note every change it promises, every
   person, PR, or thread point it names, and any sentence about AI use.

Why this order: the repro results must be fixed in your notes before
you read the plan's story. A confident plan, or a confident thread
comment, can make you read the evidence its way. The thread and the
plan comment are claims to check, never proof.

In live mode, before step 1, read `scope.md` and `voice-guide.md` as
`SKILL.md` says. Then follow the same order using the places listed in
`references/evidence-guide.md` under "In live mode".

## Evidence gathering

For each check, collect these notes before grading. Quote the exact
words you use.

1. **diagnosis-grounded:** the plan's stated cause (quote), and the
   numbered repro results from Read order step 4.
2. **fix-at-cause:** the plan's main change (quote) and the plan's
   stated cause (quote). The main change is the edit the plan says
   fixes the reported failure. If the plan lists several changes, the
   main change is the one aimed at the reproduced failure.
3. **scope-bounded:** a list of every change in the plan, one line
   each, from every part of the plan (scope, proposed changes,
   approach, files, test plan).
4. **out-of-scope-named:** every "not in scope", "not touching",
   "deferred", or "leaving alone" statement, from the plan or the
   comment.
5. **executable:** every file path, module, function, or code area the
   plan names, and the plan's approach in one line.
6. **test-decisive:** the test plan (quote), the run it names, and the
   result it says the run must show.
7. **comment-faithful:** the changes the comment promises (from Read
   order step 6) next to the plan's change list (from step 3 above).
8. **thread-engaged:** the direction comments and named fixes from
   Read order step 3, and what the comment says about each one.
9. **ai-disclosure:** the policy line from Read order step 1, and any
   AI sentence from the comment.
10. **unknowns-stated:** any risk, unknown, or open question the plan
    names about its own change.

## Check execution

1. Grade the checks in the order they appear in the rubric table.
2. For each check, apply its pass condition and the Definitions for
   any word in bold. Use only the notes from Evidence gathering.
3. Grade `pass` when the condition is met. Grade `fail` when the
   evidence is present and does not meet the condition. Grade
   `unclear` only when the package has no text at all for the part
   the check reads (for example, no test plan anywhere).
4. For diagnosis-grounded, go through the numbered repro results one
   by one. For each result, ask the five conflict questions in the
   Definitions. Stop at the first conflict and grade `fail`. If no
   result conflicts, grade `pass`.
5. For scope-bounded, go through the change list one line at a time.
   Mark each change with the number of the "needed for this fix"
   category it belongs to. If any change belongs to no category,
   grade `fail` and name that change.
6. For thread-engaged, go through the direction comments one at a
   time. If the comment does not engage even one of them, grade
   `fail` and name it. If there are no direction comments, grade
   `pass`.
7. Grade each check on its own. Do not let one check's result change
   another check's grade. A plan with a wrong cause can still pass
   executable, for example.
8. For every check, write one line of evidence: the quote or fact that
   decided the grade. For a `fail`, quote both sides, for example the
   plan's cause and the repro result that conflicts with it.
9. Do not re-read the whole package for a check. Go back to a part
   only if your notes do not hold the words the check needs.

## Verdict assembly

1. List every required check with its grade.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail` or `unclear`, the verdict is
   `reject`. `unclear` counts as `fail`.
4. Report preferred checks, but never use them to change the verdict.
5. In the short summary before the JSON block, name each required
   check that failed and quote its evidence line. If the verdict is
   `accept`, say "all required checks passed".
6. End with the JSON block exactly as `SKILL.md` describes, with one
   entry per check in rubric order. Nothing comes after it.
