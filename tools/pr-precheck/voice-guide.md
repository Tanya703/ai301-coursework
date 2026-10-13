# Voice guide: how I talk upstream

<!--
DRAFT — make this yours before you rely on it. The rules below are
drafted from what your Unit 1 selection file actually says about you
(a built-from-scratch RAG stack, and #68 being a retrieval bug you
recognise), but the wrong/right pairs have to be lines YOU might
really write, or the skill is holding your draft against someone
else's conscience. Rewrite any pair that does not sound like you.
-->

## Who I am in threads

I am a first-time contributor to this repo, and an experienced
engineer outside it: I have built and debugged a full RAG stack of my
own — ingestion, embeddings, a vector store, a retrieval layer, an
agent on top, tests and migrations — so retrieval bugs are code I
recognise rather than code I am guessing at. What I know well is my
own stack; what I do not yet know is this one. Readers can expect me
to have actually run the thing before I say anything about it, to show
what I ran, and to say plainly when a result is a guess.

## Rules I write by

### Rule: I show the run, or I do not make the claim

If I say something happened, the command and the output go in the same
comment. Familiarity with a failure mode in my own code is not
evidence about this codebase, and it is the thing most likely to make
me skip the terminal.

- Wrong: "This is the classic empty-corpus case — BM25 divides by the
  average document length, so an empty index always blows up here."
- Right: "Reproduced on an empty index: `KeywordSearcher.index([])`
  then `.search('test')` raises `ZeroDivisionError: division by zero`
  at `bm25.py:47`. Full traceback below."

### Rule: I promise investigation, never a fix and never a date

I claim the work by saying what I am going to look at next. I do not
promise a patch, a timeline, or an outcome I have not yet earned —
especially not in a claim comment written before I have reproduced
anything.

- Wrong: "I'd like to take this one — should have a PR up in a day or
  two."
- Right: "I'd like to investigate this one. Next step is to confirm
  how `KeywordSearcher.index()` is reached with zero documents, then
  I'll post what I find either way."

### Rule: a hypothesis stays in the subjunctive

When I think I know the cause, I write it as the thing it is. My prior
experience makes confident root-cause prose very easy to write and
that is exactly why it needs marking.

- Wrong: "The bug is that the corpus length is never checked before
  the BM25 constructor runs."
- Right: "My guess, not yet verified: the corpus length looks
  unchecked before the BM25 constructor runs. I have not traced the
  call path yet, so treat that as a hypothesis."

### Rule: I report a failed reproduction as a result

If I cannot make the bug happen, I post that — with what I ran, what I
got instead, and what I think differs about my setup. I do not go
quiet on the thread and I do not quietly widen my attempt until
something breaks.

- Wrong: *(no comment posted, because it did not reproduce)*
- Right: "I could not reproduce this on Windows 11 / Python 3.11.9 at
  `main@<sha>`. Steps and output below — the index built without
  error. The reporter is on Linux, which may matter here; happy to try
  a closer setup if someone can tell me theirs."

### Rule: I write to one reader, in my own words

No greeting-card openers, no emoji section headers, no restating the
issue back at the person who filed it. A maintainer reading fifty
threads gets my shortest honest version.

- Wrong: "Hi team! 👋 Great catch on this one — really important bug!
  🐛 Happy to help however I can!"
- Right: "Confirmed on the empty-index path — details below."

## Things I never post

- A fix, a date, or a guaranteed outcome. Investigation is all I ever
  promise.
- "Any update on this?" to a maintainer who owes me nothing.
- A root cause stated as fact when I have only read the code and not
  run it.
- A confident reproduction of behavior I have not actually matched
  against the issue's own description — the polished report of the
  wrong bug is my likeliest failure mode, not the missing one.
- Piggybacking on a classmate's reproduction ("same as above, can
  confirm"). My proof goes up in my own words, from my own
  environment, or not at all.
- Anything I have not re-read once after writing it, in the tone a
  stranger will read it in rather than the tone I wrote it in.
