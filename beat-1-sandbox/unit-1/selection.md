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

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62

**Verdict output**

```
Now I have all the evidence I need. Let me compile the grades.

---

**Evidence summary (repo-level, applies to all three issues):**

- **archived**: no
- **last push**: Sept 16, 2026 (2 days ago — within 90 days ✓)
- **last 5 commits**: Sept 16 ×3, Aug 24 ×2 — all within 180 days; 3 of 5 within 90 days ✓
- **maintainer responsiveness**: Aburke225 carries COLLABORATOR badge; commented on issue #52 (opened Sept 10 → COLLABORATOR comment Sept 16 = 6 days, well within 45 days) ✓
- **contribution policy**: no CONTRIBUTING.md anywhere, no AI policy file — silence passes ✓

---

**Issue #72** — `verify_password` raises `UnknownHashError` on malformed stored hashes

| Check | Grade | Evidence |
|---|---|---|
| repo-alive | **pass** | Not archived; last push Sept 16 (2d); 5/5 commits within 180d |
| maintainer-responsive | **pass** | Aburke225 (COLLABORATOR) commented on #52 within 6 days of opening |
| unclaimed | **pass** | assignees: none; linked PRs: none; no comments at all |
| bounded-scope | **pass** | One bug: `UnknownHashError` propagates instead of returning `False`; two files for the same fix |
| spec-is-concrete | **pass** | Names `core/security.py`, exact fix behavior (return `False`), specific xfail test H-05 to unmark |
| policy-permits-newcomer | **pass** | No CONTRIBUTING.md or AI policy found; silence passes |
| solution-located *(preferred)* | **pass** | Names file, exact exception type, concrete fix direction |
| newcomer-signalled *(preferred)* | **pass** | Labels: `good first issue`, `tier-1` |
| discussion-settled *(preferred)* | **pass** | Empty thread |

All required checks pass → **ACCEPT** (all 3 preferred also pass)

---

**Issue #62** — Health check references `settings.redis_host` which does not exist

| Check | Grade | Evidence |
|---|---|---|
| repo-alive | **pass** | Same repo facts |
| maintainer-responsive | **pass** | Same: Aburke225 (COLLABORATOR) on #52 within 6 days |
| unclaimed | **pass** | assignees: none; linked PRs: none; no comments |
| bounded-scope | **pass** | One attribute-name mismatch in `api/routes/health.py`; single coherent fix |
| spec-is-concrete | **pass** | Names files, exact mismatched attributes (`redis_host`/`redis_port` vs `redis_url`), reproduction steps given |
| policy-permits-newcomer | **pass** | No CONTRIBUTING.md or AI policy; silence passes |
| solution-located *(preferred)* | **pass** | Names `api/routes/health.py` and the exact wrong vs correct attribute names |
| newcomer-signalled *(preferred)* | **pass** | Labels: `good first issue`, `tier-1` |
| discussion-settled *(preferred)* | **pass** | Empty thread |

All required checks pass → **ACCEPT** (all 3 preferred also pass)

---

**Issue #69** — Output parser crashes on top-level JSON array fallback

| Check | Grade | Evidence |
|---|---|---|
| repo-alive | **pass** | Same repo facts |
| maintainer-responsive | **pass** | Same: Aburke225 (COLLABORATOR) on #52 within 6 days |
| unclaimed | **pass** | assignees: none; linked PRs: none; no comments |
| bounded-scope | **pass** | One crash path: `.items()` called on list in `rag/generator/output_parser.py`; two files for same fix |
| spec-is-concrete | **pass** | Names `rag/generator/output_parser.py`, exact error (`.items()` on array), xfail test H-02 to unmark |
| policy-permits-newcomer | **pass** | No CONTRIBUTING.md or AI policy; silence passes |
| solution-located *(preferred)* | **pass** | Names file and specific code path causing the crash |
| newcomer-signalled *(preferred)* | **pass** | Labels: `good first issue`, `rag`, `tier-1` |
| discussion-settled *(preferred)* | **pass** | Empty thread |

All required checks pass → **ACCEPT** (all 3 preferred also pass)

---

**Ranking (all three accepted — ranked by fit profile):**

All three pass every check and all three preferred checks. Fit profile: Python/TS comfortable, wants a real bug fix traceable to a specific cause, bounded.

1. **#62 (top pick)** — Most directly traceable: a clear attribute-name mismatch (`redis_host` → `redis_url`) that propagates through exception handling. You can pinpoint the root cause in two lines before writing any code. Bounded to `api/routes/health.py`. Fits the "traceable to a specific cause" preference perfectly.

2. **#72** — Security utility fix in `core/security.py`; requires understanding passlib's exception hierarchy to write the defensive `except UnknownHashError: return False`. Slightly more library surface to learn but still clean. Good for someone who wants to read real application code.

3. **#69** — RAG domain adds a layer of LLM-output context to absorb before the fix makes full sense. Estimated effort is 2–4h vs 1–2h for the others. Still a good bug, just the highest domain overhead of the three.

---

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "Not archived; last push Sept 16 (2 days); all 5 recent commits within 180 days"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) commented on #52 within 6 days of opening, well within 45-day threshold"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; zero comments in thread"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single attribute-name mismatch in api/routes/health.py; one coherent fix, not a tracker"},
      {"name": "spec-is-concrete", "grade": "pass", "evidence": "Names api/routes/health.py, exact wrong (redis_host/redis_port) vs correct (redis_url) attributes, reproduction steps provided"},
      {"name": "policy-permits-newcomer", "grade": "pass", "evidence": "No CONTRIBUTING.md or AI policy found anywhere in repo; silence passes"},
      {"name": "solution-located", "grade": "pass", "evidence": "File, wrong attribute names, and correct attribute named explicitly in body"},
      {"name": "newcomer-signalled", "grade": "pass", "evidence": "Labels: good first issue, tier-1"},
      {"name": "discussion-settled", "grade": "pass", "evidence": "Empty thread; no disagreement"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "Not archived; last push Sept 16 (2 days); all 5 recent commits within 180 days"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) commented on #52 within 6 days of opening, well within 45-day threshold"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; zero comments in thread"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "One bug fix: catch UnknownHashError in core/security.py and return False; second file is the corresponding test"},
      {"name": "spec-is-concrete", "grade": "pass", "evidence": "Names core/security.py, exact fix (return False on UnknownHashError), specific xfail test H-05 to unmark"},
      {"name": "policy-permits-newcomer", "grade": "pass", "evidence": "No CONTRIBUTING.md or AI policy found; silence passes"},
      {"name": "solution-located", "grade": "pass", "evidence": "File named, exception type named, concrete fix direction stated"},
      {"name": "newcomer-signalled", "grade": "pass", "evidence": "Labels: good first issue, tier-1"},
      {"name": "discussion-settled", "grade": "pass", "evidence": "Empty thread; no disagreement"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "repo-alive", "grade": "pass", "evidence": "Not archived; last push Sept 16 (2 days); all 5 recent commits within 180 days"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "Aburke225 (COLLABORATOR) commented on #52 within 6 days of opening, well within 45-day threshold"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; zero comments in thread"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "One crash path: .items() on a list in rag/generator/output_parser.py; two files for the same fix"},
      {"name": "spec-is-concrete", "grade": "pass", "evidence": "Names rag/generator/output_parser.py, exact error (.items() on array), xfail test H-02 to unmark"},
      {"name": "policy-permits-newcomer", "grade": "pass", "evidence": "No CONTRIBUTING.md or AI policy found; silence passes"},
      {"name": "solution-located", "grade": "pass", "evidence": "File and specific crashing code path named in body"},
      {"name": "newcomer-signalled", "grade": "pass", "evidence": "Labels: good first issue, rag, tier-1"},
      {"name": "discussion-settled", "grade": "pass", "evidence": "Empty thread; no disagreement"}
    ],
    "verdict": "accept"
  }
]
```
```

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run, first rubric draft (5 required + 3 preferred checks): **16/20 scored items**. Missed issue-09, issue-14, issue-19 (all `clear-accept`, wrongly rejected) and issue-20 (`scope`, wrongly accepted). Category floor held (all categories ≥1), but 16/20 is below the 18/20 bar.
2. Partial re-run with `--only issue-09,issue-14,issue-19,issue-20,issue-05,issue-10,issue-15` after revising `unclaimed`, `maintainer-responsive`, `bounded-scope`, and adding `spec-is-concrete`: **7/7 scored items** agreed (checking both the 4 disagreements and 3 issues at risk of breaking from the rewording).
3. Full confirming run: **19/20 scored items**, bar PASS. Only miss: issue-15 (rubric graded `accept`, gold is `reject`).
4. Full run with `--save-run eval-run.txt` (the committed run): **19/20 scored items**, bar PASS — matches the agreement line in the committed `eval-run.txt`.

**Issue analysis**

`issue-09` (category `clear-accept`). My rubric's verdict: **accept**. Gold label: **accept**.

The bundle (`conda/conda#7617`) has a comment from 2022-01-20 where a non-author says "I'd like to take a swing at this as my first open-source contribution," and a maintainer replies "go for it." On my first rubric draft, this failed my `unclaimed` check outright — any claim comment blocked the issue, with no regard for age. But the thread continues: a stale-bot marks the issue inactive in 2023-01-21, and the maintainer just comments "(bump)" — no further activity from the claimant since 2022. Gold's note is explicit about this: "the 2022 claim is stale and the maintainer invited takers." I revised `unclaimed` to require that a blocking claim be dated within 90 days of the bundle's capture date, *or* show no evidence of lapsing since; a claim followed by a stale-bot mark and years of silence counts as abandoned rather than active. With that change, `unclaimed` passes (evidence: "last claim... is 2+ years before capture and [the tracker] auto-unassigns after 14 days inactivity"), and the issue accepts, matching gold.

