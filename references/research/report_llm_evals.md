Research complete. Report follows.

---

# Evaluating LLM Creativity (2022–2026): What Is Measured, What Is Broken

## Bottom line up front

Three findings should reshape any LLM-graded creativity harness:

1. **Zero-shot LLM judges are near-useless on originality specifically, while being fine on fluency-type dimensions.** On the only expert-validated creative writing benchmark, LLM judges scored Cohen's κ of 0.016 (GPT-3.5), 0.035 (GPT-4) and −0.006 (Claude) against expert majority vote, against a human expert ceiling of κ ≈ 0.41. A separate 2025 audit found the same judge, scoring the same instance, agreed with itself only 40% of the time across three runs, and correlated with human reviewers at Pearson r = 0.159.
2. **Fine-tuning a grader on human creativity ratings works, and works cheaply.** The same task where semantic-distance scorers reach r = 0.12–0.26 and zero-shot prompting reaches r ≈ 0.63 is solved to r = 0.81 by fine-tuning on human-judged responses, and r = 0.61 is reached with only 804 training labels. This is the single most actionable result in the literature.
3. **Novelty and capability are orthogonal or anti-correlated, so a tournament that selects on quality actively selects against diversity.** NoveltyBench measures r = −0.27 with Chatbot Arena rank and r = −0.48 with MMLU-Pro. Frontier models produce fewer than 4 distinct responses in 10 samples.

---

## 1. Human psychometric tests applied to LLMs

### What the studies claim

**Alternate Uses Task (AUT).** Stevenson et al. (2022), *Putting GPT-3's Creativity to the (Alternative Uses) Test* ([arXiv:2206.08932](https://arxiv.org/abs/2206.08932)), collected expert ratings of originality, usefulness and surprise and concluded that on the whole humans outperformed GPT-3, with GPT-3 showing higher means on some dimensions but greater variance.

