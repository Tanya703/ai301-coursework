# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**Verdict output**

```
Good — issue #52 confirmed a COLLABORATOR response. Now I have all the evidence. Let me produce the graded output.

---

**Repo-level facts (all three issues share these):**

| Signal | Value |
|---|---|
| Archived | `false` |
| Last push | 2026-09-16 (5 days ago) |
| Latest release | none |
| Last 5 commits | All by "Andrew Burke" (non-bot) |
| Maintainer first-response sample | Issue #52 (closed): COLLABORATOR response from Aburke225 found |
| AI / contribution policy | `docs/CONTRIBUTING.md` present — no AI ban, silence on AI use → passes |

---

**Issue #69 — Output parser crashes on top-level JSON array fallback**

| Check | Grade | Evidence |
|---|---|---|
| `maintainer-alive` | pass | All 5 commits by Andrew Burke (non-bot); issue #52 has COLLABORATOR response |
| `repo-in-use` | pass | Not archived; last push 2026-09-16, 5 days before today |
| `scope-bounded` | pass | One fix: guard `.items()` call in `output_parser.py` when LLM returns array; no umbrella, no design dispute |
| `unclaimed` | pass | Assignees: none; no linked PR; 2 student claim comments — house rule: student claims don't block |
| `ai-policy-allows` | pass | CONTRIBUTING.md has no AI restriction; PR template has no AI-ban checkbox |
| `good-first-signal` | pass | Labels include "good first issue" |
| `release-recent` | fail | No releases exist in repo |
| `maintainer-in-thread` | fail | Both comments are NONE association (student claims) |

**Verdict: accept**

---

**Issue #68 — Keyword search raises ZeroDivisionError when index is empty**

| Check | Grade | Evidence |
|---|---|---|
| `maintainer-alive` | pass | All 5 commits by Andrew Burke (non-bot); issue #52 has COLLABORATOR response |
| `repo-in-use` | pass | Not archived; last push 2026-09-16, 5 days before today |
| `scope-bounded` | pass | One fix: guard empty corpus in `KeywordSearcher.index()` before BM25Okapi call; no umbrella, no dispute |
| `unclaimed` | pass | Assignees: none; no linked PR; 1 student claim comment — house rule: student claims don't block |
| `ai-policy-allows` | pass | CONTRIBUTING.md has no AI restriction |
| `good-first-signal` | pass | Labels include "good first issue" |
| `release-recent` | fail | No releases exist in repo |
| `maintainer-in-thread` | fail | 1 comment, NONE association (student claim) |

**Verdict: accept**

---

**Issue #64 — Relevance scorer "partial overlap" test fixture has full query overlap**

| Check | Grade | Evidence |
|---|---|---|
| `maintainer-alive` | pass | All 5 commits by Andrew Burke (non-bot); issue #52 has COLLABORATOR response |
| `repo-in-use` | pass | Not archived; last push 2026-09-16, 5 days before today |
| `scope-bounded` | pass | One fix: correct the test fixture so query terms only partially match chunk; no umbrella, no dispute |
| `unclaimed` | pass | Assignees: none; no linked PR; 0 comments — cleanest of the three |
| `ai-policy-allows` | pass | CONTRIBUTING.md has no AI restriction |
| `good-first-signal` | pass | Labels include "good first issue" |
| `release-recent` | fail | No releases exist in repo |
| `maintainer-in-thread` | fail | 0 comments |

**Verdict: accept**

---

**Ranking (fit profile applied):**

1. **#68** — Retrieval path bug (BM25 / `KeywordSearcher`) is exactly the student's domain: they've built and debugged a retrieval stack end-to-end and want to see how a real project handles retrieval failure modes. Only one student claim comment, so lightly contested.
2. **#69** — RAG generator output parsing is the next closest fit (prompt handling, LLM output processing). Two student claims but house rule means it's still free.
3. **#64** — Test-only fix with no production-code change; relevant to evaluation but offers less hands-on exposure to the production retrieval/agent code the student wants to learn. Perfectly unclaimed (0 comments).

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "All 5 commits by Andrew Burke (non-bot); issue #52 has COLLABORATOR response from Aburke225"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=false; last push 2026-09-16, 5 days before today"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single fix: guard empty corpus in KeywordSearcher.index() before BM25Okapi; no umbrella or design dispute"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees: none; no linked PR; 1 student claim comment — house rule: student claims do not block"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "CONTRIBUTING.md present, no AI restriction stated; silence passes"},
      {"name": "good-first-signal", "grade": "pass", "evidence": "Labels include 'good first issue'"},
      {"name": "release-recent", "grade": "fail", "evidence": "No releases exist in the repository"},
      {"name": "maintainer-in-thread", "grade": "fail", "evidence": "1 comment, author_association=NONE (student claim)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "All 5 commits by Andrew Burke (non-bot); issue #52 has COLLABORATOR response from Aburke225"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=false; last push 2026-09-16, 5 days before today"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single fix: guard .items() call in output_parser.py when LLM returns array; no umbrella or design dispute"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees: none; no linked PR; 2 student claim comments — house rule: student claims do not block"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "CONTRIBUTING.md present, no AI restriction stated; silence passes"},
      {"name": "good-first-signal", "grade": "pass", "evidence": "Labels include 'good first issue'"},
      {"name": "release-recent", "grade": "fail", "evidence": "No releases exist in the repository"},
      {"name": "maintainer-in-thread", "grade": "fail", "evidence": "2 comments, both author_association=NONE (student claims)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "All 5 commits by Andrew Burke (non-bot); issue #52 has COLLABORATOR response from Aburke225"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=false; last push 2026-09-16, 5 days before today"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single fix: correct test fixture so query terms only partially match chunk; no umbrella or design dispute"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees: none; no linked PR; 0 comments"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "CONTRIBUTING.md present, no AI restriction stated; silence passes"},
      {"name": "good-first-signal", "grade": "pass", "evidence": "Labels include 'good first issue'"},
      {"name": "release-recent", "grade": "fail", "evidence": "No releases exist in the repository"},
      {"name": "maintainer-in-thread", "grade": "fail", "evidence": "0 comments in thread"}
    ],
    "verdict": "accept"
  }
]
```
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Calibration smoke test, 4 unscored worksheet issues (`--only calib-01,calib-02,calib-03,calib-04 --include-calibration`):
   > agreement: 0/0 scored items

   (Calibration issues are never scored, hence 0/0; all 4 verdicts matched my own predictions before spending on the scored set.)

