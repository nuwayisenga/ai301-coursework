# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

**Where it lives:**
- In an eval bundle: the repro report's opening lines, before the steps. Look for OS name and version, tool/software version, platform details (GPU, shell, backend, window manager, language runtime version).
- In live mode: the draft repro comment's opening block; the issue body's "environment" section for what the reporter used; the repo-facts bug-report template asks for what the repo expects.

**What good looks like:**
The record names the OS, the version of the tool being tested, and any platform detail the issue's trigger depends on (e.g., shell type for a shell-specific bug, GPU driver for a rendering bug). If the version tested differs from the issue's target, the report says so explicitly. A line like "fzf v0.74.3, macOS 15.2 (Apple M2)" is sufficient; a report with no OS and no version is not.

## Steps

**Where it lives:**
- In an eval bundle: the numbered or bulleted steps in the repro report, read against the issue's stated trigger in the issue body and thread highlights.
- In live mode: the draft repro comment's steps section; the issue body for the original trigger; the repo's docs for any setup prerequisite the issue assumes.

**What good looks like:**
A stranger starting from a clean install could follow the steps to the trigger. The steps name the exact commands, inputs, or configuration used. A step that requires a private file, private repo, or undisclosed environment variable that is load-bearing for the trigger is not followable. Steps may skip obvious prerequisites (installing the tool) without failing — what matters is that the trigger itself is reachable from what is written.

## Behavior shown

**Where it lives:**
- In an eval bundle: the artifacts embedded in the repro report — terminal output excerpts, log lines, command output blocks, screenshots described in text. Read them against the issue's description of the observed bug (the error type, crash, wrong output, or visible symptom in the issue body and thread highlights).
- In live mode: the actual output or screenshot in the draft comment; the issue body for the expected vs. actual contrast.

**What good looks like:**
The artifact shows the exact behavior the issue describes: the same error code or message, the same crash type, the same wrong output or visual symptom. An adjacent artifact (a different error, a graceful failure where a crash was expected, a different wrong value) does not confirm the issue even if the report narrates it as doing so. An honest cannot-reproduce is also a pass: the report shows what actually happened, names what differed from the issue's conditions, and does not claim to have reproduced what it did not.

## Honesty

**Where it lives:**
- In an eval bundle: the report's conclusion sentences and any confidence claims (words like "definitely", "100%", "guaranteed"), read against the artifacts shown earlier in the same report.
- In live mode: the draft repro comment's closing statements; the artifacts shown in the same comment.

**What good looks like:**
The conclusion matches the evidence. A report showing the bug says "I reproduced this." A report that could not trigger the bug says "I could not reproduce this on [setup], here is what I tried and what differed." A report that shows an adjacent or wrong artifact but claims strong certainty is making a claim its evidence does not support — that fails. A report that acknowledges uncertainty honestly ("I was not able to trigger the exact crash but here is what I observed") passes if the artifact is faithful to what the reporter actually saw.

## Comms

**Where it lives:**
- In an eval bundle: the claim comment and repro report, read against the repo-facts contribution policy and AI-use policy lines (CONTRIBUTING.md or AI_POLICY.md), the bug-report template asks, and the issue context.
- In live mode: the draft comments; the repo's CONTRIBUTING.md and any AI policy linked from it; the issue body for context the claim should acknowledge.

**What good looks like:**
The claim comment names the specific issue and states a concrete next step (reproduce, investigate, test a patch) without promising a fix, a timeline, or a guarantee. The repro comment follows the repo's bug-report template structure if one is stated. If the repo's policy requires disclosing AI use (all course packages are treated as AI-assisted), the comments include that disclosure. A repo with no stated AI policy or no template has nothing to violate; silence in the policy passes. A generic claim ("+1", "any updates?", "I'll take this" with no specifics) fails the claim check regardless of the repo's policy.
