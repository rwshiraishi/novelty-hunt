Research complete. Report follows.

---

# The Vendi Score: Deep Research Report

Primary sources read directly: the TMLR camera-ready PDF (arXiv:2210.02410v2, text-extracted via `pdftotext`), the AISTATS PDF (arXiv:2310.12952v3), the NeurIPS 2024 FKEA PDF (arXiv:2407.02961), the PyPI page, and the GitHub README. Claims from those are **VERIFIED**. Claims resting only on search snippets are **UNVERIFIED**.

## 0. Publication facts (VERIFIED)

The paper's own title block reads: "Published in Transactions on Machine Learning Research (07/2023), Reviewed on OpenReview, https://openreview.net/forum?id=g97OHbQyk1". Authors: Dan Friedman (Princeton CS) and Adji Bousso Dieng (Princeton CS + Vertaix). arXiv v1 Oct 5 2022, v2 Jul 2 2023. Keywords listed: "diversity, evaluation, entropy, ecology, quantum statistical mechanics, machine learning."

The name is an acronym-ish coinage, stated explicitly: "This entropy can be seen as the von Neumann entropy associated with K (Bengtsson and Życzkowski, 2017), so we call our metric the Vendi Score, for the **von Neumann diversity**." (VERIFIED)

## 1. Exact mathematical definition (VERIFIED, verbatim)

**Definition 3.1 (Vendi Score).** Let x₁,…,xₙ ∈ 𝒳 denote a collection of samples, let k : 𝒳 × 𝒳 → ℝ be a positive semidefinite similarity function, with k(x,x) = 1 for all x, and let K ∈ ℝ^{n×n} denote the kernel matrix with entry K_{i,j} = k(xᵢ,xⱼ). Denote by λ₁,…,λₙ the eigenvalues of **K/n**. The Vendi Score (VS) is defined as the exponential of the Shannon entropy of the eigenvalues of K/n:

```
VS_k(x_1, ..., x_n) = exp( - Σ_{i=1}^{n} λ_i log λ_i )          (1)
```

"where we use the convention **0 log 0 = 0**."

Every element the caller asked to confirm is confirmed: normalization is exactly **K/n**, eigenvalues are of K/n (not K), the functional form is exp of Shannon entropy, and the 0log0 convention is stated explicitly in the definition itself.

**Well-definedness argument (verbatim):** "the eigenvalues of K/n are nonnegative (because k is positive semidefinite) and sum to one (because the diagonal entries of K/n are equal to 1/n)." The k(x,x)=1 requirement is therefore load-bearing: it forces tr(K/n) = 1, making the spectrum a probability vector.

**Lemma 3.1 (trace form):**
```
VS_k(x_1, ..., x_n) = exp( - tr( (K/n) log(K/n) ) )              (2)
```
This is the exponential of the von Neumann entropy of K/n, with K/n "playing the role of the density matrix" (citing Bengtsson & Życzkowski 2017; Bach 2022).

**Range (VERIFIED, Theorem 3.1, property 1, verbatim):** "If k(xᵢ,xⱼ) = 0 for all i ≠ j, then VS_k(x₁,…,xₙ) is maximized and equal to **n**. If k(xᵢ,xⱼ) = 1 for all i, j, then VS_k(x₁,…,xₙ) is minimized and equal to **1**."

So the range is [1, n]: all-identical → 1, all-mutually-orthogonal (zero pairwise similarity) → n. The interpretation the paper pushes: "a sample with a Vendi Score of m can be understood to be as diverse as a sample consisting of m completely dissimilar elements."

**Other properties of Theorem 3.1 (VERIFIED):** (2) *Identical elements* — merging two elements with k=1 leaves VS unchanged; (3) *Partitioning* — for mutually-orthogonal groups S₁..S_m, VS_k(S₁,…,S_m) = exp(H(p₁,…,p_m)) · Π_i VS_k(Sᵢ)^{pᵢ}, a geometric mean; (4) *Symmetry* under permutation.

