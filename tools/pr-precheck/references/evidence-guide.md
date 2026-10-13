# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- Where it lives. Eval bundle: the plan is the "Plan context" block,
  the paragraph starting "Plan:": its "Scope, one bounded change"
  sentence, its approach, "Not in scope:", "Files:", any "Known limit"
  or "Deliberately deferred" sentence, and any deviation note. The
  change is the "### Diff" block under "Candidate PR" (one `+++ b/`
  header per changed file, then its hunks); the fidelity claims are in
  "### Title" and "### Description". Live: `plan.md` (scope, files,
  approach, not-in-scope, and its `## Deviations` section) against
  `git diff main...HEAD`, and the title and description in
  `pr_draft.md`.
- What good looks like: every `+++ b/` file is one the plan names, a
  test file for the fix, or a minimal supporting edit; every hunk does
  what the plan's approach says and nothing the not-in-scope line
  excludes. Everything the plan promised (including a docs or changelog
  update) appears in the diff, or the plan or description says it was
  deferred and why. Drift in the "more" direction looks like a new
  option, flag, setting, rename pass, or rewrite riding along; in the
  "less" direction, a promised piece missing while the description
  says it is done. A description saying "exactly as planned" over
  either one is the tell.

## Test evidence (harness category: not-tested)

- Where it lives. Eval bundle: the "### Test evidence" section (and any
  Before/After blocks in the "### Description"), read against the
  plan's "Test plan:" sentence and the "Repro evidence" line in "Plan
  context". Live: `test_evidence.md` (the unit-2 repro re-run on main
  and on the branch, plus the output of `make test-unit`,
  `make test-integration`, `make lint`, `make typecheck`) against the
  test plan in `plan.md`.
- What good looks like: the same failing input the repro used, run on
  the branch, with the after output shown and different from the
  before ("before: len == 99999 / after: 100000"; "before: exit 101
  panic / after: exit 2 argument error"). Every failing scenario the
  test plan names is covered. The repo's suite or checks are named with
  a result ("pytest ... 2841 passed"), and a failing or empty check is
  shown with its reason rather than left out. "Tested locally",
  "verified working for a day", or "tests pass" alone is not evidence;
  neither is a re-run of a control or of a different code path.

## Diff quality (harness category: unreviewable)

- Where it lives. Eval bundle: the "### Diff" block's added (`+`) lines
  and the "### Commits" list. Live: `git diff main...HEAD` and
  `git log --oneline main..HEAD`.
- What good looks like: every added line is the fix, its test, or a
  comment explaining the fix. Debris tells: `print`/`eprintln!`/
  `console.log`/`DEBUG` lines; `// let x = ...` or `# old_call()` kept
  as comments; functions nothing calls, or `#[allow(dead_code)]` and
  `_unused` names keeping them alive; TODOs on unrelated lines;
  re-indent, whitespace, or import-reordering hunks; identical lines
  removed and re-added; edits in files the change has no reason to
  touch. Commits titled "wip", "fmt + cleanup", or "misc cleanups while
  debugging" usually point at the debris.

## Standards and comms (harness category: standards-wall)

- Where it lives. Eval bundle: the "Repo facts" block, the "pull
  requests:" line (template sections, checklist items, required lines
  and entries) and the "contribution policy" line (AI policy), read
  against "### Title", "### Description", and the diff's file headers.
  Live: `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with
  `Closes #`, Changes, Testing checklist, Screenshots / Demo, Notes for
  Reviewers) and `docs/CONTRIBUTING.md`, plus the course rule that the
  AI-use disclosure goes in Notes for Reviewers, read against
  `pr_draft.md`.
- What good looks like: each template section appears with content
  about this change; checklist items are addressed truthfully (the CI
  box left unticked until CI runs); the required issue line carries
  the number ("Closes #68", "fixes #23471"); a required changelog or
  whatsnew entry shows up as a file in the diff. Where the policy
  requires AI disclosure, the description says AI was used, which tool,
  and how far, in the author's words. Where the repo has no template
  and no AI obligation, a plain description that references the issue
  is compliant. Boilerplate left in a section, a missing checklist, or
  no disclosure under a "must disclose" policy is the wall.
