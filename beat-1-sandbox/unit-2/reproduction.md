# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

JadeJaguar

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-5904800807

Hi, I'd like to work on this one. The DB probe in `api/routes/health.py` calls `db.execute("SELECT 1")` with a plain string, and the issue says SQLAlchemy 2.x rejects that with `ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')`.

Next I will set up the stack from the repo docs, call `GET /health`, and post a repro report here with my environment, steps, and the output I see. This is my first contribution to PathReview. I am keeping this to the SQL probe only, since the Redis settings bug is tracked in #62.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61#issuecomment-5905493651

Reproduced on current `main` against the Docker Postgres from `docker-compose.yml`. The health check reports Postgres as unhealthy with the exact `ArgumentError` from the issue, while the same database answers `SELECT 1` when it is wrapped in `text()`.

**Environment**

- OS: Windows 11 (build 10.0.26200), commands run in Git Bash as `docs/SETUP.md` says
- Code: my fork of `main` at commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`
- Python 3.12.10, SQLAlchemy 2.1.1, asyncpg 0.31.0, FastAPI 0.142.1 (installed by `make setup`)
- Docker 29.6.2, Docker Compose 5.3.1, `postgres:16-alpine` and `redis:7-alpine` both healthy
- `.env` copied from `.env.example` with no changes

**Steps**

```bash
git clone https://github.com/codepath/pathreview-ai301-fa26-s3.git
cd pathreview-ai301-fa26-s3
git checkout 2f4e82f52efbcfcc57d65b3fa5348672163ca088
cp .env.example .env
docker compose up -d
make setup
.venv/Scripts/uvicorn api.main:app --host 0.0.0.0 --port 8000
```

Then, in a second terminal:

```bash
curl -s -i http://localhost:8000/health
```

`make setup` ran cleanly for me on Windows (migrations and seed data both worked). If `scripts/seed_db.py` fails for you with a cp1252 encoding error, another comment in this thread fixed it by setting `PYTHONUTF8=1` first. On macOS or Linux, use `.venv/bin/uvicorn`.

**What I saw**

The response:

```
HTTP/1.1 503 Service Unavailable
x-request-id: f6deaa60-7e9b-4370-8ac2-bf5378076723

{"detail":{"status":"unhealthy","dependencies":{"postgres":"unhealthy","redis":"unhealthy","vector_db":"healthy"},"safety_events_last_hour":0,"timestamp":"2026-09-30T06:01:18.965778"}}
```

The API log for that same request:

```
2026-09-30 01:01:18 [error    ] postgres_health_check_failed   error="Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')" request_id=f6deaa60-7e9b-4370-8ac2-bf5378076723
2026-09-30 01:01:19 [error    ] redis_health_check_failed      error="'Settings' object has no attribute 'redis_host'" request_id=f6deaa60-7e9b-4370-8ac2-bf5378076723
INFO:     127.0.0.1:65166 - "GET /health HTTP/1.1" 503 Service Unavailable
```

**Control run**

To check that the database itself is reachable, I ran `SELECT 1` twice on the app's own session (`core.database.AsyncSessionLocal`), once wrapped in `text()` and once as a plain string. I ran it from the repo root by piping it into `.venv/Scripts/python -` in Git Bash. You can also save it as a file and run it with `.venv/Scripts/python`.

```python
import asyncio
from sqlalchemy import text
from core.database import AsyncSessionLocal

async def main():
    async with AsyncSessionLocal() as s:
        print("with text():", (await s.execute(text("SELECT 1"))).scalar())
        try:
            await s.execute("SELECT 1")
        except Exception as e:
            print("plain string:", type(e).__name__, "-", e)

asyncio.run(main())
```

Output (SQL echo lines trimmed):

```
with text(): 1
plain string: ArgumentError - Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')
```

**Expected:** `"postgres": "healthy"`, since the database is up and answers `SELECT 1`.

**Actual:** `"postgres": "unhealthy"`, because `db.execute("SELECT 1")` in `api/routes/health.py` raises `ArgumentError` under SQLAlchemy 2.1.1. The route catches it and marks Postgres unhealthy, which by itself makes the route return 503. (In my run Redis also failed, see the note below.)

**Notes**