**Generalization (VERIFIED, Definition 7.1, appendix):** the *probability-weighted* Vendi Score replaces K/n with K̃_p = diag(√p) K diag(√p) for an explicit distribution p ∈ Δₙ, and takes exp of the Shannon entropy of its eigenvalues. Lemma 7.1: when all elements are mutually dissimilar, this reduces to exp H(p). The unweighted VS is the p = uniform case, justified because "x₁,…,xₙ were sampled independently, and so p(xᵢ) ≈ 1/n."

**Also flagged in the paper (VERIFIED):** VS is exactly the **effective rank** of K, "introduced by Roy and Vetterli (2007) in the context of signal processing." This is a second, independent ancestor the paper acknowledges alongside ecology and quantum statistical mechanics.

## 2. Conceptual framing and the ecology lineage (VERIFIED)

The framing is stated directly: "Our approach is based on work in ecology, where biological diversity has been defined as the exponential of the entropy of the distribution of species within a population (**Hill, 1973; Jost, 2006; Leinster, 2021**). This value can be interpreted as the effective number of species in the population."

Cited ecology/effective-number literature, exactly as it appears:
- **Hill (1973)** — Hill numbers. Cited in the intro and again for effective numbers.
- **Jost (2006)** — cited twice, including as the anchor for "diversity is the exponential of the entropy of the distribution of the species under study."
- **Leinster (2021)** — the book, recommended for "a more extensive mathematical discussion of entropy and diversity in the context of biodiversity."
- **Patil and Taillie (1982)** and **Adelman (1969)** (economics) — cited for the value of effective numbers.

**On Leinster & Cobbold 2012 — important nuance (VERIFIED).** The paper does cite it, but as *related prior work it distinguishes itself from*, not as the framework it adopts. Verbatim: "Related to the metric we propose here is a similarity-sensitive diversity metric proposed in ecology by **Leinster and Cobbold (2012)**, and which was introduced in the context of ml by **Posada et al. (2020)**. This metric is based on a notion of entropy defined in terms of a **similarity profile**, a vector whose entries are equal to the expected similarity scores of each element. **Like IntDiv, it does not account for correlations between features.**"

That is the paper's whole argument for why VS is not Leinster-Cobbold: L&C entropy is built on row-sums of the similarity matrix (a similarity profile), whereas VS is built on the *spectrum* of the matrix. Row-sums are blind to correlation structure among features; eigenvalues are not. Figure 1c is the constructed counterexample.

**Rényi connection in the original paper: UNANSWERED as such.** The 2022/2023 paper works only at q=1 (Shannon). The explicit Hill-number/Rényi correspondence is made retroactively by the follow-up (Section 4 below), which characterizes the original VS as "the Hill number of order q = 1... extended using similarity."

**Reference-free: VERIFIED.** From the abstract: "unlike many existing metrics in ml, the Vendi Score does not require a reference dataset or distribution over samples or labels, it is therefore general and applicable to any generative model, decoding algorithm, and dataset from any domain where similarity can be defined." Inputs required are exactly: the sample, and a psd similarity function with unit diagonal.

**Also connected (VERIFIED):** the paper relates VS to **Determinantal Point Processes** (VS uses Σλᵢlogλᵢ where DPP likelihood uses Πλᵢ; DPP likelihood "is always equal to 0 if the sample contains any duplicates") and to **spectral clustering** ("the Vendi Score is somewhat more general than the number of connected components: it provides a meaningful measure even for fully connected graphs, and captures within-component diversity").

## 3. Computational cost (VERIFIED)

**Naive:** "Calculating the Vendi Score for a sample of n elements requires finding the eigenvalues of an n × n matrix, which has a time complexity of **O(n³)**."

**The dual trick, verbatim:** "when embeddings of the observations (or feature vectors) are available... one can use similarity functions defined as inner products between the embeddings φ(x) ∈ ℝ^d, with d ≪ n. That is, we can use the similarity matrix K = XᵀX, where X ∈ ℝ^{n×d} is the embedding/feature matrix with row X_{i,:} = φ(xᵢ). The eigenvalues of K/n are the same as the eigenvalues of the covariance matrix XXᵀ/n, therefore we can calculate the Vendi Score **exactly** in a time of **O(d²n + d³) = O(d²n)**."

