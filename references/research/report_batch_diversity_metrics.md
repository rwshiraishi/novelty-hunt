All six areas are covered. Final report:

---

# Diversity Metrics for Batches of Generative Outputs

Legend: **[V]** = read in primary source this session. **[U]** = secondary/abstract only. **UNANSWERED** = could not confirm.

---

## 1. Distinct-n (Li et al. 2016) and the EAD fix

### Original definition [V]

Li et al. 2016 ("A Diversity-Promoting Objective Function for Neural Conversation Models", NAACL). As restated verbatim by the critique paper: **Distinct = N / C**, where **N** is the number of distinct n-grams and **C** is the total number of n-grams in the generated set. Typically reported as distinct-1 and distinct-2.

### The flaw: length sensitivity [V]

Liu, Sabour, Zheng, Ke, Zhu & Huang, ["Rethinking and Refining the Distinct Metric"](https://aclanthology.org/2022.acl-short.86/) (ACL 2022 short, pp. 762–770; [arXiv:2202.13587](https://arxiv.org/abs/2202.13587)).

Every additional token increments the denominator `C` unconditionally but increments the numerator `N` **only if that n-gram is novel**. So distinct-n declines monotonically with length even when the underlying word distribution is held fixed. The paper demonstrates this on (a) a designated sampler `P(X=k) = ∫₀^R v^k e^{-λ}/k! dλ` with v = 30522 (BERT vocab), and (b) OpenSubtitles, 2000 sentences sampled per length. Both decline sharply.

Critical operational consequence, stated by the authors: **length is trivially manipulable via decoding tricks** (length penalty coefficient), so distinct-n is gameable. They also note the flaw was known in psycholinguistics since Chotlos (1944) and Malvern et al. (2004), where the same ratio was used for child language diversity and showed the same adverse decline.

### The fix: Expectation-Adjusted Distinct (EAD) [V]

Replace the denominator with the **expected** number of distinct n-grams rather than the raw count:

```
EAD = N / E[N̂]                                              (Eq. 1)
```

Since `E[N̂]` requires `P(l_{t} ≠ u_j, ..., l_1 ≠ u_j)` for every vocabulary item — not efficiently computable, and distribution-dependent — they substitute the **upper bound** (every token equiprobable):

```
E[N̂_upper] = V · [1 − ((V−1)/V)^C]                          (Eq. 3)

EAD = N / ( V · [1 − ((V−1)/V)^C] )                          (Eq. 4)
```

where **V** = vocabulary size, **C** = total number of n-gram tokens. Your recollection of the formula was exactly right. The load-bearing assumption is explicit: *"the scaling effect of the upper bound is approximately proportional to that of other sets of generated responses."*

### Validation against human judgment [V]

10 dialogue generation methods, human raters (≥3 per set, 100 subsets of 15 responses per method, workers with Pearson < 0.65 against the average filtered out). Table 2:

| Dataset | Correlation | Distinct | **EAD** |
|---|---|---|---|
| DailyDialog | Pearson | 0.67‡ | **0.70‡** |
| DailyDialog | Spearman | 0.42† | **0.62†** |
| DailyDialog | Kendall τ | 0.27 (n.s.) | **0.47†** |
| OpenSubtitles | Pearson | 0.56† | **0.60†** |
| OpenSubtitles | Spearman | 0.62† | **0.65‡** |
| OpenSubtitles | Kendall τ | 0.51‡ | **0.56‡** |

† p<0.1, ‡ p<0.05. **Read this honestly: the gains are modest** (Pearson +0.03/+0.04), and the one dramatic improvement is Kendall τ on DailyDialog, where the original was non-significant. This is a bias correction, not a new metric class.

### EAD's own failure mode [V]

The authors flag it themselves: EAD assumes an idealized uniform distribution. On **Twitter** they observed EAD *still declining* on longer texts, because the 280-char limit induces users to compress information. Their prescription: **before using EAD, plot score vs. length on your training data and confirm flatness.** EAD is safe on large open-domain corpora (OpenSubtitles, Reddit); it is not universally length-invariant.

**Implementation:** reference code at [github.com/lsy641/Expectation-Adjusted-Distinct](https://github.com/lsy641/Expectation-Adjusted-Distinct). Trivial cost — O(total tokens), one hash set. `V` must be chosen (they use BERT's 30522).

---

## 2. Self-BLEU (Zhu et al. 2018, Texygen)

### Exact definition [V]

Zhu, Lu, Zheng, Guo, Zhang, Zhang, Wang & Yu, ["Texygen: A Benchmarking Platform for Text Generation Models"](https://arxiv.org/abs/1802.01886) (SIGIR 2018). Verbatim:

> *"Regarding one sentence as hypothesis and the others as reference, we can calculate BLEU score for every generated sentence, and define the average BLEU score to be the Self-BLEU of the document. A higher Self-BLEU score implies less diversity of the document, and more serious mode collapse of the GAN model."*

So for a set S of n generated texts: `Self-BLEU(S) = (1/n) Σᵢ BLEU(sᵢ | S \ {sᵢ})`. Report as `1 − Self-BLEU` if you want a diversity-signed quantity. Texygen reports it at BLEU-2 through BLEU-5 (Table 3: SeqGAN BLEU-2 0.950, MaliGAN 0.918, RankGAN 0.959).

### Failure modes

1. **O(n²) cost** [V]. Explicitly named as the disqualifying problem by Liu et al. 2022: *"Self-BLEU proposed by Zhu et al. (2018) is extremely time-consuming as its computation complexity is O(n²), where n denotes the size of the test set."* Each of n sentences is BLEU'd against n−1 references. At n = 10⁴ this is 10⁸ BLEU evaluations.
2. **Surface form only** [V, via Tevet & Berant]. It is an n-gram overlap metric, so by construction it measures form diversity. Tevet & Berant classify it in the n-gram family and demonstrate empirically (§3) that this family fails at content diversity.
3. **Reward-hacking by incoherence** [U]. A model emitting random tokens achieves near-perfect Self-BLEU diversity. Must be paired with a quality metric.
4. **Sample-size dependence** — Self-BLEU on n=100 and n=10,000 are not comparable; each sentence has more references in the larger set. UNANSWERED whether anyone has formally characterized this bias.

**Implementation:** `Texygen` repo ([geek-ai/Texygen](https://github.com/geek-ai/Texygen)), TensorFlow-era, largely unmaintained. In practice people reimplement with NLTK's `sentence_bleu`. Not recommended above n ≈ 10³.

---

## 3. Embedding-space dispersion, and the human-validation numbers

### Exact formulas [V]

From Cox, Wang, Abdul, von der Weth & Lim, ["Directed Diversity: Leveraging Language Embedding Distances for Collective Creativity in Crowd Ideation"](https://arxiv.org/abs/2101.06030) (CHI 2021), Table 3 — the cleanest single side-by-side of the collective-dispersion family, drawing from ecology, recommender systems, and TCS:

| Metric | Definition | Failure mode (their words) |
|---|---|---|
| **Remote-Clique** | `(1/N²) Σ_{i,j} d(xᵢ, xⱼ)` | *"insensitive to highly clustered points"* |
| **Chamfer Distance** (a.k.a. Remote-pseudoforest) | `(1/N) Σᵢ min_{j≠i} d(xᵢ, xⱼ)` | *"biased when points are clustered"* |
| **MST Dispersion** (Remote-tree) | `(1/\|E_MST\|) Σ_{(xᵢ,xⱼ)∈E_MST} d(xᵢ,xⱼ)` | ecology functional diversity |
| **Span** | `percentile_{P%} d(xᵢ, x̄_M)` | radius; they use 90th pct. to centroid |
| **Sparseness** | `(1/N) Σᵢ d(xᵢ, x̃_M)`, x̃_M = medoid | robust to outliers |

Note **Remote-Clique divides by N², not N(N−1)/2** — i.e. it includes the zero diagonal and double-counts pairs. Halve-and-correct if you want the strict mean over unordered pairs.

"Mean pairwise cosine distance" as used in the LLM literature = Remote-Clique with `d = 1 − cos`. The common LLM convention `diversity = 1 − mean pairwise cosine similarity` is identical.

### Tevet & Berant 2021 — the human-validation numbers [V]

Tevet & Berant, ["Evaluating the Evaluation of Diversity in Natural Language Generation"](https://aclanthology.org/2021.eacl-main.25/) (EACL 2021; [arXiv:2004.02990](https://arxiv.org/abs/2004.02990)). **This is the single most important paper in the whole area and its result is routinely misquoted.**

**Framework:** define a *diversity parameter* `d` that controls one aspect of diversity; score a metric by **Spearman's ρ between the metric's value and `d`**. Two instantiations:
- **decTest** — `d` = a decoding parameter (temperature, top-p, top-k) on GPT-2. Tests **form diversity**.
- **conTest** — `d` ∈ {0,1}, a binary content-diversity flag given to AMT workers who are instructed to keep *form* diversity high in both classes. Tests **content diversity**. 200 sets of 5 responses per task.

They also define a **diversity-to-similarity reduction**: from any symmetric sentence-similarity `m_sim`, define `m̃_div(S) = −(1/C(|S|,2)) Σ_{i>j} m_sim(sᵢ,sⱼ)` — i.e. negated mean pairwise similarity. This is how they turn BERT-STS, sent-BERT, and BERT-Score into diversity metrics, and it is Remote-Clique by another name.

**Table 2 — decTest (form diversity), Spearman ρ with temperature:**

| Metric | storyGen | respGen | promptGen |
|---|---|---|---|
| **distinct-n** | **0.76** (.03) | **0.89** (.01) | **0.91** (.01) |
| cos-sim (n-gram) | 0.71 (.04) | 0.89 (.01) | 0.87 (.02) |
| BERT-STS | 0.64 (.04) | 0.81 (.02) | 0.84 (.02) |
| sent-BERT | 0.65 (.03) | 0.80 (.02) | 0.74 (.03) |
| BERT-Score | 0.69 (.04) | 0.87 (.01) | 0.88 (.02) |
| **absHDS (human)** | **0.69** | **0.81** | **0.79** |

**Table 4 — conTest (content diversity), Spearman ρ and Optimal Classifier Accuracy:**

| Metric | storyGen ρ / OCA | respGen ρ / OCA | promptGen ρ / OCA |
|---|---|---|---|
| **distinct-n** | 0.57 / 0.77 | **0.34** / 0.67 | **0.33** / 0.68 |
| cos-sim (n-gram) | 0.56 / 0.77 | 0.33 / 0.66 | 0.36 / 0.67 |
| BERT-STS | 0.60 / 0.78 | 0.46 / 0.72 | 0.65 / 0.82 |
| **sent-BERT** | **0.77 / 0.90** | **0.59 / 0.79** | **0.68 / 0.81** |
| BERT-Score | 0.59 / 0.77 | 0.49 / 0.74 | 0.40 / 0.69 |
| **absHDS (human)** | **0.85 / 0.95** | **0.63 / 0.81** | **0.78 / 0.89** |
| aspHDS_form | 0.35 / 0.65 | 0.56 / 0.79 | 0.40 / 0.68 |
| aspHDS_content | 0.84 / 0.94 | 0.67 / 0.83 | 0.75 / 0.88 |

### What these numbers actually say

1. **The result is conditional, not a ranking.** On *form* diversity, distinct-n is the **best** metric tested (0.76–0.91) and **beats humans** (0.69–0.81). On *content* diversity, n-gram metrics **collapse** (0.33–0.57) and sent-BERT is the best automatic metric (0.59–0.77) but is **still clearly below humans** (0.63–0.85). Anyone citing this paper as "distinct-n is bad" is half-quoting it.
2. **Humans underperform automatic metrics on decTest.** The authors read this as evidence about the *decoding parameter*, not about humans: *"temperature mostly controls superficial changes to the generated text."* This is the origin of the widely-repeated claim that decoding-parameter sweeps change form but not meaning.
3. **n-gram metrics saturate.** Figure 4: on conTest, distinct-n pushes both the high- and low-content-diversity classes to near-maximal values — the histograms overlap almost completely.
4. **The McDiv-nuggets stress test** [V]: a 3K subset of their 6K McDiv benchmark, sampled so that **distinct-n's correlation is exactly zero by construction** (form diversity neutralized). All neural metrics degrade substantially on it. Humans get **ρ = 0.70** on respGen vs. best neural metric (sent-BERT) at **0.60**. This is the sharpest available statement of the automatic-metric gap.
5. `simHDS` (rate pairwise similarity, aggregate) scored **0.74** vs `absHDS` (rate the whole set) at **0.81** on respGen, **while costing 3× more**. If you elicit human diversity judgments, ask about the set, not about pairs.

**No metric in this paper is a Vendi-style spectral metric** — the paper predates it. Remote-Clique/Chamfer/MST as such are also not evaluated here; only mean-pairwise (which is Remote-Clique).

---

## 4. Vendi Score

Friedman & Dieng, ["The Vendi Score: A Diversity Evaluation Metric for Machine Learning"](https://arxiv.org/abs/2210.02410), TMLR 07/2023 ([OpenReview](https://openreview.net/forum?id=g97OHbQyk1)).

### Exact definition [V, verbatim]

**Definition 3.1.** Let x₁,…,xₙ ∈ 𝒳; let k : 𝒳×𝒳 → ℝ be positive semidefinite with k(x,x)=1 for all x; let K ∈ ℝ^{n×n} with K_{ij} = k(xᵢ,xⱼ). Let λ₁,…,λₙ be the eigenvalues of **K/n**. Then:

```
VS_k(x₁,…,xₙ) = exp( − Σᵢ₌₁ⁿ λᵢ log λᵢ )                    (1)
```

*"where we use the convention 0 log 0 = 0."* Equivalently (Lemma 3.1):

```
VS_k(x₁,…,xₙ) = exp( − tr( (K/n) log(K/n) ) )               (2)
```

Well-definedness: eigenvalues of K/n are nonnegative (k is PSD) and **sum to 1** (diagonal entries of K/n are all 1/n). So VS is the exponential of the **von Neumann entropy** of K/n treated as a density matrix — hence the name (*"Vendi"* = **v**on **N**eumann **di**versity).

**Range [V, Theorem 3.1.1]:** *"If k(xᵢ,xⱼ)=0 for all i≠j, then VS is maximized and equal to **n**. If k(xᵢ,xⱼ)=1 for all i,j, then VS is minimized and equal to **1**."* So VS ∈ [1, n]: all-identical → 1, mutually orthogonal → n. Interpretation: *"a sample with a Vendi Score of m can be understood to be as diverse as a sample consisting of m completely dissimilar elements."*

**Other axioms [V]:** identical-elements invariance; **partitioning** — for mutually-orthogonal groups, `VS(S₁,…,S_m) = exp(H(p₁,…,p_m)) · Πᵢ VS(Sᵢ)^{pᵢ}` (a geometric mean); permutation symmetry.

**Also:** VS is exactly the **effective rank** of K (Roy & Vetterli 2007) — an independent ancestor the paper acknowledges.

### Hill-number / ecology framing [V]

Verbatim: *"Our approach is based on work in ecology, where biological diversity has been defined as the exponential of the entropy of the distribution of species within a population (**Hill, 1973; Jost, 2006; Leinster, 2021**). This value can be interpreted as the effective number of species."* Also cites Patil & Taillie (1982) and Adelman (1969, economics).

**Important nuance on Leinster & Cobbold 2012** [V]: the paper cites it as *related work it distinguishes itself from*, **not** as the framework it adopts. L&C build entropy from a **similarity profile** (row-sums of K); VS builds it from the **spectrum** of K. Their stated argument: *"Like IntDiv, it does not account for correlations between features."* Row-sums are blind to feature correlation; eigenvalues are not. Figure 1c is the constructed counterexample. Do not describe VS as "Leinster-Cobbold applied to ML."

**Reference-free [V]:** requires only the sample and a PSD similarity function with unit diagonal. No reference dataset, no labels, no classifier.

### Computational cost [V]

- **Naive: O(n³)** (eigendecomposition of n×n).
- **Dual trick:** when embeddings φ(x) ∈ ℝ^d with d ≪ n are available and k is an inner product, `K = XXᵀ`; the eigenvalues of K/n equal those of the d×d covariance `XᵀX/n`, so VS is computed **exactly** in **O(d²n + d³) = O(d²n)**. The paper notes this is *"the same complexity as existing metrics such as FID."* This is `score_dual` in the package.
- **Nyström:** one sentence only — *"can be approximated using column sampling methods (Williams and Seeger, 2000)."* No complexity figure, no error analysis. **UNANSWERED: no standalone Nyström-Vendi paper located.**
- **Sample complexity:** Bach (2022) gives convergence rate ∝ **1/√n**.

**Linear-time variant [V]:** Ospanov, Zhang, Jalali, Cao, Bogdanov & Farnia, ["Towards a Scalable Reference-Free Evaluation of Generative Models"](https://arxiv.org/abs/2407.02961) (NeurIPS 2024), the **FKEA** method (Fourier-based Kernel Entropy Approximation). For shift-invariant kernels, draw r random Fourier features (Bochner), form a **2r × 2r** proxy covariance which shares the nonzero spectrum. **Cost O(n), linear in sample size**; required feature size **r = O(log n / ε²)** (Theorem 2). Also proves a **lower bound Ω(n^ω), ω ≈ 2.372** for exact VENDI_α at α ≠ 2 — order-2 (RKE) is the exception at Θ(n²). Empirically scales to n = 250k on ImageNet where exact VENDI hits memory overflow. Code: [github.com/aziksh-ospanov/FKEA](https://github.com/aziksh-ospanov/FKEA).

### The q-order generalization [V]

Pasarkar & Dieng, ["Cousins of the Vendi Score"](https://arxiv.org/abs/2310.12952), **AISTATS 2024** (PMLR v238).

```
VS_q(x, k) = exp( (1/(1−q)) · log Σ_{i ∈ supp(λ)} λᵢ^q )     (Eq. 6)
```

Sum runs over **nonzero eigenvalues only** — matters at q<1. This is `exp(Rényi entropy of order q)` = the similarity-sensitive Hill number of order q. Original VS = q=1.

**The stated defect of q=1** [V, verbatim abstract]: *"the Vendi Score treats each item with a level of sensitivity **proportional to the item's prevalence**. This is undesirable in settings where there is a significant imbalance in item prevalence."* Concretely: under class imbalance VS₁ **fails to detect the introduction of a rare class**, returning ≈3 despite 4 classes present. So the failure is **undercounting rare modes**, not overcounting.

- **q < 1** upweights rare items; **q > 1** upweights common ones; **q = ∞** depends only on the largest eigenvalue → maximally sensitive to duplication, blind to rare modes.
- Monotone: `VS_∞ ≤ … ≤ VS₁ ≤ VS₀` (Eq. 7). *(Eq. 8's VS₂/VS_∞ bound direction was ambiguous in text extraction — treat as UNVERIFIED.)*
- All VS_q satisfy the five Leinster-Cobbold axioms; *"Hill numbers have been shown to be the only family of diversity metrics that satisfy the axioms of diversity."*
- **Differentiability** is pitched as the main practical gain — VS can be *optimized*, not just measured, unlike FID/KID/recall/coverage.

**Their recommendation [V, verbatim]:** *"We recommend pairing sample quality metrics with a Vendi score of small order (**q ∈ [0.1, 0.5]**) for diversity and the Vendi score of **infinite order** for duplication and memorization."* VS_∞ correlates strongly with C_T-modified, a memorization metric. Their headline empirical finding: models with low FD/KD and high human error rate are **memorizing training samples and creating duplicate clusters around them**.

### How it differs from IS and FID [V, from the paper's §2]

- **FID:** *"measures the Wasserstein-2 distance between two Gaussian distributions, one fit to the embeddings of the reference sample and another to the sample being evaluated."* Needs a **reference dataset** + pretrained embedder. Defines diversity as **coverage of a reference**.
- **Inception Score:** *"evaluates diversity using the entropy of the marginal distribution of class labels predicted by an ImageNet classifier."* Needs a **classifier + predefined labels**. Conflates quality with diversity.
- **Number of Modes:** counts unique predicted labels. Also label-dependent.
- **VS:** *"a reference-free metric, measuring the **intrinsic diversity** of a set rather than the relationship to a reference distribution. This means that the Vendi Score should be used alongside a quality metric, but can be applied in settings where there is no reference distribution."*

**Its demonstrated failure-mode wins [V, with numbers]:**
- **StackedMNIST, 1000 modes (Table 1):** Number-of-Modes saturates at 1000 for Self-cond. GAN, PresGAN, and real data alike — useless. VS separates them: 746.7 / 866.6 / 943.7. They further compute "Mode Diversity" = exp H(p̂(y)) and show VS scores *lower still*, proving low VS *"cannot be entirely attributed to imbalances in the mode distribution"* — i.e. VS detects **sub-class** collapse.
- **MOSES molecules (Figure 3):** HMM VS=55.8 / IntDiv=0.853; AAE VS=142.1 / IntDiv=0.849; Real VS=127.7 / IntDiv=0.847. **IntDiv ranks HMM most diverse; VS ranks it least.** Cause: HMM produces low pairwise similarity but with **clusters of near-identical molecules**. Average-similarity metrics (Remote-Clique included) are blind to duplicate clusters; spectral ones are not. **This is the strongest argument for VS over mean-pairwise-cosine.**
- Caveat: the paper's constructed failure demos target **IntDiv and NoM**, not FID. UNANSWERED: no paper-internal synthetic experiment isolating an FID failure mode.

### Criticisms

**Self-acknowledged [V, Limitations section, verbatim]:**
1. *"it is possible to get a high diversity score by, for example, sampling **random noise**… Therefore VS should be used alongside a quality metric."*
2. *"VS is dependent on the choice of similarity function. **If the similarity function is too sensitive, all sets will appear very diverse, while if it is not sensitive enough, all sets will have low diversity.**"*
3. High cost when no low-dimensional embedding exists.

**External:**
- **Prevalence insensitivity at q=1** — the Cousins paper is itself the sharpest criticism of the original [V].
- **Finite-sample non-convergence** [U] — Ospanov & Farnia, "Do Vendi Scores Converge with Finite Samples? Truncated Vendi Score…" (PMLR v286, UAI 2025) + [arXiv:2410.21719](https://arxiv.org/abs/2410.21719). Reported claim: for infinite-dimensional feature maps, VS at **n below ~20,000** may not have converged to the population value. **If true, this means published Vendi numbers at typical n are partly sample-size artifacts and are non-comparable across papers using different n.** Verify directly before relying on it.
- **Prompt confounding** [U] — Conditional Vendi Score ([arXiv:2411.02817](https://arxiv.org/pdf/2411.02817), Jalali et al.): unconditional VS **cannot separate prompt-induced from model-induced variability**. For prompted LLM evaluation this is close to disqualifying — a diverse prompt set inflates VS without the model being diverse at all.
- **Embedding-induced bias** [U] — same paper, citing Stein et al. 2023.
- **Bandwidth sensitivity and cross-kernel non-comparability: UNANSWERED as published critiques.** Both follow logically from the definition, the original paper's limitation #2 concedes the mechanism, and RBF γ is a free hyperparameter tuned by grid search in the Cousins experiments — but I found **no paper stating either as a formal critique**. Real exposure, no citation.
- **Human validation: UNANSWERED — none found.** No study validates VS against human diversity judgments as its primary object. The closest is Cousins Figure 4 correlating VS_∞ with *human error rate*, which measures realism-confusability, not perceived diversity. **This is the single biggest gap in the Vendi literature and the sharpest contrast with distinct-n/sent-BERT, which have Tevet & Berant.**

### Implementation [V]

- **PyPI: `vendi-score`** (import `vendi_score`). `pip install vendi-score`. Extras: `[images]`, `[text]`, `[molecules]`, `[all]`.
- **Version 0.0.3, released Aug 30 2022. MIT license.** Python ≥3.7.
- **GitHub: [github.com/vertaix/Vendi-Score](https://github.com/vertaix/Vendi-Score)**, MIT, ~163 stars.
- **API:** `vendi.score(samples, k)`, `vendi.score(samples, k, q=1.)`, `vendi.score_K(K)`, `vendi.score_dual(X, normalize=True)`. Helpers: `text_utils.ngram_vendi_score`, `text_utils.embedding_vendi_score` (BERT/SimCSE), `image_utils`, `molecule_utils`.
- **`vendi.score_X` does not exist** — you were thinking of `score_dual`.
- **Version discrepancy flag:** PyPI shows only 0.0.3 (Aug 2022), but the README documents the `q` parameter from the 2024 follow-up. If q-order matters, install from a GitHub clone, not PyPI.
- Also: [Vendi-Score-R](https://github.com/vertaix/Vendi-Score-R), [Quality-Weighted-Vendi-Score](https://github.com/vertaix/Quality-Weighted-Vendi-Score), [FKEA](https://github.com/aziksh-ospanov/FKEA) (use above n ≈ 10⁴).

**Adoption:** Stein et al. 2023 (NeurIPS image-gen eval — used q=1, found it *"work more effectively as a measure of **per-class** diversity"*, a qualified endorsement); Berns et al. 2023 (mode balancing); Pasarkar et al. 2023 (Vendi Sampling, molecular simulation); Nguyen & Dieng, Quality-Weighted Vendi Scores (ICML 2024, [arXiv:2405.02449](https://arxiv.org/abs/2405.02449), reports 70–170% increase in effective discoveries); Vendi-RAG ([2502.11228](https://arxiv.org/html/2502.11228v2)); Vendi Information Gain ([2505.09007](https://arxiv.org/html/2505.09007v1)).

---

## 5. Quality-Diversity

### Novelty Search — Lehman & Stanley 2011 [V]

["Abandoning Objectives: Evolution Through the Search for Novelty Alone"](https://www.cs.swarthmore.edu/~meeden/DevelopmentalRobotics/lehman_ecj11.pdf), Evolutionary Computation 19(2):189–223.

**Equation (1), sparseness:**
```
ρ(x) = (1/k) Σ_{i=0}^{k} dist(x, µᵢ)
```
where µᵢ is *"the ith-nearest neighbor of x with respect to the distance metric dist."*

⚠️ **Notational correction:** the paper prints the lower index as **i=0** with normalizer k — literally k+1 terms over k. This is a typo in the original; every reimplementation uses i=1..k. Cite it as printed, implement it as i=1..k.

**Population AND archive [V]:** confirmed — the nearest-neighbor computation takes individuals from *both* the current population and the permanent archive. The paper cites Mouret (2009) showing this outperforms archive-only.

**k = 15 [V]** — Table 1 (Appendix A), and it applies to **both** the maze and the biped experiments, not just the maze.

**Archive threshold:** ρ_min exists and Table 1 gives *"Initial Archive Threshold"* = **6.0 (maze), 1.0 (biped)**. **UNANSWERED: the 2011 paper never states the adaptation rule.** I searched the full text for "threshold / raise / lower / adjust / dynamic" — ρ_min appears exactly twice, once in prose and once in Table 1. The commonly-cited dynamic rule (raise when many are added in a window, decay when none added over N evaluations) comes from the C++ implementation and later write-ups. **Do not attribute a numeric adaptation rule to Lehman & Stanley 2011.**

Cully & Demiris ([arXiv:1708.09251](https://arxiv.org/abs/1708.09251)) criticize the ρ_min rule directly: because ρ is an *average* over k neighbors, a near-duplicate can still clear the threshold if the rest of the collection is far away, giving uneven archive density. Their fix: threshold on distance to the **single nearest neighbor**, which directly bounds density.

**Behavior characterizations [V]:**
- **Maze:** the BC is the robot's **ending position only**; `dist` = **squared Euclidean distance between ending positions**. Method of traversal is ignored.
- **Biped:** center of gravity sampled at fixed time offsets; BC = concatenated `(x'₁,y'₁,…,x'_m,y'_m)`; `dist` = sum of squared distances. Temporal sampling is what distinguishes gaits ending at the same place by different means.

**Archive bounding [V]:** §6.2 tests a ring-buffer archive (overwrite earliest when full) across 40 hard-maze runs; size can be bounded without significant performance loss.

**Headline results [V]:** medium maze — novelty search 18,274 evaluations (sd 20,447) vs fitness-based NEAT 56,334 (sd 48,705), 40 runs, p<0.001.

### MAP-Elites — Mouret & Clune 2015 [V]

[arXiv:1504.04909](https://arxiv.org/abs/1504.04909). Algorithm (Fig. 2, verbatim structure):

```
(P ← ∅, X ← ∅)                       # empty N-dim map: performances P, solutions X
for iter = 1 → I:
    if iter < G:  x' ← random_solution()
    else:         x  ← random_selection(X); x' ← random_variation(x)
    b' ← feature_descriptor(x');  p' ← performance(x')
    if P(b') = ∅ or P(b') < p':
        P(b') ← p';  X(b') ← x'
return (P, X)
```

⚠️ **Mouret & Clune 2015 does NOT define "QD-score."** I grepped the full text — the term does not appear. Its four metrics (§9.4) are:
1. **Global Performance** — best solution found, normalized by best known.
2. **Global Reliability** — `G(m) = (1/n(M)) Σ_{x,y} m(x,y)/M(x,y)`, averaged over **all cells any run of any treatment ever filled**, empty cells counting 0.
3. **Precision (opt-in reliability)** — same ratio, averaged **only over cells this run filled**.
4. **Coverage** — filled cells / cells theoretically fillable. **The paper is candid that this denominator is unknown and is approximated by `n(F_M)`, the union of cells filled by any run of any treatment.** Modern "filled / total grid cells" is a simplification of the original, not the original.

### QD-score — Pugh, Soros & Stanley 2016 [V]

["Quality Diversity: A New Frontier for Evolutionary Computation"](https://www.frontiersin.org/journals/robotics-and-ai/articles/10.3389/frobt.2016.00040/full), Frontiers in Robotics and AI 3:40. Behavior space discretized into t bins {N₁,…,N_t} (t=1024 for their DirectionBC):

```
QD-score = Σᵢ Qᵢ,    Qᵢ = highest fitness found in bin Nᵢ,  Qᵢ = 0 if bin empty
```

The authors themselves note it is similar to Mouret & Clune's *global reliability*. Independently corroborated: QDAIF cites Pugh et al. 2016 for exactly this formula. **UNANSWERED whether the term first appeared in Pugh et al.'s earlier GECCO 2015 paper** — cite 2016 as the formalization, not as first use.

Subtle methodological point [V]: **search BC and evaluation BC are decoupled.** Diversity for scoring is always measured w.r.t. one chosen BC regardless of which BC drove search.

**Caveats:**
- **Non-negativity** [V via pyribs docs]: `GridArchive(..., qd_score_offset=0.0)`. The docs state that with negative objectives, QD-score would *penalize the algorithm for finding new cells*; you must pass an offset that is **subtracted** (objectives as low as −300 → pass −300).
- **Resolution non-comparability** [V]: Pugh's max achievable score is 1,024,000 = 1024 bins × perfect fitness. The ceiling scales with bin count, so QD-scores are meaningless across different archive resolutions.

### The central dependence: behavior descriptor choice determines everything [V]

This is stated forcefully, and the evidence is stronger than usually reported.

**Pugh, Soros & Stanley 2016** — the key source. They define a BC's **alignment** (degree to which finding novelty also raises fitness) and find: most published QD applications drive search with an **unaligned** BC; unaligned BCs **negatively impact** performance and on hard problems cause outright failure. In the symmetric gauntlet maze, **DirectionBC-driven treatments performed significantly worse than plain fitness-based search** — searching for unaligned diversity did worse than pure objective search. They reject naive BC concatenation because behavior space grows exponentially.

**Grillotti & Cully** [V]: hand-coded BDs (1) require prior task knowledge, since the right BD depends on morphology, task, environment, and damage conditions, and different BD choices give different QD performance; (2) **constrain the diversity the container can express** — if BD is end-position (x_T,y_T), diversity outside that projection is inexpressible.

**QDAIF 2023** [V, the modern restatement]: QDAIF *"still requires specified definitions of diversity axes."* Their example: with a horror↔romance axis, it will not autonomously explore first-person vs third-person perspective, and they observed such pathologies in the Stories domain.

### Learned / automatic behavior descriptors

**AURORA** — Grillotti & Cully, ["Unsupervised Behavior Discovery With Quality-Diversity Optimization"](https://arxiv.org/abs/2106.05648), IEEE TEVC [V]:
- QD phase returns fitness + **raw sensory data**; an encoder (PCA or conv autoencoder) projects it to a latent vector which **is** the BD.
- Encoder-update phase: trained on sensory data of all container members. **Because training changes the latent space, all BDs of all container members are re-computed with the updated encoder after every training.** That is the container refresh.
- Interval between updates **increases linearly**; the encoder is **never reset**, only fine-tuned.
- Unstructured archive with adaptive distance threshold `d_min` (avoids CVT reconstruction). Two adaptation methods: **VAT** (Volume Adaptive Threshold, from Cully 2019 — pick d_min from descriptor-space volume; on change, empty and re-add the container) and **CSC** (Container Size Control, their contribution), a proportional controller:
  ```
  d_min ← d_min · (1 + K_CSC · (|C| − N_C^target))
  ```
- **Cully 2019 (GECCO), "Autonomous skill discovery with QD and Unsupervised Descriptors": UNVERIFIED** — could not locate the paper directly; VAT is attributed to it by Grillotti & Cully, but I read the mechanism only in the 2021 paper.

**QDAIF** — Bradley et al./CarperAI, ["Quality-Diversity through AI Feedback"](https://arxiv.org/abs/2310.13032) [V]. **The key mechanical detail is under-cited:** both quality and diversity come from **LM logits, not embeddings**. The LM is prompted for a "yes"/"no" answer, and the score is the **log-ratio of the two response probabilities**. Consequences:
- They found qualitative behavior changes do **not** map uniformly onto logits (non-linear calibration of instruction-tuned models), so they **abandon MAP-Elites' equal-size bins for custom non-uniform bins**. An LLM-derived BD axis is not metrically uniform — this matters if you plan to build one.
- Mutation operator is **LMX** (language-model crossover), variants LMX-rewrite and LMX-guided.
- Domains: opinion writing (sentiment axis), short stories (genre romance↔horror; ending happy↔tragic), poetry (genre, tone).
- Metric: **QD score, citing Pugh et al. 2016.**
- **Human validation [V, with numbers]:** 28 experiments, 5 stories each, 6 annotators, each text reviewed by 2 people, **280 annotations**, Likert quality, Mann-Whitney U at p ≤ 0.05.
- They run a **semantic-embedding (Sentence-BERT) feedback baseline** as an ablation — logit-derived vs embedding-derived BDs is a comparison they actually made.

**TAXONS, ELM ([arXiv:2206.08896](https://arxiv.org/abs/2206.08896)), OMNI-EPIC: UNANSWERED** — no primary source read. **DeLeNoX** [V, as cited in AURORA]: alternates exploration with a "transformation phase" retraining an autoencoder; AURORA notes DeLeNoX **always retrains** where AURORA fine-tunes.

### Libraries

**pyribs** ([docs.pyribs.org](https://docs.pyribs.org/en/stable/), ICAROS Lab, USC) [V]:
- ⚠️ **The pip package is `ribs`, not `pyribs`.** `pip install ribs` / `pip install ribs[visualize]`. **MIT license.**
- Architecture: **archives + emitters + schedulers**, coordinated via an **`ask()` / `tell()`** interface.
- Archives: `GridArchive`, `CVTArchive`, `SlidingBoundariesArchive`, `CategoricalArchive`, `ProximityArchive` (novelty-based/unstructured), `DensityArchive`, `DNSArchive`, `DiscountArchive`. Plus `ArchiveStats` (`num_elites`, `coverage`, `qd_score`), `cqd_score()`, `CQDScoreResult`.
- Official implementation of **CMA-ME** and **CMA-MEGA**, plus **CMA-MAE** and Discount Model Search.
- ⚠️ **Vocabulary:** pyribs says **"measures" / "measure space"**, not "behavior descriptor" / "behavior space."

**QDax** ([github.com/adaptive-intelligent-robotics/QDax](https://github.com/adaptive-intelligent-robotics/QDax), [arXiv:2202.01258](https://arxiv.org/abs/2202.01258), JMLR 25(108), 2024) [V]:
- `pip install QDax`. Python ≥3.11 + JAX. **MIT.** AIRL Imperial College + InstaDeep.
- Implements MAP-Elites, CVT MAP-Elites, **AURORA**, Dominated Novelty Search, PGA-ME, DCRL-ME, QDPG, CMA-ME, OMG-MEGA, CMA-MEGA, MOME, MEES, ME-PBT, ME-LS.
- **Speedup [V, verbatim abstract]:** runtimes cut by *"two factors of magnitudes, turning days of computation into minutes"*; generation counts reducible by two orders of magnitude without hurting QD performance; thousands of simultaneous evaluations on **a single GPU/TPU** vs a few dozen on CPU physics simulators.

**Sferes2: UNANSWERED** — not fetched.

---

## 6. Recent LLM-diversity work (2023–2026)

### arXiv ID audit

All IDs you supplied were **correct** except: "diversity-rewarded RLHF" — the closest real artifact is [arXiv:2410.06084](https://arxiv.org/abs/2410.06084) "Diversity-Rewarded CFG Distillation" (DeepMind), and **it is text-to-music, not text.** Verbalized Sampling = **[arXiv:2510.01171](https://arxiv.org/abs/2510.01171)** (Zhang, Yu, Chong, Sicilia, Tomz, Manning, Shi). Chakrabarty "Art or Artifice?" = **arXiv:2309.14556**. DivPO = **arXiv:2501.18101**. Anderson et al. = **arXiv:2402.01536**.

### Kirk et al. 2024, "Understanding the Effects of RLHF on LLM Generalisation and Diversity" ([arXiv:2310.06452](https://arxiv.org/abs/2310.06452), ICLR 2024) [V metrics]

**Three metrics, all chosen because Tevet & Berant validated them against humans:**
1. **EAD** (syntactic) — distinct n-grams averaged over n=1…5, with the Liu et al. 2022 expectation adjustment.
2. **Sentence-BERT** (semantic) — 1 − mean pairwise cosine.
3. **NLI diversity** (logical) — contradictions vs entailments over sampled sentence pairs.

⚠️ Correction to your prompt: there is **no separate fourth "N-gram diversity" metric**. EAD *is* the n-gram metric. You missed **NLI diversity**.

**Per-input vs across-input** [V]: K=16 samples/input, N=500 inputs, temp 1. Per-input = diversity of π(y|x) averaged over inputs. Across-input = diversity of the union of *one* output per input, i.e. of the marginal π(y).

**Setup:** LLaMA 7B; SFT vs PPO (KL β=0.05) vs Best-of-N (N=16, temp 0.7); TL;DR summarisation + AlpacaFarm.

**Findings [V, qualitative]:** RLHF has *"much lower output diversity than SFT"* per-input on EAD and Sent-BERT; across-input the gap is *"much smaller"*; **BoN matches or exceeds SFT across-input diversity** — generalisation without the per-input diversity tax; NLI diversity shows **no meaningful difference** anywhere; **raising the KL penalty *decreased* per-input diversity** rather than recovering it.

❌ **UNANSWERED: the exact numbers.** Diversity appears only in **Figures 5 and 6** — there is **no diversity table in this paper**. There is no citable "RLHF reduces diversity by X%" figure. Code at [github.com/facebookresearch/rlfh-gen-div](https://github.com/facebookresearch/rlfh-gen-div) if you need to regenerate it. Do not let a secondary source persuade you the number exists.

### Padmakumar & He 2024 ([arXiv:2309.05196](https://arxiv.org/abs/2309.05196), ICLR 2024) [V, full]

38 Upwork writers, 10 topics, 300 essays across Solo / GPT-3 / InstructGPT. **No cosine similarity anywhere** — similarity is **Rouge-L** or **BertScore** (`deberta-xlarge-mnli`).

- Homogenization: `hom(d|t)` = mean pairwise sim to other essays on the same topic. Corpus-level, key-point Rouge-L: **Solo 0.1536 / GPT-3 0.1578 / InstructGPT 0.1660** (InstructGPT > both, p<0.05; GPT-3 vs Solo n.s.).
- Unique n-gram fraction, 4-gram: 0.973 / 0.967 / **0.953**; 5-gram: 0.991 / 0.988 / **0.977** (χ², p<0.05).
- Unique key-point fraction at clustering threshold 0.6: **0.941 / 0.927 / 0.877** (permutation test, 1000 perms, p<0.05).
- **Mechanism: model suggestion similarity (Rouge-L) InstructGPT 0.20 vs GPT-3 0.11** — the feedback-tuned model's own suggestions are ~2× less diverse.
- **Attribution result:** user-written 5-gram distributions and user key-point homogenization are **unchanged** across conditions. The homogenization is carried entirely by model-contributed text.
- No quality difference (all n.s.).

### Verbalized Sampling & typicality bias ([arXiv:2510.01171](https://arxiv.org/abs/2510.01171)) [V]

**The theory is the contribution — a data-level, not algorithmic, explanation of mode collapse.** Reward model `r(x,y) = r_true(x,y) + α·log π_ref(y|x) + ε(x)`, α>0 a typicality weight (grounded in mere-exposure effect, availability heuristic, processing fluency). Under KL-regularized RLHF the optimal policy becomes:

```
π*(y|x) ∝ π_ref(y|x)^γ · exp(r_true/β),    γ = 1 + α/β > 1
```

When true rewards are flat across a response set — exactly the creative-task case — this reduces to pure temperature sharpening. **Mode collapse falls out as a theorem, not a bug.**

**Empirical α [V]:** on **6,874 HelpSteer response pairs with identical correctness ratings**, fitted **α̂ = 0.57 ± 0.07** (Llama-3.1-405B-Base as π_ref) and **α̂ = 0.65 ± 0.07** (GLM-4.5-Base), both **p < 10⁻¹⁴**.

**Metrics:** semantic diversity = 1 − mean pairwise cosine (`text-embedding-3-small`), negatives clipped; ROUGE-L; human 4-point Likert; **Distinct-1/2/3**; KL vs a RedPajama reference distribution; Coverage-N; Precision.

**The cleanest mode-collapse measurement in the literature [V]** — Tulu-70B post-training ablation: base **45.4%** → post-SFT **20.8%** → post-DPO **10.8%**. Direct prompting retains only **23.8%** of base diversity. VS prompting holds ~30% across all stages, recovers **66.8%** of base diversity, and beats direct by **182.6%** post-DPO. Human ratings +25.7%. **Larger models lose more and gain more** — GPT-4.1/Gemini-2.5-Pro gain 1.5–2× more than their Mini/Flash siblings.

### West & Potts, "Base Models Beat Aligned Models at Randomness and Creativity" ([arXiv:2505.00047](https://arxiv.org/abs/2505.00047)) [V]

Metric is **Pearson χ² divergence from uniform** (not entropy, not KL). Random 10-length integer sequences, χ² (lower better): 8B **Base 13.9** vs Llama-Instruct 115.1, Tulu-DPO 100.8, Tulu-Full 129.1. Repetition mode: base = **3** (matching true randomness); **every aligned model's mode = 0**.

**The sharpest single result in the whole area:** on 720 pairwise human poetry comparisons, base models win originality 4/4 contests, and **originality vs. preference Spearman ρ = −0.08 while pleasantness vs. preference ρ = +0.34.** That pair is the empirical shadow of Verbalized Sampling's α — preference data rewards pleasantness, not originality.

### NoveltyBench ([arXiv:2504.05228](https://arxiv.org/html/2504.05228v2)) [V]

k=10 samples/prompt, temp 1.0. Two metrics:
- **distinct_k** = number of functional equivalence classes among k samples. Equivalence rule: two outputs differ iff *"a user who has seen one generation would likely benefit from seeing the other."* Partition model: fine-tuned **deberta-v3-large** on 1,000 author-annotated pairs — **79% accuracy, F1 = 0.811**, AUC 0.81.
- **utility_k** = `[(1−p)/(1−p^k)] · Σᵢ p^(i−1) · 𝟙[cᵢ ≠ c_j ∀ j<i] · uᵢ`, patience **p = 0.8**. Duplicates contribute **zero**. Quality uᵢ from Skywork-Reward-Gemma-2-27B. As p→0 it degenerates to ordinary single-generation eval — a clean generalization.

**Results (distinct / utility, out of 10):** gemma-2-2b-it **6.66**/4.63; Llama-3.2-1B **7.74**/2.81; Llama-3.1-405B 4.20/3.39; Llama-3.3-70B 3.49/2.87; gpt-4o 3.88/3.27; Claude-3 Opus 3.04/2.67; **Claude-3.5 Sonnet 2.76**/2.36 (least diverse tested); gemini-1.5-pro 2.85/2.73.

**Headline:** frontier closed models produce **fewer than 4 distinct responses per 10 samples**, with **inverse scaling within families** (Llama-3.2-1B 7.74 vs Llama-3.3-70B 3.49; gemma-2-2b-it 6.66 vs 27b-it 4.03).

**Field census statistic [V]:** of **67 benchmark papers at COLM 2024 / ICLR 2025, over 90% evaluate on a single or best generation.** That is the answer to why nobody measures this.

### Homogenization studies

**Doshi & Hauser**, *Science Advances* 10(28) eadn5290 (2024) [U, paywalled]. Measure is **cosine similarity of the focal story's embedding to the *average* embedding of all other stories** — note this is a **centroid** similarity, not mean pairwise; it is a different statistic from Remote-Clique. **+10.7% similarity** in the one-GenAI-idea condition vs no-AI control. Individual gains for low-DAT writers in the five-idea condition: quality +26.6%, enjoyment +22.6%. UNANSWERED: embedding model, N, regression coefficients. Data on Dryad (doi:10.5061/dryad.qfttdz0pm).

**Anderson, Shah & Kreminski** ([arXiv:2402.01536](https://arxiv.org/abs/2402.01536), C&C 2024) [U]. 36 participants, 1,271 ideas, ChatGPT vs Oblique Strategies. **Group-level homogenization confirmed; individual-level homogenization NOT found** — each individual's own idea set was similarly diverse in both conditions. UNANSWERED: embedding model, means, p-values.

**Worth noting — the apparent contradiction resolves.** Anderson (individual OK / collective harmed) and Kirk (per-input harmed / across-input barely harmed) point in opposite directions on which level collapses. They are consistent once you see that Kirk measures one model sampled repeatedly while Anderson measures many humans sharing one model: the human user supplies the between-user variation the model lacks, so the model's per-input collapse becomes the population's across-user collapse only when the model's contribution dominates. **Padmakumar's attribution result — user text unchanged, model text carries the whole effect — is the bridge.**

### Diversity-aware decoding and training

| Method | ID | Criterion | Numbers |
|---|---|---|---|
| **DivPO** | [2501.18101](https://arxiv.org/abs/2501.18101) | Preference pairs chosen by **diversity**: chosen = most diverse above a quality threshold; rejected = least diverse below | [U] Persona **+45.6%** attribute diversity; creative writing **+74.6%** story diversity at comparable win rate; instruction following **+46.2%** diversity, **+2.4%** winrate over DPO |
| **Diversity-Rewarded CFG Distillation** | [2410.06084](https://arxiv.org/abs/2410.06084) | `r_D(y₁,y₂) = 1 − cos(E(y₁),E(y₂))`, E = 25M ViT music embedder, REINFORCE | [V] CFG distillation alone drops diversity to ~0.37; β=15 wins at 0.73/0.79; merged λ=0.5 wins diversity 57%, quality 51%. **Music, not text.** Claims their embedding reward correlates with human diversity judgment better than token-level entropy |
| **Determinantal Beam Search** | [2106.07400](https://arxiv.org/abs/2106.07400) | Each beam step = subdeterminant maximization over a PSD kernel; standard beam search = the diagonal-kernel case | [U] More diverse than standard beam search with minimal BLEU trade-off. Note the direct link to DPPs and hence to Vendi's spectral family |
| **Diverse Beam Search** | [1610.02424](https://arxiv.org/abs/1610.02424) | Beam budget split into G groups + inter-group dissimilarity penalty | [U] Pre-LLM; captioning/VQA only |
| **Diversity-Aware Policy Optimization** | [2505.23433](https://arxiv.org/abs/2505.23433) | Token-level diversity objective on positive samples | [U] **+3.5%** avg across 4 math benchmarks — diversity as a *capability* lever |

### Metric census 2023–2026 [U, qualitative read across ~15 papers — a hypothesis, not a measurement]

1. **Distinct-n** — still the default; **EAD** is the version serious papers use (Kirk 2024).
2. **Embedding self-cosine (1 − mean pairwise)** — now co-equal, arguably ahead in 2025–26. **Encoders vary and are almost never justified:** Sentence-BERT (Kirk), `text-embedding-3-small` (VS), Jina v2, custom ViT (DeepMind), unstated (Doshi & Hauser, Anderson). **This is the field's biggest methodological weakness — cross-paper numbers are not comparable.**
3. **Self-BLEU** — widely cited, increasingly as the thing being replaced.
4. **Human judgment** — present in **every** high-quality paper here. Nobody credible ships a diversity claim on automatic metrics alone.
5. **Trained semantic-equivalence classifiers** — NoveltyBench's deberta partition model; clear direction of travel, but n≈1.
6. **Vendi Score** — methodologically the most principled, but **not a headline metric in any major LLM creative-writing benchmark found.** Its own follow-up (Conditional Vendi) documents a disqualifying gap for prompted eval.
7. **NLI diversity** — used by Kirk, where it **detected nothing**; probably why it hasn't spread.
8. **Distributional metrics** (KL vs reference, Coverage-N, Precision) — emerging, and arguably the most promising: they measure whether the output *distribution* matches a target, not merely whether outputs differ from each other.

**Best single unread lead:** Shypula et al., **"Evaluating the Diversity and Quality of LLM Generated Content" ([arXiv:2504.12522](https://arxiv.org/abs/2504.12522))** — per its own appendix headings it states no prior work had assessed whether *neural* diversity metrics capture semantic content diversity. Likely the strongest census + critique citation. **Could not decode the PDF; unread. UNANSWERED.**

---

## Cross-cutting summary

| Metric | Cost | Human-validated? | Primary failure mode |
|---|---|---|---|
| distinct-n | O(tokens) | **Yes** — ρ 0.76–0.91 form, **0.33–0.57 content** (T&B 2021) | Length bias; semantically blind; saturates |
| EAD | O(tokens) | **Yes** — modest gains over distinct-n (Pearson +0.03/+0.04, Kendall +0.20 on DailyDialog) | Assumes uniform token distribution; still biased on length-capped corpora (Twitter) |
| Self-BLEU | **O(n²)** | Indirectly (n-gram family, T&B) | Intractable at LLM scale; form-only; reward-hackable by incoherence |
| Mean pairwise cosine / Remote-Clique | O(n²d) | **Yes** — sent-BERT ρ 0.59–0.77 content, best automatic but below humans | *"Insensitive to highly clustered points"*; encoder choice unjustified across papers |
| Chamfer | O(n²d) | No direct validation found | *"Biased when points are clustered"* |
| **Vendi Score** | O(n³), or **O(d²n)** with embeddings, or O(n) via FKEA | **NO — UNANSWERED, none found** | Kernel/bandwidth sensitivity (self-acknowledged); undercounts rare modes at q=1; possible non-convergence below n≈20k; conflates prompt- and model-induced variability |
| Novelty sparsity ρ(x) | O(n log n) w/ k-d tree | N/A (search operator) | BD choice dominates; average-over-k lets near-duplicates into the archive |
| QD-score / coverage | O(1) per eval | N/A | Needs non-negative fitness offset; **not comparable across archive resolutions**; BD choice dominates |

**Three findings worth carrying forward:**

1. **The Tevet & Berant result is conditional and routinely misquoted.** distinct-n is the *best* metric for form diversity and *beats humans* there; it fails only on content diversity. Pick your metric by which aspect you mean.
2. **Vendi's real advantage over mean-pairwise-cosine is cluster detection, and it is demonstrated** (MOSES: IntDiv ranks the duplicate-cluster-producing HMM *most* diverse, VS ranks it *least*). But **Vendi has no human validation as a diversity metric** — a genuine gap, and the sharpest contrast with the n-gram and embedding families.
3. **The most principled recent instrument is NoveltyBench's `utility_k`**: it multiplies distinctness by quality with a patience discount, so incoherence cannot buy diversity — structurally solving the "pair it with a quality metric" caveat that every reference-free metric (Vendi included) carries as an unresolved footnote.
