# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Tanya703

---

## Posted upstream

**Claim comment**

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

**Package analysis**

[Pick one scored package (`pkg-01` through `pkg-20` — the four `calib-` packages are never
scored). Name it by id, say what your rubric decided and what the gold label said, and
explain why your rubric read it that way.]

**Check rationale**

From `rubric.md`, the `artifact-shows-the-issue` row as it now reads:

> | `artifact-shows-the-issue` | The report's artifacts (output excerpts, logs, screenshots, measurements) read against the behavior the issue's description and thread actually report | Either (a) an artifact shows the behavior the issue reports — the same error, the same failure mode, the same exit condition — rather than an adjacent or earlier one; or (b) the report states it could not reproduce and the artifact shows what the attempt produced instead. An artifact showing only that the software runs, or showing a graceful validation, syntax, or compile error where the issue reports a crash or a wrong result, fails. | required |

The `(a) or (b)` disjunction is the whole design of this check, and it
is what I rejected a simpler version in favour of. The obvious way to
write it is the single clause: *an artifact shows the behavior the
issue reports*. That clause is right about every wrong-target and
no-evidence package — an argument-validation error standing in for a
capacity crash, a version banner standing in for a blank pane, a root
cause asserted with no transcript at all — and it is wrong about a
whole family of packages the eval set deliberately contains. An honest
cannot-reproduce has no artifact showing the issue's behavior, because
the behavior did not happen; under the single clause it fails a
`required` check and the package rejects. But the gold labels treat an
evidenced cannot-reproduce as `accept`, and the assignment is explicit
that reporting a failed reproduction faithfully is exactly what the
reproduce phase is for.

So the check has to ask a different question than "did the bug
appear?" It asks whether the artifact shows *the thing the report says
it shows*. Branch (a) is for a report claiming a reproduction; branch
(b) is for a report claiming it could not get one, where the evidence
owed is the attempt — the real commands and the real output that came
back instead. That keeps the three cases the set separates apart:
branch (b) passes the honest cannot-reproduce, the second sentence
still fails the artifact that only proves the program ran, and a
report that claims a reproduction it does not have is caught by branch
(a) rather than escaping through (b), because (b) requires the report
to have *stated* it could not reproduce.

**Trade-offs**

`env-deviation-disclosed` is where I accept a real miss. It reads:

> Where the run's version, platform, or configuration differs from
> what the issue targets, the report says so in its own words.

That condition is satisfied by *mentioning* the deviation. It asks
nothing about whether the deviation leaves the evidence worth
anything. So a report that runs an old release against an issue
confirmed only on `main`, and writes one honest line saying it did
exactly that, passes a check that exists to catch old-release runs —
even though the artifact is still evidence about that old version
rather than about the reported bug.

I accept the miss because the stricter version I considered — requiring
the report to argue that the deviation does not invalidate the result —
is a check on the quality of an argument rather than on an observable
fact, and two graders would not apply it the same way twice. The
`SKILL.md` grading discipline is explicit that a pass condition has to
be a rule someone else could apply and get my answer, and "did they
justify the gap convincingly" is not that rule. It also does not leave
the case uncovered in practice: a report that runs the wrong version
and narrates the result as the reported bug still fails
`artifact-shows-the-issue` and `stated-outcome-matches-evidence`, so
the package rejects anyway. What genuinely escapes is the narrow case
of a report that deviates, discloses honestly, and draws no
conclusion — which is a weak submission but not a dishonest one, and
holding it is a cost I would rather pay than make the check
unrepeatable.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
