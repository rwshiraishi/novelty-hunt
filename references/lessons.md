# Lessons ledger (living document)

Read after every full-mode run; append a run section (per-mechanism scorecard —
including mechanisms that produced nothing — findings, cost). Statuses:
CANDIDATE (seen once) → PROMOTED (confirmed twice, or once with an airtight
causal chain; edit SKILL.md/references, date-stamp, record `landed-in`) →
DEMOTED (counter-evidence; revert the edit, never silently delete). UNANSWERED
questions stay listed until a run settles them. Guardrails: never weaken an
anti-bias or verification rule; every rule cites its run; a mechanism only ever
praised is not being evaluated.

## L-N1 — execution-tests-before-scoring — PROMOTED
- **Rule**: Run cheap execution tests on the top core AND top-novelty candidates
  BEFORE any scoring; results rewrite the effectiveness bar.
- **Evidence**: Run 2026-08-14 (DataSculpt): overturned the conventional favorite
  (text-only floor missed 305 non-text strokes; assessed 0/15 on a clean figure)
  AND falsified the naive form of the top-novelty candidate (ablation threshold
  passed the exact figure it targeted). Two for two, $0, minutes each.
- **Landed-in**: SKILL.md Phase 3.5 (new phase).

## L-N2 — panel-ranking-is-a-hypothesis — PROMOTED
- **Rule**: The scored ranking is not a decision until an adversarial/red-team
  pass attacks the top pick and the Wildcard. Weight the adversarial phase above
  the scoring phase.
- **Evidence**: 3/3 recorded DataSculpt tournaments (2026-07-14, 2026-07-28,
  2026-08-14) overturned the scored #1 at red-team. 2026-08-14: panel's #1 rested
  on a premise false on 2/2 measured figures (one grep); the panel ranked the
  eventual winner LAST (Wildcard, eff. 4.67) and the red team promoted it to
  first. Ranking was wrong at the top and bottom simultaneously.
- **Landed-in**: SKILL.md Phase 5 pick rules; scoring-protocol.md code-verifying
  judge; Wildcard-never-dropped rule vindicated (keep).

## L-N3 — follow-the-failure-into-the-code — PROMOTED
- **Rule**: When an execution test fails, read the code at the failure point;
  the fix's neighbourhood often contains a candidate no generator produced.
- **Evidence**: Run 2026-08-14: the occlusion-floor traversal (paint order +
  true-geometry sampling + effective alpha) surfaced only from investigating why
  the floor missed non-text ink; no generation move named it. Single observation
  but mechanism is clear; promoted as a cheap additive move, not a constraint.
- **Landed-in**: SKILL.md Phase 3.5; generation-moves.md Move 7.

## L-N4 — same-family-panel-is-one-judge — PROMOTED
- **Rule**: All judges on one model family = one judge with k voices; the spread
  column is decorative; say so in the output.
- **Evidence**: Run 2026-08-14: 20 candidates × 3 personas, zero DISPUTED, max
  spread 2 (κ far above human expert ceiling ≈ 0.4); the reliability instrument
  carried no information and flagged nothing while the #1 was false. Confirms
  the protocol's pre-existing warning with field data.
- **Landed-in**: SKILL.md Phase 4 repeats bullet; scoring-protocol.md panel design.

## L-N5 — judges-get-raw-text — PROMOTED
- **Rule**: Judges receive raw generator text alongside the merged label; the
  archive merger must not author the scoring brief.
- **Evidence**: Run 2026-08-14: lead's archive called A17 "mostly an ENABLER";
  a judge returned "self-admittedly an enabler" as its own finding — verbatim
  inheritance nearly counted as independent confirmation. Fully explains L-N4's
  suspicious agreement: three personas cannot decorrelate a shared input.
- **Landed-in**: SKILL.md Phase 4; scoring-protocol.md judge input rule.

## L-N6 — gate-evidence-carries-its-limitations — PROMOTED
- **Rule**: Each frozen gate criterion names its evidence and that evidence's
  known limitations; quarantined numbers cannot become scoring anchors.
- **Evidence**: Run 2026-08-14: the E2 gate froze "refusal-spend 59% vs <25%
  target" — a figure the same diagnosis had explicitly quarantined as
  harness-contaminated (n=63, 12 byte-identical force-approved concepts). The
  contaminated number then shaped every E2 score. Airtight chain.
- **Landed-in**: SKILL.md Phase 0.3.

## L-N7 — tame-field-voids-the-diagnostic — PROMOTED
- **Rule**: The 5th-quintile anti-novelty diagnostic not firing on a field where
  everything is comfortably implementable is not evidence the framing worked;
  route "name one credible absent approach" to the adversary.
- **Evidence**: Run 2026-08-14: no collapse (5.71 vs 5.53), but all 20 candidates
  shared two unexamined premises; the red team's absent-approach answer
  (per-channel authorship) became the winning direction.
- **Landed-in**: SKILL.md Phase 4 bias diagnostic; generation-moves.md
  field-tameness check.

## L-N8 — budget-goes-to-generators-tests-redteam — CANDIDATE
- **Rule (provisional)**: Under a fixed budget: keep the disjoint-move
  generators, both execution tests, and the red team; cut the 3-persona panel to
  one code-verifying judge. Keep the panel's qualitative sections if kept at all.
- **Evidence**: Run 2026-08-14 bottom line — value concentrated in those three;
  the scoring apparatus ranked a falsified candidate #1 and the winner last. One
  run; needs confirmation before the panel is cut by default.

## L-N9 — dont-score-the-field-twice — PROMOTED
- **Rule**: After a full hunt, enter solution-tournament at its adversarial
  phases only; running both scoring stacks scores the same field twice.
