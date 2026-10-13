# Plan: issue #68, keyword search raises ZeroDivisionError on an empty index

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

## Diagnosis

`KeywordSearcher.index()` always passes the tokenized corpus to
`BM25Okapi`, even when `chunks` is empty. `rank_bm25`'s
`BM25._initialize` then computes `self.avgdl = num_doc /
self.corpus_size` with `corpus_size == 0`. My repro (Windows 11, Python
3.11.9, `rank-bm25` 0.2.2, `main@2f4e82f`) shows the crash in `index()`
itself, before any search runs:

```
File "...\rag\retriever\keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
File "...\site-packages\rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero
```

The controls rule out the rest of the class. `search()` with `index()`
never called returns `[]` through its existing guard
(`keyword_search_empty_index` warning), and a three-chunk index builds
and searches normally. So the fault is limited to `index()` not
handling the empty case. `search()` already handles it once `self.bm25`
is `None`.

## Scope

In scope: one guard in `KeywordSearcher.index()`. When `chunks` is
empty, set `self.chunks = []` and `self.bm25 = None`, log it, and
return without constructing `BM25Okapi`. Remove the `xfail` marker from
`test_empty_index` so it runs as a regular regression test.

Not in scope:
- Changing `search()`. Its guard already returns `[]` when `self.bm25`
  is `None`.
- Chunks whose text tokenizes to nothing (for example, `[{"text": ""}]`).
  I ran it, and it also raises `ZeroDivisionError`, but on a different
  line (`rank_bm25.py:101`, `self.average_idf = idf_sum /
  len(self.idf)`) and from a different input than this issue reports.
  I'll report it on the thread as a separate finding (see Risks)
  instead of folding it into this fix.
- `HybridSearcher` in `rag/retriever/hybrid.py`, the tokenizer, or
  swapping or pinning the BM25 library.

## Files I'll touch

- `rag/retriever/keyword_search.py`: an early return in `index()`.
- `tests/unit/test_keyword_search.py`: delete the
  `@pytest.mark.xfail(strict=True, reason="issue #68 (manifest H-01)...")`
  decorator on `test_empty_index`. The test body stays as is.

## Approach

1. At the top of `index()`, add `if not chunks:`. Set `self.chunks =
   []` and `self.bm25 = None`, log `keyword_index_empty` through the
   existing `structlog` logger, and return.
2. Leave the non-empty path unchanged.
3. Remove the `xfail` decorator. It is `strict=True`, so if it stayed,
   the passing test would turn into a failure.
4. Run the keyword-search test file, then `make test-unit` and `make
   lint`, which the PR template asks for.

## Test plan

Re-run my Unit 2 repro steps against the fix:

1. `python -c "from rag.retriever.keyword_search import KeywordSearcher;
   KeywordSearcher().index([])"`. Before: `ZeroDivisionError: division by
   zero` at `rank_bm25.py:52`. Expected after: no traceback, exit code
   0, one `keyword_index_empty` log line.
2. `index([])` followed by `search("python", top_k=10)`. Expected after:
   `[]`, the same result as the never-indexed control.
3. Control: the three-chunk index still returns the `python
   programming` chunk first with `bm25_score` about 0.51, the same as
   before the fix.
4. `pytest tests/unit/test_keyword_search.py`. Before: `16 passed, 1
   xfailed`. Expected after: `17 passed`, with `test_empty_index`
   passing as a regular test.

## Risks and unknowns

- I haven't checked whether `HybridSearcher` calls `index([])` on a
  real code path, such as a profile with no documents ingested yet. The
  fix makes that call safe either way, but I'm not claiming it's how
  users hit the bug.
- Chunks that are all empty strings still crash after this fix. I ran
  `index([{"id": 1, "text": ""}])` and got `ZeroDivisionError` at
  `rank_bm25.py:101` in `_calc_idf` (empty vocabulary), not at line 52.
  The guard I'm adding checks `chunks`, not tokens, so that case is
  unchanged. A maintainer might prefer one guard on "no tokens in the
  corpus" that covers both. If review asks for that, it's a one-line
  change to the condition, and I'd add a second regression test for it.
- I ran this on Windows, and CI runs on Linux. Nothing in the change
  depends on the platform, but I'll check the CI jobs on the PR.

## Deviations

The code change matches the plan: the same early return in `index()`
and the same removed `xfail` decorator. No other files changed, and
every expected result in the test plan came out as written (`index([])`
exits 0 with a `keyword_index_empty` log line, `search()` after it
returns `[]`, the three-chunk control ranks the same with score
0.5108..., and the test file now shows `17 passed`).

Verification, updated 2026-10-12: when I built the change, my local
venv had only the packages `keyword_search.py` needs, so I first ran
just the keyword-search test file and `ruff` on the two touched files.
Before opening the PR I installed the full dev dependency set
(`pip install -e ".[dev]"`) and ran all four checks the PR template
lists: `make test-unit` (376 passed, 52 xfailed, all of them other
issues' seeded bugs), `make lint` (clean), `make typecheck` (clean),
and `make test-integration`, which stops with "no tests ran" because
Path Review has no integration tests yet. That covers approach step 4
and goes past it: the plan named only `make test-unit` and `make
lint`. The output is in `test_evidence.md` and the PR description.
