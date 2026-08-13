# Evidence base — measured constants behind novelty-hunt

Every rule in SKILL.md traces to a measured result from the 2026-08 deep-research sweep (17 reports in `research/`). This file is the index: constant → source. Read before editing the skill; do not replace measured values with invented ones.

## Mode collapse and generation (research/report_llm_diversity.md, report_batch_diversity_metrics.md)

- Tulu-70B semantic diversity: base **45.4%** → SFT **20.8%** → DPO **10.8%**; direct prompting retains **23.8%** of base. Verbalized Sampling recovers **66.8%**, gives **1.6–2.1×** over direct, +182.6% post-DPO. Typicality bias in preference data fitted at **α ≈ 0.57–0.65** (p < 1e-14); mode collapse follows as a theorem (π* ∝ π_ref^γ, γ>1). [Zhang et al., arXiv:2510.01171]
- Frontier models: **<4 functionally distinct responses per 10 samples**; inverse scaling within families (Llama-3.2-1B 7.74 distinct vs 3.3-70B 3.49; Claude-3.5-Sonnet worst tested at 2.76). Functional-equivalence test: "a user who has seen one would still benefit from the other." [NoveltyBench, arXiv:2504.05228]
- Novelty × capability: **r = −0.27** (Arena), **r = −0.48** (MMLU-Pro). Quality selection is anti-novelty selection. [NoveltyBench]
- Temperature/KL cannot recover diversity: KL-penalty reduction made per-input diversity *worse* [Kirk et al., arXiv:2310.06452]; below-human-parity survives full temperature grids [Azad & Baten, arXiv:2605.06540].
- Preference data rewards pleasantness not originality: originality × human preference **ρ = −0.08**; pleasantness × preference **ρ = +0.34**; base models won originality 4/4 vs aligned. [West & Potts, arXiv:2505.00047]
- Raw diversity vs effective diversity: preference-tuned models have lower raw but **higher quality-gated** diversity — measure diversity among gate-passers only. [Shypula et al., arXiv:2504.12522]
- Group homogenization is invisible per-artifact: ChatGPT users' collective divergence dropped (d = 0.47) while individual idea-set diversity was unchanged (d = 0.12). [Anderson et al., arXiv:2402.01536]

## The hit shape (research/report_atypical_combinations.md)

