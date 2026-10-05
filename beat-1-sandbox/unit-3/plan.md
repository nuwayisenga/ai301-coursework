## Issue #62 — Fix plan

**Diagnosis**

The health endpoint's Redis probe calls `redis.Redis(host=settings.redis_host, port=settings.redis_port, db=0, decode_responses=True)` (lines 46–51 of `api/routes/health.py`). The `Settings` class in `core/config.py` only defines `redis_url: str` — it has no `redis_host` or `redis_port` attributes. When the probe runs, Python raises `AttributeError: 'Settings' object has no attribute 'redis_host'`. The `except Exception` block catches it and logs `redis_health_check_failed error="'Settings' object has no attribute 'redis_host'"`, so Redis is always reported unhealthy regardless of actual connectivity. The repro confirms Redis is reachable (`redis-cli ping` → `PONG`) — the failure is the attribute mismatch, not a network issue.

**Scope**

In scope:
- `api/routes/health.py`: replace the broken `redis.Redis(host=..., port=...)` instantiation with `redis.from_url(settings.redis_url)`, which uses the field that actually exists on `Settings`
- `pyproject.toml`: remove the `attr-defined` error code from the `api.routes.health` mypy override — it suppresses this exact attribute error, and removing the bug removes the need for the suppression

Out of scope:
- `core/config.py`: `redis_url` is already correct; no changes needed there
- Any refactor of the health check structure or other health probes
- Any other issue suppressed in the same mypy override block

**Approach**

In `api/routes/health.py`, replace lines 46–51:
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

In `pyproject.toml`, remove `"attr-defined"` from the `disable_error_code` list for the `api.routes.health` module override.

**Test plan**

Re-run the same steps used to reproduce the bug, with Docker Compose running:

1. `redis-cli ping` → still returns `PONG` (unchanged; confirms Redis is reachable)
2. `curl -s http://localhost:8000/health | python3 -m json.tool` → response body contains `"redis": "healthy"` and top-level `"status": "healthy"` with HTTP 200 (was: `"redis": "unhealthy"`, HTTP 503)
3. `make test-unit` exits 0 with no failures

The second step is the decisive check: it directly observes the symptom the issue reports — the health endpoint returning unhealthy for Redis when Redis is running — and confirms it is fixed.

**Risk**

Minimal. `redis.from_url` is the canonical way to initialize a Redis client from a URL string in the same library. The default `redis_url` value (`redis://localhost:6379/0`) expresses the same host, port, and database the original code intended. No logic changes, no new dependencies, no effect on other probes in the same handler.

## Deviations

Nothing changed; the plan held. The fix was exactly what the plan described: replaced the broken attribute access with `redis.from_url(settings.redis_url)` and removed the `attr-defined` mypy suppression from `pyproject.toml`.
