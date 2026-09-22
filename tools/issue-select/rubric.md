# Rubric: is this a good first issue?

Five required checks. Four are the families from the lecture: the
maintainer is alive, the repo is in use, the scope fits a newcomer, and
nobody else is already on it. The fifth is the contribution-policy
surface, because a repo that bans AI-assisted work rejects this workflow
before a maintainer reads a line of the code. Three preferred checks
never change a verdict; they rank the issues that survive.

Every recency threshold is measured against the capture date stamped at
the top of the bundle in eval mode, and against today in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `maintainer-alive` | Repo facts block: the `last 5 default-branch commits` list, reading the author name and message of each, and the `maintainer first-response sample` list | Both hold. First, at least 1 of the 5 commits has an author name not ending in `[bot]`, or is a bot merge whose message names a pull request from a contributor or branch that is not itself a bot account. Second, at least 1 sampled issue records a first owner, member, or collaborator response as a number of days rather than `no maintainer comment in thread` | required |
| `repo-in-use` | Repo facts block: the `archived:` flag on the repo line, and the `last push to any branch` date | Archived is no, AND the last push is within 180 days of the capture date | required |
| `scope-bounded` | The issue title, body, and labels, plus every comment in the Comments section | The issue asks for one concrete change, AND none of the following appear: a checklist of sub-items presented as an umbrella or tracking issue, a request for help using the project rather than changing it, a maintainer comment saying the change needs core or internal rework, or a design disagreement in the thread that no maintainer has settled | required |
| `unclaimed` | Repo facts block: the `this issue: assignees` and `linked PRs` fields, plus every comment in the Comments section | Assignees is none, AND no linked pull request is open, AND no comment within the 60 days before the capture date claims the work, unless a later comment or a closed unmerged pull request shows that attempt was dropped | required |
| `ai-policy-allows` | Repo facts block: the `contribution policy` line | The line states no AI restriction, OR permits AI-assisted work subject to conditions such as disclosure, human review, testing, or the contributor understanding the change. Fails only when AI-assisted contribution is prohibited outright with no complying path | required |
| `good-first-signal` | The issue's `labels:` list on the opened-by line | Labels include `good first issue`, `good-first-issue`, `help wanted`, or another newcomer-facing label | preferred |
| `release-recent` | Repo facts block: the `latest release` line | A release is dated within 365 days of the capture date | preferred |
| `maintainer-in-thread` | The Comments section: each comment's author and its author_association | At least 1 comment is from an OWNER, MEMBER, or COLLABORATOR | preferred |

## Verdict rule

Accept if all five required checks grade `pass`. Reject if any required
check grades `fail`.

Preferred checks never change the verdict. Report their grades, and on an
accepted issue use them to rank it against the other accepted candidates.

`unclear` on a required check counts as `fail`: a first issue whose
evidence cannot be verified is not a first issue worth taking. Note that
`scope-bounded`, `unclaimed`, and `ai-policy-allows` pass on the absence
of a disqualifying signal, so absent evidence is a pass on those three,
not an `unclear`. Grade `unclear` only when the bundle omits a fact a
check names outright, such as a missing repo-facts line.
