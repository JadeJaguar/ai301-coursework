# Plan for #61: health check DB probe passes a raw SQL string

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61
My repro: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-5905493651
Branch: `fix/61-health-check-sql-text` on my fork (JadeJaguar/pathreview-ai301-fa26-s3)

## Repro evidence this plan builds on

From my posted repro: Windows 11 (10.0.26200), Git Bash, commit
`2f4e82f52efbcfcc57d65b3fa5348672163ca088`, Python 3.12.10,
SQLAlchemy 2.1.1, asyncpg 0.31.0, FastAPI 0.142.1, Docker
`postgres:16-alpine` and `redis:7-alpine` both healthy, `.env` copied
from `.env.example` with no changes.

1. `curl -s -i http://localhost:8000/health` returned:

```
HTTP/1.1 503 Service Unavailable
x-request-id: f6deaa60-7e9b-4370-8ac2-bf5378076723

{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-09-30T06:01:18.965778"}}
```

2. The API log for that same request:

```
2026-09-30 01:01:18 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" request_id=f6deaa60-7e9b-4370-8ac2-bf5378076723
2026-09-30 01:01:19 [error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'" request_id=f6deaa60-7e9b-4370-8ac2-bf5378076723
```

3. Control run on the app's own session (`core.database.AsyncSessionLocal`),
   same database:

```
with text(): 1
plain string: ArgumentError - Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')
```

## Diagnosis

`api/routes/health.py` line 32 runs `await db.execute("SELECT 1")` with
a plain string. SQLAlchemy 2.x rejects a plain string with
`ArgumentError` before it uses the connection. The route's
`except Exception` catches that error, logs
`postgres_health_check_failed`, and marks Postgres `"unhealthy"`.

Every repro result fits this cause:

- Step 1 and step 2: Postgres is reported unhealthy, and the log for
  the same `request_id` names the exact `ArgumentError`.
- Step 3, the control: the same session and the same database return
  `1` when the SQL is wrapped in `text()`, and raise the same error
  when it is a plain string. So the database is reachable and the
  only thing that differs is the plain string.
- The Redis line in step 2 is a different error
  (`'Settings' object has no attribute 'redis_host'`). That is #62. It
  is not evidence about this issue.

## Scope

In scope:

- Wrap the probe in `text()` in `api/routes/health.py`, and import
  `text` from `sqlalchemy`.
- Add one new unit test file for the probe.

Not in scope:

- The Redis probe and `settings.redis_host`. That is #62, so `/health`
  will still return 503 after this fix, with `"redis":"unhealthy"`.
- The vector_db probe.
- The broad `except Exception` around the probe. Whether it should
  catch a code error is a separate question, not this bug.
- `pyproject.toml`. The mypy suppressions for `api.routes.health` stay
  as they are (see Risks and unknowns).
- `core/database.py`, and the `datetime.utcnow()` deprecation warning
  in the same file.

I checked for the same bug elsewhere: a search for `execute(` in
`api/`, `core/`, and `scripts/` finds `health.py` line 32 as the only
call that passes a plain string. Every other call passes `select(...)`
or a built statement.

## Files

- `api/routes/health.py`: one import line, and line 32.
- `tests/unit/test_health.py`: new file.

## Approach

1. In `api/routes/health.py`, add `from sqlalchemy import text`.
2. Change line 32 to `await db.execute(text("SELECT 1"))`.
3. Add `tests/unit/test_health.py`, following the pattern in
   `tests/unit/test_review_service.py` (an `AsyncMock` session,
   `@pytest.mark.unit`, `@pytest.mark.asyncio`). The fake session's
   `execute` runs SQLAlchemy's own statement check,
   `coercions.expect(roles.StatementRole, statement)`. This is the
   check that raises the `ArgumentError` inside `AsyncSession.execute`.
   So the test needs no database and no new dependency.
   - Test 1 calls the real `health_check` handler and asserts
     `dependencies["postgres"] == "healthy"`. The route still raises
     503 because of #62, so the test reads the body from the
     `HTTPException` detail and checks only the Postgres result.
   - Test 2 is a control: when `execute` raises `OSError`, Postgres
     must still read `"unhealthy"`. This shows the fix does not hide
     real database failures.
4. Run `make lint`, `make format`, `make typecheck`, and
   `make test-unit` before each commit.
5. Two commits, in this order: `test(api): add unit tests for the
   /health postgres probe`, then `fix(api): wrap the /health postgres
   probe in text()`.

## Test plan

1. **Re-run my Unit 2 repro on the branch.** Same commit base, same
   steps, same Docker stack:

