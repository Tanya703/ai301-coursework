# Scope: where to look, and who is looking

<!--
This file is the skill's field of view. The rubric (rubric.md) decides
whether an issue is GOOD; the scope decides which issues are candidates
at all, and whose hands the issue would land in. It applies in live mode
only: in eval mode the bundle is the whole world and this file is
ignored.

Two parts. Staff wrote the first; you write the second.
-->

## Where candidates come from

Only issues in the course's Path Review repository are candidates:

- Repo: `codepath/pathreview-ai301-fa26-s3`

Do not search, fetch, or grade issues from any other repository, however
promising. The wider GitHub comes later in the course; for now the field
is Path Review.

**Path Review house rule.** Path Review is a classroom, and your
classmates are not strangers. Ignore the usual claim signals here: other
students' claim comments (and there may be several on one issue) do not
block an issue, and finding some on the issue you want is normal. Claim
anyway: course credit attaches to the pull request you open, not to
whether it merges, so a shared issue costs nobody anything. Everything
else in the rubric applies as written.

## Your fit profile

<!-- YOU write this part: a few sentences about you. What languages and
tools you have actually used, what you want to get better at, anything
you want to avoid. The skill uses this only to RANK the issues your
rubric accepts, never to change a verdict: fit cannot rescue an issue
your rubric rejects, and cannot sink one it accepts. -->

Python is the language I actually work in. I have built a retrieval-augmented
generation service end to end: FastAPI routes, a chunking and embedding
ingestion path, a vector store, and an agent layer over the top, with pytest
for the tests and alembic for the schema migrations. So I am comfortable in a
Python codebase that has a test suite I can run, and I can read my way around
an unfamiliar service without needing the whole thing explained first.

What I want to get better at is working inside someone else's Python project
rather than my own: reading an existing test suite, matching conventions I did
not choose, and getting a change reviewed by a maintainer. Issues that touch
retrieval, embeddings, prompt handling, evaluation, or API behaviour are the
ones I will learn the most from, because I have debugged those failure modes
in my own code and want to see how a real project handles them.

What I would rather avoid on a first contribution: frontend and CSS work,
build-system and packaging changes, and anything whose main difficulty is
getting a toolchain to install. I want the difficulty to be in the code, not
in the environment.
