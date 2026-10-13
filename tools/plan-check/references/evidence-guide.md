# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives. Eval bundle: the plan's cause appears under
  "Candidate plan" (a "Cause:" or "Diagnosis:" line, or the first
  paragraph). The behavior it must explain is in "Repro evidence": the
  numbered steps, the artifact blocks, the "Control"/"Control runs"
  lines, and the "Expected"/"Actual" lines. Live: the cause is in the
  student's `plan.md`; the evidence is their posted repro comment on
  the issue (or the house repro pack quoted in the drafts).
- What good looks like: the cause names the code path that the failing
  run exercises, and every control run comes out the way that cause
  predicts. A control showing the same input succeeding through the
  blamed component, or the bad value already present before the blamed
  step runs, rules the cause out. That holds even when the thread or
  the issue title suggested that cause.

## Scope

- Where it lives. Eval bundle: the plan's "Scope"/"Change" paragraph
  (its "In:" and "Out:"/"Not in scope:" lines), the numbered approach
  steps, and any file list. Live: the scope and files sections of
  `plan.md`.
- What good looks like: one change at the site the evidence isolates,
  plus regression tests for it. Related work is pushed out of scope
  explicitly. A bounded plan can be a deliberately narrowed fix (one
  platform, one branch) with the rest deferred. It is unbounded when
  the approach steps add work the issue never asked for: dependency
  migrations, new options or settings panels, UI rework, module
  restructures, rewrites into a new architecture, or CI and harness
  changes.

## Executability

- Where it lives. Eval bundle: the approach steps and the file,
  function, or code path names inside the plan. Live: the files and
  approach sections of `plan.md`.
- What good looks like: a stranger could open the named file and start
  the first step without asking which layer, which option, or which
  file. Hedges such as "somewhere", "whichever is easier", "not sure
  which", or "profile first and decide" mean the decision has not been
  made yet.

## Test plan

- Where it lives. Eval bundle: the "Test"/"Test plan" paragraph, read
  next to the repro evidence's steps and "Expected" line. Live: the
  test plan section of `plan.md`, read against the steps in the
  student's repro comment.
- What good looks like: it names the repro re-run, or a regression
  test, and the specific result that proves the fix: an exit code, an
  output value, a color flip at a given step, a test that no longer
  panics. "Run the full suite", "nothing else should break", or
  "should feel fast" names no outcome for the fix itself.

## Honesty

- Where it lives. Eval bundle: a "Risk"/"Unknowns" line in the plan,
  hedges inside the approach, and any certainty words in the plan
  comment. Live: the risks and unknowns section of `plan.md` and its
  `## Deviations` section after the build.
- What good looks like: unknowns are named as unknowns, each with what
  will be done if it goes the wrong way. Confidence matches what the
  repro shows. A deviation found mid-build is written under
  Deviations, not left only in the diff.

## Comms

- Where it lives. Eval bundle: "Thread highlights" (each line carries
  the author's association in parentheses: OWNER, MEMBER, COLLABORATOR,
  CONTRIBUTOR, NONE) and the "Repo facts" block's "contribution policy"
  line, read against the "Candidate plan comment". Live: the issue
  thread with author associations, linked PRs, and the repo's
  CONTRIBUTING and AI policy files, read against the draft comment.
- What good looks like: where a maintainer isolated a culprit, proposed
  or rejected an approach, posted a patch, or pointed at a PR, the
  comment names that and says how the plan follows it, or why it
  departs. Where the repo's policy requires AI disclosure, the comment
  discloses AI use and its extent. A comment that could be pasted onto
  a thread with no maintainer input, when this thread has some, is
  boilerplate.
