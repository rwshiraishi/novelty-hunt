Web search budget is exhausted; here is the report from what I gathered.

---

# Diversity metrics & mode collapse in LLMs (2023–2026)

**Legend:** ✅ VERIFIED = read in the paper's own full text (HTML/PDF body). ⚠️ UNVERIFIED = from abstract, secondary source, or search snippet only. ❌ UNANSWERED = could not confirm.

---

## 0. arXiv ID audit (you asked me to correct you)

| You said | Reality | Verdict |
|---|---|---|
| 2310.06452 = Kirk et al., "Understanding the Effects of RLHF on LLM Generalisation and Diversity" | Correct. Kirk, Mediratta, Nalmpantis, Luketina, Hambro, Grefenstette, Raileanu. ICLR 2024, v3 Feb 2024 | ✅ correct |
| 2309.05196 = Padmakumar & He, "Does Writing with Language Models Reduce Content Diversity?" | Correct. Vishakh Padmakumar, He He. ICLR 2024 | ✅ correct |
| 2504.05228 = NoveltyBench | Correct, but the exact title is "**NoveltyBench: Evaluating Language Models for Humanlike Diversity**" — Zhang, Diddee, Holm, Liu, Liu, Samuel, Wang, Ippolito | ✅ correct |
| 2410.04265 = Lu et al., "AI as Humanity's Salieri … CREATIVITY INDEX" | Correct ID. Full title is "AI as Humanity's Salieri: **Quantifying Linguistic Creativity of Language Models via Systematic Attribution of Machine Text against Web Text**" — Ximing Lu, Melanie Sclar, Skyler Hallinan, Niloofar Mireshghallah, Jiacheng Liu, Seungju Han, Allyson Ettinger, Liwei Jiang, Khyathi Chandu, Nouha Dziri, Yejin Choi | ✅ correct |
| "Verbalized Sampling" (no ID given) | **arXiv:2510.01171** (v3). Jiayi Zhang\*, Simon Yu\*, Derek Chong, Anthony Sicilia, Michael R. Tomz, Christopher D. Manning, Weiyan Shi. Northeastern / Stanford / WVU | ✅ exists |
| Chakrabarty "Art or Artifice?" | **arXiv:2309.14556**, CHI 2024, DOI 10.1145/3613904.3642731. Chakrabarty, Laban, Agarwal, Muresan, Wu | ✅ |
| DivPO (Meta) | **arXiv:2501.18101**, "Diverse Preference Optimization," Jack Lanchantin et al. (Meta + NYU + ETH Zurich), v1 Jan 30 2025, v4 May 22 2025 | ✅ |
| Anderson et al. C&C 2024 | **arXiv:2402.01536**, DOI 10.1145/3635636.3656204. Barrett R. Anderson, Jash Hemant Shah, Max Kreminski | ✅ |
| "diversity-rewarded RLHF" | Closest real artifact is **arXiv:2410.06084, "Diversity-Rewarded CFG Distillation"** (Google DeepMind; Cideron, Agostinelli, Ferret, Girgin, Elie, Bachem, Perrin, Ramé). **Caution: it is text-to-music (MusicLM), not text.** | ⚠️ your framing was off |

---

## 1. Mode collapse / diversity loss from alignment

### 1.1 Kirk et al. 2024 (arXiv:2310.06452) ✅ metrics VERIFIED, ❌ headline numbers UNANSWERED

**Metrics — your recollection was ~2/3 right.** They used **three** metrics, all validated against human judgment by Tevet & Berant (2021):

1. **EAD — Expectation-Adjusted Distinct n-grams** (syntactic). Distinct n-grams averaged over n = 1…5, with the Liu et al. (2022) expectation adjustment that removes "the bias towards shorter outputs." ✅
2. **Sentence-BERT** (semantic). Embed each output, take mean pairwise cosine similarity, score = 1 − mean similarity. ✅
3. **NLI diversity** (logical). An NLI model over sampled sentence pairs; more contradictions / fewer entailments = more diverse. ✅ **You missed this one.**