Two caveats worth flagging for the caller. First, the cost is stated as **O(d²n + d³) = O(d²n)**, which matches the caller's guess of O(nd²). Second, the paper's transpose notation is inconsistent in the extracted text (it writes K = XᵀX with X ∈ ℝ^{n×d}, then calls XXᵀ/n the covariance matrix); the mathematical content is unambiguous — you eigendecompose the d×d Gram/covariance matrix of features rather than the n×n matrix of samples, and the nonzero spectrum is identical. This is the `score_dual` path in the package.

The paper explicitly notes parity with FID here: "This is the same complexity as existing metrics such as fid (Heusel et al., 2017), which require calculating the covariance matrix of Inception embeddings."

**Nyström (VERIFIED but thin):** "When embeddings aren't available, the Vendi Score can be approximated using column sampling methods (i.e. the Nyström method; Williams and Seeger, 2000)." That is the entire treatment — one sentence, no complexity figure, no error analysis. A dedicated "Nyström Vendi Score" paper does not appear to exist as such; the Nyström analysis lands in the Ospanov/Farnia line below. (UNANSWERED: any standalone Nyström-Vendi paper.)

**Sample complexity (VERIFIED):** "Bach (2022) proves that empirical estimator of the kernel entropy has a convergence rate proportional to **1/√n**." (Appendix 7.5.)

### FKEA and the complexity lower bounds (VERIFIED from arXiv:2407.02961)

Ospanov, Zhang, Jalali, Cao, Bogdanov, Farnia, "Towards a Scalable Reference-Free Evaluation of Generative Models," NeurIPS 2024. This paper is sharper about cost than the original, and partly contradicts the optimistic reading of the O(d²n) trick.

- **Lower bound (Theorem 1 + Remark 1):** computing VENDI_α for **α ≠ 2** is at least as hard as multiplying two n×n matrices, hence lower-bounded by **Ω(n^ω), ω ≈ 2.372**. The order-2 case (RKE, Rényi-2) is the exception, computable in **Θ(n²)** over basis {×, +, log} via a Frobenius norm.
- **Practical exact cost:** "the computation of VENDI_α scores is performed by the eigendecomposition of the n × n kernel matrix that requires **O(n³)** computations for precise computation and **O(n²M)** computations using a randomized projection onto an M-dimensional space."
- **FKEA method:** for a shift-invariant kernel k(x,x') = κ(x−x') with κ(0)=1, draw ω₁..ω_r iid from κ̂ (Bochner), build RFF map φ̃_r : ℝ^d → ℝ^{2r}, and form the **2r × 2r** proxy covariance C̃_{X,r} = (1/n) Σᵢ φ̃_r(xᵢ)φ̃_r(xᵢ)ᵀ. "the 2r × 2r matrix has the same non-zero eigenvalues as the n × n RFF proxy kernel matrix (1/n)K̃_r." Then FKEA-VENDI_α = exp(H_α(C̃_{X,r})) over the 2r eigenvalues; FKEA-RKE = ‖C̃_{X,r}‖_F^{-2}.
- **Complexity:** "the computation needed for the proxy kernel covariance matrix is **O(n)** and grows only linearly with sample size n." The required feature size is **2r with r = O(log n / ε²)** for ε-accurate eigenspace approximation (Theorem 2), i.e. "a complexity logarithmically growing with the dataset size."
- **Empirical:** Table 1 on ImageNet, n from 10k to 250k. "VENDI and RKE become computationally intractable due to memory overflow. In contrast, the FKEA method efficiently scales up to n = 250k samples." (I did not extract the wall-clock numbers from Table 1 — **UNANSWERED** on specific seconds.)
- Code: `github.com/aziksh-ospanov/FKEA`. Note the repo casing in search results is inconsistent (`FKEA` vs `fkea`); GitHub is case-insensitive on redirect.
- FKEA uses **DINOv2** embeddings for most image experiments, citing evidence that DINOv2 "can yield scores more aligned with the human notion of diversity" — relevant to the embedding-sensitivity critique in Section 5.

