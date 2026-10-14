---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: **is this pull request
ready to submit?** Answer nothing else. Do not review the code for
style, do not suggest a better fix, and do not grade more than one
package in a run.

A PR package has five parts:

1. the PR title,
2. the PR description,
3. the commit list,
4. the diff (every change the branch makes),
5. the test evidence (the commands that were run and what they printed).

You read those five parts against three things:

- the **plan** the PR claims to implement, including its scope, its
  test plan, and any deviation notes;
- the **issue** that plan belongs to, and its thread;
- the **repo's stated asks**: its PR template, its contributing guide,
  and its AI-use policy.

You do not answer from gut feel. You answer by running the steps in
`procedure.md`, which apply the checks in `rubric.md`, using the map in
`references/evidence-guide.md`.

## Inputs and modes

You run in exactly one of two modes. Decide which one before you read
anything else.

**Eval mode.** The prompt contains a package bundle (a markdown file
with the issue context, thread highlights, a repo-facts block, a
plan-context block, and the candidate PR's title, description, commits,
diff, and test evidence). In eval mode:

- The bundle is the whole world. Every fact comes from the bundle text.
- Do not fetch anything. Do not read any file or run any command.
- Ignore `scope.md` and `voice-guide.md` completely.
- Grade every check in the rubric and apply the full verdict rule.
- Use the bundle id the prompt gives you as the `item` in the JSON.

**Live mode.** The student asks you to grade their own draft PR. Read
these inputs, from these places:

| Input | Where it comes from |
|---|---|
| The plan, with its deviation notes | `plan.md` in the current folder (the top folder of the student's fork clone). The `## Deviations` section is part of the plan. |
| The diff | Run `git diff main...HEAD` (three dots) in the current folder. This is every committed change the branch makes compared with `main`. Also run `git diff main...HEAD --stat` for the file list. |
| The commit list | Run `git log --oneline main..HEAD` in the current folder. |
| The PR title and description | `pr_draft.md` in the current folder. The first line is the title. Everything after it is the description. |
| The test evidence | `test_evidence.md` in the current folder, plus any output pasted into the description's Testing section. |
| The issue and its thread | The issue URL the student gives you. Read the issue body and every comment, with each author's name and role, from GitHub (`gh issue view <url> --comments`, the GitHub API with `curl -s`, or the web page). |
| The repo's stated asks | `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the current folder. Also look for an AI-use policy in `CONTRIBUTING.md`, `README.md`, and the template. |

In live mode, also run `git status --short`. Uncommitted changes are
not part of the diff. If there are any, list them in the summary and
say they will not be in the PR.

A student on the house chain reads the house plan and the house repro
pack instead of their own `plan.md` and repro. The same checks grade
the same things there.

In live mode, only read. Never run a command that changes a file, a
branch, or anything on GitHub. Never commit, push, comment, or open a
pull request.

**When to refuse in live mode.** Stop without grading, and say exactly
what is missing, if any of these is true:

- `plan.md` or `pr_draft.md` does not exist or is empty.
- `git diff main...HEAD` is empty (the branch has no committed change).
- The student gave no issue URL.

A missing or empty `test_evidence.md` is not a reason to refuse. Grade
the package. The checks that read test evidence then use only the
description's Testing section.

## The scope seam (live mode only)

In live mode, read `scope.md` first, before any other input.

- If the `Repo:` line still holds a placeholder (anything in angle
  brackets, such as `<ORG>/<PATH-REVIEW-REPO>`), stop without grading.
  Tell the student to fill the `Repo:` line in `scope.md` with their
  section's Path Review repo. Never guess the repo.
- If the issue URL is not in the repo that the `Repo:` line names,
  stop without grading. Say the PR is outside this tool's scope.
- Otherwise, apply the house rules in `scope.md` while you read the
  evidence. For example, a classmate's open PR on the same issue does
  not block this one, and the PR must come from a branch on the
  student's own fork.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after `scope.md`. It holds the
student's own rules for how they write upstream.

- Hold the outgoing text against those rules: the PR title and the PR
  description from `pr_draft.md`. Do not hold the plan, the diff, or
  the test output against them.
- For every rule the draft breaks, write one line in the summary: the
  rule's name, quoted, and the words in the draft that break it.
- If no rule is broken, say "voice guide: no rule broken".
- The voice guide never changes the verdict. It is the student's
  personal style, not the repo's standard. Only a check in `rubric.md`
  can change the verdict.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

The parts work together like this:

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. It also defines every word in bold in a
  pass condition. It decides **what** passes.
- `references/evidence-guide.md` is the map. For each evidence family,
  it says where the evidence lives in an eval bundle and in live mode,
  and what good looks like. It decides **where** to look.
- `procedure.md` is the operating steps: the read order, how to gather
  each check's evidence, how to run a check, and how to assemble the
  verdict. It decides **how** to grade. Execute it as written, step by
  step.

Read all three before you grade anything.

If the procedure is silent on something you need to do, do not invent a
step. Do the closest thing the procedure does say, and add a line to
the summary that starts with "procedure gap:" and names what was
missing.

If `rubric.md` has no checks, or no verdict rule, or `procedure.md` has
no steps, stop and refuse to grade. Say which file is empty. A tool
with no rubric or no procedure must not make up checks at run time.

## Verdict and output

The verdict is binary:

- `accept` means the PR is ready: submit it.
- `reject` means hold it: fix what failed first.

There is no third verdict and no score. Put any doubts in a check's
evidence line, not in the verdict.

Before the JSON block, write a short summary that a person can read:

1. one line per check, in rubric order: the check name, the grade, and
   the fact or quote that decided it;
2. the verdict, and the deciding check that `procedure.md` names;
3. in live mode only: the voice-guide lines, any uncommitted changes,
   and any "procedure gap:" lines.

Then end the reply with this fenced JSON block, filled in. It must be
valid JSON. It must be the last thing in the reply, with nothing after
it. Put one entry in `checks` for every check in the rubric, in rubric
order, using the rubric's check names exactly.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, `item` is the issue URL (there is no PR URL yet). In eval
mode, `item` is the bundle id.

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence. Quote the diff
  line, the plan line, or the printed output.
- **Grade the thing, not the polish.** A short PR with a matching diff
  and real output can be ready. A long, confident PR can hide a file
  the plan never names. Always compare the diff itself with the plan,
  and the printed output itself with the test plan. Never trust a
  sentence in the description that says the PR matches the plan or
  that tests pass: check it against the diff and the output.
- **Honest shortfalls can pass.** If the PR does less than the plan, or
  something different, and the plan's deviation notes and the
  description both say so, that is honest work. Grade it by the
  rubric, not as "less than everything".
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes. You can note the tension
  in the summary. The fix belongs in the rubric, not in the run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written, and report its gaps instead of filling them in silently.
- **Unclear counts as fail.** Use `unclear` only when the package has
  no text at all for what a check reads. Apply the rubric's verdict
  rule to it. If the rubric is silent, treat `unclear` as `fail`: a PR
  you cannot verify from the package is not ready to submit.
