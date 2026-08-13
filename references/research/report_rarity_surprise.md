# Novelty/Creativity Metrics: Frequency-Rarity vs. Information-Theoretic Surprise

Search budget exhausted (200/200); some gaps marked UNANSWERED below.

---

## PART A — Frequency / statistical-rarity originality

### A1. The classic computation

**Wallach & Kogan (1965) uniqueness index** — the dominant operationalization:
1. Pool all responses from all participants in the sample.
2. Count frequency of each distinct response.
3. A response scores 1 (original) if **exactly one person in the sample gave it**, else 0.
4. Person's originality = sum of unique responses.

**Guilford / TTCT weighted-infrequency scoring** — bins by relative frequency in the norming sample. Torrance's TTCT awards points for responses occurring in **less than 5%** of the sample (some variants: 2 points <1%, 1 point 1–5%, 0 points >5%). Guilford's original proposal weighted each response by its inverse frequency; Silvia et al. note this was abandoned largely because it is laborious, not because it is worse.

**Percentage/ratio correction (Hocevar 1979)**: originality ÷ fluency. Later work (Forthmann, Szardenings & Holling 2018) concluded that **for correlational studies, ratio quality scores must be the default choice** — i.e., the raw sum is essentially uninterpretable.

### A2. Failure mode 1 — the fluency confound (fatal, and quantified)

