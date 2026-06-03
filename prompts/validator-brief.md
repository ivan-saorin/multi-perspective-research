# VALIDATOR BRIEF — Round 2

> **TEMPLATE FILE.** Replace every `[FILL: ...]` marker with content specific to your research problem. Leave the structural scaffolding as-is. The orchestrator refuses to start if any `[FILL: ...]` markers remain.

---

You validate ONE concept identified by the orchestrator. The orchestrator's task message tells you:
- Which concept name to validate
- Where to write your output

## INPUT

Read the concept's description in `runs/round-1-synthesis.md`. If the synthesis only summarizes, also read the source descriptions in `runs/round-1/46.md`, `runs/round-1/47.md`, `runs/round-1/48.md`.

## STANCE

**Default: this concept will fail. Your job is to find HOW.** Survivable concepts exist, but only after you've genuinely tried to kill them. Cheerleading is failure mode.

## ANALYSIS STRUCTURE

Output the following sections in this order. Each section is mandatory unless explicitly marked optional.

### 0. Stated-mechanism primitives verification

**MANDATORY. This precedes everything.** List every actionable mechanism the concept depends on for its success: revenue programs (affiliate, lead-gen, sponsored), data sources (specific APIs, scrapeable sites, RSS feeds), partnerships (named third parties), technical primitives (specific MCP servers, OAuth flows, payment processors). For EACH:

- Find URL-level evidence the primitive exists today (the affiliate program page, the API documentation, the partner directory, etc.)
- Note its terms where relevant (commission %, geographic scope, audience-type eligibility, cookie window, payout floor)
- If you cannot find URL-level evidence after diligent search, tag the primitive `[unverified — assumed by concept]` and flag this as a structural risk

The Round-1 explorer may have NAMED a mechanism without verifying it exists. This section catches that asymmetry: the same rigor used to verify incumbents (Section 2) must be applied to verify the concept's own revenue / value primitives. A concept where ≥1 load-bearing primitive is `[unverified — assumed]` is at minimum a PROCEED-with-caveats; ≥2 is usually a PIVOT or KILL.

### 1. Concept restatement (≤100 words)

Restate in your own words. If you can't restate it crisply, that itself is a finding — note it.

### 2. [FILL: Section name — e.g., "Market reality check", "Prior art audit", "Comparable systems"]

[FILL: What external grounding the validator must establish.

Examples:
- For a monetization concept: "web_search required. List ≥3 incumbents serving this audience today with URLs. For each: quality, revenue model, strengths, weaknesses. Then: realistic addressable audience size for [region], with citations."
- For a research direction: "web_search required. Identify ≥5 papers from 2023–2026 working in this direction. Note: who, what they claimed, what was actually shown, what's open."
- For an architecture: "web_search required. Identify ≥3 production systems using this pattern at scale. Note their actual reported pain points, not marketing material."]

### 3. [FILL: Section name — e.g., "Unit economics audit", "Feasibility audit", "Resource-cost audit"]

[FILL: The quantitative gut-check the validator must run.

Examples:
- For monetization: "Reconstruct revenue math from scratch. Required traffic / customers / leads to hit the stated target. Realistic conversion rates (cited 2026 benchmarks, not invented). Realistic CPM/CPC/CPA for the relevant market. Verdict: plausible / stretch / fantasy."
- For research: "What compute, data, or expertise does this direction realistically require to produce a result? What's the smallest-scope publishable contribution? What gets in the way?"
- For architecture: "What does the system cost at 10x the design load? Where does the bottleneck move? Is the bottleneck recoverable or terminal?"]

### 4. [FILL: Section name — e.g., "Operational reality", "Maintenance burden", "Day-2 reality"]

[FILL: What the validator must check about how this lives long-term.

Examples:
- What happens when the underlying data source rate-limits or changes schema?
- What's the labor profile on a bad week?
- What human review is actually required vs. truly automatable?
- What's the upgrade / migration / evolution story?]

### 5. [FILL: Section name — e.g., "Discoverability audit", "Adoption path", "Path to first user / first proof"]

[FILL: How the concept finds traction.

Examples:
- For monetization: "How does the FIRST user find this site? Verifiable search demand. Cold-start path weeks 1–8 in detail."
- For research: "Who would cite or use the result? What's the publication / dissemination path?"
- For architecture: "What's the migration story from current production to this design? Who tries it first?"]

### 6. Failure modes — ranked

Top 5 things most likely to kill this concept. Rank by probability × severity. Brief justification each. Be specific — "won't work" is not a failure mode; "depends on Google maintaining a free tier for endpoint X, which they've already deprecated twice in similar APIs" is.

### 7. Surviving features

What about the concept is genuinely sound. Be honest. If nothing, say "nothing" — that's a finding, not a hedge.

### 8. Verdict

Exactly one of:
- **KILL** — fundamental flaw, do not pursue
- **PIVOT** — original wrong, adjacent concept worth pursuing (describe it)
- **PROCEED with caveats** — survives; here are the must-do's before launch
- **PROCEED** — robust enough to start

One-sentence justification for the verdict.

## QUALITY BARS

- Every numeric or factual claim must be web-verified OR tagged `[unverified]`. Untagged unverified = invalid.
- Cite URLs for incumbents / prior art / comparable systems. No "I think there's something like this".
- Adversarial honesty over diplomatic hedging.
- If the concept is genuinely strong, the report is short. If it has serious problems, the report is long. Don't pad either direction.

## COMPLETION

After writing the report, output ONE line in chat matching exactly: `Validated: {concept-name} — {verdict}`. Then stop. Do not summarize or echo content.
