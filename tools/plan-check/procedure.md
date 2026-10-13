# Procedure: how this skill grades a plan package

## Read order

1. Read the issue (title, body, any stated expected versus actual).
   Write down, in one line, the behavior being reported. Every later
   check measures the plan against this line, so it comes first.
2. Read the repro evidence next, before the plan. Write down: (a) what
   the failing run shows, (b) every control or contrasting run and
   what it rules in or out, and (c) the expected-versus-actual lines.
   Reading the evidence before the plan keeps the plan's own framing
   from deciding what the evidence "means".
3. Read the thread highlights (live: the issue thread), and the issue
   body itself when its author is an OWNER, MEMBER, or COLLABORATOR.
   For each of these written by an OWNER, MEMBER, COLLABORATOR, or
   maintainer-role account, write down any direction it gives: a culprit file or code
   path, an approach proposed or rejected, a patch or test build, a
   testing request, a pointer to an open PR. If there are none, write
   "no maintainer direction".
4. Read the repo-facts block (live: CONTRIBUTING, AI policy file,
   templates). Write down whether an AI-use disclosure is required,
   in the policy's words, or "no disclosure obligation".
5. Only now read the candidate plan, then the candidate plan comment.

## Evidence gathering

1. For `diagnosis-fits-evidence`: copy the plan's cause sentence. If
   none is stated, write down what the plan changes and treat that as
   the implied cause. Place it next to the control-run notes from read
   step 2.
2. For `change-is-bounded`: list every distinct piece of work the plan
   commits to (each approach step, each in-scope item, each file).
   List the not-in-scope items separately; they are deferrals, not
   work.
3. For `stranger-can-start`: copy the file, function, or code path
   names the plan gives, and any hedging phrase ("somewhere",
   "whichever", "not sure", "investigate first", "maybe also").
4. For `test-is-decisive`: copy the test plan's stated outcome, the
   thing someone would look at to say pass or fail.
5. For `follows-thread-direction`: take the maintainer-direction notes
   from read step 3. In the plan and comment, find the sentence where
   each piece of direction is followed or named. Record "not found"
   where none exists.
6. For `meets-repo-ai-policy`: take the note from read step 4. Search
   the plan comment for any AI disclosure and copy it, or record "no
   disclosure".
7. For `unknowns-stated-honestly`: copy the plan's risks or unknowns
   and any certainty words.

## Check execution

1. Run the checks in rubric order. Grade each one only on its own
   pass condition and the evidence recorded for it above. Do not carry
   one check's result into another.
2. `diagnosis-fits-evidence`: for each control run, ask "if the
   plan's cause were true, would this control have come out the way it
   did?" If any control answers no, fail and quote that control. If
   all controls are consistent and the cause explains the failing run,
   pass.
3. `change-is-bounded`: for each listed piece of work, ask "would the
   reported behavior still be fixed and proven without this?" If yes,
   and the item is not a deferral, it is extra work. Any extra work
   means fail, and the evidence names the item.
4. `stranger-can-start`: pass when a location is named and one
   approach is chosen. Fail when a hedging phrase from gathering step 3
   governs the approach itself, or when no location is named.
5. `test-is-decisive`: pass when the stated outcome would read
   differently on unfixed and fixed code. Fail when the only outcome is
   "suite passes", "nothing breaks", or a feeling.
6. `follows-thread-direction`: pass when there was no maintainer
   direction, or every piece of direction was found in gathering step
   5. Fail when any piece is "not found" and the plan does something
   else.
7. `meets-repo-ai-policy`: pass when no obligation exists, or when the
   obligation exists and a disclosure was found. Fail otherwise.
8. When the evidence a check needs is truly absent from the package
   (for example, no test plan at all), grade that check `fail` and say
   what is missing. Use `unclear` only when the evidence is present but
   genuinely ambiguous, and say what makes it ambiguous.
9. A check may be graded from the notes alone. Re-read the package
   only when a note is missing or a quote has to be pulled for the
   output.

## Verdict assembly

1. Collect the grades for the six `required` checks. Treat every
   `unclear` among them as `fail`.
2. If all six are `pass`, the verdict is `accept`. Otherwise it is
   `reject`.
3. Ignore `unknowns-stated-honestly` for the verdict. Report it.
4. In the output JSON, every check's `evidence` line quotes or names
   the fact that decided it. For a `reject`, the first failing
   required check's evidence must quote the plan text or package fact
   that failed it.