[Hocevar (1979), *J. Educational Psychology* 71(2), 191–196](https://eric.ed.gov/?id=EJ203979): N=60 college students, three Guilford tasks (Alternate Uses, Plot Titles, Consequences). Originality scores were reliable and intercorrelated — **but when ideational fluency was partialled out, all reliabilities showed substantial decrements and the intercorrelations dropped to zero or near zero.** Conclusion: the reliability and convergent validity of originality scores was a function of individual differences in *fluency*, not originality.

Magnitudes reported in [Silvia et al. (2008), *Psychology of Aesthetics, Creativity, and the Arts* 2(2), 68–85](https://www.researchgate.net/publication/232480788):
- Latent-variable reanalysis of Wallach & Kogan's own data: **fluency ↔ uniqueness r = .89**.
- TTCT verbal manual (Torrance 2008): **median r = .88 between originality and fluency subscores.**

This is not a nuisance correlation; at r ≈ .88–.89 the originality score is close to a linear re-expression of "how many things did you write down." The mechanism is mechanical and unavoidable: P(at least one unique response) is monotone increasing in the number of responses. Runco & Albert (1985) replicated: in nongifted samples, originality and flexibility were "seriously confounded" by fluency and were not reliable indices of divergent thinking.

Forthmann (2021, *J. Creative Behavior*) shows this is formally the **same artifact** as Simonton's equal-odds baseline in scientific creativity — the equal-odds rule can be *derived* from Forthmann et al.'s (2020) expected-correlation result. Worth noting: the fluency contamination effect is a general property of any "count of rare items" metric, not something specific to psychometrics.

### A3. Failure mode 2 — sample-size dependence

The score of a *fixed* response changes when you change the sample. Silvia et al.'s example: a response has a much higher chance of exceeding the 95th percentile of rarity in a sample of 50 than in a sample of 1,000. Consequences:
- Uniqueness scores are **not comparable across studies** with different N.
- The base rate of uniqueness declines as N grows, raising the effective criterion.
- A participant's score depends on the *relative performance of the other people who happened to be in the sample* — a person-level score contaminated by sample composition.
- [Forthmann, Szardenings & Holling (2020), *BJEP*](https://bpspsychub.onlinelibrary.wiley.com/doi/10.1111/bjep.12325): statistical-infrequency estimates need large samples for fine-grained scoring, and **unique responses are always associated with the lowest measurement precision** — precisely the responses that carry the score are the ones estimated worst.

### A4. Failure mode 3 — "rare but random" (the one that matters most for machine metrics)

Uniqueness scoring **cannot distinguish a novel-and-appropriate response from a nonsensical one.** Both are frequency-1. Literature phrasing: frequency-based originality has been criticized for being confounded by fluency, **for being "blind to fuzzy responses,"** and for sample-size dependence. Reviews citing Silvia et al. state the index "overlooks the quality of responses by letting uncommon but unreasonable responses score highly."

This is the structural point: **statistical rarity is a one-sided measure.** Creativity is standardly defined as novelty *conjoined with* appropriateness/value (Runco & Jaeger). Rarity measures only the first conjunct, so it is maximized by the degenerate strategy of emitting garbage. Any metric that scores rarity alone — corpus log-frequency, PMI, perplexity, KL surprise — inherits this failure exactly.

Empirical confirmation of the asymmetry comes from a different literature (see B3): in [Saakyan et al., "Death of the Novel(ty)" (arXiv:2509.22641)](https://arxiv.org/abs/2509.22641), of 8,618 expert-writer annotations, **only 3% and 5% of "perceived novel" annotations were of non-pragmatic and non-sensical expressions respectively** — i.e., humans essentially *refuse* to call something novel unless it also makes sense. Sense is a precondition for perceived novelty, not an independent dimension. A rarity metric has this backwards.

### A5. How weakly does uniqueness correlate with subjective originality ratings?

Honest answer: **Silvia et al. (2008) is cited constantly for this claim, but I could not retrieve the specific uniqueness↔subjective-rating r from the paper itself — full text was paywalled/403 on every mirror I tried (Ovid 402, UNC-G repository 301-redirected, ScienceDirect 403).** The findings I *can* verify from secondary sources:

- Uniqueness ↔ fluency: **r ≈ .88–.89** (verified, above).
- Subjective ratings ↔ fluency: "much lower correlations between ratings and fluency than between uniqueness scores and fluency" — direction verified, exact values **UNANSWERED**.
- Silvia et al.'s generalizability analysis: subjective ratings of unusual-uses and instances tasks yield dependable scores with only **2–3 raters**; validity study N = 226 predicting DT from Big Five/Plasticity–Stability.
- Direct uniqueness ↔ subjective-rating correlation coefficient: **UNANSWERED.** Forthmann et al. are cited as reporting "positive correlations with frequency-based originality" for subjective scoring, which suggests it is not near-zero — treat "uniqueness barely correlates with rated originality" as *plausible but unverified at a numeric level*. Anyone building on this should retrieve Silvia et al. (2008) Tables 3–5 directly, plus [Plucker et al. (2011), "Is Originality in the Eye of the Beholder?", *J. Creative Behavior*](https://onlinelibrary.wiley.com/doi/abs/10.1002/j.2162-6057.2011.tb01081.x), which directly compares scoring techniques.

Silvia's own summary of the practical situation, from the follow-up (Silvia, Martin & Nusbaum 2009): "All research is at least slightly painful, but uniqueness scoring is uniquely painful."

### A6. Corpus-based rarity for text/ideas — what's actually validated

The closest well-validated cousins are **semantic distance** measures, not raw frequency. [Beaty & Johnson (2021), SemDis, *Behavior Research Methods* 53(2), 757–780](https://pmc.ncbi.nlm.nih.gov/articles/PMC8062332/) — real numbers against human creativity ratings:

| Study | N | Task | Latent semantic-distance ↔ human ratings |
|---|---|---|---|
| 1 | 171 | AUT box+rope | **r = .91** (multiplicative composition) |
| 1 | 171 | same, additive composition | **r = −.37** |
| 2 | 142 | AUT box+rope | **r = .75** |
| 3 | 133 | AUT brick (single item) | **r = .45** |
| 4a | 62 | verb generation | creativity r = .49; **novelty r = .73**; appropriateness **r = −.81** |
| 4b | 56 | verb generation | creativity r = .73; **novelty r = .95**; appropriateness **r = −.93** |
| 5 | 58 | noun associations | mean originality **r = .85** |

Two things to take from this table. First, the metric is enormously sensitive to implementation choice: **the same data, composed additively instead of multiplicatively, flips from r = .91 to r = −.37.** That is a warning about how much of a "validated" distance metric's performance lives in preprocessing rather than theory. Second, and more important: **semantic distance correlates strongly with rated *novelty* (r = .73–.95) and strongly *negatively* with rated *appropriateness* (r = −.81 to −.93).** That is the rare-but-random confound, measured. Distance buys novelty by spending appropriateness, roughly one-for-one.

Word-count contamination persists too: multiplicative SemDis ↔ word count r = .47 (human ratings ↔ word count r = .41) — the fluency confound reappearing in machine dress.

For contrast, the current ceiling: [Organisciak, Acar, Dumas & Berthiaume (2023), *Thinking Skills and Creativity* 49, 101356](https://files.eric.ed.gov/fulltext/ED629879.pdf) fine-tuned LLMs on ~27,000 human-judged AUT responses across nine studies and reached **r = 0.81 with human raters**, approaching human inter-rater agreement, while reporting prior semantic-distance systems at **r = .12–.26** on the same evaluation data. Note the tension with the SemDis table — different data, different aggregation; both cannot be describing the same construct performance. The reconciliation is that SemDis's high r's are *latent-factor to mean-rating* correlations at the person level, while .12–.26 are response-level. **Anyone quoting these numbers must state the level of analysis or the comparison is meaningless.**

**PMI-based novelty**: I found no validated PMI-based idea-novelty measure with human-judgment correlations. **UNANSWERED / likely nonexistent as a validated instrument.** Related: [Zhang & Evans, "Language Model Perplexity Predicts Scientific Surprise and Transformative Impact" (arXiv:2509.05591)](https://arxiv.org/abs/2509.05591) — >2M papers, perplexity from 5 open LLMs on papers published immediately post-training-cutoff. Findings are qualitative in the abstract: high perplexity predicts more variable review ratings, longer editorial delays, greater reviewer uncertainty, bimodal outcomes (over-represented among both the most celebrated *and* the most discounted work), fewer short-term citations, more interdisciplinary engagement, DARPA-vs-NIH funding skew — **and the pattern reverses in the humanities**, where the *least* surprising work is most celebrated. Exact effect sizes **UNANSWERED** (PDF exceeded fetch size limit). The bimodality finding is the most useful datum in the whole report for anyone designing a novelty metric: high surprise is a variance amplifier, not a quality signal.

### A7. Implementation cost, Part A

- Uniqueness scoring: trivial compute, but requires a **sample** (it is not a per-item function), demands response normalization/synonym merging (the actual labor), and produces scores that are not portable across datasets. Cheap to run, expensive to trust.
- Corpus log-frequency / PMI: trivial; needs a reference corpus; inherits every domain-mismatch problem the corpus has.
- Semantic distance (SemDis): cheap, open platform, but the additive/multiplicative result above means you must validate your specific pipeline, not import someone's r.
- Fine-tuned LLM scoring (Ocsai): needs thousands of human labels, but is the only approach in this family that measures the *conjunction* rather than one conjunct.

---

## PART B — Information-theoretic and Bayesian surprise

### B1. Baldi & Itti Bayesian surprise — the one thing here that IS validated (against the wrong construct)

[Itti & Baldi, "Bayesian surprise attracts human attention," *Vision Research* 49(10), 1295–1306, 2009](https://pmc.ncbi.nlm.nih.gov/articles/PMC2782645/) (NIPS version 2005; note the paper is 2009, not 2010 — a citation error worth not propagating).

**Exact definition.** Over a model space M with prior P(M) and data D:

> S(D, M) = KL(P(M|D), P(M)) = ∫ P(M|D) log[P(M|D)/P(M)] dM

Surprise is the *average* log-ratio of posterior to prior over the model space. The paper is emphatic on a point that most citers miss: **surprise requires integration over the model space**, and is therefore not the same as negative log-likelihood, outlier-ness, or feature-space distance. Their own illustration: a continually blinking light stays a temporal outlier forever but is surprising only once; random snow continuously excites low-level detectors yet becomes unsurprising as soon as the prior collapses onto the snow model.

**Concrete implementation.** 72 low-level feature maps (intensity, color, orientation ×4 directions, motion, flicker). Each patch is a 72-D vector of Poisson rate parameters; the conjugate prior over the Poisson rate λ is a **Gamma(α, β)**, so the posterior is Gamma(α+n, β+N) and the KL has a closed form involving the digamma function ψ. Temporal surprise S_T = KL(γ(λ;α′,β′), γ(λ;α,β)); spatial surprise S_S computed identically over a Gamma neighborhood prior; the two are combined **additively**.

**Validation numbers (human eye fixations on natural video).** KL between the distributions of metric values at human saccade targets vs. random saccade targets, 100× resampled:

| Metric | KL score |
|---|---|
| variance (static) | 0.074 ± 0.003 |
| orientation | 0.077 ± 0.004 |
| intensity | 0.084 ± 0.004 |
| entropy (best static) | **0.151 ± 0.005** |
| motion | 0.179 ± 0.005 |
| saliency (best competitor) | **0.205 ± 0.006** |
| **surprise** | **0.241 ± 0.006** |
| human-derived upper bound (inter-observer) | **0.679 ± 0.011** |

Surprise beat every other metric at p < 10⁻¹⁰⁰ on t-tests for equality of KL scores: **~20% above saliency, ~60% above entropy.** **72% of all human saccades** landed on locations more surprising than average; rising to **76%, 80%, and 84%** as two, three, or four observers agreed on a common gaze target.

**Blunt caveat that everyone drops.** Surprise reaches 0.241 of a 0.679 human ceiling — **about 35% of the achievable inter-observer signal.** "Best known attractor of human attention" is true and also means the best available bottom-up model explains roughly a third of what one human predicts about another.

**Has Bayesian surprise EVER been validated against human NOVELTY or CREATIVITY judgments?** As far as I can establish: **no.** It has been validated against *gaze*, which is a fast, involuntary, low-level orienting response — a construct with no established mapping onto deliberative creativity judgment. The design-creativity community imported it (Grace, Maher, Fisher & Brady 2014; Maher & Fisher's data-intensive work cites "Baldi and Itti, 2010; Itti and Baldi, 2005" for defining priors) but **I found no study reporting correlations between Bayesian surprise and human creativity or novelty ratings.** Marked **UNANSWERED-leaning-NO**: absence of evidence here is meaningful because I searched the citing literature directly and the citing papers themselves describe such validation as future work (see B4).

**Failure modes.** (i) Everything depends on the model space M and the prior — surprise is not a property of the data, and two defensible model choices give different answers with no principle to arbitrate. (ii) Additive combination of temporal and spatial surprise is a free parameter presented as a definition. (iii) It measures *belief change*, which is agnostic to whether the change is toward truth, nonsense, or noise — the rare-but-random confound in Bayesian clothing. (iv) It needs a *stream* of data to update over; it is not defined for a single static artifact without inventing an observation sequence.

**Cost.** The vision implementation is genuinely cheap per-frame (conjugate updates, closed-form KL), but the original evaluation consumed **over one CPU-year** of simulation. For a new domain, the expense is not the KL — it is specifying a conjugate model family that makes the integral tractable. Outside exponential-family/conjugate setups you are into sampling, and the "exact computation" advantage evaporates.

### B2. Schmidhuber's compression progress — beautiful theory, essentially zero direct validation

Sources: [Schmidhuber, "Driven by Compression Progress" (arXiv:0812.4360, 2009)](https://arxiv.org/abs/0812.4360); "Formal Theory of Creativity, Fun, and Intrinsic Motivation (1990–2010)," *IEEE Trans. Autonomous Mental Development* 2(3), 2010.

**Exact claim.** Subjective *beauty* of data D to observer O at time t is the negative of the number of bits needed to encode D given O's current compressor. **Interestingness is the first derivative of beauty** — the *change* in compressibility as the compressor learns, i.e. the steepness of the learning curve. Curiosity/creativity = an intrinsic reward proportional to compression progress; the agent acts to maximize it. Crucially the measure is **observer-relative and time-indexed**: no objective order/complexity ratio is postulated (unlike Birkhoff/Bense information aesthetics).

**Direct empirical validation against human judgments: I found none.** Be blunt about this. The theory is presented as explaining beauty, novelty, surprise, interestingness, attention, curiosity, creativity, art, science, music, and jokes, and it does so without a single reported correlation coefficient against human ratings in the source papers. What exists is:

- **Adjacent, not direct**: predictive-processing work in music that shares the inverted-U intuition. [Gold, Pearce, Mas-Herrero, Dagher & Zatorre (2019), *J. Neuroscience* 39(47), 9397–9409](https://www.jneurosci.org/content/39/47/9397) used IDyOM information content and entropy to predict musical pleasure; Cheung et al. (2019), *Current Biology* 29, 4084–4092 found uncertainty and surprise *jointly* predict pleasure and amygdala/hippocampus/auditory-cortex activity. These validate an **inverted-U in predictability**, which is *consistent with* compression-progress but is also consistent with half a dozen other accounts and does not test the derivative claim. Exact effect sizes **UNANSWERED** (search budget exhausted before retrieval).
- [Pearce & Wiggins, "Information dynamics: patterns of expectation and surprise in the perception of music," *Connection Science* 21(2–3), 2009](https://www.tandfonline.com/doi/full/10.1080/09540090902733756) — model structural analysis "largely agrees with that of an expert human listener" (n = 1 expert; qualitative agreement, not a validated correlation), and proposes predictive information rate as an explanation of the inverted-U.
- Secondary sources cite empirical tests in visual art (Mayer & Landwehr 2020) and small-scale text (van de Cruys 2021). **I could not verify these against the primary sources — UNANSWERED.**

**Critiques worth carrying.** Togelius and colleagues offer a notably hedged endorsement — the theory "arguably holds explanatory power at least under certain circumstances" — and flag that the agent stores all observations indefinitely and retrains its compressor on the whole history, which is computationally prohibitive. The observer-relativity that makes the theory elegant also makes it **near-unfalsifiable**: any disagreement with human ratings is absorbable as a difference in the observer's compressor. [Schmidhuber's own lab has since noted](https://people.idsia.ch/~juergen/creativity.html) that next-token prediction loss is a poor indicator of interesting computation. Alternatives proposed in reaction: **coherence progress** ([Springer, 2011](https://link.springer.com/chapter/10.1007/978-3-642-22887-2_3)), a compressor-independent measure.

**Cost.** Prohibitive in the literal form (retrain compressor on full history per observation). Practical approximations (gzip-delta, model-loss-delta) are cheap but discard the very thing — an idealized learning compressor — that gives the theory its force. **Verdict: adopt the vocabulary, do not expect the measurement.**

### B3. Perplexity/surprisal as text novelty — real numbers, and they are not flattering

**Computation.** Surprisal of token wᵢ = −log P(wᵢ | context) under an LM; perplexity of a span = exp(mean surprisal). Trivially cheap. Usually log-standardized before use (skew).

**The best-designed validation available.** [Saakyan, Kim, Muresan & Chakrabarty, "Death of the Novel(ty)" (arXiv:2509.22641)](https://arxiv.org/abs/2509.22641), code at [github.com/asaakyan/ngram-creativity](https://github.com/asaakyan/ngram-creativity). Design: **8,618 expression-level annotations by paid expert writers** (Upwork, trained, ~$100/batch of 10) on human- and AI-generated text, rating each pre-highlighted expression for **novelty, sensicality, and pragmaticality** separately. Creativity = all three simultaneously. Mixed-effects logistic regression with random intercepts for annotator and seed passage; Randolph free-marginal κ for agreement (low prevalence made standard κ inappropriate).

Results:
- **Standardized log perplexity is significantly associated with creativity: OR ≈ 1.96 per SD, p < 0.001.** Real, positive, and modest.
- **~91% of top-quartile n-gram-novel expressions were NOT judged creative.** Precision at the top of the rarity distribution is ~9%.
- **~25% of unique creative expressions fall below mean perplexity** (n = 605, Wilson 95% CI [0.21, 0.28]) — a quarter of genuinely creative expressions are *not* n-gram novel at all. Annotator rationales cite contextual factors.
- **Higher perplexity predicts LOWER pragmaticality** in AI-generated text: OLMo β = −0.17, χ²(1) = 4.89, p = 0.027. The rare-but-random confound, directly measured, with a p-value.
- Of the annotations: 722 flagged non-pragmatic, 274 non-sensical; **only 3% / 5% of "perceived novel" ratings were on non-pragmatic / non-sensical expressions** — humans treat sense as a precondition for novelty.
- Out-of-distribution check on expert-edited text pairs: novelty β = 0.04, OR = 1.30, p < 0.001; pragmaticality β = 0.28, SE = 0.05, OR ≈ 1.33.

**Metaphor novelty.** [Momen et al. (arXiv:2601.02015, EACL 2026)](https://arxiv.org/abs/2601.02015): cloze-style surprisal conditioned on full-sentence context, **16 causal LM variants**, corpus-based and synthetic metaphor-novelty datasets. Finding: **significant *moderate* correlations** with human novelty scores, plus a striking scaling divergence — on corpus-based data correlation strength **decreases with model size (inverse scaling)**; on synthetic data it increases. Conclusion in the authors' own words: surprisal "remains limited as a metric of linguistic creativity." **Exact r values UNANSWERED** (abstract page only; numbers are in the full paper / [github.com/OmarMomen14/surprisal-metaphor-novelty](https://github.com/OmarMomen14/surprisal-metaphor-novelty)). The inverse-scaling result deserves attention: a better language model is a *worse* novelty detector on naturalistic data. That is not a bug you can engineer around by scaling up.

**Divergent thinking.** "A pleasant surprise: perplexity from large language models assesses divergent thinking" (Raffaelli et al., preprint ~Oct 2025; also [*Thinking Skills and Creativity*](https://www.sciencedirect.com/science/article/abs/pii/S1871187126000957), "Tracking Divergent Thinking: Perplexity as a Unified Metric of Surprise"). Two studies: (1) perplexity of verbalized thought during goal-directed DT and unprompted stream-of-consciousness predicts DT performance; (2) perplexity as automated originality scoring for open-ended real-world problems, benchmarked against DSI. Authors caution that **word count correlates with originality and biases length-sensitive automated measures** — the fluency confound again. **Exact correlations UNANSWERED** (ScienceDirect 403, ResearchGate not retrieved).

**The structural confound, stated plainly.** Critical reviews ([EACL 2026 critical analysis of creativity evaluations](https://aclanthology.org/2026.eacl-long.297.pdf)) find perplexity "captures fluency rather than conceptual novelty." Optimizing *against* perplexity produces neural text degeneration (bland, repetitive); optimizing *for* it produces incoherence. There is no monotone region where perplexity means "creative" — high perplexity is a mixture of {genuinely novel, ungrammatical, off-topic, typo, rare proper noun}, and the mixture proportions are dominated by the last four.

**Cost.** Near-zero (one forward pass). This is precisely why it is over-used. It is the cheapest available metric and among the least valid; those two facts are causally related.

### B4. Surprise in computational creativity evaluation

**Ritchie (2007), "Some Empirical Criteria for Attributing Creativity to a Computer Program," *Minds and Machines* 17(1), 67–99.** Fourteen criteria built on **typicality** and **quality** of outputs, defined over the inspiring set vs. produced set. Deliberately empirical rather than philosophical. Applied to Wasp (poem generator), Divago (conceptual blender), Dupond (paraphraser) — the [CISUC application study](https://old.cisuc.uc.pt/publication/show/1043) discusses results *and the methodology's difficulties*. Note: Ritchie's criteria are *ratios over rated sets* — they require human ratings of typicality and quality as **inputs**; they do not replace them. Anyone hoping Ritchie gives an automatable novelty score has misread the framework.

**Wiggins (2006), "A preliminary framework for the description, analysis and comparison of creative systems," *Knowledge-Based Systems* 19(7), 449–458.** First formal logic-based creativity framework; formalizes Boden. ⟨U, L, ℛ, 𝒯, ℰ⟩: universe of possibilities U, rule set ℛ constraining the conceptual space, traversal rules 𝒯, evaluation ℰ. Key result: **transformational creativity is exploratory creativity at the meta-level** (search over the space of generative systems). Characterizes failure modes — hopeless uninspiration (no valued concepts in U), conceptual uninspiration (none in the agent's space), generative uninspiration (search can't find them). **This is a descriptive/analytical vocabulary, not a metric.** It yields no number and was never validated against human judgment; that is not a criticism of it, but it is routinely miscited as if it were an evaluation method. Ritchie himself ([*New Generation Computing*, 2006](https://link.springer.com/article/10.1007/bf03037334)) argued Boden's transformational hypothesis is "too vaguely defined to support falsifiable claims."

**Grace, Maher, Fisher & Brady — surprise in design.** [Maher, Brady & Fisher, ICCC 2013](https://www.computationalcreativity.net/iccc2013/download/iccc2013-maher-brady-fisher.pdf). Position: novelty, value, and surprise are three *distinct necessary* characteristics; **surprise is violated expectation, and is therefore novelty in a space of *projected* designs rather than in the space of *existing* designs.** This is the sharpest conceptual contribution in the whole area and is worth stealing: it explains *why* rarity fails. Their argument against rarity, in their own framing: "as rare events recur, as they are apt to do, many observers would regard them as less surprising," so "rarity may not in fact be a sufficient basis for assessing surprise."

Implementation: linear regression over 9 numeric attributes of **572 automobile designs, 1878–2009** (Dowlen 2012; only 190 entries complete on all nine); surprise = distance from the regression line projected forward in time. Three variants: full-history regression, rolling-window regression, etc. They found full-history models let early outliers ("black swans") perpetually skew all subsequent surprise measurements, motivating windowed models whose window size "has a large influence on the results."

**Validation against human judgments: explicitly none.** The paper's own Directions for Further Research, item 5: *"we want to match the results of our models in identifying surprising designs to human judgments of surprise, and of course to assessments of creativity."* Stated as future work in 2013. The follow-up ([Grace, Maher, Fisher & Brady, "Modeling Expectation for Evaluating Surprise in Design Creativity," 2014/DCC](https://www.researchgate.net/publication/299841191)) and the data-intensive IJDCI paper build the expectation model further; **whether either ever reported human-judgment correlations: UNANSWERED** — search budget exhausted before I could retrieve them. My prior, given that the 2013 paper frames it as future work and the 2016 [Yannakakis & Liapis "Searching for Surprise"](https://www.computationalcreativity.net/iccc2016/wp-content/uploads/2016/01/Searching-for-Surprise.pdf) survey cites them as *models* rather than as *validated measures*, is that no such validation was published — but treat that as unverified.

---

## Consolidated verdict

| Metric | Exact computation | Human-judgment validation | Verdict |
|---|---|---|---|
| Uniqueness scoring (W&K) | freq = 1 in sample | Validated as **broken**: r ≈ .88–.89 with fluency; reliability→0 when fluency partialled (Hocevar 1979) | Do not use uncorrected. Ratio-correct at minimum. |
| TTCT <5% infrequency | percentile threshold on norming sample | Same confounds; sample-size-dependent | Same. |
| Corpus log-frequency / PMI | trivial | **No validated instrument found — UNANSWERED/none** | Unvalidated. |
| Semantic distance (SemDis) | cosine over embeddings, multiplicative composition | r = .45–.91 with rated creativity (person-level); **but novelty r≈.73–.95 / appropriateness r≈−.81 to −.93** | Real, but measures one conjunct and trades against the other. Pipeline-sensitive (.91 → −.37 on composition choice). |
| Fine-tuned LLM (Ocsai) | supervised on 27k human labels | **r = 0.81**, near inter-rater ceiling | Best available. Requires labels. |
| **Bayesian surprise (Itti & Baldi)** | KL(P(M\|D), P(M)); Poisson-Gamma conjugate, closed form | Validated on **gaze**: KL 0.241 vs 0.205 saliency vs 0.679 human ceiling; 72–84% of saccades. **Never on novelty/creativity judgments.** | Mathematically exact and empirically real — for the wrong construct. Importing it to creativity is an unvalidated leap. |
| **Schmidhuber compression progress** | dC/dt, first derivative of compressibility | **None found.** Adjacent inverted-U music work only, which does not test the derivative claim. | Beautiful theory, zero direct validation, near-unfalsifiable by construction, computationally prohibitive as specified. |
| **LM perplexity/surprisal** | −log P(w\|ctx) | **OR ≈ 1.96/SD** for creativity; **91% of top-quartile novel not creative**; higher perplexity → **lower** pragmaticality (β=−0.17, p=.027); metaphor novelty: moderate r with **inverse scaling** | Real but weak signal, dominated by the rare-but-random confound. Cheapest and least valid. |
| Ritchie criteria / Wiggins CSF | ratios over rated sets / formal ⟨U,L,ℛ,𝒯,ℰ⟩ | Ritchie applied to 3 systems; Wiggins is descriptive, no metric | Vocabulary and analysis, not measurement. Both require human ratings as input. |
| Grace/Maher design surprise | regression projection, windowed | **Explicitly future work in the source paper.** | Best *conceptual* framing available (surprise = novelty in projected space); no validated instrument. |

**The single most useful finding across both families**, if you build one thing: every rarity-family metric measures novelty by spending appropriateness, and the exchange rate has now been measured twice independently — SemDis (appropriateness r ≈ −.81 to −.93) and expert annotation (91% of high-novelty expressions rejected as not creative, higher perplexity → lower pragmaticality). Human raters treat sense as a **precondition** for novelty, not a co-equal dimension. A metric that scores the conjunction, or that at minimum gates rarity on a sensicality check, is the only design consistent with the measured data.

**Marked UNANSWERED:** exact Silvia et al. (2008) uniqueness↔subjective-rating r; exact metaphor-surprisal correlations and model list; exact Zhang & Evans effect sizes; whether Grace/Maher ever published human validation of design surprise; Mayer & Landwehr (2020) and van de Cruys (2021) as compression-progress tests; exact Gold et al. (2019) / Cheung et al. (2019) effect sizes. Web search budget (200/200) exhausted; several primary PDFs returned 402/403.
