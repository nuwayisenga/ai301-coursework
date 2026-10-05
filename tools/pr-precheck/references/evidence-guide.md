# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives:**
- In an eval bundle: the plan-context block's scope statement (in-scope files, not-in-scope exclusions) and deviation notes. The candidate PR's unified diff (which files changed and what changed in them). The candidate PR's description's fidelity claims.
- In live mode: the student's `plan.md` scope and deviation sections; the output of `git diff main...HEAD` from the working copy; the draft PR description.

**What good looks like:**
Every file touched in the diff appears in the plan's in-scope list or is covered by a deviation note. A deviation note must name the change and give a reason — "also updated CHANGELOG.md, required by CONTRIBUTING.md" is a note; a silently changed file with no mention is drift. The description's claims must match the diff: if the description says "implements the plan exactly" but the diff adds an unrequested config option or rewrites an unrelated module, that claim fails regardless of whether the extra work is adjacent to the fix. Drift can go either direction — more than the plan (unrequested scope) or less than the plan with the description claiming otherwise (missing work the description says is present).

## Test evidence (harness category: not-tested)

**Where it lives:**
- In an eval bundle: the candidate PR's test-evidence section; the plan-context block's test plan (named scenarios, observable outcomes, and any repo-check requirements like "run the parser test suite").
- In live mode: the student's captured terminal output, test run transcripts, or before/after command output; the student's `plan.md` test plan section.

**What good looks like:**
For each scenario the plan's test plan names, the evidence section shows: (a) a before state — the crash, wrong output, or failing assertion that the issue described — and (b) an after state — the corrected output or passing assertion that proves the fix works. The before/after must be specific to the issue's symptom, not to an adjacent path. Evidence that shows only "cargo test passes" or "tested locally, works now" without naming the scenario and its observable outcome fails. Evidence that runs the unchanged path (the path that never triggered the bug) also fails — it proves the tool still works where it always worked, not that the fix addressed the bug.

A scenario the plan explicitly defers in its deviation notes is not a required evidence scenario.

## Diff quality (harness category: unreviewable)

**Where it lives:**
- In an eval bundle: the candidate PR's unified diff and the commit list.
- In live mode: the output of `git diff main...HEAD` and `git log main..HEAD --oneline`.

**What good looks like:**
The diff contains only the fix and its direct consequences: the changed logic, any new or updated tests for the fix, and any metadata the repo's contributing guide requires (CHANGELOG entry, whatsnew entry). Everything else is debris. Specific debris tells:
- Debug print statements left in production code paths
- Commented-out code (failed first attempts, experimental blocks, removed logic left as a comment)
- Dead functions or dead imports that exist only because the author forgot to clean up
- Formatting-only hunks (re-indentation, trailing-whitespace removal, line-length fixes) on lines the fix itself does not touch
- Unrelated files changed (a second module rewritten, a config option added) that the plan never named

Commit messages of "wip", "fix", "fix fix", or "fmt + cleanup" without context are also a signal — they indicate the branch was not cleaned before submission.

## Standards and comms (harness category: standards-wall)

**Where it lives:**
- In an eval bundle: the repo-facts block's `pull requests:` line (which names the template's required sections and any AI-use policy) and the candidate PR's description.
- In live mode: the repo's PR template file (`.github/pull_request_template.md` or equivalent) and CONTRIBUTING.md; the draft PR description.

**What good looks like:**
The PR's description visibly fills every section the repo's template marks as required. A section that is present but left as boilerplate (the template's own placeholder text still showing) or written as a blanket "N/A" for a required field is not filled. Specific required-section examples from real repos: a `closes #xxxx` line with the actual issue number, a `whatsnew` entry when the contributing guide requires one, a `How Has This Been Tested` section with actual test output. A repo whose template has no explicitly required sections passes automatically.

For AI disclosure: a repo that explicitly requires disclosing AI use (phrases like "all AI usage must be disclosed", "AI-assisted contributions must state the tool used", "if you used an AI tool, state it in your PR") mandates a disclosure line in the PR description. Every package is treated as AI-assisted work — the only question is whether the repo's stated policy explicitly mandates disclosure. A permissive policy ("you may use AI tools") or no policy at all passes automatically.

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->