**Related, unread (UNVERIFIED, search-level only):** Ospanov & Farnia, "Do Vendi Scores Converge with Finite Samples? Truncated Vendi Score for Finite-Sample Convergence Guarantees" (PMLR v286, UAI 2025) introduces a **t-truncated Vendi score** provably converging to its population limit with n = O(t) samples, and shows existing Nyström and FKEA approximations converge to the truncated score's asymptotic limit. Also "On the Statistical Complexity of Estimating VENDI Scores from Empirical Data" (arXiv:2410.21719). I did not read either full text.

## 4. The q-order generalization (VERIFIED from arXiv:2310.12952v3)

Amey P. Pasarkar and Adji Bousso Dieng, "Cousins Of The Vendi Score: A Family Of Similarity-Based Diversity Metrics For Science And Machine Learning," **AISTATS 2024** (PMLR v238).

**Setup (verbatim):** Hill number D_q is the exponential of the Rényi entropy H_q of order q, H_q(p) = (1/(1−q)) log Σᵢ pᵢ^q, D_q(p) = exp(H_q(p)). q=0 gives support size ("an uninformative measure of diversity"); q=1 gives exp of Shannon entropy; q=∞ gives 1/maxᵢ pᵢ.

**Definition (verbatim, Eq. 6):**
```
VS_q(x, k) = exp( (1/(1-q)) log Σ_{i ∈ supp(λ(x,k))} λ_i(x,k)^q )
```
"Here λ(x,k) denotes the set of eigenvalues of the normalized similarity matrix induced by the input similarity function k(·,·), and supp(λ(x,k)) denotes the indices for the nonzero eigenvalues." Note the support restriction: the sum runs over **nonzero** eigenvalues only, which matters for q<1 where zero eigenvalues would otherwise be counted.

**The stated defect of q=1 (verbatim, abstract):** "the Vendi Score treats each item in a given collection with a level of sensitivity **proportional to the item's prevalence**. This is undesirable in settings where there is a significant imbalance in item prevalence."

Concretely, from the body: "the Vendi Score (q = 1), under class imbalance, **fails to detect the introduction of a rare class** (the black square), leading to a score of ≈ 3 despite the presence of 4 classes. The Vendi scores with orders smaller than 1 are more sensitive to those rare classes and accurately measure diversity under class imbalance." So the claim is **undercounting of rare modes**, not overcounting.

**q < 1 vs q > 1:** q controls "the relative weight assigned to rare or common items." Small q upweights rare items; large q upweights common ones. q=∞ depends only on the largest eigenvalue, making it maximally sensitive to duplication and insensitive to rare modes.

**Ordering properties (VERIFIED):**
- Monotone decreasing in q: VS_∞ ≤ … ≤ VS₁ ≤ VS₀ (Eq. 7).
- VS₂ ≤ VS_∞ ≤ √(VS₂) (Eq. 8). *(Note: as extracted this reads oddly given monotonicity — the radical placement in the PDF text is ambiguous. Treat the exact bound direction as **UNVERIFIED**; the monotonicity result is unambiguous.)*
- All VS_q satisfy the five Leinster-Cobbold axioms: effective number, partitioning, identical species, monotonicity, permutation symmetry. The paper credits Leinster & Cobbold (2012) for the axiom list and states "Hill numbers have been shown to be the only family of diversity metrics that satisfy the axioms of diversity."
- **Differentiability** is pitched as the main practical gain: "the Vendi scores are differentiable, which makes them amenable to gradient-based methods," enabling *enforcing* diversity, not just measuring it. Contrast drawn explicitly: "Enforcing diversity with other existing diversity metrics such as FID, KID, recall, and coverage is currently computationally impossible. Indeed, these metrics are either non-differentiable or may be challenging to optimize as they require querying large pre-trained networks at each optimization iteration."

