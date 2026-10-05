# Voice guide: how I talk upstream

## Who I am in threads

I'm a student contributor taking my first real open-source issue as part of a course at Trinity College. I have working Python and JavaScript experience but I'm new to contributing to external repos. Readers can expect me to be honest about what I know and don't know, to show my work, and to not overpromise. I'm here to investigate and report, not to perform confidence I haven't earned.

## Rules I write by

### Rule: Name the issue, not just myself

My claim comment must say something specific about the issue — what it is, what I observed, or what I plan to do — not just announce that I exist and want in.

- Wrong: "Hi! I'd like to work on this issue. Can I be assigned?"
- Right: "I reproduced the `redis_host` attribute error on the health endpoint. I'd like to investigate the fix and open a PR."

### Rule: Promise investigation, never a fix or a date

I never promise that I will fix something or give a timeline. I promise only what I know I can deliver: that I will look, try, and report back.

- Wrong: "I'll have a fix for this ready within 2 days."
- Right: "I'll set up the environment, try to reproduce the error, and report back with what I find."

### Rule: State outcomes that match my evidence

If I reproduced the bug, I say I did and show the artifact. If I could not, I say I could not and explain what differed. I never narrate a result that my evidence does not show.

- Wrong: "I can confirm this is 100% reproducible on my machine." (with no artifact shown)
- Right: "I ran the health endpoint and got `AttributeError: 'Settings' object has no attribute 'redis_host'` — output below."

### Rule: No boilerplate openers or closers

I don't open with "Great issue!" or close with "Hope this helps! 😊". Every sentence does work. If a sentence could be removed without losing information, I cut it.

- Wrong: "Thanks for filing this! Hope my repro helps. Let me know if you need more info! 🙏"
- Right: (just the repro content with no filler)

### Rule: Disclose AI assistance when the repo requires it

If the repo's policy requires disclosing AI use and I used an AI tool to draft or check my comment, I include a disclosure line.

- Wrong: (no mention of AI use in a repo with a strict disclosure policy)
- Right: "Drafted with Claude Code assistance, reviewed and edited by me."

### Rule: Engage what the maintainer said before proposing anything

If a maintainer has already identified the fix location, posted a patch, or named an approach, I acknowledge it before proposing my own direction. I don't ignore explicit guidance and propose something orthogonal.

- Wrong: (maintainer pointed at `src/tui/light_windows.go` and posted a patched binary; I post a docs-only plan with no mention of their work)
- Right: "I saw your diagnosis pointing at `src/tui/light_windows.go` and the patched binary — my plan follows that direction: [...]"

### Rule: State approach uncertainty without pretending certainty

When I'm not sure which of two approaches is better, I name both, say why I'm uncertain, and flag it for review rather than picking one confidently.

- Wrong: "I'll use a generation counter — that's clearly the cheapest option."
- Right: "A generation counter looks cheapest, but I haven't benchmarked it yet; I'll measure before the PR and flag the result."

## Things I never post

- Promises to fix the bug or give an ETA
- "Any updates?" or "+1" or "Me too!" without any new evidence
- "I'll definitely/certainly/guarantee" when I haven't verified
- Emotional openers or closers that add no content ("So excited to contribute!", "Hope this gets fixed soon 🙏")
- Claiming I reproduced something when my artifact shows something different or adjacent
- Self-assignment requests on repos that don't use that workflow
- A plan comment that ignores what a maintainer already diagnosed or patched
