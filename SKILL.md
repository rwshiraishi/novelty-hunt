---
name: novelty-hunt
description: Searches a solution/idea space for truly unique, original candidates and scores them with a bias-controlled novelty-and-effectiveness protocol grounded in the creativity-measurement literature. Use when the user wants "truly unique ideas", "original solutions", "something nobody's tried", "think outside the box", "unconventional options", "creative alternatives", or when conventional approaches have failed and the obvious field feels exhausted. Domain-agnostic - technical designs, product/business ideas, naming, and creative text. For scoring competing implementations for production quality, see solution-tournament; for direction/go-no-go decisions, see council; for visual variants, see design-shotgun.
---

# Novelty Hunt

Find genuinely original candidates, protect them from the quality filter that normally kills them, and score them honestly. Every mechanism here is backed by a measured result; the constants and citations live in `references/evidence.md`.

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
3. **Freeze the effectiveness gate before searching**: 3-6 decomposed, anchored pass/fail or 0-2 questions ("does it satisfy constraint X?", "is there a mechanism, not just a label?"). Criteria written after seeing candidates become rationalizations.

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

**Stop rule (loop-until-dry)**: stop when a full round adds no new grid cell and no new equivalence class - lightweight: 1 dry round; full: 2.

## Phase 3 - Gate (before any scoring)

Cheap pass/fail on every archived candidate, in this order:
1. **Sense gate**: is it coherent and pragmatically possible? (Sense is a precondition for novelty; most maximally-rare output is junk.)
2. **Core-distance gate**: is it actually distinct from every CORE item, or a dressed-up paraphrase?
3. **Degenerate gate**: is it a slop pattern - vague label with no mechanism, buzzword recombination, deliberately-weird-with-no-content?

Failures die here with a one-line reason. Nothing in this phase scores novelty; a gate is not a rating.

**Hard vs soft constraints**: a hard external constraint (name already taken, violates a stated requirement, physically/legally impossible) is a gate — the candidate dies here. Soft desiderata (memorability, cost, elegance) are effectiveness questions in Phase 4. If a constraint could be worked around, it is soft.

## Phase 4 - Score

Score the survivors on both dimensions; score the core on **effectiveness only** (core novelty is the floor by definition - rating it produces ceremonial near-1 rows). Separate dimensions, separate calls or clearly separated passes - never one "creativity" number (judges collapse dimensions into a single latent score if you let them).

- **Novelty (gestalt, retrieval-grounded)**: for each candidate, first write 1-2 sentences of forced analysis - *what exactly is original here, relative to the executed core?* Then one holistic 1-7 rating. No novelty checklist (checklists measurably destroy novelty-judgment validity).
- **Effectiveness (decomposed)**: answer the Phase-0 anchored questions; map the answer pattern to a score and **rescale to 1-7** so the two columns share a scale (label both columns "1-7" in the output). Structure helps on this axis only.
- **Framing (mandatory, verbatim spirit)**: judge blind to which move produced each candidate; "compare the candidates to one another, not to an external standard"; "multiple valid solutions exist for this problem"; use the full scale; randomize candidate order between passes.
- **Repeats**: k=2-3 passes with distinct judge personas (skeptical operator, domain enthusiast, outsider). Report per-candidate spread **per dimension** (novelty spread and effectiveness spread separately); spread ≥ 3 on either = DISPUTED on that dimension. Near-perfect agreement across personas is shared bias, not validity - note it, don't celebrate it.
- **Ties at the top**: settle with pairwise duels judged in both presentation orders, on the holistic question "which candidate better delivers novel AND effective for this problem" - the duel is for pick selection, not for revising dimension scores.
- **Bias diagnostic**: if the highest-novelty quintile shows an effectiveness collapse, flag it - that is the documented anti-novelty judge signature, and those candidates get re-examined, not auto-buried.
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
- The **Wildcard is never dropped**. Judges are structurally blind to the tail (the most transformative candidates score as the least useful); routing it to the user with a cheap validation test is the only honest handling.
- Where a cheap execution test exists (spike, render, dry-run, 10-minute prototype), run it or name it - proposal-stage novelty advantages measurably reverse under execution, and one real result outranks every judge score. If a named test cannot run (tooling, budget, access), report it as BLOCKED with what would unblock it - never silently drop it.
- Honesty line: these scores are one synthetic panel's attribution, not a measurement of creativity; a different panel would produce a different, equally legitimate ordering.

## Interop with solution-tournament

When the hunt's output is a code/architecture implementation choice, hand the archive to `solution-tournament` as its Phase 1 field: frozen candidate texts (90-150 word mechanisms), named axes, hashes. Declare novelty as a weighted rubric dimension in the tournament's Phase 0 if originality should count there - never bolt it on after scoring. Conversely, when a tournament's red-team asks "name one credible approach absent from the field," run this skill's Phase 2 as the remedy.

## Related skills

- **solution-tournament** - production-quality scoring of competing implementations; consumes this skill's archive as its candidate field.
- **council** - direction and go/no-go decisions (a hunt picks an artifact; a council picks a path).
- **design-shotgun** - divergent generation for visual variants.
- **brainstorming** happens here; validation of a chosen business idea belongs to **product-discovery**.

## References

- `references/generation-moves.md` - prompt templates for every move, archive mechanics, full-mode fan-out.
- `references/scoring-protocol.md` - judge prompt blocks, persona panel, bias controls, the Mueller two-framing self-test.
- `references/evidence.md` - the measured constants and citations behind every rule above; read before editing this skill.
- `references/research/` - the 17 underlying deep-research reports (2026-08 sweep).