**Check rationale**

The `unclaimed` check, quoted as currently written in `rubric.md`:

> `assignees: none` AND `linked PRs: none` (or every linked PR is closed, not merged) AND no comment from a non-author claiming the issue (e.g. "I'll take this", "working on this now", "PR incoming", a bot's claim-confirmation reply) dated within 90 days of the capture date with no sign of abandonment since. A claim older than 90 days, or one followed by a stale-bot mark, an auto-unassignment, or several months of silence, is abandoned and does not block — evidence the abandonment, don't just note the claim's age. A claim inside the 90-day window, or one with no evidence of lapsing, still blocks.

Reasoning: my first draft blocked on the mere existence of a claim comment, which is too blunt — a "first contribution" claim from years ago that nobody followed up on isn't actually competition for a newcomer today. The 90-day window plus an explicit abandonment carve-out (stale-bot mark, auto-unassignment, or months of silence) tries to separate "someone is actively working on this right now" from "someone once said they would, and then didn't." I required the grader to *evidence* the abandonment rather than just note the claim is old, so it can't wave away a claim that's merely a few weeks stale with no actual sign of lapsing.

**Trade-offs**

The same leniency that makes `unclaimed` correctly accept issue-09 lets a real caution signal slip through elsewhere: `issue-15` (`zulip/zulip#19589`, category `scope`) is graded `accept` by my rubric on a re-run, but gold says `reject`, noting "years of design debate and two abandoned PRs." That bundle shows *many* claim/auto-unassign cycles (at least eight different contributors claiming and lapsing between 2021 and 2024) and two linked PRs that were closed without merging. My `unclaimed` check reads every one of those claims as abandoned (correctly, by its own definition) and passes — but it has no way to notice that a long *history* of abandoned attempts and unmerged PRs is itself evidence the issue is harder or more contested than it looks, which is exactly what sank it in gold's read. The check only asks "is someone working on it right now," not "has this repeatedly failed to close." I re-ran this single issue with `--only issue-15` during revision and confirmed it flips between `accept` and `reject` run to run — the rubric doesn't reliably see the problem gold is pointing at, and I'm accepting that miss rather than adding a check I can't state a clean, generalizable pass condition for. Issue-15 is one of the `scope` category's 4 genuinely arguable calls, and the eval run still clears the 18/20 bar with the category floor intact.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

> **Draft below — read this and rewrite it in your own words before submitting.** I put
> together a starting answer from what you told me (Python/JS-TS comfortable, want a real
> bug fix in app code, no strong frontend/backend preference, want it traceable and
> bounded) plus what the live-mode run surfaced. This field is graded on being in your own
> words, so treat this as a scaffold, not a final answer.

1. **Fit to interests and time available.** #62 is a small FastAPI health-check bug: `settings.redis_host` doesn't exist on the `Settings` object, so the health probe crashes instead of reporting Redis as down. It's in Python, in a domain (backend config/API code) I'm comfortable navigating, and both my rubric and the skill's own time estimate put it at the low end (roughly 1–2 hours), which fits picking a first issue I can actually finish rather than one that turns into a multi-day rabbit hole.

2. **What the verdict identified correctly, and what I weighed that the rubric couldn't.** The rubric correctly flagged that the fix is precisely located — it names the exact file (`api/routes/health.py`) and the exact wrong-vs-right attribute names, so there's no guessing about where to look or what "done" means. What the rubric can't weigh is my own comfort level: of the three accepted issues, #62 needs the least new domain knowledge (no passlib exception hierarchy to learn like #72, no RAG/LLM-output context to absorb like #69), so it's the one where I'll spend my time on the actual fix and the PR process rather than on background reading.

3. **Anticipated difficulty in claiming it.** Low-to-moderate. I'll need to find where `Settings` is defined (likely a `config.py` or `settings.py`), confirm the correct attribute is `redis_url` (per the issue body), and reproduce the crash — probably by hitting the health endpoint locally or running the relevant test. The main friction I expect is environment setup (getting the FastAPI app or its test suite running locally at all) rather than the fix itself, which is a small, well-specified change.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