> **Correction:** there is *no separate fourth "N-gram diversity" metric*. EAD **is** the n-gram metric. So the set is {EAD, Sentence-BERT, NLI}, not {EAD, Sent-BERT, N-gram}.

**Per-input vs across-input** ✅ VERIFIED. K = 16 samples per input, N = 500 inputs, temperature 1.
- **Per-input**: apply D to each input's 16-output set, average over inputs → diversity of π(y|x).
- **Across-input**: apply D to the union of *one* output (the first) per input → diversity of the marginal π(y).

**Setup** ✅: LLaMA 7B (main), OPT at 5 sizes (appendix, summarisation only). Methods: SFT; RLHF via PPO with KL coefficient **β = 0.05**; **Best-of-N with N = 16, temperature 0.7**. Tasks: TL;DR summarisation (OOD = CNN/DailyMail) and AlpacaFarm instruction following.

**Findings** ✅ qualitative:
- Per-input: RLHF has "much lower output diversity than SFT" on EAD and Sentence-BERT.
- Across-input: SFT slightly more diverse, but "the difference is much smaller than in the per-input case."
- **BoN has similar or higher across-input diversity than SFT** on EAD and Sentence-BERT — i.e. BoN buys generalisation without the per-input diversity tax that PPO imposes.
- NLI diversity shows **no meaningful difference** between any method, in either setting.
- Raising the KL penalty **decreased** per-input diversity rather than recovering it (counterintuitive, worth flagging).
- Instruction-following diversity showed no meaningful differences; the authors attribute this to the metrics being suited to short outputs. Diversity results are therefore effectively **summarisation-only**.

❌ **UNANSWERED: the exact numeric diversity values.** Kirk et al. report diversity only in **Figures 5 and 6** — there is no diversity table. The only numbers in the text are generalisation numbers (RM accuracy 75.8 ID / 71.6 OOD; ~3.5% head-to-head winrate shifts). **So there is no citable "RLHF reduces diversity by X%" number in this paper.** If you need one, you must digitise the figures or run their code (`github.com/facebookresearch/rlfh-gen-div`). Do not let a secondary source convince you otherwise.

### 1.2 Padmakumar & He 2024 (arXiv:2309.05196) ✅ VERIFIED in full

**Study**: 38 Upwork writers, 10 topics, 100 essays per condition (300 total) — Solo / GPT-3 / InstructGPT co-writing. Decoding: temp 0.9, top-p 1, freq+presence penalty 0.5, 30-token suggestions.

**Metrics** ✅ (note: **no cosine similarity anywhere** — they use Rouge-L and BertScore):
- **Essay homogenization** hom(d|t) = mean pairwise sim(d, d′) over other essays on the same topic; **corpus homogenization** = mean over all essays. Similarity function is **Rouge-L** or **BertScore** (`microsoft/deberta-xlarge-mnli`), applied at raw-essay level and at key-point level.
- **Lexical diversity**: fraction of unique n-grams, n = 1–5.
- **Key point diversity**: agglomerative clustering of extracted key points (complete linkage, distance = 1 − Rouge-L); diversity = unique clusters / total key points, swept over thresholds.
- **Model suggestion diversity**: pairwise similarity among the 5 continuations per query (Self-BLEU-like).

