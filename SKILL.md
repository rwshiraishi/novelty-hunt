---
name: novelty-hunt
description: Explore functionally distinct ideas and compare originality with usefulness, delivering conventional, frontier, and wildcard picks. Use for original product concepts, naming, positioning, creative text, or technical alternatives when familiar answers are insufficient. Originality is relative to a stated baseline, not a claim of worldwide uniqueness.
---

# Novelty Hunt

Explore original candidates, preserve meaningful alternatives, and deliver useful picks. The workflow draws on creativity research and recorded runs; study results inform the method but do not establish its effectiveness across every domain. Constants and citations live in `references/evidence.md`.

## Calibration (read first, always)

**Skip floor.** Do not run this skill when: the ask is trivial (variable names, one-liner fixes), a conventional answer is obviously sufficient, or the user wants speed over originality. A hunt costs roughly 10-25k tokens lightweight, 40-90k full. If the fix is smaller than the hunt, answer directly and say why the hunt was skipped.

**Mode selection.** Lightweight (default): single agent, 1-2 search rounds, persona-panel self-scoring, archive of 6-12 candidates. Full mode only on an explicit trigger: user asks for an exhaustive hunt, a prior conventional fix failed, high-stakes one-shot decision (naming, positioning, architecture bet), or user says "full". A mode named by the user always wins over the trigger list; if the triggers disagree with the named mode, run the named mode and say so in one line. Full mode spawns generator and judge subagents and uses cross-model scoring where configured; read `references/generation-moves.md` and `references/scoring-protocol.md` before starting full mode. Lightweight mode needs only this file.

**Lightweight honesty caveat**: in a single-agent run, persona passes share one context — the spread column measures injected variance, not independent disagreement, and order randomization cannot undo anchoring. Print this caveat in the output; real judge independence exists only in full mode's separate subagents/models.

**The two facts that shape everything:**
1. Selection on quality is selection against novelty (measured r = -0.27 to -0.48). Novel candidates must be protected *structurally* (the archive), never left to survive a quality cut.
2. The winning shape is not maximum novelty. Hit work across 17.9M papers = a highly conventional core plus one injected atypical element, peaking at the 85th-95th percentile of conventionality. Hunt for the atypical injection, not for wall-to-wall weirdness.

## Phase 0 - Frame and execute the conventional core

Before generating anything original:

1. **Domain route**: technical / product-business / creative-text. This picks the effectiveness rubric variant (rubrics do not transfer across domains; a generic rubric discriminates nothing).
2. **Execute the conventional core**: plainly generate the 2-4 obvious, standard answers a competent practitioner would give. Label them CORE-1..n. This is simultaneously (a) the pre-registered expected answer, (b) the frozen reference set that novelty is measured against, and (c) real candidates - the obvious answer often wins and must be allowed to.
3. **Freeze the effectiveness gate before searching**: 3-6 decomposed, anchored pass/fail or 0-2 questions ("does it satisfy constraint X?", "is there a mechanism, not just a label?"). Criteria written after seeing candidates become rationalizations. **Each criterion names its evidence AND that evidence's known limitations** — a number the diagnosis itself quarantined (contaminated sample, stale measurement) must not silently become a scoring anchor; a frozen gate is only as good as the evidence frozen into it (lessons L-N6).

Everything in the core is by definition novelty = floor. Nothing generated later may be scored novel merely by paraphrasing a core item.

## Phase 1 - Map the axes

Name 3-6 structural axes along which solutions could differ (e.g., where-the-logic-lives, data model, mechanism class, who-acts, time-of-action, what-is-inverted). Axes must be structural, not adjectives. These form the archive grid. If you cannot name at least two credible axes, the space is too narrow - say so and stop.

## Phase 2 - Search (the hunt)

Run rounds of the generation moves; archive by grid cell, best candidate per cell. The archive is what protects novelty: a candidate earns its slot by being a **new functional equivalence class** (the dedup rule does the real work), with the grid cell recording *where* it differs - not by beating others on quality. With several axes most candidates land in fresh cells; a fresh cell alone is not distinctness.

**Moves (each round, pick 2-3; details and prompt templates in `references/generation-moves.md`):**
- **Verbalized sampling**: generate k candidates as an explicit distribution with probabilities, and demand the low-probability tail ("include the answers you'd normally suppress as too unlikely"). This is the single most effective diversity lever (1.6-2.1x measured).
- **In-context regeneration**: show the full archive so far; require the next candidate to be *functionally* distinct from every entry ("a user who saw all of these would still benefit from yours").
- **Atypical injection**: take a conventional core candidate and inject one element from a distant domain or an unusual pairing. This is the Uzzi hit-shape move and usually produces the eventual Frontier pick.
- **Inversion / constraint perturbation**: negate an assumption every core candidate shares; solve under the negation.
- **Distant analogy**: name a field with a structurally similar problem; port its standard solution.
- **Lens shift**: regenerate as a distinct persona (different discipline, different incentives). In full mode, different subagents or models.
- Say "be original / be creative" explicitly in every generation prompt - it measurably moves originality (d ≈ .79) and costs nothing.