Koivisto & Grassini (2023), *Best humans still outperform artificial intelligence in a creative divergent thinking task*, Sci Rep 13:13601 ([nature.com](https://www.nature.com/articles/s41598-023-40858-3)), compared n = 256 humans against three chatbots on the AUT. Chatbots beat the human *average*; the best human ideas matched or exceeded the chatbots. Note the 2024 Author Correction (Sci Rep 14:4239, [PMC10879133](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10879133/)): a bug in the analysis code inverted the reported relationship between fluency and max score. A headline finding in a Nature-family journal survived peer review with a sign error in it, which is itself evidence about how much confidence this subliterature earns.

Hubert, Awa & Zabelina (2024, Sci Rep) reported GPT-4 exceeding humans on divergent thinking tasks. **UNANSWERED**: I could not retrieve exact effect sizes; nature.com redirects to an auth wall and my web-search budget was exhausted.

**Torrance (TTCT).** Guzik, Byrge & Gilde (2023, *Journal of Creativity*) is the source of the widely repeated "GPT-4 scores in the top 1% for originality." **UNANSWERED on specifics** (paywalled, unverified). But the structural problem is stateable without the paper: a TTCT percentile is a rank against a *human norming sample*. Placing a machine in that distribution presumes the norms transfer, which is exactly what is in dispute. Treat "top 1% of humans" as a category error rather than a measurement, and do not build on it.

### Why these results do not license the conclusions drawn from them

The critique is not that the numbers are wrong but that the instrument does not mean the same thing when pointed at a model:

- **Predictive validity does not transfer.** Divergent-thinking tests earn their standing in psychology as *predictors* of later creative achievement in people. Baer's long-running critique of TTCT validity (see *The Gold Standard for Assessing Creativity*, Baer & McKool) applies with more force to machines: a score with no downstream criterion is not a creativity measurement, it is a text statistic.
- **Fluency confound.** Human AUT scoring is entangled with how many ideas a respondent produces. An LLM's "fluency" is a sampling parameter, not a trait, so any originality score conditioned on fluency is comparing incommensurable quantities. The Koivisto correction was precisely about this term.
- **Contamination.** AUT/TTCT prompts, and in many cases their published scoring rubrics and exemplar responses, are on the open web. **UNANSWERED**: I found no study that quantifies contamination rates for these specific instruments.
- **Prompt sensitivity.** Documented directly downstream: the same judge changes its overall preference when "is the solution conventional?" is flipped to "unconventional?" (Lu et al. 2025, below). Single-run psychometric scores on LLMs carry no reliability estimate.
- **Consensual Assessment Technique (CAT), the actual gold standard**, requires domain experts rating products against each other. Kaufman et al. (2008, *Creativity Research Journal* 20(2):171–178) compared expert and novice raters and found they are not interchangeable. Non-expert agreement is not a substitute, which matters below.

**Verdict on the hype:** "GPT-4 is more creative than 99% of humans" is unsupported. The defensible version is narrower: *on single-item divergent-thinking prompts scored by originality raters, frontier chatbots beat the mean human response and lose to the best human responses.* That is a statement about the low end of the human distribution, not about creativity.

---

## 2. Novelty versus memorization

**Creativity Index / DJ Search.** Lu et al. (2024), *AI as Humanity's Salieri* ([arXiv:2410.04265](https://arxiv.org/abs/2410.04265)), reconstructs text from web snippets and measures L-uniqueness (fraction of words appearing only in novel n-gram contexts of length ≥ L). Measured results, against a ~900M-document RedPajama reference:

| Comparison | Human advantage | p |
|---|---|---|
| Overall (professional writers vs LLMs) | +66.2% | — |
| Novels | +52.2% | 6.9e-27 |
| Poetry | +31.1% | 1.5e-15 |
| Speeches | +115.3% | 6.1e-31 |
| Novels, verbatim + semantic matching | +102.5% | 2.6e-12 |
| Novels, vs GPT-4 | +30.3% | 2.3e-12 |

Alignment costs measurable novelty: RLHF drops Creativity Index by 30.1% verbatim (p = 1.3e-12) and 8.9% under verbatim+semantic matching (p = 0.01), across GPT base/post-RLHF, LLaMA 2 base/chat, and OLMo base/instruct. At L = 5, machine text mean L-uniqueness is 32.8% against human 56.6%. Differences *between* LLMs were not significant (p = 0.09), which is important: this metric separates human from machine, not model from model.

**The rebuttal, and it is decisive for grader design.** Saakyan, Kim, Muresan & Chakrabarty, *Death of the Novel(ty): Beyond n-Gram Novelty as a Metric for Textual Creativity* (ICLR 2026), collected **8,618 expert-writer annotations** of novelty, pragmaticality and sensicality via close reading. Findings:

- n-gram novelty is genuinely associated with expert-judged creativity (odds ratio ≈ 1.96 per SD of standardized log perplexity), **but roughly 91% of top-quartile n-gram-novel expressions were not judged creative.** Precision is terrible.
- In open-source LLM text, higher n-gram novelty predicts *lower* pragmaticality. In human text there is no such effect (β = 0.01, SE = 0.09). Novel-sounding machine text is disproportionately nonsense.
- An LLM-as-judge novelty score aligned with expert preferences better than Creativity Index did (β = 0.63, SE = 0.26 on an out-of-distribution preference set).

Lu et al. (2025), *Rethinking Creativity Evaluation* ([arXiv:2508.05470](https://arxiv.org/abs/2508.05470)), adds that Creativity Index separates creative from uncreative examples in creative writing but **fails in problem-solving**, and that perplexity ranks in the *opposite* direction from CI on the same data (rejected paper introductions show *lower* perplexity).

**Net:** n-gram novelty is a good machine-text *detector* (AUROC +30.2% over DetectGPT) and a poor creativity *metric*. Use it as a floor check, never as a score.

---

## 3. Homogenization and diversity collapse

**Doshi & Hauser (2024)**, Science Advances 10:eadn5290 ([DOI](https://www.science.org/doi/10.1126/sciadv.adn5290)): 293 writers, 600 evaluators. AI story ideas raised evaluated novelty and usefulness, most in the 5-idea condition, with the largest gains going to writers who were less creative at baseline, while AI-assisted stories were measurably more similar to each other. **UNANSWERED**: exact effect sizes and cosine-similarity deltas. The paper is behind a 403, the UCL repository copy 404s, and the summary sources carry only the Ns.

**Anderson, Shah & Kreminski (C&C 2024)**, [arXiv:2402.01536](https://arxiv.org/abs/2402.01536), 36 participants, ChatGPT versus an Oblique Strategies deck:

- Group level: ideas produced with ChatGPT were less divergent from the group mean embedding (M = 0.24, SD = 0.07) than with Oblique Strategies (M = 0.28, SD = 0.08); t(32) = 2.154, p = 0.038, d = 0.47.
- Individual level: **no difference** (M = 0.65 vs 0.66, t(32) = 0.944, p = 0.352, d = 0.12).
- Self-attributed creative responsibility fell from 63.6% to 48.2% (t(32) = 3.21, p = 0.003, d = 0.67).

The individual-versus-group split is the whole story: each participant's own idea set stayed as varied as before; it was the *between-participant* variety that collapsed. A per-artifact grader cannot see this failure mode at all.

**Padmakumar & He (ICLR 2024)**, [arXiv:2309.05196](https://arxiv.org/abs/2309.05196), 38 hired writers: co-writing with **InstructGPT** significantly reduced lexical and content diversity and increased inter-author similarity; co-writing with **base GPT-3 did not**. The effect traced to the model contributing less diverse text. Feedback tuning is the culprit, not language models per se.

**Kirk et al. (ICLR 2024)**, [arXiv:2310.06452](https://arxiv.org/abs/2310.06452): RLHF substantially reduces per-input diversity versus SFT; across-input diversity also drops, which they call the first rigorous demonstration of across-input mode collapse from RLHF. Two design-relevant details: best-of-N sampling retains diversity comparable to or above SFT, so the reward model alone does not explain the loss; and **lowering the KL penalty reduced per-input diversity further rather than restoring it.** You cannot tune your way back.

**Verbalized Sampling** ([arXiv:2510.01171](https://arxiv.org/abs/2510.01171)) locates the cause upstream in the data: *typicality bias* in human preference annotations, where annotators favor familiar text. Asking a model to verbalize a distribution over responses with probabilities recovers 1.6–2.1x diversity over direct prompting on poems, stories and jokes without measured loss of factual accuracy or safety.

**NoveltyBench** ([arXiv:2504.05228](https://arxiv.org/abs/2504.05228)): 1,100 prompts (100 curated, 1,000 from WildChat), 20 frontier models, distinct-k plus a utility measure that penalizes functional equivalence to prior responses. Models including GPT-4o, Claude 3.5 Sonnet and Gemini 2.0 Pro produce **fewer than 4 distinct responses in 10 samples**. Novelty scales *inversely* with model size within families. Correlation with Arena Overall r = −0.27; with MMLU-Pro r = −0.48.

**The important counterweight.** Shypula et al., *Evaluating the Diversity and Quality of LLM Generated Content* (COLM 2025), argue that diversity without a quality threshold is meaningless (random tokens are maximally diverse). Measuring **effective semantic diversity**, diversity among outputs that clear a quality bar, preference-tuned models show *lower* raw diversity but *higher* effective diversity than SFT or base models. Both claims in the literature are true of different quantities. If your harness cares about usable variety, measure diversity conditioned on passing quality, not raw dispersion.

**Azad & Baten (2026)**, [arXiv:2605.06540](https://arxiv.org/abs/2605.06540), estimate diversity collapse ex ante from model-only generations without human interaction data. All tested models sit below human parity even under a concept-bucket kernel that ignores wording (GPT-5.4 ρ = 0.866 [0.833, 0.894]; Claude Sonnet 4.5 ρ = 0.715 [0.665, 0.759]; Gemini 2.5 Flash ρ = 0.938 [0.920, 0.953]), and the below-parity pattern survives sweeping the temperature grid.

---

## 4. LLM-as-judge on creative work

### The general-purpose number does not transfer

MT-Bench ([arXiv:2306.05685](https://arxiv.org/abs/2306.05685)) reports GPT-4 agreeing with human preferences at over 80%, matching human-human agreement, and names position bias, verbosity bias, self-enhancement bias and limited reasoning as limitations. That 80% is on general helpfulness. It is routinely and wrongly cited as license for creative grading.

### The creative-specific numbers

**TTCW** (Chakrabarty et al., CHI 2024, [arXiv:2309.14556](https://arxiv.org/abs/2309.14556)): 14 binary tests over Fluency (5), Flexibility (3), Originality (3), Elaboration (3); 10 expert writers, 48 stories, 2,016 labels.

| Source | Mean pass rate across 14 tests |
|---|---|
| New Yorker professionals | 84.7% |
| Claude v1.3 | 30.0% |
| GPT-4 | 27.9% |
| GPT-3.5 | 8.7% |

Expert reliability: Fleiss κ 0.27–0.66 per test, mean **0.41**; aggregate per-story counts correlate at Pearson **0.69**. That is your ceiling.

LLM judges against expert majority, Cohen's κ: **GPT-3.5 0.016, GPT-4 0.035, Claude −0.006**. Only two of 42 test-model cells exceeded 0.2 (GPT-4 on Perspective & Voice Flexibility 0.26, Originality in Thought 0.31). Claude went negative on Scene vs Exposition (−0.23), Narrative Pacing (−0.22), Understandability (−0.17). Cost of the human alternative: about 30 minutes and $20 of expert time per story.

**Rethinking Creativity Evaluation** ([arXiv:2508.05470](https://arxiv.org/abs/2508.05470)), the most damaging audit for grader-loop designs:

- **Self-consistency: the same judge scoring the same instance agreed with itself on only 40% of cases across three runs.**
- Pearson correlation with human reviewers on research-idea "contribution": **0.159**; the best cell across models and dimensions was **0.234** (GPT-4o).
- Conditioning jointly on multiple creativity-relevant dimensions produced accuracies of **0.51 and 0.54**, at chance.
- Flipping prompt polarity ("conventional" to "unconventional") flipped overall preferences; the judge also disproportionately classified responses into one label (99.3% / 86.0% confusion asymmetry).

**Expert versus non-expert divergence.** Ismayilzada, Stevenson & van der Plas (ICCC 2025, [arXiv:2411.02316](https://arxiv.org/abs/2411.02316)) had 60 LLMs and 60 humans write five-sentence cue-word stories. Automated metrics showed LLM stories more linguistically complex but lower on novelty, surprise and diversity. **Expert raters agreed with the automated metrics. Non-expert raters and LLM judges rated LLM stories as more creative than human stories.** Inter-judge ICC among LLM judges was 0.43. This is the clearest documented case of the failure mode a harness designer should fear: the cheap judge and the crowd both prefer the polished-but-typical artifact, and only the expert does not.

### Bias magnitudes worth calibrating against

- **Position bias.** Wang et al., [arXiv:2305.17926](https://arxiv.org/abs/2305.17926): merely reordering candidates let Vicuna-13B beat ChatGPT on **66 of 80** queries (82.5%) with ChatGPT judging. Mitigations: multiple evidence calibration, balanced position calibration (score both orders), and entropy-flagged human review.
- **Verbosity bias.** Saito et al., [arXiv:2310.10076](https://arxiv.org/abs/2310.10076): GPT-4 prefers longer answers in creative writing tasks, and when the word-count gap is large it *almost always* picks the longer one, diverging from human preference. Dubois et al., Length-Controlled AlpacaEval ([arXiv:2404.04475](https://arxiv.org/abs/2404.04475)): regressing out length raises Spearman correlation with Chatbot Arena from **0.94 to 0.98** and improves robustness to deliberate verbosity gaming.
- **Self-preference.** Panickssery, Bowman & Feng ([arXiv:2404.13076](https://arxiv.org/abs/2404.13076)): GPT-4 identifies its own outputs at **73.5%** accuracy out of the box; fine-tuning pushes GPT-3.5 and Llama 2 past **90%** self-recognition, and self-preference strength rises **linearly** with self-recognition, with controlled experiments supporting a causal reading. A judge from the same family as a contestant is a rigged tournament.
- **Panels.** Verga et al., *Replacing Judges with Juries* ([arXiv:2404.18796](https://arxiv.org/abs/2404.18796)): a panel of several smaller models from **disjoint families** achieved higher Cohen's κ with human judgments than a single GPT-4 judge, with substantially lower cost and less intra-model bias. (The exact cost multiple is quoted in secondary sources as roughly 7x; I could not confirm the figure in the extracted text, so treat it as **UNVERIFIED**.)
- **Pairwise versus absolute scoring.** Liu et al., PairS (COLM 2024, [arXiv:2403.16950](https://arxiv.org/abs/2403.16950)): converting evaluation into pairwise comparisons with uncertainty-pruned search beats direct scoring on Spearman correlation at O(N log N) comparisons, and the framework measures judge *transitivity*, which is a usable reliability diagnostic for a tournament.

---

## 5. Benchmarks worth knowing

- **CreativityPrism** ([arXiv:2510.20091](https://arxiv.org/abs/2510.20091)): 8 tasks, 3 domains (divergent thinking, creative writing, logical reasoning), taxonomy of quality / novelty / diversity, 17 models. Frontier models lead open models by 0.10 (about 15%) on writing and reasoning but show **no significant advantage on divergent thinking**. Critically: **novelty metrics show weak or negative correlation with other metrics**, and strength in one dimension rarely transfers.
- **EQ-Bench Creative Writing v3** ([eqbench.com](https://eqbench.com/creative_writing.html)): LLM-judged, reporting Rubric Score, Elo, plus explicit **Slop** (frequency match against a curated master list of over-represented LLM phrasings) and **Repetition** (summed frequency of top words, bigrams, trigrams) penalties, with user-adjustable vocab and slop controls. The anti-slop penalty is the practically useful idea; the judge model and rubric items are not documented on the leaderboard page.
- **Research ideation.** Si, Yang & Hashimoto (ICLR 2025, [arXiv:2409.04109](https://arxiv.org/abs/2409.04109)): 49 expert idea writers, 79 blind reviewers, style normalized by an LLM so prose quality could not leak. LLM ideas judged **more novel** (p < 0.05), slightly less feasible. Then the follow-up, *The Ideation-Execution Gap* ([arXiv:2506.20803](https://arxiv.org/abs/2506.20803)): 43 researchers each spent 100+ hours actually executing a randomly assigned idea into a 4-page paper. LLM ideas' scores **dropped significantly more than human ideas on every metric** (novelty, excitement, effectiveness, overall; p < 0.05), and rankings **flipped** so human ideas came out ahead. The novelty advantage was an artifact of judging proposals rather than outcomes.
- Also in the space: BrainTeaser (lateral thinking puzzles), MacGyver (creative problem solving under constraints), NoveltyBench, and Vendi Score plus its extensions ([arXiv:2210.02410](https://arxiv.org/abs/2210.02410), [arXiv:2310.12952](https://arxiv.org/abs/2310.12952)) for similarity-based diversity with an interpretable effective-number-of-modes reading.

---

## 6. Documented gaming and failure modes

Ranked by how well evidenced they are:

1. **Judge prefers polished-typical over novel.** Best evidence: Ismayilzada et al. (LLM judges and non-experts rate LLM stories above human stories while experts and automated novelty metrics say the opposite), plus Saakyan et al. (91% of highly novel expressions are not creative, and in LLM text novelty correlates with non-pragmaticality). Note the human-side precedent: Mueller, Melwani & Goncalo, *The Bias Against Creativity* (Psychological Science 2012), found people express desire for creativity while reliably rejecting it. LLM judges inherit this from preference data.
2. **Verbosity inflation.** Measured; correctable by length-controlled regression (0.94 → 0.98).
3. **Position bias.** Measured at 82.5% verdict flips in the worst case; correctable by order swapping.
4. **Self-preference.** Measured, causal, and worsens as judges get better at self-recognition.
5. **Run-to-run instability.** 40% self-consistency across three runs on identical input. This alone invalidates single-pass scoring.
6. **Prompt-polarity flips.** Rewording the question inverts preferences.
7. **Judging proposals rather than outcomes.** The ideation-execution gap is the cleanest documented case of a creativity score reversing when a real criterion is finally applied.
8. **Benchmark contamination.** Asserted throughout, quantified nowhere I could verify. **UNANSWERED.**

---

## 7. FIT / COST verdict for a blind-grader tournament harness

### Copy these

**A. Fine-tune the grader on human ratings. Highest-value item in this report.** Organisciak, Acar, Dumas & Berthiaume (2023), *Beyond semantic distance: Automated scoring of divergent thinking greatly improves with large language models* (Thinking Skills and Creativity), fine-tuned models (Ocsai) on human-judged AUT responses:

| Method | Correlation with human raters |
|---|---|
| Semantic-distance baselines | r = 0.12–0.26 |
| Best zero/few-shot prompting | r ≈ 0.63 (0.66 prompt-level) |
| Fine-tuned | **r = 0.81** |
| Fine-tuned on 5% of data (804 labels) | r = 0.61 |

Cost: roughly 800 to 2,000 human-labeled artifacts gets a grader that beats every prompt-based approach in the literature, and transfer across prompts was demonstrated. For a statistician this is the obvious play: your grader is a calibrated instrument, not an oracle.

**B. Split the score. Never emit one creativity number.** Every source that decomposed found the dimensions dissociate: TTCW's Fluency/Flexibility/Originality/Elaboration have wildly different judge reliability (κ from −0.23 to 0.31 on the same judge); CreativityPrism found novelty weakly or negatively correlated with other dimensions; Saakyan et al. found novelty and pragmaticality anti-correlated in LLM text. Score novelty, quality/pragmaticality, and surprise separately, report them separately, and combine only with an explicit, stated weighting.

**C. Pairwise with both orders, then aggregate.** Pairwise ranking (PairS) beats absolute scoring; balanced position calibration kills the largest single bias; PairS's transitivity measure gives you a per-judge reliability statistic for free. Cost: O(N log N) comparisons instead of O(N).

**D. Panel of judges from disjoint model families.** Better human correlation than one large judge, cheaper, and structurally immune to the self-preference bias that is linear in self-recognition. Hard rule: never let a judge from the same family as a contestant vote on it.

**E. Report a reliability estimate on every run.** Given 40% self-consistency, any single-pass score is noise. Sample each judgment k times, report the dispersion, and treat instances with high dispersion the way Wang et al. do, as flagged for human review. This is also the natural place for Krippendorff's alpha or ICC against your human-labeled holdout: the TTCW expert ceiling of κ ≈ 0.41 and Pearson 0.69 tells you what "good" means. Do not chase agreement above the human ceiling; that is a sign of a degenerate rubric.

**F. Length-control the score.** Fit the GLM, predict at zero length difference. Measured payoff was 0.94 → 0.98 rank correlation.

**G. Add a slop/repetition floor check.** EQ-Bench's master-slop-list matching and n-gram repetition score are cheap, deterministic, and catch the exact degradation a judge rewards.

**H. Measure diversity at the population level, conditioned on quality.** Two hard lessons: Anderson et al. showed the collapse is invisible per artifact and only appears between participants; Shypula et al. showed raw diversity is the wrong quantity and *effective semantic diversity* (dispersion among outputs that pass a quality bar) reverses the sign of the conclusion. For a tournament, this means tracking dispersion across the surviving pool each round, because selection on quality is selection against variety (r = −0.27 to −0.48 between novelty and capability).

**I. If you need diverse candidates, use Verbalized Sampling, not temperature.** 1.6–2.1x measured diversity gain, training-free, no accuracy loss. Temperature does not recover the loss (Azad & Baten temperature grid; Kirk et al. KL-penalty result showing lower KL made per-input diversity *worse*).

**J. Judge outcomes where an outcome exists.** The ideation-execution gap is a direct warning about proposal-stage tournaments. If your artifacts are solutions or designs that can be executed, tested, or rendered, the execution result outranks any judge score.

### Do not copy these

- **Zero-shot LLM judging of originality.** κ ≈ 0.03 against experts, r ≈ 0.16 against human reviewers, chance-level accuracy under multi-dimension conditioning. It is fine for fluency, coherence and mechanics; it is not a novelty instrument.
- **n-gram novelty or Creativity Index as a creativity score.** 91% false-positive rate at the top quartile, anti-correlated with usefulness in LLM text, fails outside creative writing. Keep it as a memorization floor check only.
- **Perplexity as a creativity proxy.** Ranks in the opposite direction from CI on the same data.
- **Any TTCT/AUT percentile claim about a model.** Norm samples do not transfer; the flagship result carries a published analysis-code correction.
- **Non-expert or crowd validation of a creativity grader.** Non-experts and LLM judges systematically inverted the expert verdict on the same stories. Kaufman et al. (2008) established that expert and novice raters are not interchangeable; the LLM era confirmed it.
- **Single-run scores, single judge, single presentation order.** Each of the three is independently documented to flip outcomes.

### Cost summary

| Item | Cost | Expected payoff |
|---|---|---|
| Human-label 800–2,000 artifacts, fine-tune grader | Real money; TTCW-style expert rating ran ~$20 and 30 min per story | r 0.12–0.26 → 0.81 |
| Panel of 3 disjoint-family small judges | Cheaper than one frontier judge | Better human correlation, kills self-preference |
| Both-order pairwise + k repeats | ~2k× inference | Removes the 82.5% position flip, gives a reliability estimate |
| Length-control GLM | Trivial | +0.04 rank correlation, gaming resistance |
| Slop/repetition floor | Trivial, deterministic | Blocks the dominant reward-hack |
| Population-level effective diversity per round | Cheap (embeddings + quality gate) | Detects the failure mode per-artifact scoring cannot see |

---

## UNANSWERED

- Doshi & Hauser exact effect sizes and cosine-similarity deltas (Science Advances 403s; UCL repository copy 404s).
- Hubert, Awa & Zabelina (Sci Rep 2024) exact effect sizes.
- Guzik et al. (2023) methodology and norming sample behind the "top 1%" claim; the structural objection stands regardless.
- Any quantified measurement of AUT/TTCT/RAT contamination in training corpora. This is a real gap in the field, not just in my sweep.
- PoLL's exact cost multiple versus a single GPT-4 judge (secondary sources say roughly 7x; not confirmed in the extracted text).
- EQ-Bench Creative Writing v3's judge model identity and rubric item list.

Full-text PDFs for the papers I extracted numbers from are in (local session cache, not included) as `*.pdf` with matching `*.pdf.txt` extractions, if the team lead wants to verify any figure.