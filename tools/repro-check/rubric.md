# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `env-recorded` | The repro report's environment record, read against the platform and version the issue names in its description and the repo-facts block | The report names the OS/platform, the version or commit of the software under test, and any component the reported behavior depends on (driver, shell, terminal, build profile, backend). One terse line naming all of them passes; a long, well-formatted report that names none of them fails. Where the issue is specific to a platform or a component, the record says which one was used. | required |
| `steps-rerunnable` | The repro report's steps, together with every input, config file, and command the package supplies — in the report itself or in the issue it is answering | Someone else with the named environment could reach the same starting state from what the package gives them. An input counts as supplied if it is quoted, described precisely enough to rebuild, or is a public artifact they can fetch — including one the issue itself already contains. What fails is an input a stranger cannot obtain or rebuild at all: a private repository, a config the author says they cannot share, or unstated local state. Steps that give nothing to follow fail for the same reason. | required |
| `artifact-shows-the-issue` | The report's artifacts (output excerpts, logs, screenshots, measurements) read against the behavior the issue's description and thread actually report | Either (a) an artifact shows the behavior the issue reports — the same error, the same failure mode, the same exit condition — rather than an adjacent or earlier one; or (b) the report states it could not reproduce and the artifact shows what the attempt produced instead. An artifact showing only that the software runs, or showing a graceful validation, syntax, or compile error where the issue reports a crash or a wrong result, fails. | required |
| `steps-hit-the-trigger` | The exact commands and inputs the report ran, read against the syntax, input, or sequence the issue names as the trigger | The run exercises the trigger the issue names. If the report altered the issue's input, expression, or invocation, the altered form still exercises the reported code path and the change is called out in the report. A run that silently substitutes different syntax or a different input shape, then presents the result as the issue's behavior, fails. Where the report states it could not reproduce and names the condition it was unable to achieve, this passes: failing to reach the trigger is that report's finding, not a flaw in its method. | required |
| `env-deviation-disclosed` | The report's environment record and version line, read against the version, platform, and configuration the issue and its thread confirm the bug on | Where the run's version, platform, or configuration differs from what the issue targets, the report says so in its own words. Where they match, or where there is no deviation to disclose, this passes. A run on an older release than the issue confirms the bug on, presented without noting the gap, fails: the result is evidence about that older version, not about the reported bug. | required |
| `stated-outcome-matches-evidence` | Every conclusion the claim comment and the report assert — reproduced, not reproduced, root cause, severity, scope — read against the artifacts actually shown in the package | Each assertion rests on something shown. A report naming a root cause shows the observation it rests on; a report saying it reproduced shows the reproduction; a report saying it could not reproduce is backed by the attempt and names what differed. Confidence language ("guaranteed reproducible", "I verified this race condition") with nothing shown fails, and an expected-versus-actual statement that contradicts the artifact beside it fails. | required |
| `claim-is-specific-and-honest-about-intent` | The claim comment, read against the issue's own specifics | The claim names something only a reader of this issue could name — the failing input, the error text, the file or code path, the version — and says what the author intends to do next. Naming a fix direction or the code they mean to change passes: that is what claiming work sounds like. Three things fail: a comment interchangeable with one on any other issue, a guaranteed outcome ("will definitely fix", "guaranteed reproducible"), and a promised delivery date or timeline. | required |
| `follows-repo-comment-conventions` | The repo-facts block's contribution policy, bug-report template asks, and AI-use policy (live mode: `CONTRIBUTING`, the issue template, any AI policy doc), read against both comments as written | The comments satisfy the obligations the repo's policy actually states. Where the policy requires disclosing AI assistance, the comments disclose it; where it restricts AI-written prose, the comments meet that restriction; where it is permissive about AI use or silent on it, this passes with nothing extra required. A policy that merely remarks on AI without stating an obligation creates none. This work is AI-assisted, so a repo whose policy requires disclosure, paired with comments that never disclose, fails however strong the proof is. | required |
| `control-run-shown` | The report's artifacts, looking for a contrasting run beside the failing one | A working or contrasting case is shown next to the failure, so a reader can see what changed rather than taking the failure on trust. | preferred |
| `next-step-named` | The closing lines of the claim comment or the report | The author names a concrete next step — the code path they will read, the hypothesis they will test — rather than ending on an offer to help. | preferred |

## Verdict rule

`accept` if and only if every `required` check grades `pass`. A single
`required` fail holds the package: `reject`.

`preferred` checks never change the verdict. They are recorded in the
output so the author can see what would make an already-postable
package better.

`unclear` on a `required` check counts as `fail`. Proof a grader cannot
verify is proof that is not ready to be posted upstream, and the fix is
to show the evidence rather than to argue the grade.

One exception, per the claim-only rule in `SKILL.md`: on a claim-only
draft, the checks whose evidence is the repro report
(`env-recorded`, `steps-rerunnable`, `artifact-shows-the-issue`,
`steps-hit-the-trigger`, `env-deviation-disclosed`,
`control-run-shown`) grade `unclear` with evidence `not yet
applicable: claim-only draft` and are left out of the verdict rule
entirely. The verdict then answers only whether the claim comment is
ready to post, on
`stated-outcome-matches-evidence`,
`claim-is-specific-and-honest-about-intent`, and
`follows-repo-comment-conventions`.
