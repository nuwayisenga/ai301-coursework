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

nuwayisenga

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62#issuecomment-5988275602

**Plan for #62 — redis_host/redis_port AttributeError in health endpoint**

**Diagnosis**

The health endpoint's Redis probe builds a client with `redis.Redis(host=settings.redis_host, port=settings.redis_port, ...)`. `Settings` in `core/config.py` only defines `redis_url` — no `redis_host` or `redis_port` attributes exist. The probe raises `AttributeError: 'Settings' object has no attribute 'redis_host'`, the `except Exception` block catches it silently, and the endpoint always returns `"redis": "unhealthy"`. My repro confirmed Redis is reachable (`redis-cli ping → PONG`) — the failure is the attribute mismatch, not a connectivity issue.

**Scope**

In scope:
- `api/routes/health.py`: replace the broken `redis.Redis(host=..., port=...)` call with `redis.from_url(settings.redis_url, decode_responses=True)`
- `pyproject.toml`: remove the `attr-defined` entry from the `api.routes.health` mypy override (it suppresses this exact error; fixing the bug makes the suppression unnecessary)

Out of scope: `core/config.py` (the `redis_url` field is correct), any other health probe, any refactor of the health check structure.

**Approach**

Replace lines 46–51 of `api/routes/health.py`:
```python
r = redis.Redis(
    host=settings.redis_host,
    port=settings.redis_port,
    db=0,
    decode_responses=True,
)
```
with:
```python
r = redis.from_url(settings.redis_url, decode_responses=True)
```

`redis.from_url` is the canonical way to initialize a Redis client from a URL string and correctly parses host, port, and database from the existing `redis_url` field.

**Test plan**

Re-run the repro steps with Docker Compose running:
1. `redis-cli ping` → `PONG` (unchanged)
2. `curl -s http://localhost:8000/health | python3 -m json.tool` → `"redis": "healthy"` and HTTP 200 (was: `"redis": "unhealthy"`, HTTP 503)
3. `make test-unit` exits 0 with no failures

**Risk**

Minimal — one-line change using the same library. The default `redis_url` (`redis://localhost:6379/0`) expresses the same host, port, and database the original code intended.

---
*Drafted with Claude Code assistance, reviewed and edited by me.*

---

## Your branch

**Branch**

fix/62-redis-health-check-attr-error

**Evidence**

Before (from Unit 2 repro, issue #62 unpatched):

```
$ redis-cli ping
PONG

$ curl -s "http://$(docker compose port api 8000)/health" | python3 -m json.tool
{
    "detail": {
        "status": "unhealthy",
        "dependencies": {
            "postgres": "unhealthy",
            "redis": "unhealthy",
            "vector_db": "unavailable"
        },
        "safety_events_last_hour": 0,
        "timestamp": "..."
    }
}
# HTTP 503

$ docker compose logs api --tail=5 | grep redis
... redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"
```

After (fix/62-redis-health-check-attr-error branch, Docker not available during this session — unit tests run instead):

```
$ source .venv/bin/activate && python -m pytest tests/unit/ -x -q
.........................x........x.....x........................x......  [ 67%]
..........xx.x..xx...xxxxx...xxx.xxx.xx.....................x........... [ 84%]
..........x....xx..xx.......x...............x.x.....................     [100%]
375 passed, 53 xfailed, 3 warnings in 19.77s
```

Pre-commit hooks (ruff, black, mypy) all passed on commit — mypy no longer flags `attr-defined` on `api/routes/health.py` after removing the suppression, confirming the attribute error is gone.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 (full): 19/20 — pkg-11 (wrong-cause category) graded `accept` but gold label is `reject`.

After tightening `references/evidence-guide.md` to explicitly call out "Control:" lines and specify that a control run contradicting the named cause means fail regardless of plausibility, re-ran canary `--only pkg-11,pkg-01,pkg-06,pkg-12,pkg-18` → pkg-11 now correctly grades `reject`.

Run 2 (confirming full run with `--save-run eval-run.txt`): 20/20.

```
agreement: 20/20 scored items  (bar: 18/20: PASS)
categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4
```

**Package analysis**

pkg-11 (wrong-cause category). My rubric graded `accept`; gold label is `reject`.

The plan diagnosed the yq collect operator (`[.a]`) as the defect. The rubric's `diagnosis-grounded` check should have caught a control run in the repro-evidence block showing `yq -n '{} | ([.a] | length)'` evaluated to `1`, proving the collect operator works correctly in isolation. Instead, on the first run, the check passed because the rubric's evidence guide described looking for artifact matches without explicitly addressing how to handle a control run that rules out the named component. The plan sounded plausible (collect operators can behave unexpectedly with missing keys), and without explicit guidance on control runs, the rubric accepted it.

**Check rationale**

From `rubric.md`, the `diagnosis-grounded` check reads:

> "The cause named in the plan is consistent with what the repro artifacts show. A diagnosis that contradicts a control run (e.g., blames a component the control proves working), names a cause the repro evidence rules out, or ignores what the repro evidence directly shows fails. A terse diagnosis that names the same cause the repro evidence isolates passes even if short."

This wording was tightened after the pkg-11 false accept on the first run. The original rubric did not explicitly mention "contradicts a control run" as a fail condition — it described the general principle of consistency with repro artifacts. After pkg-11 showed that a plausible-sounding diagnosis could pass when a control run directly ruled out the named cause, I added the parenthetical "(e.g., blames a component the control proves working)" and extended `references/evidence-guide.md` with:

> "Look specifically for lines labeled 'Control:' or equivalent isolating runs in the repro-evidence block. A control that shows the named component working correctly in a simpler context — for example, the same expression succeeding at the top level but failing inside a wrapper — rules that component out as the primary defect. When a control run directly contradicts the named cause (the plan says 'X is broken' but the control shows X working), grade diagnosis-grounded as fail regardless of how plausible the diagnosis sounds."

**Trade-offs**

The tightened control-run guidance makes `diagnosis-grounded` stricter: a plan that names a component as the cause will now fail if *any* control run shows that component working in a simpler context, even if the component could theoretically behave differently in the specific wrapper context where the bug occurs. This risks a false reject if a component is genuinely context-sensitive (works at top level, fails inside a specific wrapper for a legitimate reason). I accepted this trade-off because: (a) the 20/20 full run shows no current false rejects from this tightening across all 20 packages, and (b) a control run that cleanly contradicts the named cause is strong evidence — a plan that ignores it is not ready to build from regardless.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
