# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `diagnosis-fits-evidence` | The plan's stated cause (its diagnosis line, or the cause implied by what it changes), read against every step, control run, and artifact in the repro-evidence block (live mode: the student's posted repro comment, or the house repro pack quoted in the drafts) | The stated cause explains what the repro evidence shows, and no step, control run, or artifact in the evidence contradicts it. A cause that blames a component the evidence's own control run shows working (the same input succeeding with that component in the loop, the symptom appearing with that component absent, or the wrong value already present before that component runs) fails, however confident or detailed the plan is. A cause adopted from the thread or the issue passes only if the repro evidence does not rule it out. A plan that changes code with no cause stated at all fails. | required |
| `change-is-bounded` | The plan's in-scope and not-in-scope statements, its approach steps, and its file list, read against the behavior the issue reports and the repro reproduces | Every piece of work the plan commits to is needed to fix the reported behavior or to prove it fixed (the fix itself, regression tests for it, the smallest supporting change the fix needs). Work the issue never asked for — a migration or dependency upgrade, a rewrite or restructure of the surrounding module, a new user-facing option or setting, a UI change, a CI or test-harness overhaul, "while I'm here" fixes elsewhere — fails, even when the core fix inside it is correct. Deferring related work out of scope, with or without reasons, passes; a plan that narrows the issue honestly is bounded. | required |
| `stranger-can-start` | The plan's approach and file list, read as instructions for someone who has never spoken to the author | The plan names where the change goes (a file, function, or code path) and commits to one approach a stranger could begin implementing today. It fails when a real decision is left for build time: no files or area named, "somewhere", "whichever is easier", "not sure which layer", a menu of alternatives with none chosen, or "investigate/profile first and then decide". A named unknown with a chosen approach and a stated fallback passes; an unknown that is the approach fails. | required |
| `test-is-decisive` | The plan's test plan, read against the repro evidence's steps and its expected-versus-actual lines | The test plan names an observable outcome that would differ between the unfixed and fixed code for the reported behavior: the repro steps re-run with the expected result stated, or a regression test asserting a specific value, exit code, output, or absence of the error. A test plan that only says to run the existing suite, that "nothing should break", or that something "should feel" better, with no observable outcome for the fix itself, fails. | required |
| `follows-thread-direction` | The thread highlights' comments by OWNER, MEMBER, COLLABORATOR, or maintainer-role accounts, and the issue body when one of those accounts wrote it (live mode: the issue body, the thread's maintainer comments, and linked PRs), read against the plan's approach and the plan comment | Where a maintainer has given direction in the thread — isolated a culprit file or code path, proposed or rejected an approach, posted a patch or test build, asked for specific testing, or pointed at an open PR — the plan follows that direction or the comment explicitly engages it (names it and says how the plan relates, including why it departs). A plan that takes a different route and a comment that never mentions the maintainer's direction fails, however reasonable the route. Where the thread holds no maintainer direction, this passes. | required |
| `meets-repo-ai-policy` | The repo-facts block's contribution policy and AI policy (live mode: `CONTRIBUTING`, any `AI_POLICY` file, PR or issue templates), read against the plan comment as written | Every package is AI-assisted work. Where the repo's policy requires disclosing AI use, the plan comment discloses it (that AI was used, and to what extent where the policy asks). Where the policy is permissive, silent, or only remarks on AI without stating an obligation, this passes with nothing extra required. A comment that omits a disclosure the policy requires fails, however strong the plan is. | required |
| `unknowns-stated-honestly` | The plan's risks or unknowns, and any certainty language in the plan and comment, read against what the repro evidence actually establishes | The plan names at least one thing it has not verified, or the evidence leaves nothing material unverified, and nothing in the plan or comment claims certainty ("guaranteed", "definitely fixes", "no risk") beyond what the evidence shows. | preferred |

## Verdict rule

`accept` if and only if every `required` check grades `pass`. A single
`required` fail holds the package: `reject`.

`preferred` checks never change the verdict. They are recorded so the
author can see what would make a ready plan better.

`unclear` on a `required` check counts as `fail`: a plan a grader
cannot verify from the package is not ready to build from, and the fix
is to put the missing fact into the drafts.

Grade each required check on its own pass condition. A plan that is
short passes every check it meets; a plan that is long, polished, and
confident fails every check it misses. Length and formatting never
move a grade.
