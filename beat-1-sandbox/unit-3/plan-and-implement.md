# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Tanya703

**Plan comment**

TODO: paste the plan comment's permalink here after posting.

Plan, built from my reproduction above (Windows 11, Python 3.11.9,
`rank-bm25` 0.2.2, `main@2f4e82f`).

**Cause:** `KeywordSearcher.index([])` passes an empty corpus to
`BM25Okapi`, whose `_initialize` divides by `corpus_size` (0):
`rank_bm25.py:52  self.avgdl = num_doc / self.corpus_size ->
ZeroDivisionError`. My controls show `search()` already returns `[]`
when `self.bm25` is `None`, and that a non-empty index works. So the
gap is only in `index()`.

**Change (one guard):** in `rag/retriever/keyword_search.py`, `index()`
returns early on empty `chunks`, leaving `self.bm25 = None` and
`self.chunks = []` and logging it. In `tests/unit/test_keyword_search.py`,
I'll remove the strict `xfail` on `test_empty_index` (H-01), per the
PR template. Out of scope: `search()`, `HybridSearcher`, the tokenizer,
and chunks whose text is empty.

**Test:** re-run my repro. `index([])` should exit 0 with no traceback,
`index([])` then `search("python")` should return `[]`, and the
three-chunk control should rank the same as before. The test file should
go from `16 passed, 1 xfailed` to `17 passed`.

**A related case that this plan leaves out on purpose:** chunks that are
all empty strings also crash. `index([{"id": 1, "text": ""}])` raises
`ZeroDivisionError` at `rank_bm25.py:101` (`idf_sum / len(self.idf)`,
empty vocabulary), which is a different line and a different input
from this issue. My guard checks `chunks`, so that case stays as it is.
If you'd rather have one guard on "no tokens in the corpus" that covers
both, it's a one-line change to the condition plus a second test. Tell
me and I'll do that instead.

I drafted this plan with help from Claude (an AI assistant). I ran
every command above myself, and I've reviewed the plan.

---

## Your branch

**Branch**

fix/68-empty-keyword-index

**Evidence**

Environment: Windows 11 Pro (10.0.22631), Python 3.11.9, rank-bm25 0.2.2,
structlog 26.1.0. Before = `main@2f4e82f`; after = branch
`fix/68-empty-keyword-index` at `4fd7891`.

Before the fix (`main@2f4e82f`):

```
$ python -c "from rag.retriever.keyword_search import KeywordSearcher; KeywordSearcher().index([])"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "D:\pathreview-ai301-fa26-s3\rag\retriever\keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "D:\pathreview-ai301-fa26-s3\.venv\Lib\site-packages\rank_bm25.py", line 83, in __init__
    super().__init__(corpus, tokenizer)
  File "D:\pathreview-ai301-fa26-s3\.venv\Lib\site-packages\rank_bm25.py", line 27, in __init__
    nd = self._initialize(corpus)
         ^^^^^^^^^^^^^^^^^^^^^^^^
  File "D:\pathreview-ai301-fa26-s3\.venv\Lib\site-packages\rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
                 ~~~~~~~~^~~~~~~~~~~~~~~~~~
ZeroDivisionError: division by zero

$ python -c "from rag.retriever.keyword_search import KeywordSearcher; print(KeywordSearcher().search('python', top_k=10))"
2026-10-12 19:48:50 [warning  ] keyword_search_empty_index
[]

$ python -c "from rag.retriever.keyword_search import KeywordSearcher; s=KeywordSearcher(); s.index([{'id':1,'text':'python programming'},{'id':2,'text':'java code'},{'id':3,'text':'rust systems'}]); print(s.search('python', top_k=10))"
2026-10-12 19:48:51 [info     ] keyword_index_built            chunk_count=3
2026-10-12 19:48:51 [info     ] keyword_search_complete        query_len=1 results_count=3
[{'id': 1, 'text': 'python programming', 'bm25_score': 0.5108256237659907}, {'id': 2, 'text': 'java code', 'bm25_score': 0.0}, {'id': 3, 'text': 'rust systems', 'bm25_score': 0.0}]

$ python -m pytest tests/unit/test_keyword_search.py -k empty_index --runxfail
E       ZeroDivisionError: division by zero
.venv\Lib\site-packages\rank_bm25.py:52: ZeroDivisionError
FAILED tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index
====================== 1 failed, 16 deselected in 1.15s =======================

$ python -m pytest tests/unit/test_keyword_search.py -q
........x........                                                        [100%]
16 passed, 1 xfailed in 1.11s
```

After the fix (`fix/68-empty-keyword-index`):

```
$ python -c "from rag.retriever.keyword_search import KeywordSearcher; KeywordSearcher().index([])"
2026-10-12 19:51:02 [info     ] keyword_index_empty
exit=0

$ python -c "from rag.retriever.keyword_search import KeywordSearcher; s=KeywordSearcher(); s.index([]); print(s.search('python', top_k=10))"
2026-10-12 19:51:03 [info     ] keyword_index_empty
2026-10-12 19:51:03 [warning  ] keyword_search_empty_index
[]

$ python -c "from rag.retriever.keyword_search import KeywordSearcher; s=KeywordSearcher(); s.index([{'id':1,'text':'python programming'},{'id':2,'text':'java code'},{'id':3,'text':'rust systems'}]); print(s.search('python', top_k=10))"
2026-10-12 19:51:03 [info     ] keyword_index_built            chunk_count=3
2026-10-12 19:51:03 [info     ] keyword_search_complete        query_len=1 results_count=3
[{'id': 1, 'text': 'python programming', 'bm25_score': 0.5108256237659907}, {'id': 2, 'text': 'java code', 'bm25_score': 0.0}, {'id': 3, 'text': 'rust systems', 'bm25_score': 0.0}]

$ python -m pytest tests/unit/test_keyword_search.py -q
.................                                                        [100%]
17 passed in 0.50s

$ ruff check rag/retriever/keyword_search.py tests/unit/test_keyword_search.py
All checks passed!
$ ruff format --check rag/retriever/keyword_search.py tests/unit/test_keyword_search.py
2 files already formatted
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Run 1 (full): `agreement: 19/20 scored items  (bar: 18/20: PASS)`. Categories:
   `clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   This is the run in `eval-run.txt`.