**Empirical findings (VERIFIED, verbatim on the recommendation):** "Our results reveal that generative models with a high human error rate or low Fréchet Distance (fd) and Kernel Distance (kd) — i.e. those generative models that tend to produce samples that human evaluators cannot distinguish from real data — are those that **memorize training samples and create duplicates around the memorized training samples**. This finding calls for the need to pair sample quality metrics with a metric that reliably measures duplication or memorization and a metric that measures diversity effectively. **We recommend pairing sample quality metrics with a Vendi score of small order (q ∈ [0.1, 0.5]) for diversity and the Vendi score of infinite order for duplication and memorization.**"

VS_∞ "is strongly correlated with C_T-modified, a metric used to measure memorization." They also claim VS scores let you "indirectly evaluate memorization and coverage **without relying on training data**," which they flag as valuable "in privacy settings and as training datasets become more and more closed-source."

**Molecular results (VERIFIED):** on Alanine Dipeptide with an RBF kernel on rigid-body-invariant coordinates (method of Jaini et al. 2021), the left-handed state is ≈1% of samples. "for extreme values of q, the score is relatively unaffected, whereas for q = 0.5 and q = 1, there is a significant change." Their synthesis: "small values of q can only detect rare classes when there is **not** a large amount of intra-class diversity." In Vendi Sampling, most q converge within 0.4 k_BT within 5ns, "while the Vendi score with q = 0.1 is slower to converge"; q=∞ unexpectedly "increase[s] mixing rapidly in the initial stages," which they read as evidence that "the associated eigenvector is aligned with a useful biasing potential."

There is no single recommended q. The stated tradeoff is that q cannot be too low (loses sensitivity to intra-class variance) or too high (loses sensitivity to rare items), and the right choice is phenomenon-dependent.

## 5. Adoption and criticisms

### Adoption

**Vertaix's own line (titles/IDs VERIFIED via listings; full texts UNVERIFIED except where noted):**
- **Vendi Sampling** — Pasarkar, Bianchi, Chattopadhyay, Dieng, accelerating molecular simulations by diversifying replicas. Cited as Pasarkar et al. (2023) throughout the Cousins paper; the Cousins paper's own molecular experiments follow its setup (VERIFIED as a citation).
- **Quality-Weighted Vendi Scores and Their Application to Diverse Experimental Design** — Nguyen & Dieng, **ICML 2024**, arXiv:2405.02449, PMLR v235 pp. 37667–37682. Takes samples + similarity function + a **score function**, output interpretable as "the effective quality sum of the samples." Applied to drug discovery, materials discovery, RL. Reported **70%–170% increase in effective discoveries** over baselines. Code: `github.com/vertaix/Quality-Weighted-Vendi-Score`. (UNVERIFIED — search snippet only; I did not read the paper.)
- **Vendi-RAG** — Rezaei & Dieng, arXiv:2502.11228, uses VS to promote semantic diversity in retrieval, positioned against plain similarity search (redundant docs) and MMR (fails to capture global semantic diversity). (UNVERIFIED)
- **Vendi Information Gain** — arXiv:2505.09007, pitched as an alternative to mutual information; ecology/active-learning application arXiv:2509.10390 claims near-full-supervision accuracy with <10% of labels on Snapshot Serengeti. (UNVERIFIED)
- **Applications of the Vendi score in genomic epidemiology** — arXiv:2509.22520. (UNVERIFIED)
- **Vendi-Score-R** — an R implementation at `github.com/vertaix/Vendi-Score-R`. (UNVERIFIED)

**Third-party adoption (VERIFIED as citations inside the Cousins paper):** "It's been shown useful for measuring the diversity of datasets and generative models (Friedman and Dieng, 2022; **Stein et al., 2023**; **Diamantis et al., 2023**), balancing the modes of image generative models (**Berns et al., 2023**), and accelerating molecular simulations (Pasarkar et al., 2023)." Stein et al. 2023 is the large NeurIPS image-generation evaluation study; per the Cousins paper, they "used the Vendi score of order q = 1 and found it to work more effectively as a measure of **per-class** diversity" — a qualified endorsement, not a blanket one.

Also appearing in search results as downstream users: `seqme` (biological sequence design evaluation library), DRAGON (diffusion reward optimization, uses VS as a reference-free distributional reward). (UNVERIFIED)

