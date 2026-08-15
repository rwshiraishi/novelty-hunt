# Scoring protocol — judge blocks, panel design, bias controls

The design target: a CAT-style panel (the validated human protocol) with every documented LLM-judge failure mode explicitly countered. Zero-shot LLM judgment of originality is near-useless raw (κ ≈ 0.02–0.04 vs experts; 40% self-agreement across runs); everything below exists to claw that back.

## The judge prompt block (adapted Kaufman/Amabile CAT instructions)

Prepend to every scoring pass:

> You will rate {n} candidate solutions. Compare the candidates **to one another**, not to an external standard or an imagined ideal. **Multiple valid solutions exist for this problem** — there is no single correct answer. Use the **full 1–7 scale**: the strongest candidate in this batch on this dimension should score near 7, the weakest near 1. Rate only the dimension named below; ignore everything else, including length and polish.

Load-bearing details:
- **"Multiple valid solutions exist"** is not decoration. The opposite framing ("identify the single best") experimentally *induces* anti-novelty bias (implicit-bias shift d ≈ 0.5; a pretested creative-AND-practical stimulus dropped 0.70 points under it).
- **Batch-relative** anchors the scale; CAT scores have no absolute meaning, and pretending otherwise produces cross-run comparisons that mean nothing. Ranks within this batch are the output.
- **Blind**: strip which move/persona/round produced each candidate, and randomize candidate order on every pass (position bias alone has flipped 82.5% of verdicts in adversarial tests).

## Dimension passes (separate, in this order)

**1. Novelty — gestalt, retrieval-grounded.** For each candidate:
- (a) Forced analysis first: "In 1–2 sentences: what exactly is original here, relative to CORE-1..n — and at what structural level does it differ (goal / mechanism / component / detail)? If nothing, say so." (Forced originality analysis measurably reduces underestimation of truly novel ideas; naming the structural level against named referents is the best-validated novelty construction — structural difference, never within-batch frequency.)
- (b) Then one holistic 1–7 rating. Report the level alongside the number.
- **Never a novelty checklist.** Decomposed novelty rubrics score ρ ≈ .13–.16 against experts vs .24–.41 for holistic judgment, and binary test batteries collapse to chance for LLM judges.

**2. Effectiveness — decomposed.** Answer the Phase-0 anchored questions (pass/fail or 0–2 with concrete verbal anchors), then map the answer pattern to a score via a fixed lookup rule declared in Phase 0 — judgment becomes feature extraction plus deterministic mapping, which is auditable and diffable. Rescale to 1–7. Structure helps on this axis and only this axis.

**3. (Optional) Elegance** — one holistic 1–7, only when the domain rewards it (design, prose, naming). Skip for most technical hunts.

Do not average dimensions into a "creativity" score. If a scalar is demanded: apply the effectiveness gate (candidates below gate are reported but unranked), then weighted sum with declared weights. **Never multiply dimensions** — with judge noise at documented levels (>50% of expert rating variance is neither artifact nor stable judge), a product amplifies noise and biases the expectation.

## Panel design

- **k = 2–3 passes with distinct personas**: (1) skeptical senior operator ("what breaks"), (2) domain enthusiast ("what's exciting and why"), (3) informed outsider (adjacent field). Persona diversity is the lightweight stand-in for the independent-judges requirement; in full mode, use different model families where configured — same-family judge-and-generator is a rigged match (self-preference rises linearly with self-recognition, which hits 73–90%). **Field-verified 2026-08-14**: a same-family 3-persona panel produced zero disputes and max spread 2 across 20 candidates, and its zero-spread #1 was falsified in one grep — a same-family panel is one judge with k voices; print that in the output and treat the spread column as decorative.
- **Judge input rule (structural)**: judges receive the RAW generator text per candidate alongside the merged class label — never only the lead's compressed archive prose. One author's summary is a shared input that fully correlates the panel, and judges echo its framing back verbatim as "independent" confirmation. If only merged prose is affordable, the merger must not author the scoring brief.
- **Code-verifying judge**: when a candidate makes a checkable factual claim about the artifact, at least one judge verifies it against the artifact and reports the exact command in its card. Framing controls (blind, batch-relative) do not substitute for touching the artifact — the recorded failure had all framing controls in place and every judge accepting a false premise no one checked.
- **Two-stage scoring** for batches >8: coarse triage (low / medium / high) in one pass, then fine 1–7 scoring within tiers. Validated cheap approximation to full pairwise.
- **Pairwise duels, both orders**, only to break ties in the top tier.
- **Dispersion is the reliability report**: per-candidate spread across passes; spread ≥3 → DISPUTED (re-judge or surface to user). Report the spread column always.
- **Suspiciously high agreement is a red flag, not validation.** Human expert ceilings are κ ≈ 0.4 item-level, r ≈ 0.7 aggregate. A panel agreeing far above that shares a bias (same weights, same priors); say so in the output. Never report panel self-consistency as a quality metric.
- **Full mode, when judge count ≥4**: model judges rather than averaging them — note each judge's severity (mean offset) and discrimination (spread); an uninformative or drifted judge gets downweighted or dropped, not averaged in (JRT/MFRM insight: its value is diagnostic — it tells you which judge is broken).

## Known judge pathologies and their counters

| Pathology (measured) | Counter in this protocol |
|---|---|
| Judges prefer polished-typical; ratings *fall* with semantic diversity & surprise while experts' rise | Wildcard pick never auto-dropped; forced originality analysis; bias diagnostic below |
| Verbosity bias (longer ≈ preferred) | ≤120-word mechanism format equalizes length; instruction to ignore length |
| Position bias (82.5% flips) | Randomized order per pass; both-order duels |
| Self-preference (linear in self-recognition) | Cross-family judges in full mode; blind provenance |
| Run-to-run instability (40% self-agreement) | k repeats + dispersion + DISPUTED flag; never single-pass |
| Dimension collapse (inter-dimension r = .92–.99) | Separate passes per dimension, novelty first, different framings |
| Anti-novelty partial effect (invisible marginally, β ≈ −2 controlled, top-quintile concentrated) | 5th-quintile diagnostic below |

**5th-quintile diagnostic**: after scoring, sort by novelty and check the top quintile's effectiveness scores. A collapse concentrated there matches the documented human anti-novelty signature (score cliff at the highest-novelty band). Flag it in the output and re-examine those candidates individually — the collapse may be real infeasibility or judge bias, and no score can tell you which; only an execution test can.

## Self-test (optional calibration, run when tuning this skill)

Mueller two-framing check: take one candidate pretested/designed to be simultaneously high-novelty and high-effectiveness. Score it under (A) "identify the single best solution" framing and (B) the standard "multiple valid solutions exist" framing, k=5 each. A significant A<B gap reproduces the human bias in the judge; the protocol's framing is doing real work. No gap: framing is cheap insurance either way. Record results in evidence.md when run.

## Output contract

The scoring phase returns, per candidate: novelty (1–7), effectiveness (1–7 from anchors), spread, DISPUTED flag, one-line originality analysis. Plus batch-level: effective-diversity count, 5th-quintile diagnostic result, panel-agreement note. These feed the Phase 5 delivery table verbatim.
