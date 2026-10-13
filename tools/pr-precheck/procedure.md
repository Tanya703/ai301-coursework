# Procedure: how this tool grades a PR package

## Read order

1. Read the issue (title, body, stated expected versus actual). Write
   one line: the behavior being reported.
2. Read the plan next, before any part of the PR (eval: the "Plan
   context" block; live: `plan.md`). Write down four lists:
   (a) the files the plan names, (b) each piece of work it commits to
   (each approach step, including docs, changelog, and tests),
   (c) its not-in-scope / deferred items, and (d) each scenario its
   test plan says to re-run, with the expected-after for each, and
   any repo checks it names. Then read the repro evidence the plan
   built on and write down the failing input and its actual result.
   The plan comes before the PR because every side-by-side check
   measures the PR against it; reading the description first lets its
   framing decide what the diff "means".
3. Read the repo facts (eval: the "Repo facts" block; live:
   `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`).
   Write down (a) each template section and checklist item, (b) each
   PR requirement the contributing guide states (issue link,
   changelog/whatsnew entry, title convention), (c) the repo checks
   named, and (d) the AI policy in its own words, marked "disclosure
   required" or "no disclosure obligation". In live mode, the course
   rule makes it "disclosure required".
4. Read the thread highlights. Note any maintainer direction (a
   culprit, an approach, a deferral agreed) so a deferral cited in the
   PR can be checked against it.
5. Read the diff, file by file, before the description. For each hunk
   write one line: file, what it changes, and whether it is the fix, a
   test, a supporting edit, or something else.
6. Read the commit list.
7. Only now read the PR title and description, then the test
   evidence.

## Evidence gathering

1. For `diff-matches-plan`: pair the hunk list from read step 5 with
   plan lists (a)-(c). Mark each hunk "planned", "test", "supporting",
   or "extra". For each "extra" hunk, search the plan's deviation notes
   and the description for a sentence naming it; record the sentence or
   "not disclosed". Then walk plan list (b) and mark each item "in
   diff", or "missing"; for each missing item, search the plan and the
   description for a stated deferral and record it or "not disclosed".
2. For `description-matches-diff`: copy every sentence in the title
   and description that claims what the change does or contains,
   especially fidelity claims and "added/updated" claims. Next to each,
   write the diff fact that confirms or contradicts it.
3. For `evidence-is-decisive`: for each scenario in plan list (d),
   find its re-run in the test evidence (or the description's
   before/after). Record the input actually run, the before, and the
   after. Record "not run" for a scenario with no re-run, and "claim
   only" where the evidence states a result without output. Check that
   the input run is the plan's failing input, not a control or a
   different path.
4. For `repo-checks-reported`: list each repo check from read steps
   2(d) and 3(c), and copy its reported command and result, or record
   "not reported".
5. For `diff-is-clean`: walk every added line in the diff and copy any
   debug output, commented-out code, dead or unused function,
   unrelated TODO, or formatting/import churn, with its file. Copy any
   commit message that signals debris ("wip", "cleanup while
   debugging").
6. For `template-asks-met`: for each item from read steps 3(a) and
   3(b), copy where the description or diff satisfies it, or record
   "missing". An issue reference must carry the issue number; an entry
   file must appear in the diff's file headers.
7. For `ai-disclosure-meets-policy`: take the policy note from read
   step 3(d). If disclosure is required, copy the description's
   disclosure sentence, or record "no disclosure".
8. For `title-names-change`: copy the title.

## Check execution

1. Run the checks in rubric order. Grade each only on its own pass
   condition and the evidence recorded for it. Do not carry one
   check's result into another, except that `description-matches-diff`
   uses the "extra" marks from gathering step 1 to test fidelity
   claims.
2. `diff-matches-plan`: fail if any hunk is "extra" and "not
   disclosed", or any plan item is "missing" and "not disclosed".
   Quote the first such hunk or item. Otherwise pass.
3. `description-matches-diff`: fail if any recorded claim is
   contradicted by the diff; quote the claim and the contradicting
   fact. Otherwise pass.
4. `evidence-is-decisive`: pass only when every failing scenario in
   plan list (d) has a re-run with an observable after that differs
   from the before. Fail on any "not run", any "claim only", or a run
   of the wrong input; name the missing scenario.
5. `repo-checks-reported`: pass when at least one repo check is
   reported with its result and no check the plan or repo explicitly
   names is "not reported". A failing check with its reason counts as
   reported.
6. `diff-is-clean`: fail if gathering step 5 copied any debris line;
   quote the first one. Otherwise pass.
7. `template-asks-met`: fail if any item is "missing"; name it.
   Otherwise pass, including when the repo has no template and no
   stated PR requirement.
8. `ai-disclosure-meets-policy`: pass when there is no obligation, or
   when it exists and a disclosure was copied. Fail on "no disclosure"
   under a disclosure-required policy.
9. `title-names-change`: pass when the title names a component and a
   behavior; otherwise fail.
10. When evidence a check needs is absent from the package (for
    example, no test evidence section at all), grade it `fail` and say
    what is missing. Use `unclear` only when the evidence is present
    but genuinely ambiguous, and say what makes it ambiguous.
11. A check may be graded from the notes alone; re-read the package
    only to fill a missing note or pull a quote for the output.

## Verdict assembly

1. Collect the grades of the seven `required` checks. Treat every
   `unclear` among them as `fail`.
2. If all seven are `pass`, the verdict is `accept`. Otherwise it is
   `reject`.
3. Ignore `title-names-change` for the verdict; report it.
4. The deciding check is the first failing required check in rubric
   order. Its evidence line must quote the diff line, description
   sentence, or missing item that failed it. Every other check's
   evidence line names the fact that decided it.
5. Emit the summary, then the JSON block per SKILL.md, with the checks
   in rubric order.
