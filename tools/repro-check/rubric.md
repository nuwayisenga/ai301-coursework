# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment record (OS, shell, tool version, platform details) read against the issue's stated target and the repo-facts bug-report template asks. | The report names the OS, the relevant tool version, and any other platform detail the issue or the repo's bug-report template asks for. A version delta from the issue's target (e.g., grading on a newer release) is allowed if the reporter acknowledges it explicitly. Omitting the OS entirely, or omitting a version when the issue is version-specific, fails. | required |
| steps-followable | The repro report's steps section, read against the issue's stated trigger and starting state. | The steps are complete enough that a stranger starting from scratch could follow them: they name the starting state, the exact commands or inputs, and lead to the trigger. A step that assumes a private config file, a private repo, or an unshared environment detail that is load-bearing for the trigger fails. A missing step that is obvious from context (e.g., "install the tool first") does not fail. | required |
| behavior-matches-issue | The repro report's artifacts (output excerpts, logs, screenshots, terminal captures) read against the issue's description of the bug's observed behavior. | The artifact shows the exact behavior the issue reports — the same error type, crash, wrong output, or observable symptom — not a superficially similar one. An adjacent behavior (different error code, different error message, different failure mode) fails even if the report narrates it as confirming the issue. An honest cannot-reproduce that names what the reporter tried and what differed passes: the outcome is stated faithfully and the attempt is evidenced. | required |
| honest-outcome | The repro report's conclusion, read against the artifacts shown in the same report. | The report states the outcome that matches the evidence it actually shows. A report that shows the bug's behavior and says so passes. A report that cannot reproduce and says so with an explanation of what differed passes. A report that shows an adjacent or wrong behavior but narrates it as confirming the issue fails, as does one that makes strong confidence claims ("guaranteed reproducible", "100% confirmed") without matching artifacts. | required |
| claim-is-specific | The claim comment, read against the issue. | The claim names the specific issue (by title or symptom), states what the claimer will do next (reproduce the bug, test a patch, investigate the cause), and does not promise a fix, a timeline, or a guarantee. A generic "+1", "I'll take this", "any updates?" or a promise like "I'll have a fix in 2 days" fails. | required |
| conventions-followed | The claim comment and repro report read against the repo-facts contribution policy and AI-use policy lines, and the repo's bug-report template asks. | The comments follow the repo's stated contribution conventions. Four cases for AI policy — read the policy literally before applying any case: (1) **Explicit disclosure required**: the policy says something like "all AI usage must be disclosed" or "AI-assisted issues and comments must state the tool used" — a package treated as AI-assisted must include a disclosure statement; failing to disclose fails this check. (2) **Human-voiced requirement**: the policy says comments must be "written by humans in their own words" or "AI-generated comments may be hidden" — human-sounding comments in the contributor's own voice satisfy this; no explicit disclosure line needed. (3) **Permissive-with-responsibility**: the policy says AI tools are welcome but the contributor is responsible for their work (e.g., "must review and understand AI-generated content before including it in a PR") — no disclosure in comments is required; this passes. (4) **No AI policy stated**: passes. Separately, if the bug-report template asks for specific fields (OS, version, steps, expected vs actual), the repro report must address them. When in doubt which case applies, read the policy's exact words: only a policy that explicitly names disclosure in comments triggers case (1). | required |

## Verdict rule

Accept if and only if every `required` check grades `pass`.

- A single `required` check grading `fail` produces `reject`.
- `unclear` on a `required` check counts as `fail`. If the evidence a check names is genuinely absent from the bundle, the package is not ready to post.
- In a claim-only draft: checks whose evidence is the repro report grade `unclear` with note "not yet applicable: claim-only draft" and are excluded from the verdict rule. The verdict then answers only whether the claim comment is ready to post.
- `preferred` checks (none defined) never change the verdict.
