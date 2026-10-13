# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

TODO: paste the PR's URL from codepath/pathreview-ai301-fa26-s3 after opening it
(https://github.com/codepath/pathreview-ai301-fa26-s3/pull/<number>)

**Branch**

`fix/68-empty-keyword-index`

## Eval iterations

**Run history**

1. Partial smoke run, `--only pkg-01,pkg-09,pkg-11,pkg-13,pkg-18` (the two packages the
   gold notes call arguable, two honest-shortfall/no-template accepts, and one
   standards-wall package): 5/5. Partial, so it was not saved and has no bar result.
2. Full run #1: crashed before producing a score. On Windows the harness wrote the prompt
   to `claude`'s stdin as cp1252, and one package contains box-drawing characters, so it
   raised `UnicodeEncodeError` and no `eval-run.txt` was written. Nothing in the skill
   changed after this.
3. Full run #2, the same files re-run with `PYTHONUTF8=1`: **agreement: 20/20 scored
   items (bar: 18/20: PASS)**, categories `clear-accept 7/7  not-tested 4/4
   silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This is the run in
   `eval-run.txt`.

**Package analysis**

`pkg-13` (neovim/neovim#41337, `vim.ui.open()` on Windows). My rubric: **accept**. Gold
label: **accept** (clear-accept, marked "arguable on the deferral").

The PR does less than the whole issue. It caret-escapes `& ^ | < >`, but `%VAR%`
expansion still happens. A rubric that equates "less than everything" with "hold"
rejects it. Mine passed it on `diff-matches-plan`, because that check's second direction
is "Nothing silently missing", and the omission is not silent. The plan's own "Known
limit, stated in the plan: `%` ... is NOT covered by this change", and the description
restates it as a deviation note. The run's evidence line reads: "`%` deferral disclosed
in plan's Known Limit and restated in description; no extra hunks; no missing plan
items." `description-matches-diff` also passed, because the description claims exactly
what the diff does ("claim '% not handled' confirmed by absence of any % logic in
diff"). Evidence passed because the plan's one failing scenario (`?q=hello&t=brave`) is
re-run with exit 1 → exit 0 and the full URL, and the plan lets the no-`&` control be
stated unchanged. The template asks (Problem and Solution) are filled. So every required
check passed, and the verdict rule gives accept.

**Check rationale**

> | `evidence-is-decisive` | The test-evidence section (and any before/after in the description), read against the plan's test plan and the repro evidence's failing steps (live mode: `test_evidence.md` against `plan.md`'s test plan and the unit-2 repro) | The evidence re-runs the plan's repro (the same failing input or scenario the issue and plan name) on the branch, and shows an observable after result (output, exit code, value, rendered state, test assertion on the fixed behavior) that differs from the before. The before may come from main or from the plan's repro evidence. When the plan's test plan names more than one failing scenario, each one is shown re-run; controls the plan expects to stay unchanged may simply be stated unchanged. Fails when the evidence is a claim without output ("tested locally", "works on my machine", "verified working for a day"), only a suite result ("tests pass") with no repro re-run, only a control or a different input that never exercised the bug, or a named failing scenario of the plan left un-run. | required |

It reads this way because "not tested" shows up in four different forms, and each clause
targets one. A claim with no output (pkg-10, "verified working for a day"). A suite
result with no repro (pkg-04, "cargo test passes"). A re-run of the control or a
different path (pkg-07 runs only the single-file control, and pkg-14 exercises a GET
when the bug is in POST). One failing scenario left out (calib-04 re-runs repro 1 but
not the `--exec` abort its plan names). The Unit 3 version, `test-is-decisive`, only
asked whether a plan *named* an observable outcome. For a PR, the question is whether
that outcome was *shown*, against the same input. "Same failing input" is the
load-bearing phrase, because pkg-07 and pkg-14 both show real output. I kept two
allowances to avoid failing honest accepts. The before may come from the plan's repro
evidence, since pkg-11's "before" is only an inline comment. And controls may be
*stated* unchanged, since pkg-08, pkg-13 and pkg-19 only state theirs. Running the repo's suite
is a separate check (`repo-checks-reported`), so a PR can't pass on suite output alone.

**Trade-offs**

Making controls "may simply be stated unchanged" is a case I accept this check will
miss. A PR whose fix breaks the control path, and whose author just writes "control
unchanged" without running it, passes `evidence-is-decisive`. I took that trade because
three of the clear accepts (pkg-08, pkg-13, pkg-19) state at least one control without
output. Requiring output for controls would have turned honest, ready PRs into rejects
over a scenario the plan never expected to change. Second, `repo-checks-reported` fails
a PR when a check the plan or repo names is silently missing. On pkg-01 it failed for
the missing pre-commit result as well as the template wall. That didn't change the
verdict there, because `template-asks-met` already held it. But it means a repo with a
long checklist of checks can hold a PR that never reports one of them, even if the diff
is fine. I checked for that before the full run: every clear accept reports its repo's
suite (the smoke run's pkg-11 and pkg-13 passed), and the full run's 7/7 on clear-accept
confirms it didn't over-reject.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
