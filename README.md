# multi-perspective-research

A Claude Code project template for **multi-perspective research with adversarial validation**. Spawns three Opus instances in parallel for divergent exploration of a problem, then stress-tests the strongest concepts with red-team validators.

> One brain confirms its own bias. Three brains in parallel disagree productively. Then you kill the weak ideas before falling in love with them.

---

## What this is for

Any open-ended research question where:

- A single model run is at risk of converging on a "safe" answer
- You want **divergence first, convergence second** — not premature consensus
- Numeric or market claims need adversarial verification, not cheerful confirmation
- The deliverable is a *defensible recommendation*, not a brainstorm dump

Examples it fits well:

- "What product / business / project should I build given these constraints?"
- "Which technical architecture for this problem?"
- "Which research direction is most likely to pay off?"
- "What's the right strategy for entering market X?"
- "Which 3 of these 20 options actually deserve deeper investment?"

Examples it does **not** fit:

- Single well-defined task ("write this code", "summarize this doc") — one agent is enough
- Realtime work — this is a batch workflow, runs take time and tokens
- Anything where ground truth is already known — there's nothing to discover

---

## Architecture

```
                   ┌───────────────────┐
                   │   Orchestrator    │  Opus 4.7
                   │   (this session)  │  reads CLAUDE.md
                   └─────────┬─────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
   ┌────▼────┐          ┌────▼────┐          ┌────▼────┐
   │explorer │          │explorer │          │explorer │   Round 1 — parallel
   │  4.6    │          │  4.7    │          │  4.8    │   divergent exploration
   └────┬────┘          └────┬────┘          └────┬────┘
        │                    │                    │
        └────────────────────┼────────────────────┘
                             │
                  ┌──────────▼──────────┐
                  │  Round 1 synthesis  │  Orchestrator analyzes,
                  │  finalist shortlist │  picks 3-5 finalists
                  └──────────┬──────────┘
                             │
        ┌────────────────────┼────────────────────┐
        │                    │                    │
   ┌────▼────┐          ┌────▼────┐          ┌────▼────┐
   │validator│          │validator│          │validator│   Round 2 — parallel
   │   4.8   │          │   4.8   │          │   4.8   │   adversarial validation
   └────┬────┘          └────┬────┘          └────┬────┘
        │                    │                    │
        └────────────────────┼────────────────────┘
                             │
                  ┌──────────▼──────────┐
                  │   Final portfolio   │  Orchestrator writes
                  │    portfolio.md     │  recommendations
                  └─────────────────────┘
```

