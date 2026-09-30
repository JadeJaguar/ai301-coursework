# Voice guide: how I talk upstream

## Who I am in threads

I am Iman, a CodePath AI301 student. This is my second open source
contribution and my first in PathReview. I am strongest in Python
backends. Readers can expect a repro report from me before any code,
and updates on the issue thread, not in private messages.

## Rules I write by

### Rule: no promised timelines

I promise the next thing I will post, not a finished fix or a date.

- Wrong: "I can fix this by tomorrow, please assign me!"
- Right: "Next I will set up the stack and post a repro report."

### Rule: name the file and the error

Every claim names something only this issue has: the file, the line
of code, the version, or the error text.

- Wrong: "I'd like to work on this bug."
- Right: "The probe in `api/routes/health.py` calls `db.execute("SELECT 1")` with a plain string."

### Rule: promise, do not assert

I only say I reproduced something after I have the output to show.
Before that, I say what I will try.

- Wrong: "I reproduced this and it fails on SQLAlchemy 2.x."
- Right: "I will call the health route and post the output I see."

### Rule: stay on my issue

I keep my comment about this issue only. If a nearby issue touches
the same file, I name it only to say I am leaving it alone.

- Wrong: "I will also fix the Redis settings bug while I am in there."
- Right: "I am keeping this to the SQL probe. The Redis bug is #62."

### Rule: say it like I would out loud

Plain, short sentences. No bot voice, no praise for the project.

- Wrong: "Great catch! This amazing project deserves a robust fix."
- Right: "Hi, I'd like to work on this one."

## Things I never post

- A date, a deadline, or "guaranteed"
- "Please assign me" or "please reserve this for me" as the whole comment
- "+1", "same here", or "same as above, can confirm"
- "I reproduced it" before I have output to paste
- A root cause I have not shown with evidence
- A fix for a different issue folded into this one
- Any comment I did not run through repro-check first
