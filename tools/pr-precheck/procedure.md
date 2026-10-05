# Procedure: how this tool grades a PR package

## Read order

Read in this order before grading any check:

1. **Repo facts**: note the PR template's required sections word-for-word, and the AI-use policy word-for-word (none, permissive, or explicit-disclosure-required). If the policy mandates disclosure, note that phrase exactly.
2. **Issue description**: record the reported symptom and the expected behavior — these are what the fix must address and what the test evidence must demonstrate.
3. **Thread highlights**: flag any OWNER or CONTRIBUTOR message that names a fix location, posts a patch, or gives explicit direction. Note if none exists.
4. **Plan context**: read the plan's scope statement (in-scope files and changes, not-in-scope exclusions) and the deviation notes. Record the plan's test plan — every named scenario, observable outcome, and stated repo-check requirement. Record which scenarios, if any, are explicitly deferred in the deviation notes.
5. **Candidate PR title and description**: read as a stranger on the thread would. Note any fidelity claims ("implements the plan exactly", "no other changes", "docs now document X").
6. **Diff**: read the unified diff file by file. Note every changed file, whether it falls inside the plan's scope, and any content that looks like debris (debug prints, commented-out blocks, dead code, formatting-only changes, unrelated files).
7. **Commits**: read the commit list. Note any message that is non-descriptive ("wip", "fix", "fmt").
8. **Test evidence**: read the test-evidence section. Note which scenarios are covered with observable before/after output and which are described with assertions only ("tests pass", "tested locally").

Rationale for this order: the plan's scope and test plan must be recorded before the diff is read (so each changed file can be checked against the scope) and before the evidence is read (so each scenario can be matched against what's covered).

## Evidence gathering

For each check, gather from the already-recorded notes:

- **diff-matches-plan**: the plan's scope pair and deviation notes (from step 4); the diff's changed files (from step 6); the description's fidelity claims (from step 5). Pair each changed file against the plan's in-scope list. Pair each description claim against the diff's actual contents.
- **evidence-decisive**: the plan's test plan scenarios (from step 4, minus any explicitly deferred); the test-evidence section (from step 8); the issue's symptom (from step 2). For each required scenario, check whether the evidence section shows it with a before state and an after state.
- **diff-reviewable**: the diff content (from step 6) and the commit messages (from step 7). List any debris found.
- **standards-met** (two sub-checks, gather separately):
  - Template sub-check: the required template sections (from step 1); the PR description (from step 5). For each required section, check whether it is filled with real content.
  - Disclosure sub-check: the AI-use policy note (from step 1); the PR description (from step 5). Check whether an explicit disclosure mandate exists and whether the description includes a disclosure line.

## Check execution

Execute checks in this order:

1. **diff-matches-plan**: compare each changed file against the plan's scope. If a changed file is outside the scope and no deviation note covers it, grade fail. If the description claims something the diff does not contain, or claims "no changes beyond the plan" when the diff shows unrequested work, grade fail. If every changed file is within scope or covered by a deviation note, and description claims match the diff, grade pass. Grade unclear only if the plan's scope section is entirely absent from the package.

2. **evidence-decisive**: for each scenario the plan's test plan names (excluding deferred ones), check whether the test-evidence section shows an observable before state and an observable after state for that specific scenario. If any named scenario is covered only by "tests pass" or "tested locally" with no observable output, or not mentioned at all, grade fail. If a scenario is covered only by exercising an unchanged path (the path that does not trigger the bug), grade fail. If every required scenario has observable before/after, grade pass. Grade unclear only if the test-evidence section is absent entirely.

3. **diff-reviewable**: if any debris is found (debug prints, commented-out code, dead functions, formatting-only hunks on unrelated lines, unrelated files, non-descriptive commit messages), grade fail. If the diff contains only the fix and its direct consequences, and commits are descriptive, grade pass.

4. **standards-met**: run both sub-checks:
   - Template: if any required section is blank, boilerplate, or absent, grade fail. If all required sections are filled or the repo has no required sections, this sub-check passes.
   - Disclosure: if an explicit mandate exists and the PR has no disclosure line, grade fail. If no mandate exists or the PR includes a disclosure line, this sub-check passes.
   - Grade the check: pass only if both sub-checks pass; fail if either fails.

When evidence for a check is genuinely absent (the plan has no scope section, the test-evidence section is missing), grade the affected check `unclear` and note what is missing.

## Verdict assembly

Apply the rubric's verdict rule:
- If every required check grades `pass` → verdict `accept`.
- If any required check grades `fail` → verdict `reject`.
- If any required check grades `unclear` → treat as `fail` → verdict `reject`.

In the JSON output, include one entry per check with its name, grade, and the one-line evidence fact or quote that decided it. When multiple checks fail, report all of them in the JSON; the first failing required check in rubric order (diff-matches-plan → evidence-decisive → diff-reviewable → standards-met) is the one whose evidence line summarizes the rejection in a human-readable summary before the JSON block.

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->
