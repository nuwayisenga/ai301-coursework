# Procedure: how this skill grades a plan package

## Read order

Read in this order before grading any check:

1. **Repo facts** first: note the AI-use policy word-for-word (none, permissive, explicit-disclosure-required), the bug-report template asks, and the contribution policy.
2. **Issue description**: record the reported symptom and what behavior the reporter says should happen instead.
3. **Thread highlights** in chronological order: flag any OWNER or CONTRIBUTOR message that names a fix location, posts a patch, or gives explicit next-step direction. If no such message exists, note that.
4. **Repro-evidence block**: record (a) what the control runs show is ruled in or out as a cause, (b) the exact artifact (error message, crash, wrong output) the steps produce, and (c) what the repro evidence pins down as the trigger.
5. **Candidate plan**: read the diagnosis, scope statement, approach, named files, and test plan.
6. **Candidate plan comment**: read it as a stranger on the thread would — does it engage the thread, does it include a disclosure line if required?

Rationale for this order: the repro evidence must be understood before the diagnosis can be checked; the thread highlights must be read before the comment can be checked for engagement.

## Evidence gathering

For each check, gather from:

- **diagnosis-grounded**: the plan's diagnosis statement + the repro-evidence block's control runs and artifacts. Record the cause the plan names, and the behavior the repro evidence pins down.
- **scope-bounded**: the plan's in-scope/not-in-scope lines + the issue description. Record what the issue asks for and what the plan includes beyond that.
- **plan-executable**: the plan's approach and any named files or areas. Record whether a file is named and whether an approach is chosen.
- **test-decisive**: the plan's test plan + the repro-evidence block's observable artifact. Record the test plan's stated outcome and whether it names the specific symptom.
- **thread-and-conventions** (two sub-checks, gather separately):
  - Thread sub-check: the flagged OWNER/CONTRIBUTOR direction from step 3 of the read order; the plan comment. Record whether the comment acknowledges that direction.
  - Disclosure sub-check: the AI-policy note from step 1 of the read order; the plan comment. Record whether an explicit disclosure mandate exists and whether the comment includes a disclosure line.

## Check execution

Execute checks in this order:

1. **diagnosis-grounded**: compare the cause named in the plan against what the repro evidence's control runs and artifacts show is ruled in or out. Grade pass if consistent, fail if contradictory or ignoring the repro evidence, unclear if the repro evidence block is absent.
2. **scope-bounded**: compare what the plan includes against what the issue identifies. Grade pass if bounded to the issue, fail if unrequested changes are added, unclear if neither scope nor issue description contains enough to judge.
3. **plan-executable**: check for a named file or area and a chosen approach. Grade pass if both exist, fail if neither exists or all key decisions are deferred to build time, unclear if the plan is absent.
4. **test-decisive**: check whether the stated outcome is specific to the issue's symptom. Grade pass if the outcome is observable and issue-specific, fail if vague ("should work better") or non-discriminating ("run the test suite"), unclear if the test plan section is absent.
5. **thread-and-conventions**: run both sub-checks:
   - Thread: if flagged direction exists, check whether the comment engages it. Pass if engaged or acknowledged; fail if the comment pursues a contradictory or orthogonal approach without engagement; pass automatically if no OWNER/CONTRIBUTOR direction was flagged.
   - Disclosure: if the repo's policy explicitly mandates disclosure in comments, check for a disclosure line. Pass if present; fail if absent and policy mandates it; pass automatically if no explicit mandate exists.
   - Grade the check: pass only if both sub-checks pass; fail if either sub-check fails.

When evidence for a check is genuinely absent (the repro-evidence block is missing, the plan has no scope section, the thread highlights are empty), grade the affected check `unclear` and note what is missing.

A check may be graded from the already-recorded notes without re-reading the full package, as long as the notes contain the relevant evidence. Re-read the specific section if the notes are ambiguous.

## Verdict assembly

Apply the rubric's verdict rule:
- If every required check grades `pass` → verdict `accept`.
- If any required check grades `fail` → verdict `reject`; quote the failing check's name and the one-line evidence fact in the JSON output.
- If any required check grades `unclear` → treat as `fail` → verdict `reject`.
- Preferred checks (none defined) never change the verdict.

In the JSON output, include one entry per check with its name, grade, and the one-line evidence fact or quote that decided it.
