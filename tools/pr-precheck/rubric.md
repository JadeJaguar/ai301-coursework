# Rubric: is this pull request ready to submit?

Each pass condition below is one short rule. Words in bold have an
exact meaning in the Definitions section under the table, and the rule
means exactly what the definition says. Examples from practice packages
live in `references/evidence-guide.md`, not here.

The checks come in four groups, one per way a PR fails: silent drift
(the first three), not tested (the next two), unreviewable (one), and
standards wall (two). The last two checks are preferred.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-within-plan | Every hunk of the diff, read against the plan's changes, files, and scope, and against its deviation notes | Every hunk is **covered by the plan**. | required |
| plan-delivered | Every **planned change**, read against the diff, the plan's deviation notes, and the description | Every planned change is in the diff, or a deviation note and the description both say it was left out. | required |
| description-matches-diff | Every **content claim** in the title, description, checklist, and test evidence, read against the diff | Every content claim is true of the diff. | required |
| repro-rerun | The test evidence (and the description's testing text), read against the **failing cases** in the plan's repro evidence and the expected-after in the plan's test plan | For every failing case, the evidence shows a run of that case on the fixed code and its **observable result**, and that result is the plan's expected-after. | required |
| repo-checks-run | The test evidence and description, read against every **named check** in the plan's test plan and in the repo's stated asks | Every named check is shown with its command and its **stated result**. | required |
| no-debris | Every hunk of the diff | The diff contains no **debris**. | required |
| template-asks-met | The repo's PR template and contributing asks (the repo-facts block, or live, the template file and CONTRIBUTING.md), read against the title, description, and diff | Every **applicable ask** is met. | required |
| ai-disclosure | The repo's stated AI policy, read against the description | If the policy **requires disclosure**, the description contains a **disclosure**. If it does not, this check passes. | required |
| test-targets-fix | The test files added or changed in the diff, read against the failing cases | At least one added or changed test runs a failing case and asserts its fixed result. | preferred |
| thread-direction-engaged | The thread's maintainer comments, read against the plan and the description | The plan or description answers every **maintainer direction** in the thread. | preferred |

## Definitions

These definitions are part of the rules above. Apply them exactly.

**covered by the plan** (diff-within-plan). A hunk is covered when at
least one of these is true:

1. It is part of a change the plan says it will make: it sits in the
   code that change touches and serves that change. Comments that
   explain the change count.
2. It adds or changes a test for this fix.
3. It adds an entry (changelog, release note, docs line) that the plan
   names or that the repo's stated asks require.
4. A deviation note in the plan names it.

Any other hunk is not covered, even inside a file the plan names and
even if it is a good idea. That includes: refactoring or renaming
beyond the fix, rewording text the fix does not need, a new option,
flag, or setting, a dependency change, and a fix for a different
issue.

**planned change** (plan-delivered). An edit the plan says it will
make: a code change, a test, or a docs or changelog entry. A run in the
test plan is not a planned change. Something the plan itself calls not
in scope, deferred, or a known limit is not a planned change.

**content claim** (description-matches-diff). A sentence that says
what the PR contains or does not contain. Examples of the kinds: "adds
a test for X", "changelog entry added", "docs updated", "no other
changes", "implements the plan exactly", "no new options", "one
function touched". A claim that something was NOT added is true when
no hunk adds it. A claim about a run's result is not a content claim;
repro-rerun and repo-checks-run grade those.

**failing case** (repro-rerun). A command or step that the plan's repro
evidence shows failing on the unfixed code. A control (a step that
works on the unfixed code) is not a failing case. If the repro
evidence shows two different failing commands, there are two failing
cases.

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

**named check** (repo-checks-run). A test suite, linter, type checker,
or build command that the plan's test plan names, or that the repo's
stated asks require to pass. If neither names any, this check passes.

**stated result** (repo-checks-run). The outcome of that command:
passed or failed, with or without counts, or its printed output. The
evidence must name the command (for example "`cargo test` passes",
"`make lint`: All checks passed!"). "All tests pass" with no command
named is not a stated result. A check that did not pass still counts
when its output is shown and the description says why it did not pass.

**debris** (no-debris). Any of these in an added line or hunk:

1. A debug print or debug log line (for example `eprintln!("DEBUG`,
   `print(` used for tracing, `console.log`).
2. Commented-out code.
3. Dead code: a function, variable, or import that nothing uses, or
   code kept "for reference".
4. A TODO, FIXME, or question comment.
5. A churn hunk: a hunk whose only effect is formatting, re-indenting,
   reordering, or re-printing lines with the same content.

A hunk that makes a real change is not churn, even if the diff shows
some unchanged lines as removed and re-added inside it. A comment that
explains the fix is not debris.

**applicable ask** (template-asks-met). Something the repo's PR
template or contributing guide asks every PR, or this kind of PR, to
have: a template section, an issue link in the form the template
gives (for example `Closes #123`), a checklist, a title format, a test,
or a file entry such as a changelog or whatsnew line. An ask that
starts with a condition ("for UI changes", "when fixing a bug")
applies only when the condition is true. A section counts as met when
it has real content for this PR; "N/A" with a reason counts as met. A
required file entry is met only if the diff adds it.

**requires disclosure** (ai-disclosure). The repo's stated policy says
that AI use must be disclosed in contributions or pull requests. In
live mode, the course policy always requires disclosure, so this is
always true there. "No stated AI policy" means it does not.

**disclosure** (ai-disclosure). A sentence in the description that
says AI was used and how (the tool and what it did). If the policy
asks for more (for example the extent of the help, or that the author
reviewed every line), the disclosure must say that too.

**maintainer direction** (thread-direction-engaged). A comment by an
OWNER, MEMBER, or COLLABORATOR (or the issue author, when they are
one) that names a cause, an approach, a rule, or a question for the
fix. If there is none, this check passes.

## Verdict rule

The verdict is `accept` only if every required check is `pass`. If
any required check is `fail` or `unclear`, the verdict is `reject`.
`unclear` counts as `fail`. Preferred checks are reported but never
change the verdict.
