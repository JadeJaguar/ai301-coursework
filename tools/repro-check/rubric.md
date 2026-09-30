# Rubric: is this reproduction package ready to post?

<!--
Each check tests one idea only, so a failed check names the exact
problem. Checks are grouped by the lecture's proof families: comms
(the claim comment), environment, steps, behavior shown, honesty, and
repo conventions. Checks 1, 2, and 10 read only the claim comment and
the repo facts, so they are the ones graded on a claim-only draft.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| claim-specific | The claim comment, read against the issue title and body | Pass if the claim names at least one detail from this issue that would not fit a different issue: the version, the command, the error text, the file, or the function. Fail if the claim could be pasted on any issue unchanged, for example "this issue looks good for me" or "+1". | required |
| claim-promises-next-step | The claim comment | Pass if the claim says what the author will do next (for example a repro report, a test, or reading a named code path) and makes no promise of a finished fix, a deadline, or a guaranteed result. Fail on "I will fix it in 2 days", "guaranteed", or a request to assign or reserve the issue with no next step. If the claim says the bug is already reproduced, the repro report must back that; on a claim-only draft, a "reproduced" claim fails because nothing backs it yet. | required |
| env-recorded | The environment line of the repro report | Pass if the report names the project version (or the commit or tag, when built from source) and the OS, plus any setting the issue or its thread says changes the failure (for example a driver, a build profile, a shell, or an install method). Fail if there is no environment record, or if a setting the issue says matters is missing. | required |
| env-matches-issue | The environment line of the repro report, read against the version and platform the issue targets | Pass if the tested version and platform match the issue, or the report says what differs. Fail if the report tested a different version or platform and does not say so. | required |
| steps-rerunnable | The steps in the repro report, read against the repo facts and the issue | Pass if a stranger with only this report and the public repo could reach the trigger: every command and input the result depends on is shown, and nothing depends on a private project, an unshared file, or an unnamed setting. Fail if a step says "run the command" without naming it, or relies on something the stranger cannot get. | required |
| trigger-matches | The commands and inputs in the repro report, read against the commands and inputs in the issue | Pass if the report keeps the part of the input the issue says causes the bug (the trigger), such as the same syntax, the same kind of input, or the same flag the issue or a maintainer names as required. Changes that do not touch the trigger (an output flag, an offline mode, a different file name, a shell alias) pass. Fail if the trigger itself was changed, for example a different operator, a different argument form, or an edited expression, unless the report says so and says why. | required |
| artifact-shown | The output excerpts, logs, or screenshots in the repro report | Pass if the report shows real output from the attempt: pasted terminal output, an error message, a log excerpt, or a screenshot. Fail if the report only describes the result in words, such as "it failed" or "I saw the bug". | required |
| behavior-matches | The artifacts in the repro report, read against the behavior the issue describes (error text, exit code, crash, or wrong output) | If the report says it reproduced the bug: pass only if the artifact shows the same behavior as the issue, not a nearby one. A different error message, a graceful validation error in place of a crash, or output showing the program still working is a fail. If the report says it could not reproduce the bug, this check passes and outcome-honest decides. | required |
| outcome-honest | The repro report's conclusion and the claim comment, read against the artifacts shown | Pass if the main outcome (reproduced, or could not reproduce) is backed by an artifact shown in the report, and no claim contradicts the artifacts or the issue thread. An honest cannot-reproduce passes when it shows the real attempt and names what may have differed. A side note described in words (such as a control run or a second try) does not fail by itself. Fail if the main outcome has no artifact behind it, if the report says "confirms" over an artifact that shows something else, if a root cause is stated as verified with no evidence, or if the report extends the bug to a setting the thread says does not show it. | required |
| ai-disclosure | The contribution policy line in the repo facts, read against the claim comment and the repro report | Treat every package as AI-assisted work. Pass if the policy states no AI rule, or its rules do not ask for disclosure in issues or comments. If the policy requires disclosing AI use in issues, in comments, or "in any form", pass only if the claim or the report says AI was used (with the tool and the extent, when the policy asks for them). Fail if the policy requires disclosure and neither comment discloses. | required |

## Verdict rule

Accept if every required check passes. Reject if any required check
fails. A check graded unclear counts as a fail. There are no preferred
checks, so nothing else changes the verdict. On a claim-only draft in
live mode, only claim-specific, claim-promises-next-step, and
ai-disclosure are graded; the other checks report "not yet applicable:
claim-only draft" and are left out of the verdict.
