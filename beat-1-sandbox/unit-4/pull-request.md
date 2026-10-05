# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/93

**Branch**

fix/62-redis-health-check-attr-error

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Smoke run (`--limit 3`, packages pkg-01, pkg-02, pkg-03): 3/3.

Confirming full run (`--save-run eval-run.txt`): 20/20.

```
categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3
agreement: 20/20 scored items  (bar: 18/20: PASS)
```

No intermediate iterations were needed; the rubric passed 20/20 on its first full run.

**Package analysis**

pkg-13 (clear-accept category). My rubric graded `accept`; gold label is `accept`.

pkg-13 is a caret-escaping fix for cmd metacharacters. The plan's deviation notes explicitly defer `%VAR%` expansion with a stated reason — the maintainer discussion is cited, and the description restates the deferral. The test evidence shows before/after for the caret-escaping scenario only; it is silent on `%VAR%` expansion because that scenario was deferred. My `evidence-decisive` check passed because it exempts scenarios the plan's deviation notes explicitly defer: "A scenario explicitly deferred in the plan's deviation notes is not required to be evidenced." Without this carve-out the package would have been a false reject — the evidence covers exactly what the plan commits to, and the uncovered scenario is the one the plan honestly does not commit to.

**Check rationale**

From `rubric.md`, the `evidence-decisive` check reads:

> "The evidence shows an observable outcome specific to the issue's symptom for every scenario the plan's test plan names: a before state (crash, wrong output, or failing assertion) and an after state (the fix confirmed), for each named case. Evidence that runs only the unchanged path (a path that does not exercise the bug), or that asserts 'tests pass' or 'tested locally' without naming the specific case and its before/after, fails. A scenario explicitly deferred in the plan's deviation notes is not required to be evidenced."

This check was written to distinguish two separate failure modes that the gold labels split across different packages: (a) evidence that runs the wrong path — the unchanged one that never triggered the bug (pkg-07, pkg-14), and (b) evidence that is vague ("tested locally, works now") with no observable output for the specific symptom (pkg-04, pkg-10). Both patterns fail for the same underlying reason: neither lets a stranger verify the fix from the package. The deviation-note carve-out was included from the start because the calibration packages (calib-04) and clear-accept packages with honest shortfalls (pkg-13, pkg-16) show that a well-scoped deferral is not missing evidence — it is the plan doing its job.

**Trade-offs**

The `evidence-decisive` check requires before/after for every test-plan scenario the plan commits to. The trade-off is that it can hold a PR that clearly fixes the symptom but where the author forgot to re-run one minor named scenario from the test plan. The check has no way to distinguish "forgot to include the output" from "never ran the check." I accept this trade-off because: (a) the 20/20 run shows no current false rejects from this condition across all 20 packages, and (b) a scenario the plan names and the author does not show evidence for is genuinely unverifiable from the package — the contract's own default is that an unverifiable claim fails.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
