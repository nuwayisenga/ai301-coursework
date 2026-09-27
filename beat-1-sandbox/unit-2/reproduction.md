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

nuwayisenga

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5852715644

Hii, I'd like to work on this as my Unit 2 reproduction. I reproduced the `redis_host` AttributeError locally on commit `2f4e82f` — the health probe reads `settings.redis_host` and `settings.redis_port`, while `Settings` in `core/config.py` only exposes `redis_url`, so the AttributeError is caught and Redis is reported as unhealthy even when Redis is reachable. I'll write up a full repro report (environment, steps, artifacts) and post it here before touching any code.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5858370614

Reproduction report — issue #62: `/health` reports Redis unhealthy due to missing `redis_host` attribute

**Commit:** `2f4e82f`

**Environment**

- macOS 14.3.1, Apple Silicon (arm64)
- Python 3.11.9 (Homebrew)
- Docker 28.3.3 / Docker Compose v2.39.2
- Redis container: `redis:7-alpine` via `docker-compose.yml`

**To Reproduce**

1. Clone the fork, copy `.env.example` to `.env` (no changes needed).
2. Start services: `docker compose up -d` — postgres on port 5433, redis on port 6379.
3. Create venv and install: `python3.11 -m venv .venv && source .venv/bin/activate && make setup`
4. Start the API: `make run`
5. Confirm Redis is actually reachable:
   ```
   $ docker compose exec redis redis-cli ping
   PONG
   ```
6. Hit the health endpoint:
   ```
   $ curl -s -w "\nHTTP %{http_code}\n" http://localhost:8000/health
   ```

**Expected Behavior**

`/health` should return HTTP 200 with `"redis": "healthy"` when the Redis container is up and responding to PING.

**Actual Behavior**

HTTP response (503):
```json
{
    "detail": {
        "status": "unhealthy",
        "dependencies": {
            "postgres": "unhealthy",
            "redis": "unhealthy",
            "vector_db": "healthy"
        },
        "safety_events_last_hour": 0,
        "timestamp": "2026-09-27T17:15:30.991782"
    }
}
```

Application log (structlog, same request):
```
2026-09-27 13:13:12 [error] redis_health_check_failed  error="'Settings' object has no attribute 'redis_host'"
```

**Root cause**

`api/routes/health.py` calls `settings.redis_host` and `settings.redis_port` to build the connection for the probe, but `core/config.py`'s `Settings` class only exposes `redis_url` — `redis_host` and `redis_port` don't exist. The resulting `AttributeError` is caught by the bare `except Exception` block, logged as `redis_health_check_failed`, and Redis is unconditionally reported as `"unhealthy"`.

**Relevant Files**

- `api/routes/health.py` — health probe reads `settings.redis_host` / `settings.redis_port`
- `core/config.py` — `Settings` has `redis_url`, not `redis_host` / `redis_port`

**Acceptance Criteria**

- `/health` returns `"redis": "healthy"` when the Redis container is reachable
- The `attr-defined` suppression for this file in `pyproject.toml` is removed
- The `xfail` marker on the corresponding unit test is removed after the fix passes

Drafted with Claude Code assistance, reviewed and edited by me.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Early runs during rubric development scored lower as I debugged disagreements on the `conventions-followed` check. After fixing the AI policy logic to handle four distinct cases, the rubric stabilized. Final run: 19/20 (matching `eval-run.txt`).

**Package analysis**

`pkg-05`: gold label `accept`, my rubric decided `reject`. The package is from a repo (conda) whose AI policy reads "AI tools welcome; you are responsible for all contributions" — a permissive-with-responsibility policy. My rubric's `conventions-followed` check initially treated any AI-use policy as triggering a disclosure requirement, so it failed pkg-05 for not disclosing. The gold label accepts it because permissive-with-responsibility policies don't mandate disclosure in comments: the contributor's responsibility is for the quality of their work, not for adding a disclosure line. This is the one stable miss in the run.

**Check rationale**

Quoted from `rubric.md` as it reads now:

> `conventions-followed` | The claim comment and repro report read against the repo-facts contribution policy and AI-use policy lines, and the repo's bug-report template asks. | The comments follow the repo's stated contribution conventions. Four cases for AI policy — read the policy literally before applying any case: (1) **Explicit disclosure required**: the policy says something like "all AI usage must be disclosed" or "AI-assisted issues and comments must state the tool used" — a package treated as AI-assisted must include a disclosure statement; failing to disclose fails this check. (2) **Human-voiced requirement**: the policy says comments must be "written by humans in their own words" or "AI-generated comments may be hidden" — human-sounding comments in the contributor's own voice satisfy this; no explicit disclosure line needed. (3) **Permissive-with-responsibility**: the policy says AI tools are welcome but the contributor is responsible for their work (e.g., "must review and understand AI-generated content before including it in a PR") — no disclosure in comments is required; this passes. (4) **No AI policy stated**: passes. | required

It reads this way because I initially had a single binary rule (any AI policy mention = requires disclosure). That wrongly failed pkg-03 (ripgrep's "comments must be written by humans" policy, which is a voice requirement, not a disclosure mandate) and pkg-05 (conda's permissive policy). I added the four-case structure to match what the gold labels distinguish: case (1) for explicit mandates, case (2) for human-voice requirements, case (3) for permissive policies, and case (4) for silence.

**Trade-offs**

The four-case logic accepts anything that isn't an explicit disclosure mandate, which means it could miss a policy that uses unusual wording to require disclosure without saying so literally. I accept this: the check says "read the policy literally before applying any case," and case (1) only triggers when the policy's own words explicitly name disclosure in comments. pkg-05 is the known edge case — it still misses because the conda policy ("AI tools welcome; you are responsible") can be read as ambiguous, and the model sometimes applies case (1) instead of case (3) on re-runs. The rubric passes the bar at 19/20 with full category coverage, so this is an acceptable stable miss.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