- 17.9M papers: hit rate **9.11/100** for high-conventionality + novel-tail vs 5% base and 2.05 for low+low; impact peaks at **85th–95th percentile of conventionality then reverses**. Novel tail = rare pairings in an otherwise conventional reference frame. [Uzzi et al., Science 2013]
- Highly novel work: **+14.9% citation variance** (high risk), big-hit odds +39.8%, and *penalized* under short evaluation windows (<3yr). [Wang, Veugelers & Stephan]
- Do NOT compute co-occurrence/rarity z-scores within a batch: requirement is N ≳ c·V²/K² reference documents (≈10⁴–10⁵); batch-level pair counts are 0/1 and z = noise/noise. Use an executed, frozen conventional core as the reference instead (Kelly et al.'s point-in-time principle: freeze the reference before generation or batch members suppress each other).
- Unexpectedness vs an **executed primitive baseline** (Ge et al. serendipity structure): highest value-per-effort novelty measure for generated artifacts — the baseline is actually runnable here.

## Judge validity (research/report_llm_evals.md, report_cat_expert_judgment.md, report_systems_models.md)

- Zero-shot LLM judges on expert creative criteria: **κ = 0.016 (GPT-3.5), 0.035 (GPT-4), −0.006 (Claude)** vs expert majority; human expert ceiling **κ ≈ 0.41** item-level, **r ≈ 0.69** aggregate. [TTCW, Chakrabarty et al., CHI 2024]
- Self-consistency: same judge, same artifact, 3 runs → **40%** agreement; r with human reviewers ≈ **0.159**. Single-pass scoring is noise. [arXiv:2508.05470]
- Dimension collapse: LLM-judge inter-dimension correlations **.92–.99** (experts .83–.89) — one latent score regardless of rubric labels. Separate passes required. [Ismayilzada et al., arXiv:2411.02316]
- The inversion: expert ratings rise with semantic diversity/surprise; LLM-judge ratings rise with lexical complexity and **fall** with semantic diversity, novelty, surprise; LLM judges rated AI stories **+1.85** over human (experts: human +1.25). [Ismayilzada]
- Position bias: reordering flipped **82.5%** of verdicts in the adversarial case [Wang et al., arXiv:2305.17926]. Verbosity bias: length-controlling raised rank correlation 0.94→0.98 [arXiv:2404.04475]. Self-preference: self-recognition 73.5–90%+, preference rises linearly with it [Panickssery et al., arXiv:2404.13076]. Disjoint-family panels beat one big judge [Verga et al., arXiv:2404.18796].
- Rubric vs gestalt: decomposed novelty rubric **ρ = .13–.16** vs holistic CAT **.24–.41** against experts (Kränzle); 14 binary tests → κ ≈ 0 for LLM judges (TTCW). Structure helps effectiveness only (r = .31 quality vs −.10 novelty for structured metrics, Miller et al.). **Decompose effectiveness, keep novelty gestalt.**
- Fine-tuned graders are the ceiling if labels exist: semantic distance r = .12–.26, GPT-4 20-shot r ≈ .70, fine-tuned **r = .81** vs human ceiling .83–.88; r = .61 at 804 labels. [Organisciak et al. 2023, Ocsai]
- CAT protocol (the human template): independent expert judges, blind, batch-relative, no defined criteria, full scale, randomized order; expert α .80–.93; novice panels unreliable (random-10 α mean .53–.66, min .06); expertise must match the product population. High α = shared metric, not validity. [Amabile 1982; Kaufman et al. 2008/2013]

## Novelty–usefulness structure (research/report_novelty_usefulness_tradeoff.md, report_rarity_surprise.md)

- Anti-novelty penalty is a **partial effect**: raw r(novelty, score) = **−0.03**; controlled β = **−2.07** on a 10-pt scale, concentrated in the **top novelty quintile** (inverted-U with a cliff). >50% of expert score variance is noise (proposal 26%, evaluator 19%). [Boudreau et al., Mgmt Sci 2016]
- Framing induces the bias: "only one correct solution" essay task → implicit anti-creativity shift (F = 7.87) and a **−0.70** rating drop on a pretested creative+practical stimulus (F = 15.48). Self-report does not detect it (95% explicitly endorse creativity). [Mueller, Melwani & Goncalo, Psych Sci 2012]
- Sense is a precondition for novelty: **91%** of top-quartile n-gram-novel expressions not judged creative; only 3–5% of "novel" annotations were on non-pragmatic/non-sensical text; higher perplexity → *lower* pragmaticality in LLM text (β = −0.17). [Saakyan et al., arXiv:2509.22641]
- Rarity metrics trade appropriateness ~1:1: semantic distance × novelty ratings r = .73–.95, × appropriateness **r = −.81 to −.93**. [SemDis, Beaty & Johnson 2021]
- Forced explicit originality analysis reduces underestimation of truly novel ideas. [Licuanan, Dailey & Mumford 2007]
- Gate-then-sum, never multiply: under measured noise a product compounds relative variance and biases the expectation; keep Simonton's zero-property as a gate.
- Instruction effects: "be creative" moves originality **d = .79** (fluency d = .06) — use in generation, hold constant across anything compared. [meta-analytic, research/report_critiques.md]

## Theory commitments (research/report_boden_4c.md, report_theory.md, report_comp_creativity.md)

- Novelty must name its reference class (Boden P/H-creativity; Ritchie's inspiring set). The executed core is this skill's declared inspiring set.
- Exploratory-vs-transformational is a property of the *generative process*, unmeasurable from artifacts — never a judge question (reliable-but-meaningless). Operationalizable residue: **Ritchie Criterion 18** = novel ∧ high-value ∧ atypical → the Frontier pick.
- Creativity scores are field attributions (Csikszentmihalyi/Amabile): an LLM panel is a synthetic field; specify it, version it, and never claim it "measures creativity."
- Ideation-execution gap: LLM-idea novelty advantage at proposal stage **reversed on every metric** after 100+ hours of execution. Execution evidence outranks judges. [Si et al., arXiv:2506.20803]
- Domain specificity (Baer/APT): cross-domain creative-performance correlations trivially small; expert–novice agreement swings r = .22 (poetry) to .89 (quasi-experts, stories) by domain → domain-routed effectiveness rubrics.

## UNANSWERED (known gaps — do not fill with invented numbers)

- Whether LLM graders inherit the elaboration/length bias at human magnitudes (untested; the ≤120-word format sidesteps it).
- Whether the Mueller framing effect replicates in LLM judges (the self-test in scoring-protocol.md exists to measure exactly this).
- No validated additive-vs-multiplicative composite comparison against an external criterion exists.
- Vendi Score has no human-validation study as a diversity measure; used here only as an optional pool statistic, never a score.
- Batch-size effects on CAT-style judgment are unstudied in humans and LLMs.
