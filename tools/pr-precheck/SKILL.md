---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

This tool answers exactly one question about exactly one PR package: **is this pull request ready to submit?** A PR package is a candidate pull request — its title, description, commits, diff, and test evidence — read against the accepted plan the PR claims to implement and the issue that plan belongs to. The tool never grades more than one package per run, never answers a different question, and never answers from impression: it executes the rubric, the evidence guide, and the procedure defined in this directory.

## Inputs and modes

**Eval mode**: the package bundle is the whole world. Read the bundled text for every fact — repo facts, issue, thread highlights, plan context, candidate PR title, description, commits, diff, and test evidence. Nothing is fetched, nothing else is read. All checks run, and the full verdict rule applies.

**Live mode**: inputs come from several sources; gather each before grading any check.

- **Issue and thread**: the real GitHub issue the plan belongs to — body, thread highlights, and any maintainer direction.
- **Repo facts**: the repo's PR template asks and stated AI-use policy, from the repo's CONTRIBUTING.md or pull request template file.
- **Plan**: the student's `plan.md` in their working copy, including the Deviations section at the bottom.
- **Diff**: the output of `git diff main...HEAD` (three dots) run from the root of the working copy. This is every change the branch introduces relative to the default branch.
- **Draft PR title and description**: the student's draft, provided in the session.
- **Test evidence**: the student's captured output — terminal transcripts, test run results, before/after curl or command output — provided in the session or committed to the branch.

A house-chain student reads the house plan and the house repro pack in place of their own `plan.md` and personal repro. The same checks grade the same things.

## The scope seam (live mode only)

Before reading any other input, read `scope.md`. If the scope's `Repo:` line still carries an unfilled placeholder, stop immediately and tell the student to get the correct scope file from their instructor before running the tool. Do not guess the repo.

If the scope's `Repo:` line is filled, confirm that the PR targets that repo. If the PR targets a different repo, refuse to grade and name the mismatch.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

After completing all rubric checks, read `voice-guide.md`. Hold the draft PR title and description against each rule in the guide. Report any rule the draft breaks in the summary, naming the rule and quoting the offending text.

The voice guide never changes the verdict on its own: a draft that breaks a voice rule but passes all rubric checks still gets verdict `accept`. The rubric has a `standards-met` check that reads template compliance and AI disclosure; a voice-guide rule violation is reported in the summary but is separate from that check.

In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md` to learn what checks to execute and what the verdict rule is. Read `references/evidence-guide.md` to learn where each evidence family lives in the package — the guide is your map; do not hunt for evidence the guide has not located for you. Execute checks in the order and manner `procedure.md` describes.

When `procedure.md` is silent on a step, report the gap in the summary rather than inventing an approach. Do not improvise around missing procedure guidance.

**Refusal rule**: if `rubric.md` has no check rows (only template boilerplate) or `procedure.md` has no concrete steps (only template comments), refuse to grade and say so. An empty tool that invents checks at runtime produces noise, not judgment.

## Verdict and output

The verdict is binary: `accept` (ready to submit) or `reject` (hold). There is no third verdict, no "accept with reservations". Reservations belong in the check evidence lines.

End your reply with a fenced JSON block — valid, complete, and last. Nothing follows it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Every check names the specific fact or quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be ready; a confident well-written one can be hiding drift. Every check reads the artifact itself against the plan, the issue, and the stated standards — never against how long the description is or how many commits there are.
- **The rubric decides, not the run.** If a check passes by its stated condition but feels wrong, it still passes. Fix the rubric, not the run.
- **The procedure decides how, not the run.** Follow `procedure.md` as written; report its gaps rather than silently filling them.
- **Unclear defaults to fail.** A check whose evidence is genuinely absent from the package grades `unclear`, which the verdict rule treats as `fail`. A PR whose test evidence cannot be verified from the package is not ready to submit.

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->