2. Full run attempt at `--workers 5`: crashed with an unhandled Python traceback after two issues (`issue-12`, `issue-13`) errored out with `Claude configuration file at C:\Users\Tanya\.claude.json is corrupted: JSON Parse error: Unexpected EOF`, caused by concurrent `claude` CLI processes racing to write that file while the C: drive had 0 bytes free. No agreement line was ever printed.

3. Single-issue stability probe after freeing some disk space (`--limit 1 --workers 1`), run only to confirm the CLI would complete without corrupting the config again before spending on a full run:
   > agreement: 0/1 scored items

4. Full run, serial (`--workers 1 --save-run eval-run.txt`), the run committed:
   > agreement: 15/20 scored items  (bar: 18/20: below the bar)

   This is the final score, matching the table in `eval-run.txt`.

**Issue analysis**

`issue-14` — my rubric's verdict: **reject**. Gold label: **accept**. From the run table:

> issue-14  accept  reject   NO     failed: maintainer-alive, maintainer-in-thread (preferred)

Reasoning behind my rubric's result: `maintainer-alive` requires two things to both hold — at least one recent human-authored commit, AND at least one sampled issue in the "maintainer first-response sample" that records a real response time in days rather than "no maintainer comment in thread." `issue-14`'s last 5 commits are all human-authored (`SaifAlYounan` x3, `sergiomaldo`), which passes the first half. But the bundle's response-sample list contains exactly one issue (`#489`), and it reads "no maintainer comment in thread" — zero of one sampled issues show a real response time, so the second clause fails, and because the check is `required`, the whole issue rejects.