### Criticisms

**Self-acknowledged, verbatim from the paper's own Limitations section (VERIFIED):**
1. Reference-freeness cuts both ways: "it is possible to get a high diversity score by, for example, sampling **random noise**. This is also true of other reference-free metrics, like IntDiv and n-gram diversity. Therefore, vs should be used alongside a quality metric."
2. Kernel sensitivity: "vs is dependent on the choice of similarity function. **If the similarity function is too sensitive, all sets will appear very diverse, while if it is not sensitive enough, all sets will have low diversity.** Additionally, the wrong choice of similarity function can introduce biases that lead to skewed diversity scores."
3. Cost: "the computational cost of calculating vs can be high when the similarity function is not associated with low-dimensional embeddings."

**External criticisms:**
- **Prevalence-insensitivity at q=1** — the Cousins paper is itself a criticism of the original, and the sharpest one available: under class imbalance VS₁ silently lumps rare classes together (VERIFIED, Section 4 above).
- **Finite-sample non-convergence** — Ospanov & Farnia show that for kernels with infinite-dimensional feature maps, the empirical score at practical n may not converge to the population statistic; the reported numerical claim is that Vendi computed with **n below ~20,000** may not have converged. This is a serious practical critique: it implies published Vendi numbers at typical sample sizes may be sample-size artifacts, non-comparable across papers using different n. (UNVERIFIED — search snippet; I read the FKEA paper, not the truncation paper. Worth verifying directly if load-bearing.)
- **Embedding-induced bias** — the Conditional Vendi Score paper (arXiv:2411.02817, Jalali et al.) notes reliance on high-quality embeddings "which may introduce biases for an arbitrarily selected embedding, as discussed in Stein et al. (2023)." (UNVERIFIED)
- **Prompt confounding** — Conditional Vendi argues unconditional VS conflates model-induced diversity with prompt-induced variation, motivating a conditional variant for prompt-based models and LLMs. (UNVERIFIED)
- **Non-comparability across kernels** — this follows logically from the definition (VS is only meaningful relative to a fixed k), and the paper's own limitation #2 concedes the mechanism, but I found **no paper that states non-comparability as a formal critique**. Mark **UNANSWERED** as a distinct published criticism.
- **Bandwidth sensitivity specifically** — the RBF γ/σ is a free hyperparameter in both the original synthetic experiments and the Cousins molecular experiments ("γ is a hyperparameter of choice"; Cousins tunes hyperparameters "for each choice of q via grid search"). No paper I read reports a bandwidth sensitivity analysis. **UNANSWERED** as a published critique, though the exposure is real and the caller's suspicion is well-founded.
- **Human validation** — I found **no** study validating VS against human diversity judgments as the primary object. The closest is the Cousins paper correlating VS_∞ against *human error rate* on CIFAR-10/ImageNet256 (Figure 4), but that measures realism-confusability, not perceived diversity, and it is a correlation the authors use to argue models fooling humans are memorizing. Mark **UNANSWERED**: no human-validation-of-VS-as-diversity study located.

## 6. Difference from Inception Score and FID (VERIFIED)

The paper's Section 2 states each precisely.

**FID (Heusel et al. 2017):** "measures the Wasserstein-2 distance between two Gaussian distributions, one Gaussian fit to the embeddings of the reference sample and another one fit to the embeddings of the sample to be evaluated for diversity." It requires (a) a reference dataset and (b) a pretrained embedding network. It is a *coverage* notion: "reference-based metrics define diversity in terms of coverage of the reference."

**Inception Score (Salimans et al. 2016):** "mainly used to evaluate the perceptual quality of image generative models, evaluates diversity using the entropy of the marginal distribution of class labels predicted by an ImageNet classifier." It requires a pretrained classifier and predefined labels, and conflates quality with diversity.

**Number of Modes (Srivastava et al. 2017):** "calculated by using a classifier trained on a labeled dataset and then counting the number of unique labels predicted." The paper's summary of both: "Both is and nom define diversity in terms of predefined labels, and therefore require knowledge of the ground truth labels and a separate classifier."

