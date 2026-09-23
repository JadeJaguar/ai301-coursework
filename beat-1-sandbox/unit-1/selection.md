# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61

**Verdict output**

```text
All three are in the scoped repo (codepath/pathreview-ai301-fa26-s3).
  Graded independently against rubric.md, then ranked by the fit profile.

  Shared repo evidence (capture = today, 2026-09-23):
  - Last human commit on main: Aburke225 (COLLABORATOR), 2026-09-16 — 7
    days ago → maintainer-active passes for all three
  - archived: false, pushed_at: 2026-09-16 (no releases) → repo-in-use
    passes for all three
  - docs/CONTRIBUTING.md + PR template: CI/xfail/disclosure-free
    conditions, no AI ban → ai-policy-allows passes for all three
  - Repo has zero PRs (open, closed, or merged) → no linked PRs, no
    abandoned attempts anywhere

  ---

  Ranked read-out

  1. #61 — Health check DB probe passes raw SQL string (SQLAlchemy 2.x) —
  accept
  All five required checks pass, and it is the cleanest of the three on
  claims: no assignee, no comments at all, no timeline cross-references.
  Fit: this is almost exactly your one prior open-source contribution — a
  broken health check in a FastAPI project — so you already know the shape
  of the fix. It's also the only one of the three with no pre-written
  xfail test, which means you'd write the failing test yourself: the
  specific skill you said you want to build.

  2. #68 — KeywordSearcher.index() raises ZeroDivisionError on empty
  corpus — accept
  All five required checks pass. One claim comment (acordero4852,
  2026-09-19, author_association: NONE — a classmate); under the Path
  Review house rule, a student claim does not block. Fit: BM25 retrieval
  is named directly in your RAG experience, so the library behavior won't
  be new to you. Larger estimate (2–4 hrs).

  3. #72 — verify_password raises UnknownHashError on malformed hashes —
  accept
  All five required checks pass. One claim comment (sseid4, 2026-09-22,
  NONE) and two referenced events — both from commits in classmates' own
  coursework forks (rafiatasafi/ai301-coursework,
  xyin20/ai301-coursework), not PRs against this repo. House rule: neither
  blocks. Fit: shortest (1–2 hrs) and plain Python, but passlib/auth is
  the least connected to your stated experience.

  All three also carry the preferred good first issue label, so that
  didn't separate them.
```

```json
[
    {
      "item":
  "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61",
      "checks": [
        {"name": "maintainer-active", "grade": "pass", "evidence": "Human
  COLLABORATOR commit by Aburke225 on main 2026-09-16, 7 days before
  capture"},
        {"name": "repo-in-use", "grade": "pass", "evidence": "archived:
  false; last push to any branch 2026-09-16 (7 days), well within 180"},
        {"name": "scope-fits-newcomer", "grade": "pass", "evidence":
  "Single bounded bug opened by a COLLABORATOR with exact error text and
  repro; one file (api/routes/health.py); no umbrella, no feature ask, no
  design debate, no closed PRs"},
        {"name": "nobody-else-on-it", "grade": "pass", "evidence":
  "assignees: none; repo has zero PRs; zero comments and no timeline
  cross-references"},
        {"name": "ai-policy-allows", "grade": "pass", "evidence":
  "docs/CONTRIBUTING.md and PR template state CI/test/xfail conditions
  only; no mention of AI-generated contributions"},
        {"name": "newcomer-label", "grade": "pass", "evidence": "labels:
  bug, good first issue, api, tier-1"}
      ],
      "verdict": "accept"
    },
    {
      "item":
  "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
      "checks": [
        {"name": "maintainer-active", "grade": "pass", "evidence": "Human
  COLLABORATOR commit by Aburke225 on main 2026-09-16, 7 days before
  capture"},
        {"name": "repo-in-use", "grade": "pass", "evidence": "archived:
  false; last push to any branch 2026-09-16 (7 days), well within 180"},
        {"name": "scope-fits-newcomer", "grade": "pass", "evidence": "One
  bounded bug opened by a COLLABORATOR naming two files and an xfail
  marker (H-01) to remove; no umbrella, no feature ask, no design debate,
  no closed PRs"},
        {"name": "nobody-else-on-it", "grade": "pass", "evidence":
  "assignees: none; repo has zero PRs; only claim is acordero4852
  (author_association NONE, 2026-09-19), a classmate the Path Review house
  rule says does not block"},
        {"name": "ai-policy-allows", "grade": "pass", "evidence":
  "docs/CONTRIBUTING.md and PR template state CI/test/xfail conditions
  only; no mention of AI-generated contributions"},
        {"name": "newcomer-label", "grade": "pass", "evidence": "labels:
  bug, good first issue, rag, tier-1"}
      ],
      "verdict": "accept"
    },
    {
      "item":
  "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
      "checks": [
        {"name": "maintainer-active", "grade": "pass", "evidence": "Human
  COLLABORATOR commit by Aburke225 on main 2026-09-16, 7 days before
  capture"},
        {"name": "repo-in-use", "grade": "pass", "evidence": "archived:
  false; last push to any branch 2026-09-16 (7 days), well within 180"},
        {"name": "scope-fits-newcomer", "grade": "pass", "evidence": "One
  bounded bug opened by a COLLABORATOR naming two files and an xfail
  marker (H-05) to remove; no umbrella, no feature ask, no design debate,
  no closed PRs"},
        {"name": "nobody-else-on-it", "grade": "pass", "evidence":
  "assignees: none; repo has zero PRs; claim by sseid4 (NONE, 2026-09-22)
  and two referenced events are commits in classmates' own coursework
  forks, none of which block under the house rule"},
        {"name": "ai-policy-allows", "grade": "pass", "evidence":
  "docs/CONTRIBUTING.md and PR template state CI/test/xfail conditions
  only; no mention of AI-generated contributions"},
        {"name": "newcomer-label", "grade": "pass", "evidence": "labels:
  bug, good first issue, api, tier-1"}
      ],
      "verdict": "accept"
    }
  ]

```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Run 1, partial (`--only issue-01,issue-02`): "agreement: 1/2 scored items". issue-01 failed scope because a docs task that touched five files was read as an umbrella issue. I rewrote the umbrella wording.
2. Run 2, partial (`--only issue-01,issue-02`): "agreement: 2/2 scored items".
3. Run 3, full: "agreement: 17/20 scored items (bar: 18/20: below the bar)". Missed issue-04, issue-19, and issue-20, all on scope.
4. Run 4, partial (`--only issue-04,issue-19,issue-20`): "agreement: 3/3 scored items".
5. Run 5, full with `--save-run`: "agreement: 19/20 scored items (bar: 18/20: PASS)". This is the run in eval-run.txt.


