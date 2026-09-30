# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives.** In an eval bundle: the repro report's own
environment record, usually its first lines or a block headed
`Environment`, `System`, or similar — and sometimes only a single
inline sentence, which still counts. The thing it must be read
*against* lives elsewhere: the issue context's description (which
names the version the reporter saw the bug on, and often the OS), the
thread highlights (where maintainers narrow it to a platform, a
driver, or a build), and the repo-facts block's bug-report template
asks (which name the fields this repo expects). In live mode: the
draft comment's environment lines, read against the issue body, the
`Version`/`Environment` fields of the repo's bug-report template, and
any maintainer comment in the thread that narrows the platform.

**What good looks like.** Three things are named: the OS or platform,
the version or commit of the software under test, and whatever
component the reported behavior actually depends on — the driver on a
minikube issue, the shell on a prompt issue, the build profile on a
performance issue, the terminal on a rendering issue. Sufficiency is
judged against the issue, not against a word count: one line reading
`macOS 14.5, yq v4.44.1, zsh` is a complete record for a yq parsing
bug and an incomplete one for a Windows driver bug that never says
which driver. The failure mode to watch for is a long, confident,
beautifully formatted report with no environment anywhere in it; the
format invites you to assume the record is there.

## Steps

**Where they live.** In an eval bundle: the numbered or bulleted
sequence in the repro report, plus every fenced block it quotes —
commands, input files, config. In live mode: the same, in the draft.
Read them against the issue's own reproduction steps where it has
them, and against the trigger the issue names.

**What makes them followable.** The test is repeatability by someone
who has only the report: from the named starting state, every input
the run depends on is either quoted in full or fetchable by anyone
(a public repo at a named ref, a released package, a file whose
contents appear in the report). Steps that read `in our monorepo, with
our lint config` fail the test no matter how precise the rest is,
because nobody else can reach step one. So do steps that assume local
state the report never creates. Terseness is not a failure: four
commands that anyone can paste are more followable than twelve prose
paragraphs. What matters is whether a stranger ends up where the
author was standing.

Watch separately for steps that are followable but *miss the
trigger*: the issue reports an offset-from-end range and the steps run
a prefix range; the issue reports `a = b` and the input uses
`a: b`; the issue binds a variable and the steps leave it unbound.
Those steps are perfectly re-runnable and still prove nothing about
the reported bug, which is why the trigger is checked apart from the
followability.

## Behavior shown

**Where the artifacts live.** In an eval bundle: the fenced output
excerpts, log snippets, screenshot descriptions, and measurements
inside the repro report. What they must be read against is the issue
context — the description's statement of what goes wrong, and the
thread highlights where maintainers refine it (exit codes, error
strings, the specific wrong output). In live mode: the artifacts in
the draft, read against the live issue body and thread.

**What it means to show the issue's behavior.** The artifact contains
the reported failure itself, not a neighbour of it. Concretely, these
are all artifacts that fail the test:

- The software's own graceful error where the issue reports a crash —
  an argument-validation message and exit 1 standing in for a capacity
  overflow and exit 101; an HCL syntax error standing in for a panic;
  a compile error standing in for an `Invalid path expression` result.
- An artifact that only proves the program ran: a version banner, a
  session list, panes rendering. "It started" is not "it broke."
- Garbled-but-alive output presented as a crash.
- Nothing at all: a root cause diagnosed in prose with no transcript,
  no measurement, no log.

An honest cannot-reproduce is the case that looks like a failure and
is not. There, the artifact is not expected to show the bug; it is
expected to show *the attempt* — the real commands run, the real
output that came back instead — alongside a statement that the
behavior did not appear and a named difference that might explain why
(uniform filename lengths, a 2 MiB `ARG_MAX`, Linux and zsh where the
reporter had macOS and fish). That is evidence, and it is ready to
post. What separates it from a no-evidence package is that the attempt
is shown; what separates it from a wrong-target package is that the
report does not claim the attempt succeeded.

A contrasting or control run — the working case beside the failing one
— is the strongest form this evidence takes, because it shows the
reader what changed rather than asking them to trust that something
did.

## Honesty

**Where claims and their backing meet.** Gather two lists and lay them
side by side. First, every assertion the package makes: in the claim
comment (reproduced, will fix, severity, cause) and in the repro
report (its expected-versus-actual statement, its root cause, its
scope, its confidence language). Second, everything the package
actually shows: the artifacts, the environment record, the steps. In
live mode the two lists come from the draft; in eval mode from the
bundle. Nothing outside the package counts, including what the author
knows and did not write down.

**What good looks like.** Every item on the first list has something
on the second list holding it up, and nothing on the first list is
larger than its backing. The tells that it is not:

- Certainty language with an empty second list — "guaranteed
  reproducible", "I verified this race condition", "confirmed on two
  machines" — where no measurement, transcript, or log appears.
- A root cause named as fact when the package shows only the symptom.
  A hypothesis, labelled as one, is honest; the same sentence in the
  indicative is not.
- An expected-versus-actual statement that contradicts the artifact
  printed directly beneath it, including the case where the two are
  written the wrong way round.
- Generalising the bug past the evidence: claiming it affects a
  release or platform the package never ran on, especially one
  maintainers have said they could not reproduce on.

The symmetric error is worth naming because it is the one that gets
punished unfairly: a report that says plainly "I could not reproduce
this, here is what I ran and what I got, here is what I think differs"
is *more* honest than a confident reproduction of the wrong thing, and
should read as ready. Honesty is measured as the distance between what
is said and what is shown, in either direction.

## Comms

**Where the words meet the repo.** Three places, and all three are
read as a stranger on the thread would read them.

The claim comment against the issue: does it name this issue's
specifics — the failing input, the error string, the file or code
path, the version — or would it sit unchanged on any other issue in
any other repo? And what does it promise: investigation and a report
back, or a fix, an outcome, a date?

The comments against the repo's stated conventions: the repo-facts
block carries the bug-report template's asks and the contribution
policy, including any AI-use policy. In live mode these live in
`CONTRIBUTING`, the issue and PR templates, and any dedicated AI
policy doc. Read the policy as the repo words it, because the wordings
differ in kind and so do the obligations they create:

- **Requires disclosure** ("disclose all AI usage"): the comments must
  say so. This work is AI-assisted, so silence here is a failure, and
  it is a failure independent of how good the proof is — an excellent
  reproduction posted into a disclosure-requiring repo without
  disclosure is not ready to post.
- **Restricts AI-written prose**: the comment must meet the
  restriction, which a comment genuinely written in the author's own
  voice does.
- **Permissive about AI use** ("use it responsibly", no disclosure
  asked for) or **silent**: nothing extra is required, and a package
  that adds no disclosure passes. Do not invent an obligation the repo
  did not state.

The words against the reader: specific-and-honest reads as though a
person who looked at this particular thing wrote it. Boilerplate reads
as though the issue number were a variable — "Hi! I'd love to work on
this, please assign me, I'll have a fix up in 2 days." Note that the
opposite of boilerplate is not length. A polished, sectioned,
emoji-headed comment can be pure boilerplate, and three plain
sentences naming the exact error and the next file to read are not.
Judge what the words commit to and what they demonstrate, never their
formatting.
