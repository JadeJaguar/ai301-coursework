# Evidence guide: where proof lives in a reproduction package

<!--
The map for rubric.md. Each heading says where to look (in an eval
bundle and in live mode) and what good looks like there. Always read
the issue first, then the claim comment, then the repro report.
-->

## Environment

**Where it lives.** In an eval bundle: the "Environment" line near the
top of the "Candidate repro report" section. Compare it with the
version, OS, and setup named in the "Issue" section and the thread
highlights, and with what the "bug reports" line in "Repo facts" says
the template asks for. In live mode: the environment line of the
student's repro draft, compared with the issue body on GitHub and the
repo's bug report template.

**What good looks like.** The line names the project version (or the
commit or tag, if built from source) and the OS. It also names any
setting the issue or thread says changes the failure, such as a
driver, build profile, shell, or install method. The version and
platform match the issue, or the report says what differs.

## Steps

**Where it lives.** In an eval bundle: the numbered steps and the
command blocks (lines starting with `$`) inside the "Candidate repro
report". In live mode: the steps section of the repro draft.

**What good looks like.** Every command and input file the result
depends on is written out, so a stranger could copy them and reach
the trigger. The input and command are the same as the issue's, or
the change is named with a reason. Nothing depends on a private
project, an unshared config, or a setting the report never names.

## Behavior shown

**Where it lives.** In an eval bundle: the output blocks, log excerpts,
and screenshot descriptions in the "Candidate repro report". Read
them next to the error text, exit code, or output quoted in the
"Issue" section. In live mode: the output blocks in the repro draft,
next to the output in the issue body.

**What good looks like.** The artifact is real output from the run,
not a description. It shows the same behavior as the issue: the same
error message or panic, the same exit code, the same wrong output. A
different error (for example a syntax error in place of a panic), a
clean validation message in place of a crash, or output that shows
the program still working is an adjacent behavior, not the issue's.
A control run (the same steps with the trigger removed) is a strong
sign, but it is not required.

## Honesty

**Where it lives.** The conclusion lines of the repro report (often
"Analysis", "Actual", or the last paragraph) and any sentence in the
claim comment that says what was found. Read each claim next to the
artifacts shown in the report.

**What good looks like.** The stated outcome matches the artifacts.
Words like "confirms", "verified", "deterministic", or "also on
version X" each have a shown artifact behind them. An honest
cannot-reproduce is a good outcome: it shows the real attempt, says
plainly that the bug did not appear, and names what may have differed
(OS, shell, sizes, config). A report that sounds confident over the
wrong artifact, or claims tests it does not show, is not honest.

## Comms

**Where it lives.** The "Candidate claim comment" section, read
against the "Issue" section. The "contribution policy" line in "Repo
facts" for AI rules and templates. In live mode: the student's claim
draft, the issue page, and the repo's CONTRIBUTING and AI policy
files.

**What good looks like.** The claim names something only this issue
has (a version, a command, an error, a file, or a function). It says
the next concrete step and does not promise a fix, a date, or a
guaranteed result. It does not say the bug is reproduced unless the
report shows it. For AI rules, treat every package as AI-assisted: if
the contribution policy requires disclosure of AI use in issues,
comments, or "in any form", then the claim or the report must say AI
was used, with the tool and extent when the policy asks. A policy that
only asks for disclosure in pull requests, or only asks that comments
be in the author's own words, does not require disclosure in these
comments. "No stated AI policy" means no disclosure is needed.