```
.venv/Scripts/uvicorn api.main:app --host 0.0.0.0 --port 8000
curl -s -i http://localhost:8000/health
```

   Expected after the fix:
   - The body shows `"postgres":"healthy"`.
   - The log for that request has no `postgres_health_check_failed`
     line.
   - The response is still `503`, with `"redis":"unhealthy"` and the
     `redis_health_check_failed` line, because of #62. That 503 is
     expected and is not part of this fix.

2. **Re-run my control** on the app's session: it still prints
   `with text(): 1`. That shows the database is the same and
   reachable in both runs.

3. **New unit test, before and after.**
   `pytest tests/unit/test_health.py -v`:
   - Before the fix (test commit only): test 1 fails with
     `AssertionError: assert 'unhealthy' == 'healthy'`, and the
     captured log shows
     `postgres_health_check_failed error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')"`.
     Test 2 passes.
   - After the fix: both tests pass.

4. **Repo checks:** `make lint`, `make typecheck`, and `make test-unit`
   pass, with no new failures compared with `main`.

## Risks and unknowns

- **mypy suppressions.** CONTRIBUTING says fixing a seeded bug removes
  its suppression, and `pyproject.toml` links the
  `api.routes.health` entry to #62 "and related #61". In a dry run of
  this change in a scratch copy (Linux, Python 3.13, SQLAlchemy 2.1.3,
  `pip install -e ".[dev]"`), mypy showed no error on the probe line
  at all, before or after, because `db` has no type annotation. All
  three suppressed codes still fire on other lines after the fix:
  `index` on the `health_status` dict writes, `call-overload` on the
  `redis.Redis(...)` call, and `attr-defined` on `settings.redis_host`
  and `settings.redis_port` (#62). So none of them belongs to #61, and
  I will not change `pyproject.toml`. I will re-run `make typecheck` on
  my Windows setup during the build and record any difference under
  Deviations.
- **The test uses an internal SQLAlchemy module.**
  `sqlalchemy.sql.coercions` is not a public API, so a future
  SQLAlchemy release could move it. If a reviewer prefers, the
  fallback is a real `AsyncSession` on in-memory SQLite. That needs
  `aiosqlite` added to the dev extras, which is a bigger change, so I
  will only do it if asked.
- **Overlap with open PRs.** #82 makes the same `text()` change but
  also changes the Redis probe for #62, and adds no test. #96 and #99
  make the same `text()` change and add tests that run on SQLite; #99
  also adds `aiosqlite` to the dev extras in `pyproject.toml`. My change differs by adding no dependency and no
  `pyproject.toml` change, and by being built on my own Postgres repro
  over HTTP. If one of them merges first, I will rebase and keep only
  what still adds coverage, or close mine.
- **Not tried:** macOS or Linux on my own machine, `make run`, the
  Dockerfile image, and SQLAlchemy 2.0.x.

## Deviations

The code change held. I built exactly what the plan says, on branch
`fix/61-health-check-sql-text`, in the planned order:

- `34c7f43 test(api): add unit tests for the /health postgres probe`
- `65561ab fix(api): wrap the /health postgres probe in text()`

The diff is the one import line and the one changed line in
`api/routes/health.py`, plus the new `tests/unit/test_health.py`.
`pyproject.toml` is unchanged. The test failed before the fix with
`assert 'unhealthy' == 'healthy'` and the `ArgumentError` in the
captured log, and both tests passed after it. `make lint` passed, and
`make test-unit` gave 377 passed, 53 xfailed, 0 failed.

One step did not go as planned: `make typecheck`. The plan said it
would pass. On my Windows setup it stops before checking any project
code:

```
.venv\Lib\site-packages\numpy\__init__.pyi:737: error: Type statement is only supported in Python 3.12 and greater  [syntax]
```

The error is in numpy's own type file, not in Path Review. My venv has
numpy 2.5.3, whose type files use Python 3.12 syntax, while
`pyproject.toml` tells mypy to check as Python 3.11. I confirmed it is
not caused by my change: with my fix stashed, `make typecheck` on the
unchanged code fails with the same error. So I ran the same mypy
command with `--python-version 3.12`, which changes nothing in the
repo:

```
Success: no issues found in 76 source files
```

That confirms the dry run: the fix needs no change to the mypy
suppressions. I did not edit `pyproject.toml` to work around the numpy
error, because that is outside this issue. CI runs on Python 3.11, so I
expect the typecheck job to install a numpy that works there; I will
check the CI result on the PR in Unit 4.

The posted plan comment is still true, so I am not posting a
correction. I will post the before and after output on the issue, as
the comment promised.