**Why three Opus versions for Round 1.** Each version has different priors and stylistic tendencies. Forcing parallel exploration with no cross-visibility surfaces both **convergence signal** (same idea in ≥2 instances = real signal) and **divergence value** (a strong solo pick is interesting precisely because it didn't occur to the others).

**Why Opus 4.8 for Round 2.** Adversarial validation rewards the strongest reasoner. Replace with whichever current Opus you find most rigorous on your domain.

---

## Quick start

### Prerequisites

- [Claude Code](https://docs.claude.com/en/docs/claude-code) installed
- Claude Max subscription (this workflow is token-heavy — three parallel Opus runs with `thinking: max`)
- A research question worth asking

### Setup (5 minutes)

1. **Use this template** (GitHub button) or `git clone` it into a new project directory:
   ```bash
   git clone https://github.com/YOUR-USERNAME/multi-perspective-research.git my-research-project
   cd my-research-project
   ```

2. **Edit the two brief files** to describe your specific research problem:
   - `prompts/explorer-brief.md` — what the Round 1 explorers should produce
   - `prompts/validator-brief.md` — what the Round 2 validators should stress-test

   Both files have `[FILL: ...]` markers showing exactly where to put your problem-specific content. The structural scaffolding around them stays as-is.

3. **Launch Claude Code** in the project directory:
   ```bash
   claude
   ```

   The orchestrator reads `CLAUDE.md` automatically and will prompt you before spawning subagents.

### Run

The orchestrator follows this loop:

1. **Round 1** — spawns 3 explorers in parallel → each writes to `runs/round-1/{46,47,48}.md`
2. **Round 1 synthesis** — orchestrator reads all three, writes `runs/round-1-synthesis.md` with convergence map + finalist shortlist
3. **Round 2** — spawns one validator per finalist concept in parallel → each writes to `runs/round-2/{concept-slug}.md`
4. **Round 2 synthesis** — orchestrator writes `runs/round-2-synthesis.md` with verdicts
5. **Final** — orchestrator writes `portfolio.md`: the actual recommendation

You approve at each round boundary. The orchestrator does not loop infinitely or "improve" outputs without your call.

---

## Worked example — monetization research

To make the abstract concrete: suppose you want to find static-site projects monetizable via daily Claude+MCP scheduled tasks, targeting €200/month within 6–12 months.

**`prompts/explorer-brief.md`** would specify:
- Research task: "Identify daily-updated static-site projects with realistic €200/month path"
- Hard constraints: <€10/month infra, no GDPR-heavy data, operator reviews not authors
- Out of scope: AI-blog sites, medical/financial advice, crypto
- Per-concept fields: pitch, target user, daily payload, MCP stack, revenue mechanism, moat, time-to-revenue, failure mode, score
- Output: 8 concepts + portfolio recommendation of 2–3

**`prompts/validator-brief.md`** would specify:
- Stance: assume failure; find how
- Required sections: market reality check, unit economics audit, operational reality, discoverability audit, ranked failure modes, surviving features, verdict (KILL / PIVOT / PROCEED with caveats / PROCEED)

Run produces:
- 24 concepts total (3 × 8) → ~6 unique after dedup
- 3–5 finalists go to Round 2
- Final `portfolio.md` recommends 2–3 to actually build, with phased launch plan and kill criteria

**The same scaffolding works for any other research problem** — only the two brief files change.

---

## Customization

### Changing model versions

Edit the `model:` frontmatter in `.claude/agents/*.md`. The orchestrator does not hardcode model names anywhere — it only references agents by their `name`.

### Changing the number of explorers

To use 2 or 4 instead of 3:
1. Add or remove explorer agent files in `.claude/agents/`
2. Update the explorer list in `CLAUDE.md` (one section, clearly marked)
3. Update output filename conventions if you want them version-tagged

### Changing the number of rounds

The two-round explore→validate pattern is the recommended default. If your problem needs more:
- A **research/grounding round** before Round 1 (one agent gathers facts, explorers consume them)
- A **synthesis-deepening round** after Round 2 (focused agent answers one specific question left open)

Add these as additional steps in `CLAUDE.md` under WORKFLOW. The orchestrator will follow whatever sequence you specify.

### Per-domain agent specialization

For domains where one perspective is genuinely more valuable (e.g., "one explorer must be skeptical-by-default"), edit individual agent files to inject domain-specific stance. Default agents are intentionally neutral.

---

## Cost & runtime expectations

A full end-to-end run with `thinking: max` on all subagents is **token-heavy**:

| Phase | Agents | Output tokens (typical) | Thinking budget |
|---|---|---|---|
| Round 1 | 3 explorers | ~30k each = ~90k total | heavy |
| Round 1 synthesis | orchestrator | ~10k | medium |
| Round 2 | 3–5 validators | ~15k each = ~45–75k | heavy |
| Round 2 synthesis | orchestrator | ~10k | medium |
| Final portfolio | orchestrator | ~5k | light |

**Plan for one run to consume a meaningful share of your daily Max budget.** Don't queue it alongside other heavy work.

Tools called by subagents (especially `web_search` for validators) add latency — expect 15–40 minutes of wall-clock time for a complete run depending on tool patience.

---

## What this template is NOT

- **Not a research agent framework.** No retries, no cost tracking, no observability. Claude Code handles those (or doesn't). If you need production-grade orchestration, this is the wrong layer.
- **Not opinionated about domains.** The briefs are placeholders. You bring the problem.
- **Not a substitute for thinking.** The orchestrator's synthesis is only as good as your briefs. Vague brief in, vague portfolio out.
- **Not a way to skip verification.** Validators web-search; you still read their reports critically. The template raises the floor of rigor — it does not give you a free answer.

---

## Project structure

```
multi-perspective-research/
├── README.md                        # this file
├── LICENSE                          # MIT
├── .gitignore                       # ignores runs/
├── CLAUDE.md                        # orchestrator instructions (auto-loaded by Claude Code)
├── .claude/
│   └── agents/
│       ├── explorer-46.md           # Opus 4.6 explorer
│       ├── explorer-47.md           # Opus 4.7 explorer
│       ├── explorer-48.md           # Opus 4.8 explorer
│       └── validator-48.md          # Opus 4.8 validator (reused for each finalist)
├── prompts/
│   ├── explorer-brief.md            # FILL THIS for your problem
│   └── validator-brief.md           # FILL THIS for your problem
└── runs/                            # outputs (gitignored)
    └── .gitkeep
```

---

## License

MIT. See [LICENSE](LICENSE).

---

## Credits

Built by treating Claude Code as a multi-agent orchestrator rather than a single-context coder. If this template helps you, a star or a postcard is welcome.
