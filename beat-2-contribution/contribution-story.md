# Contribution: Proper error handling missing when output zarr file exists

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]

**Student:** Iman

**GitHub Username:** JadeJaguar

**Issue:** https://github.com/ecmwf/WeatherGenerator/issues/1575

**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

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

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

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

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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
