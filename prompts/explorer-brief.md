# EXPLORER BRIEF — Round 1

> **TEMPLATE FILE.** Replace every `[FILL: ...]` marker with content specific to your research problem. Leave the structural scaffolding (headings, output requirements, completion instructions) as-is. The orchestrator refuses to start if any `[FILL: ...]` markers remain.

---

**OUTPUT**: Write your full response to `runs/round-1/{YOUR-VERSION}.md` (your agent name tells you which: explorer-46 → 46.md, explorer-47 → 47.md, explorer-48 → 48.md). Do NOT output the response in chat. The orchestrator reads the file.

---

## RESEARCH TASK

[FILL: One paragraph stating what is being researched. Be specific about the question, the target outcome, and any time horizon. Examples:
- "Identify daily-updated static-site projects monetizable via a Claude+MCP scheduled task, with realistic path to €200/month within 6–12 months."
- "Identify candidate architectures for a real-time event-deduplication service handling 10k events/sec on a single node."
- "Identify the 5 most promising research directions for {topic} based on 2024–2026 literature."]

## CONTEXT

[FILL: Who is this for and what's their situation. Include only context that affects what makes a proposal viable.
- Operator profile: skills, team size, available time, geographic / market scope
- Existing infrastructure or constraints
- Resources available (subscriptions, tools, budget envelope)
- What success looks like for THIS operator (not in the abstract)]

## HARD CONSTRAINTS

[FILL: Bulleted list of what MUST be true of any valid proposal. Each constraint should be checkable. Examples:
- Total infra cost under €X/month
- No user accounts or payment flows in v1
- Operator review time ≤30 min/day
- Public data only OR data from APIs the operator can access today]

## OUT OF SCOPE (hard exclusions)

[FILL: Bulleted list of categories/approaches automatically disqualified. Each exclusion should be unambiguous. Examples:
- AI-generated text-only sites (penalty risk)
- Medical, financial, legal advice (regulatory)
- Anything requiring proprietary or scraped data
- Anything where personal expertise is the value (must be impersonal)]

## DELIVERABLE

Output a single Markdown file containing the three sections below.

### 1. Operating thesis (≤200 words)

How you interpret the brief, your tradeoff axes, what categories you deliberately exclude and why. This makes your output comparable to other instances running this same brief. **Do not skip this.** It is the most important paragraph for downstream synthesis.

### 2. Exactly [FILL: N] distinct [FILL: concepts / proposals / directions / architectures / strategies]

For each item, fill every field below. Use either a Markdown table or uniform sub-sections — pick one and stick to it.

| Field | Notes |
|---|---|
| `name` | short, memorable |
| `pitch` | ≤20 words |
| `verified_incumbents` | **MANDATORY for every concept.** ≥2 named incumbents / near-substitutes already serving this audience or solving this problem, each with a real URL verified via `web_search` during this run (not from memory). If after diligent search you genuinely find none, write `none-found-after: [list 3+ search queries tried]` — that emptiness IS a strong finding. Concepts submitted without this field are treated as missing-research and rejected from the finalist shortlist by the orchestrator. |
[FILL: Add domain-specific fields. Examples for a monetization brief:
| `target_user` | who reads / who pays |
| `daily_payload` | what 5–10 things change each day (give one realistic example day) |
| `tool_stack` | MCPs / APIs used, role of each |
| `revenue_primary` | mechanism + unit economics |
| `traffic_path` | SEO / direct / B2B / newsletter / API |
| `moat_after_12m` | what makes this hard to replicate later |
| `time_to_first_euro` | weeks |
| `time_to_steady_state` | months + projected € |
| `monthly_infra_cost` | € |
| `key_failure_mode` | single most likely cause of death |
| `why_not_already_dominated` | who would do this, why they haven't |

Examples for an architecture brief:
| `topology` | components and data flow |
| `failure_modes` | what breaks at scale |
| `proof_points` | known systems using this pattern |
| `complexity_cost` | implementation + operational |
]

| `score` | 1–10 with one-line justification |

### 3. [FILL: Synthesis section title — e.g., "Portfolio recommendation", "Top picks", "Recommendation"]

[FILL: What synthesis you want from each explorer. Examples:
- "Pick the best 2–3 concepts to combine into a portfolio. Justify on audience non-overlap, mechanism diversification, shared infrastructure, realistic launch phasing."
- "Pick the single strongest direction and the single most surprising one. Justify both."
- "Rank all N items by expected value × confidence. Justify the top 3 positions."]

## EVALUATION BIAS

[FILL: Two short lists.

Reward heavily:
- [criteria specific to your domain]
- ...

Penalize:
- [criteria specific to your domain]
- ...

Generic items that usually apply:
- Reward: ideas with compounding value over time, ideas with hard-to-replicate moats, ideas where operator role is review-not-author
- Penalize: ideas that are basically "Claude blog about X", ideas requiring the operator's personal voice as the value]

## CALIBRATION

- Verify factual claims (market sizes, competitor presence, benchmarks, technical feasibility) via web_search where possible.
- Tag uncertain claims `[unverified]`. The orchestrator treats untagged claims as load-bearing — they must be defensible.
- Realistic over optimistic. A concept claiming impossible numbers from week 2 is rejected.

## COMPLETION

After writing the file, output ONE line in chat matching exactly: `Round 1 complete: {your-version}`. Then stop. Do not summarize or echo content.
