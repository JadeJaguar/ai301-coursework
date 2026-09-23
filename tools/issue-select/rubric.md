# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | Repo facts: "last 5 default-branch commits" (dates and authors) and "maintainer first-response sample"; Comments section: author_association and date of each comment | At least one of these is within 60 days of the capture date: a commit by a human, a bot commit that merges a human's PR, or a comment by an OWNER, MEMBER, or COLLABORATOR. Commits by bots alone do not count. | required |
| repo-in-use | Repo facts: "archived:" on the repo line, "last push to any branch", "latest release" | archived is false, AND either the last push or the latest release is within 180 days of the capture date. If archived is true, fail no matter what else. | required |
| scope-fits-newcomer | Issue body; who opened the issue and their role (author_association); labels; Comments section; "linked PRs:" under Repo facts | Fail if any of these is true: (1) the issue is an umbrella or tracking issue (a list of separate tasks meant to be split into other issues; one task that touches several files, with the change to each file spelled out, is not an umbrella issue); (2) the issue asks for a new feature, it was not opened by an OWNER, MEMBER, or COLLABORATOR, and no one with those roles has agreed to it in the comments; (3) the thread shows the design is still being debated and no maintainer has settled it; (4) a maintainer says outright that the fix touches core internals; (5) the issue is a usage question ("how do I get this to work?"); (6) there are 2 or more closed, unmerged PRs for it. Otherwise pass. Grade only the work the issue requires: optional "suggestions" or "ideas" are not required work. A short body, a list that ends in "etc.", or a missing repro does not fail this check, especially when a maintainer opened the issue or it has a good first issue label. | required |
| nobody-else-on-it | Repo facts: "this issue: assignees:" and "linked PRs:"; Comments section: PRs mentioned and claim comments with their dates | No assignee, no open PR (linked or mentioned in a comment), and no claim comment ("I'll take this", "working on this") within 30 days of the capture date. An older claim with no PR and no later update does not block. If the sidebar and the thread disagree, trust the thread. | required |
| ai-policy-allows | Repo facts: "contribution policy" line | Fail only if the policy bans AI-generated contributions outright. Conditions (disclose AI use, test your change, understand every line) pass. No policy stated also passes. | required |
| newcomer-label | Issue labels | The issue has a "good first issue" or "help wanted" label | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails. A check is unclear only when the evidence it names is missing from the bundle; treat unclear as a fail. Preferred checks never change the verdict; they only help rank the issues that are accepted. All dates are measured against the bundle's capture date, not today.