**Dedup rule**: two candidates are the same idea if a user who saw one would not benefit from the other. Merge them; wording differences are not distinctness.

**Stop rule (loop-until-dry)**: stop when a full round adds no new grid cell and no new equivalence class - lightweight: 1 dry round; full: 2. Lightweight also stops after two search rounds. For full mode, state a round or resource budget before generation and stop at that limit even if new ideas remain; report unexplored directions instead of silently extending the hunt.

## Phase 3 - Gate (before any scoring)

Cheap pass/fail on every archived candidate, in this order:
1. **Sense gate**: is it coherent and pragmatically possible? (Sense is a precondition for novelty; most maximally-rare output is junk.)
2. **Core-distance gate**: is it actually distinct from every CORE item, or a dressed-up paraphrase?
3. **Degenerate gate**: is it a slop pattern - vague label with no mechanism, buzzword recombination, deliberately-weird-with-no-content?

Failures die here with a one-line reason. Nothing in this phase scores novelty; a gate is not a rating.

**Hard vs soft constraints**: a hard external constraint (name already taken, violates a stated requirement, physically/legally impossible) is a gate — the candidate dies here. Soft desiderata (memorability, cost, elegance) are effectiveness questions in Phase 4. If a constraint could be worked around, it is soft.

## Phase 3.5 - Execution tests BEFORE scoring (the highest-value mechanism this skill has)

Where a cheap execution test exists (spike, render, dry-run, grep against the real artifact, 10-minute prototype), run it **before any judge scores anything** — at minimum against (a) the top conventional core candidate and (b) the top-novelty candidate. Two measured reasons (lessons L-N1):
- Proposal-stage advantages reverse under execution: in the recorded runs, execution tests overturned both the odds-on conventional favorite and the naive form of the top-novelty candidate, at $0, before a single score existed. One real result outranks every judge score.
- A pre-scoring result **rewrites the effectiveness bar the judges then apply** — run after scoring it can only veto, not calibrate.

**Follow the failure into the code** (lesson L-N3): when an execution test fails, investigate WHY in the artifact/codebase — the fix's neighbourhood often contains a candidate no generation move produced. Candidates found this way enter through the Phase 3 gates like any other.

If a named test cannot run (tooling, budget, access), report it BLOCKED with what would unblock it — never silently drop it.

## Phase 4 - Score

Score the survivors on both dimensions; score the core on **effectiveness only** (core novelty is the floor by definition - rating it produces ceremonial near-1 rows). Separate dimensions, separate calls or clearly separated passes - never one "creativity" number (judges collapse dimensions into a single latent score if you let them).

- **Novelty (gestalt, retrieval-grounded)**: for each candidate, first write 1-2 sentences of forced analysis - *what exactly is original here, relative to the executed core?* Then one holistic 1-7 rating. No novelty checklist (checklists measurably destroy novelty-judgment validity).
- **Effectiveness (decomposed)**: answer the Phase-0 anchored questions; map the answer pattern to a score and **rescale to 1-7** so the two columns share a scale (label both columns "1-7" in the output). Structure helps on this axis only.
- **Framing (mandatory, verbatim spirit)**: judge blind to which move produced each candidate; "compare the candidates to one another, not to an external standard"; "multiple valid solutions exist for this problem"; use the full scale; randomize candidate order between passes.
- **Repeats**: k=2-3 passes with distinct judge personas (skeptical operator, domain enthusiast, outsider). Report per-candidate spread **per dimension** (novelty spread and effectiveness spread separately); spread ≥ 3 on either = DISPUTED on that dimension. Near-perfect agreement across personas is shared bias, not validity - note it, don't celebrate it. **When all judges share one model family, say plainly in the output that the panel is one judge with k voices and the spread column is decorative** — persona variation does not buy independence (lesson L-N4: 20 candidates × 3 personas, zero disputes, max spread 2, and the zero-spread #1 was falsified in one grep).
- **Judges receive the RAW generator text** for each candidate alongside the merged class label - never only the lead's compressed archive prose. A single author's summary is a shared input no persona can decorrelate, and judges echo its framing back as "independent" findings (lesson L-N5). If budget forces merged-prose-only, the merge must be written by an agent that does not author the scoring brief.
- **Code-verifying judge (mandatory when candidates make checkable claims)**: where a candidate asserts a factual claim about the artifact ("the plates are tagged data-deco", "the floor covers non-text ink"), at least one judge must verify it against the artifact and report the command. Distinct from the red team; cheap; catches false premises at scoring instead of after (lesson L-N2 - three judges scored a false-premise candidate 6.5 with zero spread because none opened the file).
- **Ties at the top**: settle with pairwise duels judged in both presentation orders, on the holistic question "which candidate better delivers novel AND effective for this problem" - the duel is for pick selection, not for revising dimension scores.
- **Bias diagnostic**: if the highest-novelty quintile shows an effectiveness collapse, flag it - that is the documented anti-novelty judge signature, and those candidates get re-examined, not auto-buried. **A non-firing diagnostic on a tame field is not evidence the framing worked** (lesson L-N7): if every candidate is comfortably implementable, nothing tested the tail. In that case route the field-ceiling question to an adversary explicitly: "name one credible approach absent from all N candidates" - in the recorded run that question produced the eventual winner's premise.
- Scores are **ranks within this batch only**. Never compare scores across hunts.

