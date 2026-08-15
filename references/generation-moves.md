# Generation moves — the search playbook

Each move is a validated diversity mechanism. Use 2-3 per round; vary the mix between rounds. All prompts include the literal instruction to **"be original"** (moves originality d ≈ .79; see evidence.md §Instruction effects).

## Why prompting moves are required at all

Aligned models are mode-collapsed by construction: preference data carries a typicality bias (fitted α ≈ 0.6), so RLHF/DPO sharpens the base distribution toward the familiar. Measured: a Tulu-70B retains 45.4% semantic diversity as a base model, 20.8% after SFT, 10.8% after DPO. Temperature does not recover it (measured across full temperature grids), and lowering the KL penalty makes it *worse*. Frontier models produce <4 functionally distinct answers per 10 samples, and bigger models collapse harder. The moves below are the interventions with measured recovery.

## Move 1 — Verbalized sampling (the workhorse)

Ask for a *distribution*, not an answer:

> Generate {k} candidate solutions to this problem, formatted as a probability distribution: for each, state the candidate and the rough probability that a typical expert would propose it. Cover the full distribution — include at least {k/2} candidates from the low-probability tail, the ones you would normally suppress as too unlikely, unfashionable, or strange. Be original. Do not repeat or paraphrase: {archive summaries}.

Measured effect: 1.6–2.1× semantic diversity over direct prompting; recovers ~67% of pre-alignment diversity; larger models gain more. The probability labels matter — they license the model to emit the tail it would otherwise cut.

## Move 2 — In-context regeneration

> Here are all candidates so far: {archive}. Generate one candidate that is FUNCTIONALLY distinct from every one of them — a user who has seen all of the above must still benefit from seeing yours. Differ on a structural axis ({axes}), not in wording. Be original.

The only other prompting fix with measured effect on functional distinctness (NoveltyBench). Apply the same functional-equivalence test for dedup: same-idea-different-words merges.

## Move 3 — Atypical injection (the hit-shape move)

The empirical shape of hit work (17.9M papers): **high conventionality + one rare combination**, peaking at the 85th–95th percentile of conventionality, then reversing. Not maximum novelty.

> Take {CORE-i}. Keep its conventional core intact. Inject exactly one element it would never normally be combined with — a mechanism, constraint, or framing from {distant domain / unusual pairing}. The result must still do the original job.

Prefer this move when the core is strong. It produces most Frontier picks.

## Move 4 — Inversion / constraint perturbation

> List the assumptions every candidate so far shares. Pick the most load-bearing one. Negate it. Solve the problem under the negation.

Boden's combinational/exploratory moves made operational. Also useful: remove a resource everyone assumes; make the fixed thing variable; swap the actor.

## Move 5 — Distant analogy

> Name three unrelated fields that face this problem's structure ({abstract structure, one line}). For each, state that field's standard solution, then port it here concretely — mechanism, not metaphor.

## Move 6 — Lens / persona shift

Regenerate as personas with different disciplines and incentives (e.g., a security engineer, a game designer, a logistics dispatcher, a poet). In full mode: separate subagents, and different model families when configured — cross-family generation adds genuine independence that persona text alone cannot.

## Move 7 — Execution-trace mining (post-test, free candidates)

When a Phase 3.5 execution test fails, read the code/artifact at the failure point and ask: what substrate would the correct behavior need, and what in this neighbourhood already half-provides it? The fix's neighbourhood often contains a candidate no prompting move produced (field-verified: the occlusion-floor traversal candidate came from investigating WHY a contrast floor missed non-text ink, not from any generator). Candidates enter through the Phase 3 gates like all others.

## Field-tameness check (after generation, before scoring)

If every archived candidate is comfortably implementable in the current codebase within a quarter, the field never tested the tail: shared unexamined premises are likely, and the anti-novelty diagnostic cannot fire. Ask an adversary "name one credible approach absent from ALL candidates" and list the premises every candidate shares — in the recorded run that question surfaced both the shared premises and the eventual winner.

## Archive mechanics

- Grid = the Phase-1 axes. A candidate enters the archive by occupying an empty cell, or by beating the incumbent of its cell **on that cell's own terms** (not on general quality — quality selection is novelty selection's enemy, r = −0.27 to −0.48).
- Every entry: ≤120-word mechanism (what it does, why it could work, what breaks it), cell coordinates, the move that produced it, generation round.
- Never score novelty by within-batch frequency or within-batch co-occurrence statistics — at batch scale those are noise (the reference corpus for "what is common" is the executed core plus your domain knowledge, both frozen in Phase 0).
- Population check per round (full mode): count distinct equivalence classes among candidates passing the effectiveness gate ("effective diversity"). Raw dispersion counts junk; effective diversity is the number that matters.
- Set-level aggregation axioms (violating any is a bug — averaging violates the first two): adding a low-scoring candidate must never raise a set-level score; adding a genuinely novel candidate must always raise set novelty; near-duplicates must not inflate quantity; filtering weak outputs must not raise a set score.

## Full-mode fan-out

- 2–4 generator subagents, each assigned disjoint moves and/or personas; each returns candidates in archive-entry format.
- Generators never see each other's output mid-round (independence); the lead merges, dedups by functional equivalence, updates the archive, then broadcasts the archive for the next regeneration round.
- Round cap: 3. Stop earlier on two consecutive dry rounds (no new cell, no new class).
- Budget guardrail: if the archive exceeds ~16 live candidates, stop generating and start gating — more candidates past that point buys duplication, not coverage.
