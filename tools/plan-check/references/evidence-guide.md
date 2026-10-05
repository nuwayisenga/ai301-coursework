# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

**Where it lives:**
- In an eval bundle: the repro-evidence block — specifically the control runs, step-by-step outputs, error messages, and any artifact that pins down what the bug actually is. Then the plan's diagnosis statement (usually labeled "Diagnosis" or at the top of the plan).
- In live mode: the student's posted repro comment on the issue; the plan.md's diagnosis section; the issue body and thread highlights for the original reported symptom.

**What good looks like:**
The cause named in the plan appears in or is directly supported by the repro artifacts — the same error type, the same failing component, the same behavior the control runs isolate. A diagnosis that names a component the control run shows working, or that contradicts the step-by-step evidence, fails even if it sounds plausible in the abstract.

Look specifically for lines labeled "Control:" or equivalent isolating runs in the repro-evidence block. A control that shows the named component working correctly in a simpler context — for example, the same expression succeeding at the top level but failing inside a wrapper — rules that component out as the primary defect. When a control run directly contradicts the named cause (the plan says "X is broken" but the control shows X working), grade diagnosis-grounded as fail regardless of how plausible the diagnosis sounds.

## Scope

**Where it lives:**
- In an eval bundle: the plan's in-scope and not-in-scope lines, or any bounding language ("only X, not Y"). The issue description for what the reporter actually asked about.
- In live mode: the plan.md's scope section; the issue body; the thread for any scope signals from the maintainer.

**What good looks like:**
One named fix, one file or area, with an explicit statement of what is not in scope. Extra migrations, refactors, or cross-cutting changes that the issue didn't request fail. A scope-down is fine if stated with reasons; adding unrequested work is not.

## Executability

**Where it lives:**
- In an eval bundle: the plan's approach or steps section; any named files, functions, or modules; any named ordering of work.
- In live mode: the plan.md's approach section; any specific code locations named in the plan or comment.

**What good looks like:**
Files or areas are named; the approach is chosen (not "investigate and see"); a stranger could start without asking the author what to do. A plan with no named file and no chosen approach fails. A plan that names files and approach but defers one open sub-question (e.g., "counter or flag — I'll benchmark both") while giving enough to start passes.

## Test plan

**Where it lives:**
- In an eval bundle: the plan's test plan section; the repro-evidence block's steps and observable artifacts for what the symptom looks like before the fix.
- In live mode: the plan.md's test plan; the student's repro comment for the observable output to use as a before/after baseline.

**What good looks like:**
The test plan names the specific observable outcome tied to this issue's symptom: the exact crash absent, the exact error message gone, the specific test case passing. It should be checkable without knowing whether a fix was applied — "should feel faster" or "run the test suite" are not checkable. A test plan that re-runs the repro steps and states what the correct output is, passes.

## Honesty

**Where it lives:**
- In an eval bundle: the plan's risk or unknowns section; confidence claims in the plan or comment (words like "I'll have this done", "guaranteed", "trivial fix").
- In live mode: the plan.md's risks section; the comment's tone around uncertain steps.

**What good looks like:**
Deferred decisions are named rather than hidden. Unknowns are stated. A plan that says "I haven't measured the cost yet, will benchmark before PR" is honest; a plan that presents a performance claim as settled when the evidence doesn't show it is not. False confidence claims against incomplete evidence fail the relevant check (usually `plan-executable` or `diagnosis-grounded`).

## Comms

**Where it lives:**
- In an eval bundle: the candidate plan comment, read against the thread highlights (specifically OWNER/CONTRIBUTOR messages that name a fix location, a patch, or next steps) and the repo-facts AI-use policy line.
- In live mode: the draft plan comment; the live issue thread for maintainer signals; the repo's CONTRIBUTING.md and any AI policy.

**What good looks like:**
The comment acknowledges explicit maintainer direction if it exists (a patched binary, a named file, a stated preference). A comment that ignores such direction and proposes something contradictory or orthogonal fails the thread-engagement sub-check. For AI disclosure: the comment includes a disclosure line when the repo's policy explicitly requires it. Human-voiced comments, or comments in repos with permissive or no AI policy, pass without a disclosure line.
