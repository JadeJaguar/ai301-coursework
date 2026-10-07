# Unit 3: Plan and Build

---

## Posted upstream

**GitHub username**

JadeJaguar

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-6032580971

Text as posted:

My plan for #61, built from my repro here: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-5905493651 (Docker Postgres, GET /health over HTTP).

Cause: `api/routes/health.py` line 32 passes the plain string `"SELECT 1"` to `db.execute()`. SQLAlchemy 2.x rejects it with `ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')`, and the route's `except Exception` marks Postgres unhealthy. My control run fits this: the same session returns `1` for `text("SELECT 1")` and raises the same error for the plain string, so the database is fine.

Change: import `text` from `sqlalchemy` and use `await db.execute(text("SELECT 1"))`. It is the only plain-string `execute()` call in `api/`, `core/`, and `scripts/`. I will also add `tests/unit/test_health.py`, using the repo's `AsyncMock` session pattern. Its fake `execute` runs SQLAlchemy's own statement check, so I expect it to fail on main (the captured log will show this exact `ArgumentError`) and pass after the fix, with no new dependency. As I said in my repro, the test comes first: I will commit it and show it failing before I change `health.py`.

Not touching: the Redis probe (#62), the vector_db probe, the broad `except Exception`, and `pyproject.toml`. A dry run of the change shows that none of the three mypy codes suppressed for `api.routes.health` comes from the probe line. All three still fire on the Redis and dict code after the fix, so they stay.

Test: re-run my repro. I expect `"postgres":"healthy"` and no `postgres_health_check_failed` line in the log. The response will still be 503 with `"redis":"unhealthy"` until #62 is fixed. Plus `make lint`, `make typecheck`, and `make test-unit`.

Open PRs: #82 also changes the Redis probe and has no test. #96 and #99 make the same `text()` change with SQLite-based tests, and #99 adds `aiosqlite` to the dev extras in `pyproject.toml`. Mine adds no dependency and no `pyproject.toml` change. If one of them merges first, I will rebase and keep only what still adds coverage, or close mine.

I am using Claude Code to help with the edits, and I read every diff myself. Next I will post my before and after output here.

---

## Your branch

**Branch**

fix/61-health-check-sql-text

**Evidence**

All runs are on Windows 11 (Git Bash), Python 3.12.10, SQLAlchemy 2.1.1, in my fork on branch `fix/61-health-check-sql-text`. The "before" unit test ran on the test commit `34c7f43` alone; the "after" runs are on the fix commit `65561ab`. The "before" repro is the output I posted in Unit 2; the "after" repro is the same steps against the same Docker stack (`postgres:16-alpine`, `redis:7-alpine`).

**Unit test, before the fix** (test commit only; warnings trimmed): `pytest tests/unit/test_health.py -v`

```
tests/unit/test_health.py::TestHealthCheck::test_postgres_healthy_when_session_accepts_the_probe FAILED [ 50%]
tests/unit/test_health.py::TestHealthCheck::test_postgres_unhealthy_when_database_call_fails PASSED [100%]
E   AssertionError: assert 'unhealthy' == 'healthy'
---------------------------- Captured stdout call -----------------------------
2026-10-07 01:58:37 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"
```

**Unit test, after the fix:**

```
tests/unit/test_health.py::TestHealthCheck::test_postgres_healthy_when_session_accepts_the_probe PASSED [ 50%]
tests/unit/test_health.py::TestHealthCheck::test_postgres_unhealthy_when_database_call_fails PASSED [100%]
======================== 2 passed, 3 warnings in 1.36s ========================
```

**My repro, before** (from my repro comment): `curl -s -i http://localhost:8000/health`

```
HTTP/1.1 503 Service Unavailable
x-request-id: f6deaa60-7e9b-4370-8ac2-bf5378076723

{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-09-30T06:01:18.965778"}}
```

Server log for that request:

```
2026-09-30 01:01:18 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" request_id=f6deaa60-7e9b-4370-8ac2-bf5378076723
```

**My repro, after:**

```
HTTP/1.1 503 Service Unavailable
x-request-id: 80ce9d51-4201-4d0d-a637-8f00fb42b1ef

{"detail":{"status":"unhealthy","dependencies":{"postgres":"healthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-10-07T07:13:17.928418"}}
```

Server log for that request:

```
2026-10-07 02:13:17,931 INFO sqlalchemy.engine.Engine SELECT 1
2026-10-07 02:13:17 [debug    ] postgres_health_check_passed   request_id=80ce9d51-4201-4d0d-a637-8f00fb42b1ef
2026-10-07 02:13:18 [error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'" request_id=80ce9d51-4201-4d0d-a637-8f00fb42b1ef
2026-10-07 02:13:18 [debug    ] vector_db_health_check_passed  request_id=80ce9d51-4201-4d0d-a637-8f00fb42b1ef
INFO:     127.0.0.1:64291 - "GET /health HTTP/1.1" 503 Service Unavailable
```

Control on the same database after the fix (the same script as my Unit 2 control, run from the repo root with `.venv/Scripts/python -`):

```
with text(): 1
plain string: ArgumentError - Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')
```

Repo checks after the fix:

```
$ make lint
.venv/Scripts/ruff check .
All checks passed!

$ make test-unit
================ 377 passed, 53 xfailed, 5 warnings in 12.65s =================

$ make typecheck
.venv\Lib\site-packages\numpy\__init__.pyi:737: error: Type statement is only supported in Python 3.12 and greater  [syntax]
Found 1 error in 1 file (errors prevented further checking)

$ .venv/Scripts/mypy api/ core/ ingestion/ rag/ agent/ safety/ --python-version 3.12
Success: no issues found in 76 source files
```

The `make typecheck` error is in numpy 2.5.3's own type file and happens the same way on unchanged code (checked with `git stash`), so I ran mypy with `--python-version 3.12`. This is recorded under Deviations in `plan.md`.

---

## Eval iterations

**Run history**

1. Partial run, 5 packages (pkg-02, pkg-04, pkg-09, pkg-14, pkg-20): 4/5. pkg-09 failed `thread-engaged`.
2. Partial run, pkg-09 alone with `--out`, to confirm the cause: 0/1. The grader's evidence: "the comment names PR #2089 and options 1/2 but never names #1606 or the third variant, and an existing fix is engaged only by naming it."
3. Partial run, canaries after changing the "engages" definition (pkg-09, pkg-04, pkg-20, pkg-03, pkg-08): 5/5.
4. Full run: 19/20 PASS. pkg-14 failed `diagnosis-grounded`.
5. Partial run, canaries after changing conflict type 2 (pkg-14, pkg-01, pkg-07, pkg-11, pkg-16, pkg-02): 6/6.
6. Full run: 20/20 PASS. This is the run in `eval-run.txt`.

**Package analysis**

pkg-14 (zellij-org/zellij#5174, category clear-accept). Gold label: accept. My rubric said reject in full run 4 and accept in full run 6.

In run 4 it failed `diagnosis-grounded`. The grader's evidence was: "control 2 (after rm -rf ~/.cache/zellij the next attach is clean) shows no leak with the named cause still present and same trigger (conflict type 2); the plan's cache explanation is unsupported by any repro step". My conflict type 2 then said a conflict is when "the failure does not happen in a run where the stated cause is still present with the same code, input, and trigger." The cache-clearing control changes saved state, and the plan explains it ("with an empty cache the color data is refetched along the fresh-attach path once"). But "same" was not defined, so the grader treated the cache run as the same trigger. pkg-14 had passed the same check in run 1, so this was one rule read two ways, the same problem my Unit 2 rubric had with "platform" on pkg-03. I also saw that the grader added its own reason ("unsupported by any repro step") that is not in my rubric.

I rewrote type 2 to say what kind of change counts, and added a line that only the five listed types are conflicts. In run 6 pkg-14 passed `diagnosis-grounded` and was accepted, matching gold.

**Check rationale**

The check, as it reads in `tools/plan-check/rubric.md`:

```
| diagnosis-grounded | The plan's stated cause, read against every step and every control result in the Repro evidence block | No repro result **conflicts** with the stated cause. | required |
```

and the definition of conflict type 2 it uses:

```
2. The failure goes away in a run that changes only things the stated
   cause does not depend on. A run that changes the version, the
   configuration, cached or saved state, the input, the flags, or the
   environment counts here only if the stated cause gives no role to
   the thing that changed.
```

It reads this way because of my Unit 2 feedback: the rule itself stays one short sentence, and the word that can be read two ways ("conflicts") gets an exact list in the Definitions section, with examples kept in the evidence guide. The worksheet's first version said a diagnosis passes "if the plan says what causes the bug", which let calib-03's wrong cause through, so the check reads the cause against every repro step and control. Type 2 was first written as "the same code, input, and trigger". I replaced it after pkg-14 (see Package analysis), because "same" was undefined. The new wording lists the kinds of change (version, configuration, cached or saved state, input, flags, environment) and says the run only counts when the cause gives that change no role.

**Trade-offs**

Loosening type 2 could let a wrong cause through, so before the confirming full run I re-ran canaries with `--only`: all four wrong-cause packages (pkg-01, pkg-07, pkg-11, pkg-16) plus pkg-02 and pkg-14. All six agreed, and I checked `canary2.json` to make sure each wrong-cause package was still failed by `diagnosis-grounded` itself, not only by another check: pkg-01, pkg-07, pkg-11, and pkg-16 all graded `fail` on it, and pkg-02 and pkg-14 graded `pass`. Full run 6 then agreed on all 20.

What the check still gives up: it only tests the plan's cause against the repro evidence, so it cannot catch a wrong fact in the plan comment about other work. My live run showed this. plan-check accepted my first draft, which said PR #99 "edits the mypy list", and only its summary flagged that this was wrong (#99's only `pyproject.toml` change is adding `aiosqlite`). No check in my rubric reads other PRs' diffs. The procedure also has no step for live mode's missing "Repo facts" block; the skill used the evidence guide's live-mode map instead. I left both gaps alone because `rubric.md`, `procedure.md`, and `evidence-guide.md` are frozen by the fingerprints in `eval-run.txt`.