**Vendi's position, verbatim:** "Compared to these approaches, the Vendi Score is a reference-free metric, measuring the **intrinsic diversity** of a set rather than the relationship to a reference distribution. This means that the Vendi Score should be used along side a quality metric, but can be applied in settings where there is no reference distribution."

So the caller's framing is exactly right: IS = quality+diversity via label entropy, needs a classifier; FID = distributional distance, needs a reference; VS = pure diversity, reference-free, quality-agnostic, needs only k.

### The paper's failure-mode experiments (VERIFIED, with numbers)

**Mode-within-class collapse in GANs (Table 1, StackedMNIST, 1000 modes).** VS computed with the probability product kernel k(x,x') = Σ_y p(y|x)^½ p(y|x')^½ (Jebara et al. 2004):

| Model | NoM | Mode Div. | VS |
|---|---|---|---|
| Self-cond. GAN | 1000 | 921.0 | 746.7 |
| PresGAN | 1000 | 948.7 | 866.6 |
| Original | 1000 | 950.8 | 943.7 |

NoM saturates at 1000 for all three and is useless. VS separates them and ranks both GANs below the real data. The authors also compute "Mode Diversity" = exp H(p̂(y)) to test whether VS is merely measuring mode-proportion imbalance: "The generative models score lower on vs than Mode Diversity, indicating that low scores **cannot be entirely attributed to imbalances in the mode distribution**." That is the paper's direct evidence for sub-class-level diversity capture.

**Molecular duplicates vs IntDiv (Figure 3, MOSES benchmark, 2500 valid molecules, Morgan fingerprint radius 2):** HMM VS=55.8 / IntDiv=0.853; AAE VS=142.1 / IntDiv=0.849; Real VS=127.7 / IntDiv=0.847. IntDiv ranks the HMM among the *most* diverse; VS ranks it *least*. Cause: "the hmm molecules have low pairwise similarity scores, but there are a number of **clusters of identical or nearly identical molecules**." Average-similarity metrics are blind to duplicate clusters; spectral ones are not.

**Synthetic (Figure 2):** univariate Gaussian mixtures varying number of components, mixture proportions, or per-component variance, RBF kernel. "VS behaves consistently and intuitively in all three settings... ranging between one and five in the first two rows and increasing from five to seven in the third row as we increase within-mode variance. On the other hand, the behavior of IntDiv is different in each setting: for example, IntDiv is relatively insensitive to within-mode variance, and additional modes bring diminishing returns."

Note the *comparator throughout is IntDiv, not IS/FID*. The paper reports IS/FID/Precision/Recall/VS side by side for image models (CIFAR-10, ImageNet 64×64, LSUN Cat 256×256) but its constructed failure demonstrations target average-similarity metrics. There is no synthetic experiment purpose-built to show FID failing. **UNANSWERED**: a paper-internal synthetic experiment isolating an FID failure mode.

## 7. Implementation (VERIFIED)

**PyPI package name: `vendi-score`** (import as `vendi_score`). `https://pypi.org/project/vendi-score/`

- Install: `pip install vendi-score` (the README writes `pip install vendi_score`; PyPI normalizes hyphen/underscore, both resolve).
- Extras: `vendi_score[images]`, `[text]`, `[molecules]`, `[all]`.
- **Version on PyPI: 0.0.3, released Aug 30, 2022.** Requires Python ≥3.7.
- **License: MIT.** Author: Dan Friedman.
- **GitHub: `https://github.com/vertaix/Vendi-Score`** — confirmed, 163 stars, 13 forks, MIT.

**API surface (from README):**
- `vendi.score(samples, k)` — k must be symmetric with k(x,x)=1
- `vendi.score(samples, k, q=1.)` — q-order support; "larger orders weight common elements more heavily"
- `vendi.score_K(K)` — precomputed similarity matrix
- `vendi.score_dual(X)` — the d<n dual path via the covariance matrix; `normalize=True` if rows aren't unit-norm
- Domain helpers: `vendi_score.image_utils` (`pixel_vendi_score`, `embedding_vendi_score`), `vendi_score.text_utils` (`ngram_vendi_score`, `embedding_vendi_score` with BERT/SimCSE), `vendi_score.molecule_utils`
- Examples in `examples/` notebooks