- **Evidence**: Run 2026-08-14 Q4: novelty-hunt Phase 4 duplicates the
  tournament's middle phases; the tournament's unique contribution was the
  code-verifying skeptic + red team, which produced the decision.
- **Landed-in**: SKILL.md interop section; solution-tournament Related skills.

## L-N10 — background-spawn-reports-fail — PROMOTED (harness, not skill logic)
- **Rule**: Treat `harvest_agent_tail.py` as the PRIMARY retrieval path for
  background generators/judges; prefer Workflow or sync spawns.
- **Evidence**: Run 2026-08-14: 4/4 background subagents idled without
  delivering; TaskOutput failed on all; harvest recovered 4/4. Consistent with
  foreman Run 1 and rules/common/subagent-report-delivery.md.
- **Landed-in**: SKILL.md full-mode dispatch note.

## UNANSWERED
- **U-N1**: Does the frozen gate actually prevent rationalization (its purpose),
  or only relocate anchoring? Not yet testable; one backfire recorded (L-N6).
- **U-N2**: Would a cross-family judge panel produce real disputes on the same
  field? Requires a run with a second vendor configured.
- **U-N3**: Is one code-verifying judge sufficient replacement for the 3-persona
  panel (L-N8), or does the panel's qualitative analysis pay for itself? Needs a
  run with both configurations on comparable problems.

## Run 2026-08-18 — DataSculpt, "density is an undeclared channel"

Full mode, invoked as the remedy after a solution-tournament crossbreed round hit
its PLATEAU rule (no offspring beat the parents). 3 generator subagents on
disjoint moves; 21 raw candidates, ~16 equivalence classes. Scoring panel SKIPPED
by deliberate budget allocation (see L-N8 note below).

### Per-mechanism scorecard
- **Move 6 lens/persona shift — highest yield of the run.** 6 personas, 6
  distinct archived classes, no merges within the set. The signal-processing
  persona produced the candidate that became the decisive execution test; the
  cartographer, security engineer and survey statistician each produced a live
  candidate that reached the final picks or the roster.
- **Move 1 verbalized sampling — the tail beat the head.** The p=0.30 head
  candidate (perceptual blur field) did not reach the picks; the p=0.14
  (renderer writes its own disclosure caption) and p=0.05 (field-primary
  rendering) did — the latter as the Wildcard. The probability labels did their
  job of licensing the tail.
- **Move 5 distant analogy — produced 5 classes, 2 of which converged with other
  generators** (cryptographic differential leakage testing ≡ the signal
  engineer's perturbation certificate; survey-statistics Horvitz-Thompson ≡ the
  survey persona's, independently).
- **Move 3 atypical injection — weakest of the four used.** 3 candidates
  (halftone error diffusion into CORE-1, units-typing into CORE-2, SPC control
  limits into CORE-3); none reached the picks, and the error-diffusion one was
  gated out by the execution result for flattening a true signal. Recorded
  because a mechanism only ever praised is not being evaluated.
- **Phase 3.5 execution test — decisive, third consecutive confirmation.**

### Findings

## L-N1 — execution-tests-before-scoring — PROMOTED (3rd confirmation)
- **New evidence**: Run 2026-08-18. A 4-render perturbation test (~80s, $0)
  measured the wind map's density field under a rescaled sampler and a changed
  RNG phase: r = 0.9972 / 0.9938 / 0.9964 against the shipped render, same
  hottest bin at the same amplitude. This OVERTURNED the conventional favorite
  (every flattening approach, incl. the one the agent had recommended to the
  user, would delete a true data channel) AND wounded the top-novelty candidate
  (Horvitz-Thompson reweighting exists to correct a sampler that biases the
  picture; the test showed the sampler does not bias this picture). The exact
  recorded pattern — both poles moved, before any score existed. Now 3/3.

## L-N11 — cross-generator-convergence-is-a-quality-signal — CANDIDATE
- **Rule (proposed)**: record, per archived class, how many INDEPENDENT
  generators produced it; treat multi-generator convergence as evidence the class
  is load-bearing, and single-generator classes as the true tail.
- **Evidence**: Run 2026-08-18. Generators never saw each other's output. Four
  classes were produced independently 2-3×: perturbation/differential testing
  (signal-processing persona + cryptography analogy); mutual-information leak
  measurement in bits (security persona + verbalized-sampling tail); Horvitz-
  Thompson inclusion-probability reweighting (survey persona + survey-statistics
  analogy); perceptual filtering (psychophysics persona + audio-mastering analogy
  + sampling tail). The convergent class that got executed was decisive; the
  convergence was visible BEFORE the test and would have ranked it first for
  testing. The skill currently has no field for this.

## L-N8 — budget-goes-to-generators-tests-redteam — CANDIDATE (unchanged; NOT confirmed)
- **This run cannot confirm it.** The panel was skipped, so there is no
  panel-vs-no-panel comparison from this run — only the observation that
  generation plus one execution test produced a decision the user acted on.
  Recorded explicitly so absence of a panel is not later miscounted as evidence
  the panel is dispensable.

## L-N10 — background-spawn-reports-fail — PROMOTED (further confirmation)
- **New evidence**: Run 2026-08-18, 6/6 background-spawned subagents (1 skeptic,
  4 scorers, 1 abbreviated skeptic) plus 3/3 generators went idle without
  delivering in-band; harvest_agent_tail.py retrieved all 9. Now 13/13 recorded.

### Interop
First recorded instance of tournament→hunt escalation: the tournament's plateau
rule detected field exhaustion and the hunt supplied the winner. The handoff
worked because the hunt's prompt named and BARRED the premise every tournament
candidate shared. Recommend the tournament's field-ceiling remedy always pass the
shared premise, not only the candidate list.

### Cost
3 generator calls + 1 execution test (~80s compute, $0) + 1 red-team. Panel: 0.