**Numbers** ✅:
- Corpus homogenization (key-point, Rouge-L): **Solo 0.1536 / GPT-3 0.1578 / InstructGPT 0.1660**. InstructGPT > both, *p* < 0.05; GPT-3 vs Solo n.s.
- Unique n-gram fraction (Solo / GPT-3 / InstructGPT): 1-gram 0.119/0.116/0.115; 2-gram 0.602/0.585/0.579; 3-gram 0.898/0.886/0.869; 4-gram 0.973/0.967/**0.953**; 5-gram 0.991/0.988/**0.977**. 4- and 5-gram differences significant (χ², *p* < 0.05).
- Unique key-point fraction at clustering threshold 0.6: **0.941 / 0.927 / 0.877** (permutation test, 1000 perms, *p* < 0.05).
- **Model suggestion similarity (Rouge-L): InstructGPT 0.20 vs GPT-3 0.11** — the feedback-tuned model's own suggestions are ~2× less diverse. This is the mechanism.
- **Attribution**: user-written 5-gram distributions and user key-point homogenization are *unchanged* across conditions. The homogenization is entirely carried by model-contributed text.
- No quality difference (relevance 4.30/4.15/4.10; grammaticality 4.00/4.10/4.00; depth 3.95/3.85/3.80 — all n.s.).
- Perplexity (GPT-2): 25.07 / 22.10 / 20.26.

### 1.3 Verbalized Sampling & typicality bias (arXiv:2510.01171) ✅ VERIFIED

**The typicality-bias theory** (this is the interesting contribution — a *data-level*, not algorithmic, explanation for mode collapse):
- Reward model: **r(x,y) = r_true(x,y) + α·log π_ref(y|x) + ε(x)**, α > 0 the typicality weight, π_ref a base model's log-likelihood as typicality proxy. Grounded in mere-exposure effect, availability heuristic, processing fluency, schema congruity.
- Under KL-regularized RLHF the optimal policy becomes **π\*(y|x) ∝ π_ref(y|x)^γ · exp(r_true/β)** with **γ = 1 + α/β > 1**. When true rewards are flat across a response set (the creative-task case), this collapses to pure temperature-sharpening π\* ∝ π_ref^γ. **Mode collapse falls out as a theorem, not a bug.**

**Empirical evidence for α** ✅: on **6,874 HelpSteer response pairs** with identical correctness ratings, fitted **α̂ = 0.57 ± 0.07** (Llama-3.1-405B-Base as π_ref) and **α̂ = 0.65 ± 0.07** (GLM-4.5-Base), both **p < 10⁻¹⁴**.

**Metrics used** ✅: (1) semantic diversity = 1 − mean pairwise cosine (OpenAI `text-embedding-3-small`), negatives clipped, reported as %; (2) lexical = ROUGE-L; (3) human 4-point Likert; (4) Distinct-1/2/3 (dialogue); (5) Flesch–Kincaid; (6) KL divergence vs RedPajama reference distribution (open-ended QA); (7) Coverage-N; (8) Precision.

**Numbers** ✅:
- VS gives **1.6–2.1×** diversity over direct prompting on poems/stories/jokes; human ratings **+25.7%**.
- VS recovers **66.8%** of base-model diversity; post-DPO, VS beats direct by **182.6%**.
- **Tulu-70B post-training ablation (the cleanest mode-collapse measurement I found anywhere):** base model diversity **45.4%** → post-SFT direct **20.8%** → post-DPO direct **10.8%**. Direct prompting retains only **23.8%** of base diversity. VS holds ~30% across all stages.
- Human study (30 annotators/task, 90 pairs): Poem Direct 1.90 → VS 2.39; Story 2.74 → 3.06; Joke 1.83 → 3.01.
- **Larger models lose more and gain more**: GPT-4.1 / Gemini-2.5-Pro gain **1.5–2×** more from VS than their Mini/Flash siblings.
- Synthetic math data: VS-Multi averages **37.5** vs Direct **30.6** and no-synthetic baseline **32.8** across MATH500/OlympiadBench/Minerva.

### 1.4 West & Potts, "Base Models Beat Aligned Models at Randomness and Creativity" (arXiv:2505.00047) ✅ VERIFIED

Metric is **Pearson χ² divergence** from uniform (not entropy, not KL — worth noting).

Random 10-length integer sequences, χ² (lower = better):

| Size | Base | Llama-Instruct | Tulu-DPO | Tulu-SFT | Tulu-Full |
|---|---|---|---|---|---|
| 8B | **13.9** | 115.1 | 100.8 | 52.3 | 129.1 |
| 70B | **29.2** | 43.6 | 22.9 | 21.8 | 18.3 |

- Repetition mode: base = **3 repetitions** per length-10 sequence (matches true randomness); **every** aligned model's mode = **0**.
- Rock-Paper-Scissors net score (uniform = 0.0), 70B: Base **−15.0** vs Llama-Instruct **−50.1**.
- Hide & Seek, 70B: Base **+51.0** vs Llama-Instruct **−38.0**, Tulu-Full **−33.6**.
- Creative poetry (720 pairwise human comparisons): base wins **originality in 4/4** contests. **Originality vs. preference: Spearman ρ = −0.08; pleasantness vs. preference: ρ = +0.34.** That pair of correlations is the sharpest available evidence that preference data rewards pleasantness, not originality — it's the empirical shadow of VS's α.

---

## 2. Creative-writing diversity benchmarks

### 2.1 NoveltyBench (arXiv:2504.05228) ✅ VERIFIED

**Metric — your description is right.** Two numbers, k = 10 samples/prompt, temperature 1.0:
- **distinct_k = |{c_i}|** — number of functional equivalence classes among k samples. Equivalence rule: two outputs differ iff "a user who has seen one generation would likely benefit from seeing the other."
- **utility_k = [(1−p)/(1−p^k)] · Σ_i p^(i−1) · 𝟙[c_i ≠ c_j ∀ j<i] · u_i**, patience **p = 0.8**. Duplicates contribute **zero**. As p→0 it degenerates to ordinary single-generation quality eval. Quality u_i from **Skywork-Reward-Gemma-2-27B-v0.2**, calibrated on 2,400 MT-Bench generations against GPT-4 judgments, mapped to 1–10.
- **Equivalence model**: fine-tuned **deberta-v3-large** on 1,000 author-annotated pairs; **79% accuracy, F1 = 0.811**, AUC 0.81.

**Numbers (distinct / utility out of 10)** ✅:

| Model | Distinct | Utility |
|---|---|---|
| gemma-2-2b-it | **6.66** | **4.63** |
| Llama-3.2-1B | **7.74** | 2.81 |
| Llama-3.1-8B | 6.24 | 3.76 |
| Llama-3.1-405B | 4.20 | 3.39 |
| Llama-3.3-70B | 3.49 | 2.87 |
| gpt-4o | 3.88 | 3.27 |
| gpt-4o-mini | 3.65 | 3.11 |
| Claude-3 Opus | 3.04 | 2.67 |
| Claude-3.5 Sonnet | **2.76** | 2.36 |
| gemini-1.5-pro | 2.85 | 2.73 |
| command-r7b | 4.58 | 3.35 |

**Headline** ✅: frontier closed models produce **fewer than 4 distinct responses per 10 samples**. **Inverse scaling within families** — Llama-3.2-1B (7.74) vs Llama-3.3-70B (3.49); gemma-2-2b-it (6.66) vs 27b-it (4.03); Claude-3.5-Sonnet is the *least* diverse model tested. Human baseline (8 authors, deliberately a *lower bound* on human diversity) beats most SOTA models. In-context regeneration is the only prompting fix that works — under it GPT-4o and Gemini-2.0-Pro *exceed* human cumulative utility.

**Field-level statistic worth quoting** ✅: of **67 benchmark papers at COLM 2024 / ICLR 2025, over 90% evaluate on a single or best generation.** That is the census answer to "why nobody measures this."

### 2.2 Chakrabarty et al., "Art or Artifice?" (arXiv:2309.14556, CHI 2024) ⚠️ UNVERIFIED numerics

TTCW = **14 binary tests** across Torrance's Fluency / Flexibility / Originality / Elaboration, administered by **10 professional creative writers** via Consensual Assessment Technique. Benchmark: **48 stories** (~1,400 words) — 12 by professionals, 36 by ChatGPT / GPT-4 / Claude 1.3.

- **LLM stories pass 3–10× fewer TTCW tests than professional stories.** ⚠️ (consistent across sources; I did not read the table)
- GPT-4 stronger on Originality; Claude 1.3 stronger on Fluency/Flexibility/Elaboration. ⚠️
- **No LLM judge positively correlated with expert assessments.** ⚠️
- ❌ UNANSWERED: per-model pass counts out of 14.

Note this is a **quality/creativity** instrument, not a diversity instrument — it scores a single artifact, not a set. Don't conflate it with distinct-k.

### 2.3 Lu et al., CREATIVITY INDEX (arXiv:2410.04265) ⚠️ UNVERIFIED numerics (abstract-level)

Metric: reconstructability of a text from web snippets, computed by **DJ SEARCH**, a dynamic-programming algorithm finding verbatim and near-verbatim matches against a web corpus. Fewer/shorter matches = more creative.

- Professional human authors score **66.2% higher** than LLMs. ⚠️
- **Alignment lowers CREATIVITY INDEX by an average of 30.1%.** ⚠️ **This is the single most quotable "alignment costs creativity" number in the literature** — but I only have it from the abstract, so verify against §5 before you cite it as measured.
- As a zero-shot machine-text detector it beats DetectGPT by 30.2% and beats supervised Ghostbuster in 5/6 domains. ⚠️

### 2.4 Vendi Score / Self-BLEU in creative writing ⚠️ partial

- **Vendi Score**: arXiv:2210.02410, Friedman & Dieng. Defined as **exp(Shannon entropy of the eigenvalues of a user-specified similarity matrix)**. Reference-free — no held-out distribution needed, which is why it travels well across domains.
- **Conditional Vendi / Conditional RKE**: arXiv:2411.02817. Explicitly motivated by the fact that Vendi and RKE were built for *unconditional* models and **cannot separate prompt-induced from model-induced variability** — a real problem for prompted LLM eval, since a diverse prompt set inflates Vendi without the model being diverse at all.
- Follow-up work on finite-sample behavior exists ("Do Vendi Scores Converge with Finite Samples?", "Truncated Vendi Score"). ⚠️
- ❌ UNANSWERED: I did not find a *major* LLM creative-writing benchmark whose headline metric is Vendi. It appears as a secondary metric (e.g. an Arabic educational-story generation study pairing Vendi over BAAI/bge-m3 embeddings with 1 − mean Self-BLEU). **Vendi is well-cited but not dominant in this subfield.**

---

## 3. Homogenization / collective diversity

### 3.1 Doshi & Hauser, *Science Advances* 10(28) eadn5290, 12 Jul 2024 ⚠️ UNVERIFIED (paywalled, 403; SM PDF exceeded fetch size)

**Measure** ⚠️: two cosine similarities on story embeddings — (a) story vs. its source GenAI idea, and (b) **the focal story's embedding vs. the *average* embedding of all other stories**. Measure (b) is the collective-diversity number. Note this is a *centroid* similarity, not mean pairwise — a subtle but real difference from Kirk/VS-style 1 − mean-pairwise.

**Numbers** ⚠️:
- **+10.7% similarity** between stories in the one-GenAI-idea condition vs. the no-AI control.
- Individual gains, low-DAT writers, five-GenAI-ideas condition: writing quality **+26.6%**, enjoyment **+22.6%**, boringness **−15.2%**; enough to close the gap with high-DAT writers.
- Framing: a social dilemma — individually better off, collectively narrower.
- ❌ UNANSWERED: which embedding model, exact N, regression coefficients/SEs, and the similarity figure for the five-idea condition specifically. Data is on Dryad (doi:10.5061/dryad.qfttdz0pm) if you need them.

### 3.2 Anderson, Shah & Kreminski (arXiv:2402.01536, C&C 2024) ⚠️ UNVERIFIED numerics

36 participants, four TTCT-derived divergent-ideation tasks, within-subjects: half with ChatGPT (GPT-3.5), half with **Oblique Strategies** (a non-AI creativity-support tool). **1,271 ideas** total.

- **Group-level homogenization confirmed**: ideas produced with ChatGPT were more semantically similar *across users* than those from Oblique Strategies. ⚠️
- **Individual-level homogenization NOT found**: each individual's own idea set was similarly diverse in both conditions. ⚠️
- ChatGPT users produced *more* ideas and *more detailed* ideas, but no commensurate diversity gain — quantity ≠ diversity. ⚠️
- Ownership: participants assigned themselves less responsibility for ChatGPT-assisted ideas (**M = 48.17%, SD = 26.22%**). ⚠️
- ❌ UNANSWERED: embedding model, exact similarity means, p-values, effect sizes (my fetch of the author PDF was cut off by the session limit and I could not retry within budget).

**Cross-paper convergence worth noting:** Anderson (individual OK / collective harmed) and Kirk (per-input harmed / across-input barely harmed) point in *opposite* directions on which level collapses. Kirk measures one model sampled repeatedly; Anderson measures many humans sharing one model. They are consistent once you see that a human user supplies the between-user variation that the model itself lacks — the model's per-input collapse becomes the population's across-user collapse only when the model's contribution dominates. Padmakumar's attribution result (user-written text unchanged; model-written text carries the whole effect) is the bridge. That triangulation is, I think, the most defensible synthesis available.

---

## 4. Diversity-aware decoding and training

| Method | arXiv | Diversity criterion optimized | Reported metric & numbers |
|---|---|---|---|
| **Diverse Beam Search** | 1610.02424 (Vijayakumar et al.) | Beam budget split into G groups; dissimilarity penalty between groups added to the likelihood objective (Diverse M-Best MAP) | ⚠️ Pre-LLM era; reported on captioning/VQA with n-gram diversity + oracle accuracy. ❌ no LLM-scale numbers gathered |
| **Determinantal Beam Search** | 2106.07400 (Meister et al.) | Each beam step = subdeterminant maximization over a PSD kernel; standard beam search = diagonal kernel. Related to k-DPPs | ⚠️ "more diverse sets than standard beam search with minimal BLEU trade-off"; beats stochastic beam search, slightly beats DBS. ❌ exact numbers not read |
| **D5P4** (DPP for discrete diffusion LMs) | 2603.19146 | Beam selection as MAP inference over a DPP | ⚠️ Measures **self-cosine (Jina Embeddings v2)** semantically + **Self-BLEU and EAD** lexically — a good example of the modern 2-axis convention |
| **Diversity-Rewarded CFG Distillation** | 2410.06084 (DeepMind) | **Reward r_D(y₁,y₂) = 1 − cos(E(y₁), E(y₂))**, E = 25M-param 12-layer ViT music embedder trained with semi-hard triplet loss. RL (REINFORCE) on this reward, β ∈ {0,5,10,15}, plus LERP model merging | ✅ CFG distillation alone drops diversity to ~0.37; β=15 model wins diversity comparisons at **0.73** and **0.79** vs CFG and β=0 baselines; merged λ=0.5 model wins diversity **57%** and quality **51%** vs CFG-augmented base. **Domain is music, not text.** Authors claim first RL algorithm to optimize diversity directly, and report their embedding reward correlates with human diversity judgment better than token-level entropy |
| **DivPO** (Diverse Preference Optimization) | 2501.18101 (Lanchantin et al., Meta/NYU/ETH) | Preference pairs chosen by diversity, not reward: **chosen = most diverse response above a quality threshold; rejected = least diverse below threshold.** Diversity criteria tried: model probability, word frequency, LLM-judged | ⚠️ Persona generation **+45.6%** attribute diversity; creative writing **+74.6%** story diversity at comparable win rate; instruction following **+46.2%** diversity with **+2.4%** winrate over DPO |
| **Diversity-Aware Policy Optimization for LLM Reasoning** | 2505.23433 | Token-level diversity objective on positive samples | ⚠️ **+3.5%** avg across 4 math benchmarks — diversity as a *capability* lever, not just an aesthetic one |
| **Verbalized Sampling** | 2510.01171 | Training-free: prompt the model to verbalize a distribution over k candidates with probabilities | See §1.3 ✅ |

---

## 5. Metric census, 2023–2026

**Rough ranking by frequency of appearance** (⚠️ this is my qualitative read across ~15 papers, not a systematic count — treat as a hypothesis, not a measurement):

1. **Distinct-n** — still the default. Original: Li et al. 2016. Its length-corrected variant **EAD** (Liu et al. 2022) is the version serious papers now use (Kirk 2024, D5P4 2026). Appears in Kirk, VS, D5P4, DivPO-adjacent work.
2. **Embedding self-cosine (1 − mean pairwise)** — now co-equal with distinct-n, arguably ahead of it in 2025–26 work. Encoders vary and are rarely justified: Sentence-BERT (Kirk), OpenAI `text-embedding-3-small` (VS), Jina v2 (D5P4), custom ViT (DeepMind), unstated (Doshi & Hauser, Anderson). **This inconsistency makes cross-paper numbers non-comparable and is the field's biggest methodological weakness.**
3. **Self-BLEU** (Zhu et al. 2018) — widely cited, increasingly as the thing being *replaced*; frequently reported as `1 − Self-BLEU` for sign consistency. Padmakumar's "model suggestion diversity" is a Self-BLEU variant, and they use ROUGE-L in its place.
4. **Human judgment** — present in every high-quality paper in this list (Kirk validates via Tevet & Berant; VS runs 30 annotators/task; Chakrabarty uses 10 professional writers; NoveltyBench uses 8 human authors as baseline; Doshi & Hauser and Anderson are human studies end to end). **Nobody credible ships a diversity claim on automatic metrics alone.**
5. **Trained semantic-equivalence classifiers** — NoveltyBench's deberta-v3-large partition model is the notable instance; the clear direction of travel, but n≈1.
6. **Vendi Score** — well known, methodologically the most principled (reference-free, entropy of kernel eigenvalues), but **not** a headline metric in any major LLM creative-writing benchmark I found. Its own follow-up (Conditional Vendi, 2411.02817) documents a disqualifying gap for prompted eval.
7. **NLI-based diversity** (Stasaski & Hearst 2021) — used by Kirk, where it **detected nothing**, which is probably why it hasn't spread.
8. **Distributional metrics** — KL vs a reference distribution, Coverage-N, Precision (VS); precision/recall decompositions (2402.10693). Emerging and, in my view, the most promising direction: they measure whether the output distribution matches a *target*, not merely whether outputs differ from each other.

**Explicit criticism of distinct-n / Self-BLEU** ⚠️ (assembled from secondary sources; I could not read the primary critiques in full):
- **Semantic blindness**: n-gram matching cannot capture meaning — a model emitting random words attains a perfect Self-BLEU diversity score. Distinct-n measures n-gram duplication, not diversity.
- **Length bias**: distinct-n is confounded by output length — the exact defect EAD was introduced to correct. Any paper reporting raw distinct-n on variable-length LLM outputs is reporting a length artifact partly disguised as diversity.
- **Quality-blindness**: high distinct-n is achievable by being incoherent. This is precisely why NoveltyBench's `utility_k` multiplies distinctness by a reward-model quality term — the correct structural response.
- **Named critiques to chase**: Tevet & Berant (2021) "Evaluating the Evaluation of Diversity in NLG"; Shaib et al. (2024) on standardizing diversity metrics; Shypula et al., **"Evaluating the Diversity and Quality of LLM Generated Content" (arXiv:2504.12522)** — which per its own appendix heading has a section on "Additional Neural Diversity Metrics" and states that no prior work had assessed whether **neural** diversity metrics actually capture semantic content diversity. That last one is likely your best single citation for the census question. ❌ I could not decode its PDF body (compressed streams) and the HTML fetch failed before I ran out of budget — **read arxiv.org/html/2504.12522 yourself; it is the highest-value unread item on this list.**

---

## Gaps recorded as UNANSWERED (do not let these become conclusions by absence)

1. **Kirk et al. exact diversity numbers** — do not exist in table form in the paper. Figures 5–6 only.
2. **Doshi & Hauser** — embedding model, N, regression table, five-idea-condition similarity delta. Paywalled; Dryad has the data.
3. **Anderson et al.** — embedding model, similarity means, p-values, effect sizes.
4. **Chakrabarty et al.** — per-model TTCW pass counts out of 14.
5. **Lu et al. CREATIVITY INDEX** — the 66.2% and 30.1% figures are abstract-level only; not read in Results.
6. **arXiv:2504.12522 (Shypula et al.)** body text — the likely best census + critique source, unread.
7. **Diverse beam search / determinantal beam search** — no LLM-scale numbers obtained; both are pre-LLM NMT/captioning-era results.
8. **No Vendi-headline LLM creative-writing benchmark found** — this is a genuine negative finding, but from a bounded search, so treat as "not found" rather than "does not exist."

**Budget note:** the session's 200-WebSearch cap was reached. Items 2, 3, and 6 are each one fetch away from resolution.