There's only one scored run. Two earlier launch attempts graded nothing, so they have no
score. The first crashed before grading with a Windows `cp1252` `UnicodeEncodeError` while
piping the prompt, fixed by setting `PYTHONUTF8=1`. The second had all 20 `claude -p` calls
refused because the harness was started from inside another Claude Code session. I also ran
a one-package smoke check (`--only pkg-02`: accept, agrees) to confirm the launcher worked
before spending a full run. Run 1 cleared the bar with every category matched, so I stopped
there rather than tune for pkg-14 (see Package analysis).

**Package analysis**

**pkg-14** (zellij-org/zellij#5174, category `clear-accept`). My rubric said **reject**,
and the gold label is **accept**. It was the only disagreement in the run.

Every required check passed except `stranger-can-start`. The grader's evidence for that
fail was: "Plan names only module-level areas ('zellij-server (session connection
handling)', 'zellij-client's terminal query issuance') and explicitly defers specific files
and functions: 'exact functions to be pinned in the PR after tracing the query issuance with
debug logs, which I have working.'"

My pass condition requires the plan to name "where the change goes (a file, function, or
code path)" and lists "investigate/profile first and then decide" as a fail. The pkg-14 plan
reads like a deferral on exactly that point. It names crates and a handshake path but leaves
the functions to be found by tracing. The grader applied my words correctly.

The gold label reads it differently, and I think the gold is right. The approach itself is
chosen ("consuming or draining pending OSC query responses in the client attach path ...
before pane input is wired"). The code path is named at the level of "the reattach
handshake". The only open item is which function in that path, and the plan says the debug
tracing that will find it already works. That's different from pkg-17 or pkg-18, where the
layer itself ("gocui? tcell? not sure") or the fix site ("upstream or vendored, whichever is
easier") is undecided.

The fix would be to say that a named code path with a chosen approach passes even if the
exact function isn't pinned yet. I didn't make that change. Loosening this check is exactly
what could flip pkg-17 or pkg-18, the unbuildable packages it currently catches. With 19/20
and every category matched, one honest miss on an arguable clear-accept was a better trade
than risking those.

**Check rationale**

From `rubric.md`, the `diagnosis-fits-evidence` row as it now reads:

> | `diagnosis-fits-evidence` | The plan's stated cause (its diagnosis line, or the cause implied by what it changes), read against every step, control run, and artifact in the repro-evidence block (live mode: the student's posted repro comment, or the house repro pack quoted in the drafts) | The stated cause explains what the repro evidence shows, and no step, control run, or artifact in the evidence contradicts it. A cause that blames a component the evidence's own control run shows working (the same input succeeding with that component in the loop, the symptom appearing with that component absent, or the wrong value already present before that component runs) fails, however confident or detailed the plan is. A cause adopted from the thread or the issue passes only if the repro evidence does not rule it out. A plan that changes code with no cause stated at all fails. | required |

The obvious version of this check is "the diagnosis is grounded in the
repro evidence," and I rejected it because a wrong-cause plan always
*looks* grounded. It quotes the traceback, names a real file, and
sounds confident. The wrong-cause plans in the eval set fail on a
different point: each one blames a component that the package's own
control run already cleared. In pkg-01 the same items parse fine
without `-v`, in pkg-07 a friendly error prints in the same build, and
in pkg-11 `collect` already produces null outside conditions. So the
pass condition is written as a test against the controls. For each
control, would it have come out that way if the plan's cause were
true? I spelled out the three shapes a ruling-out control takes, so a
grader doesn't have to invent them. "However confident or detailed"
is there because polish is how these plans get past a reader. The
sentence about causes adopted from the thread covers the calib-03
trap, where the thread's confident diagnosis is the one the timing
evidence rules out.

**Trade-offs**

`diagnosis-fits-evidence` can only catch a wrong cause when the
package contains a control that contradicts it. A plan that blames
the wrong component, sitting on repro evidence with no control run at
all, passes, because nothing in the package contradicts it. The check
then reads as "not disproven" rather than "proven". I accept that
miss. The stricter version would require the evidence to positively
isolate the cause. That would fail honest, terse plans like
calib-01, whose repro shows the symptom and the re-entry workaround
but no run that isolates the refresh scope. The gold labels treat
that plan as ready.

Nothing in the set shows this miss yet. Here's how I know: in run 1
the check failed exactly pkg-01, pkg-07, pkg-10, pkg-11, pkg-16, and
pkg-18, all of them gold rejects. It failed none of the seven
clear-accepts. For pkg-01 and pkg-11 it was the only required check
that failed, so without it those two flip to accept and the score drops
to 17/20. What it gives up is a hypothetical: a wrong-cause plan
whose repro has no control run. The eval set doesn't contain one, so
I can't measure that miss here. I'd catch it in live mode only by
running controls myself, as I did for my own repro on #68.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