I had actually caught this exact failure mode by hand before spending on the real run: reading the bundle directly, five fresh commits in five days (two of them security fixes referencing tracked issue numbers) is about as strong a maintainer-alive signal as an eval bundle offers, but the response-sample clause fails purely because the sample size happened to be 1 instead of the usual 5, and that one instance showed no reply. The gold label of accept matches that read — the issue itself is a bounded documentation task (six named files, exact line numbers) from a repo that is clearly alive by every other signal, and my check's AND logic can't distinguish "one thin, uninformative sample" from "five samples that all came back silent."

**Check rationale**

From `rubric.md`:

> | `unclaimed` | Repo facts block: the `this issue: assignees` and `linked PRs` fields, plus every comment in the Comments section | Assignees is none, AND no linked pull request is open, AND no comment within the 60 days before the capture date claims the work, unless a later comment or a closed unmerged pull request shows that attempt was dropped | required |

The evidence guide's fourth family calls this "the label archaeology family": a `good first issue` label is the maintainer's claim that the issue is friendly, not a claim that it is free, and both have to be checked separately. I made this a `required` check, not `preferred`, because taking an issue someone else is actively working on doesn't just waste my own time, it wastes theirs — a duplicate PR against an assignee's open branch is a real cost to a stranger, unlike the other preferred checks (label freshness, release cadence) which only affect how good the pick is for me. I built in the abandonment exception ("unless a later comment or a closed unmerged pull request shows that attempt was dropped") because the evidence guide is explicit that a closed, unmerged PR in an issue's history is a sign of a dropped attempt, not a live claim, and a rubric that can't tell the two apart would reject every issue with any history at all.

**Trade-offs**

The check gives up the ability to catch a claim that is stale but not yet formally dropped. `issue-08` (zulip/zulip#39794) shows this exactly: assignee `piyushagarwal-55` has an open linked PR (#39811), so `unclaimed` fails on both the assignee and the open-PR clauses. But the thread's last comment, from `zulipbot` on 2026-08-03 (two days before the 2026-08-05 capture), reads:

> @piyushagarwal-55 We noticed that you have not made any updates to this issue or linked PRs for 10 days. Please comment here if you are still actively working on it. Otherwise, we'd appreciate a quick `@zulipbot abandon` comment so that someone else can claim this issue and continue from where you left off.
>
> If we don't hear back, you will be automatically unassigned in 4 days. Thanks!

At capture time the assignee has not replied and has not been auto-unassigned yet, so nothing in the bundle "shows that attempt was dropped" under my check's wording, and `issue-08` still fails `unclaimed` even though the bot has already flagged it as going stale. A version of the check that treated an unanswered abandonment warning as a dropped claim would accept this issue days earlier than mine does; I accept the miss because grading a live warning as an actual abandonment risks the opposite failure, taking an issue out from under someone who comes back in the four days they were given.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. I've built a full RAG pipeline before — FastAPI routes, ingestion/embeddings, a vector store, an agent layer, pytest, alembic — and #68 is a bug in exactly that part of a stack: keyword search (`KeywordSearcher`/BM25) throwing a `ZeroDivisionError` on an empty index. It's a failure mode I've likely hit in my own retrieval code, so I already have a mental model for where the bug probably lives. It's also a single, bounded fix (guard the empty-corpus case before the BM25Okapi call), which matches the time I actually have this week.

2. The verdict correctly caught that the repo is alive by every signal that matters (fresh human commits, a real collaborator response elsewhere in the repo) and unclaimed in the way that matters (no assignee, no open PR, and the one prior claim comment doesn't block me under the Path Review house rule). What it couldn't weigh is which of the two accepted retrieval/RAG issues (#68 vs #69) would actually teach me more — the rubric can only say both clear the bar, not which bug is more interesting to debug. I ranked #68 above #69 because keyword search is closer to code I've personally written and broken than output-parsing is.

3. Claiming itself should be low-friction — I just comment, per the house rule, regardless of the one existing student comment. The real difficulty will be in reproducing the empty-index case in a codebase I haven't touched yet: confirming exactly how `KeywordSearcher.index()` is called with zero documents before I change anything.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