**Issue analysis**

issue-20 (excalidraw/excalidraw#11811, "Add company logo shape to the toolbar"). Gold label: reject. In Run 3 my rubric said accept. In Run 5 it said reject, which matches gold.

In Run 3, my scope check only failed umbrella issues, design debates, core internals changes, usage questions, and issues with 2 or more closed PRs. issue-20 had none of these. The body was clear and even had a "Success looks like" line, so it passed. But the bundle shows who opened it: "opened by cursor[bot] (NONE) on 2026-08-02, state open, labels: none", and "Comments (0 total, first 0 shown)". So a bot asked for a new feature for one company's logo, and no maintainer ever agreed the project wants it. My rubric never looked at who opened an issue or whether a maintainer approved a feature. I added rule (2) to the scope check so a feature request that no OWNER, MEMBER, or COLLABORATOR opened or agreed to now fails. That is why Run 5 rejected it.

**Check rationale**

From `rubric.md`, the scope-fits-newcomer pass condition:

"Fail if any of these is true: (1) the issue is an umbrella or tracking issue (a list of separate tasks meant to be split into other issues; one task that touches several files, with the change to each file spelled out, is not an umbrella issue); (2) the issue asks for a new feature, it was not opened by an OWNER, MEMBER, or COLLABORATOR, and no one with those roles has agreed to it in the comments; (3) the thread shows the design is still being debated and no maintainer has settled it; (4) a maintainer says outright that the fix touches core internals; (5) the issue is a usage question ("how do I get this to work?"); (6) there are 2 or more closed, unmerged PRs for it. Otherwise pass. Grade only the work the issue requires: optional "suggestions" or "ideas" are not required work. A short body, a list that ends in "etc.", or a missing repro does not fail this check, especially when a maintainer opened the issue or it has a good first issue label."

Each part came from a real miss. The note in (1) came from issue-01, where a multi file docs task was read as an umbrella issue. Rule (2) came from issue-20, a bot's feature request with no maintainer approval. The last two sentences came from issue-04 and issue-19. issue-04 was a one line issue ending in "etc." from a maintainer, and issue-19 listed "Additional suggestions" that the grader counted as required work. The evidence guide says to "grade the size of the work being asked for, not the polish of the writeup", so I wrote that idea into the check.

**Trade-offs**

The new scope wording changed issue-01. It passed in Run 2 but failed scope in Run 5, and its gold label is accept. The only change between those runs was the scope rewrite, so the rewrite changed its result. My guess is that rule (2) now catches some issues that are real, useful tasks but are opened by people who are not maintainers. I accept this miss because rule (2) catches unwanted feature requests like issue-20, and Run 5 still passed the bar at 19/20. I also used issue-04, issue-19, and issue-20 as canaries and re-ran them with `--only` in Run 4 (3/3) before the full run.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit and time.** Issue #61 is a FastAPI and SQLAlchemy bug, and those are tools I have used. My one past open source PR also fixed a broken health check, so I understand this kind of problem. The fix is in one file (api/routes/health.py), so it fits the time I have.

2. **What the verdict got right, and what I added.** The skill correctly found that the repo is active, nobody else has commented on or claimed the issue, there are no PRs for it, and there is no AI ban. What the rubric could not weigh is my own goal. #61 is the only one of my three picks with no test already written for it, so I will write the failing test myself, which is the skill I want to build. I also weighed that it is close to my past work. That makes it safer, but less of a stretch than #68.

3. **Difficulty in claiming.** Nobody has claimed it yet, so I will be the first to comment. I have not written a claim comment before, so I will need the Unit 2 voice guide to write a good one. Another classmate may claim it before me, but the house rule says that does not block me.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