- The Redis failure in the same response is a different error (`redis_host` missing on `Settings`). That is #62, and I am leaving it alone.
- The `vector-db` container exits on startup for me (`np.float_` was removed in NumPy 2.0, inside `chromadb/chroma:0.4.22`). This does not affect this bug. The health route only checks that `VECTOR_DB_URL` is set and never connects to it.
- Some earlier repros in this thread used SQLite. This one runs against the Postgres 16 service from `docker-compose.yml`.

Next I will write a unit test that calls the health route and fails on this `ArgumentError`, and share it here before changing `health.py`.

## Eval iterations

**Run history**

1. Run 1 (partial, `--limit 3`): 1/3. pkg-01 failed trigger-matches and pkg-03 failed outcome-honest. Both were false rejects. I loosened both checks.
2. Run 2 (full): 20/20, PASS. All categories matched: clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. The packages my loosened checks could have flipped (pkg-02, pkg-08, pkg-15, pkg-17) all still rejected.
3. Run 3 (full, saved with `--save-run eval-run.txt`): 19/20, PASS. All categories matched. clear-accept was 7/8, because pkg-03 flipped to reject on env-matches-issue with no change to my rubric or evidence guide.

**Package analysis**

pkg-03 (BurntSushi/ripgrep#2779). My rubric decided reject in Run 3. The gold label is accept.

The only check that failed was env-matches-issue. The issue was filed on ripgrep 13.0.0 on Kubuntu 23.10, installed with APT. The report tested ripgrep 15.2.0 on Arch Linux, installed with cargo. The report says the version differs ("The issue was filed against 13.0.0; behavior is unchanged on 15.2.0"), but it never says the OS and install method differ. My check says to pass only if "the tested version and platform match the issue, or the report says what differs." So the grader read it strictly. Its evidence line in `results.json` was: "Report notes the version differs from the issue's 13.0.0 but never mentions the OS/install-method mismatch (Kubuntu/APT vs. Arch/cargo)."

I think the gold label accepts it because the platform does not matter for this bug. It is a line number bug in the printer, and the owner's note in the thread says the trigger is `--replace` with adjacent multiline matches, not the OS. The same package passed in Run 2 with the same rubric, so pkg-03 sits right on the edge of my check. The grader can read "platform" either way.

**Check rationale**

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| trigger-matches | The commands and inputs in the repro report, read against the commands and inputs in the issue | Pass if the report keeps the part of the input the issue says causes the bug (the trigger), such as the same syntax, the same kind of input, or the same flag the issue or a maintainer names as required. Changes that do not touch the trigger (an output flag, an offline mode, a different file name, a shell alias) pass. Fail if the trigger itself was changed, for example a different operator, a different argument form, or an edited expression, unless the report says so and says why. | required |

This check started as Chandrashekar's input-matches check from the rubric swap worksheet. My first version said to fail if "the input or command was changed without saying so." In Run 1, that failed pkg-01. The issue ran `https post pie.dev/post -v 'header1: xyz' x=1`, and the report ran `http --offline post pie.dev/post 'header1: xyz' x=1`. The command changed, but the bug is caused by having exactly one custom header, and the report kept that part the same. `--offline` only prints the request without sending it. So I changed the check to judge the trigger (the part of the input that causes the bug), not the whole command line. I kept the fail cases from pkg-02 (a prefix range in place of offset-from-end syntax), pkg-08 (an edited expression), and calib-03 (a colon in place of `=`), because in those the trigger itself changed.

**Trade-offs**

env-matches-issue gives up pkg-03. I accept this miss on purpose. The check is strict about naming platform differences, and that strictness is what catches pkg-16, where the report tested pandas 1.5.3 against an issue confirmed on the latest version and never said so. The same check also passed pkg-11, whose report said it ran "on a different OS and install method." So the check works as written, and pkg-03 is the borderline case.

Before accepting the trade, I confirmed the cause instead of guessing. I read the pkg-03 entry in `results.json` (the evidence line quoted above), and I compared it with Run 2, where the same rubric passed pkg-03. That shows the flip came from the grader reading one borderline word two ways, not from an edit. Loosening the check to "only platform differences the issue says matter" might fix pkg-03, but it could also let pkg-16 through, and it would need a new confirming full run. So I kept the check as it is.

For the two checks I did loosen after Run 1 (trigger-matches and outcome-honest), I checked the packages that already agreed and could flip: pkg-02 and pkg-08 (wrong trigger) and pkg-15 and pkg-17 (claims not backed). All four still rejected in Run 2 and Run 3.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