## Phase 5 - Deliver

Output exactly this structure:

```
## Hunt results: <problem, one line>
Core executed: CORE-1..n (one line each)
Effectiveness gate: <the frozen Phase-0 questions, one line each>
Axes: <list>   Candidates archived: <n>  Gated out: <n> (one-line reasons)

| # | Candidate (one line) | Cell | Novelty 1-7 | Effect. 1-7 | Spread N/E | Notes |

Bias diagnostic: <5th-quintile result, one line>
Lightweight caveat (if applicable): spread is injected variance, not independent judgment; dry-round call is self-assessed.

**Best Conventional** - top of the core. <why + deliverable>
**Frontier** - novel AND effective AND atypical. <why + deliverable>  <- usually the answer
**Wildcard** - highest novelty, uncertain effectiveness. <why it might be great, what would prove it + deliverable>
```

Rules for the picks:
- Each pick carries the **deliverable in the form the user asked for** (the name + positioning line, the mechanism sketch, the draft copy) - not just the candidate label.
- If no distinct candidate survives for a pick, mark that pick unavailable with the reason; do not invent a candidate or revive one that fails a hard constraint. A surviving **Wildcard is never dropped**. Judges are structurally blind to the tail (the most transformative candidates score as the least useful); routing it to the user with a cheap validation test is the only honest handling.
- Execution tests already ran in Phase 3.5; the picks must cite their results. Any pick contradicting an execution result must say so and why.
- **A panel ranking is a hypothesis, not a result.** When the hunt feeds an implementation decision, the ranking is not final until an adversarial pass (solution-tournament's red team, or an equivalent refutation-briefed agent) has attacked at least the top pick and the Wildcard. In every recorded run to date (3/3), the scored #1 did not survive adversarial review unchanged (lesson L-N2).
- **Budget allocation, when constrained** (lesson L-N8): the measured value concentrates in the generation moves, the execution tests, and the adversarial pass. Cut the 3-persona panel to ONE code-verifying judge before cutting any of those three. The panel's durable contribution is its qualitative analysis, not the numbers.
- Honesty line: these scores are one synthetic panel's attribution, not a measurement of creativity; a different panel would produce a different, equally legitimate ordering.

## Interop with solution-tournament

When the hunt informs an implementation decision, hand off the raw candidate texts, conventional core, structural axes, constraints, execution results, and the novelty/effectiveness comparison. Keep the original scores labeled as hunt scores.

Choose the next step by the decision still needed:
- **The user wants to challenge the hunt's recommendation:** reuse the archive for an adversarial review and targeted experiments. Do not call this a completed solution tournament or repeat scoring without a new decision need.
- **The user requests a production-quality tournament:** start at the tournament's Phase 0, freeze its rubric, and reuse suitable hunt candidates as generation inputs. Novelty is a weighted dimension only if agreed before tournament scoring. Hunt ratings do not substitute for the tournament's different criteria or evidence requirements.
- **A tournament needs a wider field:** use this skill's generation moves to find missing alternatives; let the tournament govern their admission and scoring.

`solution-tournament` is optional. If it is unavailable, deliver the archive and explain the next review or experiment directly.

## Full-mode dispatch note (harness)

Use the host's available subagent tools and completion-report mechanism. Give each agent a bounded assignment and require a final result with candidate text or scoring evidence. For background work, record the agent identifier and output location; if a result is missing, request it once and inspect saved outputs before repeating work. Follow a local report-delivery guide if present, but do not require private paths or helper scripts that are not shipped with this skill. If independent dispatch is unavailable, state that limitation and use the lightweight workflow within the user's budget.

## Self-improvement loop

After every full-mode run, append a run section to `references/lessons.md` (scorecard per mechanism — including mechanisms that produced nothing — findings with evidence, cost ledger) and apply the promotion rules written at the top of that file: CANDIDATE on one observation, promote into this file on two confirmations or one airtight causal chain, demote with counter-evidence rather than delete. Guardrails: never weaken an anti-bias or verification rule; every new rule cites the run that earned it; a mechanism that is only ever praised is not being evaluated.

## Related skills

- **solution-tournament** - production-quality scoring of competing implementations; consumes this skill's archive as its candidate field.
- **council** - direction and go/no-go decisions (a hunt picks an artifact; a council picks a path).
- **design-shotgun** - divergent generation for visual variants.
- **brainstorming** happens here; validation of a chosen business idea belongs to **product-discovery**.

## References

- `references/generation-moves.md` - prompt templates for every move, archive mechanics, full-mode fan-out.
- `references/scoring-protocol.md` - judge prompt blocks, persona panel, bias controls, the Mueller two-framing self-test.
- `references/evidence.md` - the measured constants and citations behind every rule above; read before editing this skill.
- `references/lessons.md` - the run ledger and promotion rules behind the self-improvement loop; the L-N* citations above resolve there.
- `references/research/` - the 17 underlying deep-research reports (2026-08 sweep).