**`vendi.score_X(X)`: does not exist.** The README documents `score`, `score_K`, and `score_dual` only. If the caller has seen `score_X` referenced somewhere, it is either an older name or a confusion with `score_dual`. Mark **UNANSWERED** whether it ever existed.

**Version discrepancy worth flagging (UNVERIFIED):** PyPI shows only 0.0.3 from Aug 2022, but the README documents the `q` parameter, which comes from the AISTATS 2024 follow-up. Either PyPI is stale relative to `main`, or `q` shipped in 0.0.3. If q-order scores matter, install from GitHub (`pip install -e .` from a clone) rather than PyPI, and verify the signature. I did not confirm the released sdist's contents.

**Other implementations (VERIFIED existence, UNVERIFIED quality):**
- `github.com/vertaix/Vendi-Score-R` — R implementation
- `github.com/vertaix/Quality-Weighted-Vendi-Score` — qVS
- `github.com/aziksh-ospanov/FKEA` — linear-time RFF approximation, the one to use above ~10⁴ samples
- `seqme` (Python, biological sequence design) includes VS as a reference-free diversity metric (UNVERIFIED)

## Summary of gaps (UNANSWERED)

1. No standalone "Nyström Vendi Score" paper located; Nyström gets one sentence in the original and is analyzed only inside the Ospanov/Farnia truncation work.
2. No human-validation study of VS as a diversity measure. The Cousins human-error-rate correlation measures realism, not perceived diversity.
3. No published critique specifically on bandwidth sensitivity or cross-kernel non-comparability, despite both being live exposures the original paper's limitations section half-concedes.
4. FKEA Table 1 wall-clock numbers not extracted.
5. The VS₂/VS_∞ bound direction (Eq. 8) is ambiguous in my text extraction.
6. Whether PyPI 0.0.3 actually ships the `q` parameter.
7. No paper-internal synthetic experiment isolating an FID failure mode (the constructed failures target IntDiv and NoM).

## Sources

- [The Vendi Score (arXiv:2210.02410)](https://arxiv.org/abs/2210.02410) | [TMLR/OpenReview forum](https://openreview.net/forum?id=g97OHbQyk1) | [ar5iv HTML](https://ar5iv.labs.arxiv.org/html/2210.02410)
- [Cousins of the Vendi Score (arXiv:2310.12952)](https://arxiv.org/abs/2310.12952) | [PMLR v238](https://proceedings.mlr.press/v238/pasarkar24a/pasarkar24a.pdf)
- [Towards a Scalable Reference-Free Evaluation of Generative Models / FKEA (arXiv:2407.02961)](https://arxiv.org/abs/2407.02961) | [OpenReview](https://openreview.net/forum?id=Ex3rPvEct8) | [FKEA code](https://github.com/aziksh-ospanov/FKEA)
- [Truncated Vendi Score (PMLR v286)](https://proceedings.mlr.press/v286/ospanov25a.html) | [Statistical Complexity of Estimating VENDI Scores (arXiv:2410.21719)](https://arxiv.org/abs/2410.21719)
- [Quality-Weighted Vendi Scores (arXiv:2405.02449)](https://arxiv.org/abs/2405.02449) | [code](https://github.com/vertaix/Quality-Weighted-Vendi-Score)
- [Vendi-RAG (arXiv:2502.11228)](https://arxiv.org/html/2502.11228v2) | [Vendi Information Gain (arXiv:2505.09007)](https://arxiv.org/html/2505.09007v1) | [Conditional Vendi Score (arXiv:2411.02817)](https://arxiv.org/pdf/2411.02817)
- [vendi-score on PyPI](https://pypi.org/project/vendi-score/) | [vertaix/Vendi-Score](https://github.com/vertaix/Vendi-Score) | [Vendi-Score-R](https://github.com/vertaix/Vendi-Score-R)
