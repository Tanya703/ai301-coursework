---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request (its title,
description, commit list, unified diff, and test evidence), read
against the plan it claims to implement (scope, file list, test plan,
and any deviation notes), the issue that plan belongs to, and the
repo's stated standards (PR template, contributing guide, AI policy).
Answer only that question, for only that one package. Do not review
the code's style, suggest a better fix, re-grade the plan itself, or
grade a second package in the same run. You do not answer from gut
feel: you execute `procedure.md`, which applies `rubric.md` to
evidence located with `references/evidence-guide.md`.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (a markdown
file titled `Eval package: <id>`), you are in eval mode. If you were
asked to grade the student's own draft PR, you are in live mode.

- **Live mode**: the student's own submission, checked before it is
  opened. Read, from the top folder of the student's Path Review clone,
  on their branch:
  - `plan.md`, including its `## Deviations` section if present: the
    plan the PR claims to implement.
  - The branch's diff: run `git diff main...HEAD` (three dots) and
    `git log --oneline main..HEAD` from the working copy. That is the
    diff and commit list being graded. Uncommitted changes are not
    part of the PR; do not grade them, but mention them in the summary
    if `git status` shows tracked files modified.
  - `pr_draft.md`: its first line is the PR title, the rest is the
    PR description.
  - `test_evidence.md`: the captured repro before/after and the output
    of the repo's checks.
  - The issue: the URL the student gave. Read the issue body and its
    thread (with `gh issue view <url> --comments`, the GitHub API, or
    the web), noting each commenter's association.
  - The repo's stated asks: `.github/PULL_REQUEST_TEMPLATE.md` and
    `docs/CONTRIBUTING.md` in the working copy (these are the
    repo-facts block of an eval bundle). The course also requires an
    AI-use disclosure on every PR, in the description's Notes for
    Reviewers section; treat that as a stated policy.
  - A house-chain student reads the house plan and the house repro pack
    in place of their own plan and repro; the same checks grade the
    same things.
  Do not read other files in the working directory as part of the
  package (for example `comment.md`): the package is what the reviewer
  will see plus the plan it is held against.
- **Eval mode**: the bundle is the whole world. Every fact comes from
  the bundle text: its repo facts, issue, thread highlights, plan
  context, and candidate PR. Fetch nothing, run no commands against
  any repository, and ignore the live issue named on the `source:`
  line. Eval mode always grades a complete package: every check in the
  rubric, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. If its `Repo:` line still holds a bracketed placeholder (such as
`<ORG>/<PATH-REVIEW-REPO>`), stop without grading and tell the student
to fill the `Repo:` line in `scope.md` with their section's Path Review
repo. Otherwise, confirm the issue URL belongs to that repo and that
the working copy's `upstream` (or `origin`) remote points at that repo
or a fork of it; if not, refuse to grade and say which repo is in
scope. Never guess a scope. Apply the house rules as reading rules:
the branch must come from the student's fork, the PR delivers one
plan for one issue, and every template section gets real content.
Report a branch name that lacks a type prefix (`fix/`, `docs/`,
`feat/`, `test/`, `refactor/`, `perf/`, `chore/`), the issue number,
and a short description in the summary. In eval mode, ignore
`scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after scope. Hold the PR title and
description from `pr_draft.md` against each rule in it, and report
every rule the draft breaks in the summary, quoting the rule's name
and the offending line. The voice guide is the student's personal
style; it never changes the verdict on its own, because no rubric
check reads it. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order, before grading:

1. `rubric.md`: the table of checks (name, evidence, pass condition,
   weight) and the verdict rule. It decides WHAT passes.
2. `references/evidence-guide.md`: where each evidence family lives in
   a bundle and in live mode, and what good looks like. It decides
   WHERE to look.
3. `procedure.md`: the read order, evidence gathering, check execution,
   and verdict assembly. It decides HOW. Execute it as written, step
   by step.

Where the procedure is silent on something you need, note the gap in
your summary as "procedure gap: ..." and grade from the rubric's pass
condition alone; do not invent a step. If `rubric.md` has no filled
check rows or no verdict rule, or `procedure.md` has no filled steps,
stop and say that the tool cannot grade without a rubric and a
procedure. Do not invent checks at runtime.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means
hold. There is no third verdict and no score; put reservations in the
check evidence lines. Before the JSON, print a short readable summary:
one line per check with its grade, then (live mode) any scope and voice
notes and any procedure gaps. End your reply with the fenced JSON block
below, filled in, valid JSON, one entry per rubric check in rubric
order, using the rubric's check names. Nothing may follow it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In eval mode `item` is the bundle id (for example `pkg-07`). In live
mode, before the PR exists, `item` is the issue URL.

## Grading discipline

- Evidence first: never grade a check without naming the fact or
  quoting the line that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff against the plan,
  the evidence against the test plan, and the description against the
  diff. A terse, complete PR can be ready; a long, confident one can
  hide drift. The description's own claims ("exactly as planned", "no
  other changes") are claims to verify, never evidence.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it passes. Note the tension in the
  summary; the fix belongs in the rubric.
- The procedure decides how, not you: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Honest shortfalls are not failures: a PR that does less than the
  whole issue, or differs from the plan, and says so in its plan
  deviation notes or description is graded on what it claims.
- Unclear defaults to fail: treat `unclear` as the rubric's verdict
  rule directs; where it is silent, an unverifiable claim is a failing
  one.
