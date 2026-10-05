# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | The plan's in-scope statement and deviation notes, read side-by-side with the diff's changed files and the PR description's fidelity claims. | The diff changes only files and behavior within the plan's stated scope or its documented deviation notes. The description does not claim more or less than the diff delivers — a description saying "implements the plan exactly" over a diff that adds unrequested work fails, as does a description claiming a change the diff does not contain. A deviation note that re-ties a real mismatch (scope expanded or narrowed with a stated reason) counts as scope; a silent change with no note fails regardless of whether the change itself is correct. | required |
| evidence-decisive | The plan's test plan (its named scenarios, observable outcomes, and stated repo-check requirements), read against the PR's test-evidence section and any before/after transcripts. | The evidence shows an observable outcome specific to the issue's symptom for every scenario the plan's test plan names: a before state (crash, wrong output, or failing assertion) and an after state (the fix confirmed), for each named case. Evidence that runs only the unchanged path (a path that does not exercise the bug), or that asserts "tests pass" or "tested locally" without naming the specific case and its before/after, fails. A scenario explicitly deferred in the plan's deviation notes is not required to be evidenced. | required |
| diff-reviewable | The unified diff and the commit list. | The diff contains no debris that buries the fix: no debug print statements, no commented-out code (experiments, dead first attempts, or removed blocks left as comments), no dead functions or dead imports, no formatting-only hunks on lines the fix does not touch, and no unrelated files changed. Commit messages must be descriptive — "wip", "fix", or "fmt + cleanup" without context fails. A diff that is otherwise correct and complete but contains any of these fails. | required |
| standards-met | The repo-facts block's PR template asks and stated AI-use policy, read against the PR's title and description. | Two sub-conditions, both must hold: (a) Template compliance: every section the repo's PR template explicitly requires must be visibly filled with real content — not left blank, not left as template boilerplate, not written as "N/A" for a section the template marks required. A repo with no required template sections passes automatically. (b) AI disclosure: if the repo's stated policy explicitly requires disclosing AI use in PRs (phrases like "all AI usage must be disclosed", "AI-assisted contributions must state the tool used"), the PR must include a disclosure line — every package is treated as AI-assisted. A repo with no AI policy, or a permissive policy that does not mandate disclosure, passes automatically. | required |

## Verdict rule

Accept if and only if every required check grades `pass`.

- A single required check grading `fail` produces `reject`.
- `unclear` on a required check counts as `fail` — a PR that cannot be verified from the package is not ready to submit.
- There are no preferred checks; all checks are required.

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
|  |  |  |  |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
