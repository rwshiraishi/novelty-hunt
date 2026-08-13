# How Novelty, Originality, and Surprise Are Operationalized

## 1. Frequency-based (statistical infrequency) originality

**The operationalization.** Guilford's Structure-of-Intellect divergent-production tests and the Torrance Tests of Creative Thinking (TTCT) score a response as "original" if it is statistically infrequent in the sample — the classic thresholds being fewer than 5% of respondents (sometimes <1%, sometimes graded: 2 points for <1%, 1 point for 1–5%). The theory committed to: **originality = low empirical base rate in a reference population**. Nothing about the response's content, coherence, or usefulness enters the measurement.

**Failure mode 1 — fluency confound (the strongest documented result).** Hocevar, D. (1979). *Ideational fluency as a confounding factor in the measurement of originality.* Journal of Educational Psychology, 71(2), 191–196. doi:10.1037/0022-0663.71.2.191. Three Guilford divergent-thinking tests were scored for fluency and originality. Originality scores were reliable — until fluency was partialled out, at which point **all reliabilities showed substantial decrements and intertest correlations dropped to zero or near it**. The measured construct was largely "how many responses did you give," not "how unusual were they." Confirmed in Runco & Albert (1985), *The reliability and validity of ideational originality in the divergent thinking of academically gifted and nongifted children*, Educational and Psychological Measurement, 45(3): in the nongifted sample, originality and flexibility were "seriously confounded" by fluency and were not reliable indices. Dumas & Dunbar (2014), *Understanding fluency and originality: A latent variable perspective*, Thinking Skills and Creativity 14, revisits this with latent-variable methods.

The standard "fix" — Hocevar's originality/fluency ratio — trades one bias for another (it penalizes prolific responders). Snyder, Mitchell, Bossomaier & Pallier's and Silvia et al.'s work on scoring methods documents that ratio scores and summative scores rank people differently.

**Failure mode 2 — sample-size dependence.** The 5% threshold is defined against *this* sample. Double the sample and the set of responses that clear the threshold changes; the "originality" of a fixed response is not a property of the response. Uniqueness scoring is therefore not comparable across studies, and larger samples systematically depress uniqueness scores. Plucker, Qian & Wang (2011), *Is originality in the eye of the beholder? Comparison of scoring techniques in the assessment of divergent thinking*, Journal of Creative Behavior 45(1), 1–22, is the standard reference for how much scores move across scoring techniques on identical data.

**Failure mode 3 — infrequency ≠ creativity.** A nonsense response ("use a brick to photosynthesize"), a typo, and a genuinely brilliant use are all equally infrequent. Frequency scoring cannot separate novelty from bizarreness or error. This is the motivation for Silvia et al. (2008), *Assessing creativity with divergent thinking tasks: Exploring the reliability and validity of new subjective scoring methods*, Psychology of Aesthetics, Creativity, and the Arts 2(2), 68–85, which argued for subjective rater judgment (with latent-variable modeling of rater agreement) over uniqueness scoring precisely because uniqueness scoring is unreliable and confounded.

**Theory commitment:** originality is a population statistic, appropriateness is out of scope, and the reference population is whatever sample you happened to collect.

---

## 2. Distance-based / semantic-distance scoring

**The operationalization.** Represent the prompt (e.g., "brick") and the response ("doorstop") as vectors in a distributional-semantics space, and score originality as the distance (usually cosine distance) between them. Theory commitment: **originality = remoteness of association**, an associative-hierarchy theory of creativity descending from Mednick (1962).

**SemDis.** Beaty, R. E., & Johnson, D. R. (2021). *Automating creativity assessment with SemDis: An open platform for computing semantic distance.* Behavior Research Methods, 53(2), 757–780. doi:10.3758/s13428-020-01453-w. Five compositional vector models (three CBOW "predict" models, two count models); the recommended use is a **latent factor of the common variance across the five**, which the paper reports reliably and strongly predicts human creativity and novelty ratings, replicates the serial-order effect, and shows convergent validity with other creativity measures. Platform: semdis.wlu.psu.edu / osf.io/gz4fc.

**Open Creativity Scoring (OCS).** Dumas, D., Organisciak, P., & Doherty, M. (2020). *Measuring divergent thinking originality with human raters and text-mining models: A psychometric comparison of methods.* Psychology of Aesthetics, Creativity, and the Arts 15(4), 645–663. LSA-based, with term weighting to handle varying response lengths. Tool at openscoring.du.edu.

**The critical benchmark result.** Organisciak, P., Acar, S., Dumas, D., & Berthiaume, K. (2023). *Beyond semantic distance: Automated scoring of divergent thinking greatly improves with large language models.* Thinking Skills and Creativity 49, 101356. On a federated, deduplicated corpus pooled from nine prior AUT studies (Beaty et al. 2018; Beaty & Silvia 2012; Dumas et al. 2020; Hofelich Mohr et al. 2016; Silvia et al. 2008, 2009, 2017), evaluated against multi-rater human judgments:

| Method | Pearson r vs. human raters |
|---|---|
| Best SemDis model (ensemble) | **r = .12** (mean per-prompt r = .19) |
| Open Creativity Scoring (LSA) | **r = .26** (mean per-prompt r = .31) |
| Fine-tuned LLMs (T5, GPT-3 "Ocsai") | **r = .76 – .81** |
| Best fine-tuned model incl. non-deduplicated | r = .86 (authors caution re: data leakage) |
| Held-out (unseen) prompts, best fine-tuned | r = .63 overall (.66 prompt-level); baselines r = .14–.28 |
| GPT-3 zero-shot, plain-English prompt | r = .13 (p < .001) |
| ChatGPT zero-shot | r = .19 (p < .001) |
| GPT-4 zero-shot | **r = .53** |
| GPT-4, 5-shot | r = .66 |
| GPT-4, 20-shot | **r = .70** |
| Averaged across supervised approaches | r = .783 vs. r = .188 for semantic-distance baselines |

**The human ceiling.** Same paper: on duplicated responses, randomized pairs of human judges correlated at **r = .83**; a single human judgment against the less-noisy mean of all judgments of its duplicates correlated at **r = .88**. The authors offer .88 as "the approximate ceiling at which we could expect a model to correlate with human judgements." Fine-tuned LLM performance (.76–.81) is inside that band; semantic distance (.12–.26) is nowhere near it.

Caveat on interpreting the .12 figure: this is *overall pooled* correlation on deduplicated data across heterogeneous rater groups and prompts, which is a harsher setting than the within-study correlations SemDis's own paper reports. The rank ordering (LLM ≫ LSA > raw semantic distance) is robust; the absolute .12 is setting-dependent.

**Documented critiques of semantic distance.**
- **Elaboration bias.** Forthmann, B., Oyebade, O., Ojo, A., Günther, F., & Holling, H. (2019). *Application of latent semantic analysis to divergent thinking is biased by elaboration.* Journal of Creative Behavior 53(4), 559–575. doi:10.1002/jocb.240. Simulation shows LSA-derived distance is systematically biased by the number of words in a response (longer responses get *lower* distance). Partial remedies: stop-word removal plus simulation-based bias correction. Elaboration is now routinely entered as a covariate.
- **Preprocessing and model sensitivity.** SemDis's own design (five models, latent factor) is an admission that any single embedding model's scores are unstable. Organisciak et al. note that the treatment of varying word counts "has remained in debate, from term weighting (Dumas et al., 2020) to multiplicative composition (Beaty & Johnson, 2021)" — i.e., the score depends on a preprocessing choice with no settled answer.
- **It measures novelty only, not appropriateness.** Organisciak et al. note Beaty and Johnson found semantic distance "primarily is a measure of novelty." A distant-but-nonsensical response scores high. This is the frequency-scoring bizarreness problem reappearing in continuous form.
- **Sensitivity to near-duplicates and rater-group idiosyncrasy.** Organisciak et al. found LLM scorers also partly learn rater-group response styles; entropy of human judgments for an item did *not* correlate with model performance (r = .004).

---

## 3. Surprise as a formal quantity

Three distinct formalisms, routinely conflated:

**(a) Shannon surprisal / self-information.** `I(x) = −log p(x)`. Purely a function of the event's probability under a fixed model. Its expectation is entropy. This is the formal version of "infrequency" — and it inherits the exact defect of frequency-based originality scoring: a rare-but-inert event scores maximally.

**(b) Bayesian surprise.** Itti, L., & Baldi, P. (2009). *Bayesian surprise attracts human attention.* Vision Research 49(10), 1295–1306. doi:10.1016/j.visres.2008.09.007 (conference version: NIPS 2005). Surprise is `S(D, M) = KL(P(M|D) ‖ P(M))` — the Kullback-Leibler divergence from the prior over models to the posterior after observing data D. It measures **how much the observation changed the observer's beliefs**, not how improbable it was. Implementation: conjugate Gamma priors over Poisson firing rates, closed-form KL involving the digamma function; temporal surprise plus spatial surprise (KL between a Difference-of-Gaussians neighborhood prior and its posterior) combined additively. Empirically it predicted human gaze allocation on video better than saliency-based competitors.

**This is the cleanest formal statement of "surprise ≠ mere infrequency" in the literature.** Under `−log p`, any low-probability event is maximally surprising. Under KL(posterior ‖ prior), a low-probability event that leaves your model unchanged has surprise ≈ 0 — you expected to be occasionally wrong in exactly that way, and were. Conversely a moderately probable event that forces a large belief update is highly surprising. Surprise is *observer-relative and model-relative* by construction; surprisal is only distribution-relative. If you want one citation for the distinction, it is Itti & Baldi (2009).

**(c) Expectation-violation surprise in computational creativity.**
- Maher, M. L. (2010). *Evaluating creativity in humans, computers, and collectively intelligent systems.* Proc. 1st DESIRE Network Conference on Creativity and Innovation in Design, Aarhus, 22–28. ACM DL 10.5555/1854969.1854977. Proposes a domain- and producer-independent metric over three criteria: **novelty** (distance from other artifacts in the space — "similar but different"), **value** (performance/acceptance, with the wrinkle that a creative artifact can *change* the value system), and **unexpectedness/surprise** (**how far the artifact is from the expected next artifact**).
- Maher & Fisher (2012) operationalize this with artifacts as attribute vectors, novelty as Euclidean distance to known artifacts, clustering to define the artifact class.
- Maher, Brady & Fisher (2013), *Computational models of surprise in evaluating creative design*, ICCC 2013.
- Grace, K., Maher, M. L., Fisher, D., & Brady, K. (2015). *Data-intensive evaluation of design creativity using novelty, value, and surprise.* International Journal of Design Creativity and Innovation 3(3–4), 125–147. doi:10.1080/21650349.2014.943295. Companion: Grace, Maher, Fisher & Brady (2014), *Modeling expectation for evaluating surprise in design creativity*, Design Computing and Cognition '14, 189–206. Here surprise is explicitly the **violation of an extrapolated trend**: you fit a model that predicts the next artifact in the class from the historical sequence, and surprise is the artifact's deviation from that prediction. Novelty is a *static* distance in feature space; surprise is a *dynamic* residual against a temporal/predictive model.

**The distinction, stated formally.**
- **Novelty** = `d(x, C)` — distance from artifact x to a reference corpus/class C in some feature space. Requires only a metric and a corpus. Atemporal. An artifact can be maximally novel and completely unsurprising (a random noise sample is far from everything and predicted by no one to be otherwise).
- **Surprise** = a divergence between a predictive model's expectation and the observation — either `KL(P(M|x) ‖ P(M))` (Itti & Baldi) or `d(x, x̂)` where `x̂` is the model-predicted next artifact (Grace/Maher). Requires a *model of expectation*, and hence a specified observer with a history. An artifact can be surprising while sitting *inside* the corpus's convex hull, if the trend pointed elsewhere.

Grace et al. (2015) draws this most explicitly for artifact evaluation ("computational models of novelty capture only originality relative to the domain and do not model unexpectedness or the violation of trends"); Itti & Baldi (2009) draws it most cleanly as mathematics.

---

## 4. Novelty-for-whom: reference-class dependence

Every operationalization above is silently parameterized by a reference class. Named positions:

- **(a) Relative to the creator.** Boden's **P-creativity** (psychological) — novel to the individual mind that produced it, regardless of history. Boden, M. A. (1990/2004), *The Creative Mind: Myths and Mechanisms*. This is what educational creativity assessment mostly measures.
- **(b) Relative to the judging population.** Uniqueness scoring (§1) operationalizes exactly this — and makes it explicit that the score is a property of the *sample*, not the response. Also the implicit reference for the Consensual Assessment Technique (Amabile, 1982, *Social psychology of creativity: A consensual assessment technique*, JPSP 43(5), 997–1013), where creativity is by definition what appropriate domain judges agree it is.
- **(c) Relative to an explicit corpus.** Ritchie's **inspiring set** I (§5) is the cleanest formalization: the criteria are stated over `R − I` (results minus inspiring set), so "novel" means literally "not in the set we gave the program." Grace/Maher's clustering-defined artifact class and any embedding-distance-to-corpus method are the same move with a metric attached.
- **(d) Relative to all of human history.** Boden's **H-creativity** (historical). Also Kaufman & Beghetto's (2009) Four-C model — mini-c, little-c, Pro-c, Big-C — which stratifies (a) through (d) as a developmental ladder. Kaufman, J. C., & Beghetto, R. A. (2009). *Beyond big and little: The four C model of creativity.* Review of General Psychology 13(1), 1–12.

**How much do scores move when the reference class changes?** The strongest available evidence is indirect but decisive:
- Uniqueness scoring is definitionally sample-dependent; Plucker, Qian & Wang (2011) documents divergence across scoring techniques on identical response sets.
- Organisciak et al. (2023) shows the reference-class problem empirically in a different guise: **held-out prompts** (i.e., a shifted reference class for the same model) drop the best fine-tuned model from r = .81 to **r = .63**, and drop baselines from .26 to .14–.28. Generalization across reference classes costs roughly .18 in correlation for the best method.
- Semantic-distance scores are entirely determined by the corpus the embedding was trained on; SemDis's five-model latent-factor design exists because single-corpus scores disagree.

**UNANSWERED:** I did not locate a study that holds the artifact set fixed and systematically varies the reference corpus (e.g., scoring the same responses against a 100-person vs. 10,000-person norm sample, or against Wikipedia vs. a domain corpus) and reports the resulting rank-order change. This is a real gap; if such a study exists I did not verify it.

---

## 5. Computational-creativity evaluation frameworks — number or checklist?

**Ritchie (2007).** Ritchie, G. D. *Some empirical criteria for attributing creativity to a computer program.* Minds and Machines 17(1), 67–99. doi:10.1007/s11023-007-9066-2. Formal apparatus: a basic item set B, two rating mappings **typ** (typicality — is it a member of the intended genre?) and **val** (quality/value — is it a good one?), an **inspiring set** I ⊆ B (the artifacts that informed the program's construction), and a result set R. Deliberately keeps novelty *out* as a primitive, deriving it from typicality/value and set membership. Verified from the text: **18 criteria** (with revisions 8a and 10a for formal defects — unbounded ratios and division by zero), each of the form `AV(typ, R) > θ` or `ratio(X, Y) > θ`. Examples verified verbatim: Criterion 1 `AV(typ, R) > θ`; Criterion 9 `ratio(I ∩ R, I) > θ` (replication of the inspiring set); Criterion 10a `(1 − ratio(I ∩ R, R)) > θ`; Criteria 11–18 all operate on `R − I`, the novel results — e.g., Criterion 18 `ratio(V_{c,1}(R−I) ∩ T_{0,b}(R−I), (R−I)) > θ` = high-value, *untypical*, novel items as a proportion of novel items, which is as close as the framework comes to a pure originality measure. Four free thresholds: a (high typicality), b (untypicality limit), c (good quality), θ (comparison level).

→ **Produces numbers per criterion, but NOT a single score.** It is a vector of 18 threshold tests with four analyst-chosen parameters and no combination rule. In practice a checklist with quantitative cells.

**Colton's creative tripod** (Colton, 2008, *Creativity versus the perception of creativity in computational systems*, AAAI Spring Symposium). Requires the software to display **skill, appreciation, and imagination**; creativity is attributed only if all three legs are perceived. → **Pure checklist, no number.** Explicitly a theory of *perceived* creativity, not a measurement instrument.

**FACE and IDEA.** Colton, S., Charnley, J., & Pease, A. (2011). *Computational creativity theory: The FACE and IDEA descriptive models.* Proc. ICCC 2011, Mexico City, 90–95. Verified from the proceedings PDF:
- **FACE** describes a creative act as a tuple ⟨F_g, A_g, C_g, E_g⟩ of generative acts: **F** = framing information (natural-language justification/context), **A** = aesthetic measure generation, **C** = concept generation, **E** = expression-of-concept generation. Superscript p vs. g distinguishes acts by the programmer vs. by the software. The paper states FACE "could be used in a quantitative way" (counting/comparing tuple richness, e.g. `⟨C_g, E_g⟩ < ⟨A_g, C_g, E_g⟩`), a **comparative** way (an agreed ordering such as `⟨C_p⟩ > ⟨A_g⟩ > ⟨C_g⟩ > ⟨E_g⟩`), a **process-based** way, and a **qualitative** way. It does define real aggregate measures over a session S: `average(S) = (1/n)Σ a_g(c_gi, e_gi)`, `best_ever(S)`, `worst_ever(S)`, and `precision(S) = (1/n)|{(c_gi,e_gi) : a_g(...) > t}|`.
- **IDEA** models *impact*. Two audience ratings per member m: **well-being** `wb_m(A) ∈ [−1, +1]` and **cognitive effort** `ce_m(A) ∈ [0, 1]`. Derived measures, all in [0,1] (verbatim from the paper): `disgust = (1/2n)Σ(1 − wb_i)`; `divisiveness = (1/n)Σ|wb_i − m(A)|`; `indifference = 1 − (1/n)Σ|wb_i|`; `popularity = (1/2n)Σ(1 + wb_i)`; `provocation = (1/n)Σ ce_i`. Compounds: `acquired_taste = (pop + prov)/2`; `instant_appeal = (1 + pop − prov)/2`; `opinion_splitting = (1 + div − prov)/2`; `opinion_forming = (div + prov)/2`; `shock = (1 + dis − prov)/2`; `subversion = (dis + prov)/2`; `triviality = (1 + ind − prov)/2`. IDEA also defines a distance measure d over creative acts with thresholds l (too similar) and u (incomparable), yielding six **software development stages**: developmental → fine-tuned → re-invention → **discovery** → **disruption** → disorientation, with the claim that only discovery and disruption have impact.

→ **FACE is descriptive/comparative with optional aggregate numbers over sessions; IDEA produces genuine numbers in [0,1] — but they measure audience reaction (impact), not novelty.** Note that IDEA's stage model is the sharpest statement in the literature of *novelty having an upper bound*: past threshold u, an artifact is not creative, it is uninterpretable ("disorientation"). Neither FACE nor IDEA outputs a single creativity score.

**SPECS.** Jordanous, A. (2012). *A Standardised Procedure for Evaluating Creative Systems: Computational creativity evaluation based on what it is to be creative.* Cognitive Computation 4(3), 246–279. doi:10.1007/s12559-012-9156-1. Three steps: (1) state what creativity means for *this* system, (2) derive tests/standards from that statement, (3) test the system against them. The **fourteen key components of creativity** (corpus-derived from academic papers on creativity): active involvement and persistence; dealing with uncertainty; domain competence; general intellect; generation of results; independence and freedom; intention and emotional involvement; **originality**; progression and development; social interaction and communication; spontaneity/subconscious processing; thinking and evaluation; **value**; variety/divergence/experimentation. Users need not use all fourteen and may add domain-specific components.

→ **A meta-procedure, not a metric.** It tells you to define your own numbers and is explicitly agnostic about them. Originality and value appear as two of fourteen named components with no operationalization supplied.

**Plain summary:**

| Framework | Output |
|---|---|
| Frequency/uniqueness scoring | Single number, sample-relative |
| Semantic distance (SemDis, OCS) | Single continuous number per response |
| LLM scoring (Ocsai) | Single number, calibrated to human rating scale (1–5) |
| Bayesian surprise (KL) | Single number, requires an explicit generative model |
| Maher / Grace novelty-value-surprise | Three numbers, no combination rule specified |
| Ritchie (2007) | 18 threshold tests, 4 free parameters, no aggregate |
| Colton tripod | Checklist (3 items), no number |
| FACE | Descriptive tuple + optional session aggregates |
| IDEA | 12 numbers in [0,1] — measures impact, not novelty |
| SPECS | Procedure only; 14 named components, no metric |

---

## FIT/COST verdict: picking a novelty operationalization for blind LLM grading of artifacts

Assumed setting: LLM graders score artifacts blind; a system designer must choose one operationalization to instrument.

### Ranked recommendation

**1. LLM judgment with few-shot anchors and human-calibrated rubric — recommended primary.**
- *Empirical support:* GPT-4 zero-shot r = .53; 5-shot r = .66; 20-shot r = .70; **fine-tuned r = .76–.81** against a human ceiling of r = .83–.88 (Organisciak et al. 2023). No other automated method is in this range.
- *Cost:* Low if you already run LLM graders. Fine-tuning needs labels — but the paper shows **1% of their training data already beat semantic-distance baselines**, and 5% of data (804 labels) gave r = .61/.55 on smaller models. A few hundred human-labeled anchors is a realistic budget.
- *Known failure modes:* (i) **Prompt-engineering sensitivity** — the .13 → .53 jump from GPT-3 to GPT-4 zero-shot and the .53 → .70 few-shot jump both show the score is a function of scaffolding, not just of the artifact; freeze the prompt and version it. (ii) **Fine-tuned models learn rater-group idiosyncrasy**, not just the construct — Organisciak et al. explicitly flag this. (iii) **Explanations are unreliable** — the paper warns LLM chain-of-thought rationales can be hallucinated post-hoc and are not evidence of how the score was reached. (iv) **The elaboration confound is untested here**: the authors state as an open question whether the elaboration bias that afflicts semantic models "still remains in LLM scoring, and to what degree." Treat as **UNANSWERED** and instrument it yourself by regressing scores on response length.
- *Theory commitment:* originality is whatever a calibrated panel of human judges would say it is (a CAT-style consensual definition), which quietly imports the judging population as the reference class.

**2. Explicit-corpus distance (embedding novelty vs. a declared reference set) — recommended as a secondary, diagnostic channel, never as the primary score.**
- *Empirical support:* weak as a standalone predictor of human judgment (r = .12–.26 pooled; .19–.31 per-prompt).
- *Value it does add:* it is the only operationalization that is **auditable and reference-class-explicit**. You can state exactly what corpus "novel" is relative to, swap it, and see the score move. Run it as a **duplicate/near-duplicate detector and a corpus-drift monitor**, not as an originality grade.
- *Failure modes:* elaboration/length bias (Forthmann et al. 2019 — mitigate with stop-word removal and length as a covariate); embedding-model and preprocessing dependence (use an ensemble/latent factor per Beaty & Johnson 2021, not one model); **measures novelty only, blind to appropriateness**; high scores on nonsense.

**3. Predictive surprise (KL prior→posterior, or residual against an expectation model) — recommended only where you genuinely have a sequence and a model.**
- *Fit:* This is the correct formalization if what you actually care about is "did this artifact change what we expect next," e.g. scoring successive generations, tracking whether a system is exploring, or detecting trend violations. Itti & Baldi (2009) for the KL form; Grace et al. (2015) for the trend-residual form.
- *Cost:* **High.** Requires an explicit generative/predictive model over the artifact space, a specified observer with history, and a design decision about what "prior" means for a stateless blind grader. For one-shot blind grading of independent artifacts there is *no prior to update*, so Bayesian surprise is undefined without inventing one.
- *Failure mode:* the surprise number is a property of the model you built, not of the artifact — model misspecification is indistinguishable from artifact novelty.
- *When it wins:* IDEA's stage model and Grace et al. both point at the same asset — surprise, unlike distance, has a natural *upper* bound where the artifact stops being interpretable. If you need to distinguish "boldly novel" from "incoherent," a predictive model gives you that; distance never will.

**4. Frequency / uniqueness scoring — do not use as the primary operationalization.**
- *Failure modes, all documented:* fluency confound reducing partialled reliabilities to near zero (Hocevar 1979; Runco & Albert 1985); sample-size dependence making scores non-comparable across runs; no separation of novelty from nonsense; scoring-technique-dependent rank orders (Plucker, Qian & Wang 2011).
- *Where it is still right:* as a **cheap deduplication and mode-collapse detector** across a batch of generations. "How many of the 200 artifacts are the same idea" is a legitimate and well-posed frequency question. It is the *individual* originality score that frequency scoring cannot support.

### Design guidance that falls out of the above

- **Score novelty and appropriateness separately and never fuse them into one number.** Every operationalization in §1–§3 that produces a single number measures novelty *only*; Beaty & Johnson's own finding that semantic distance "primarily is a measure of novelty" and the persistence of the bizarreness problem from Guilford through embeddings are the same defect recurring for sixty years. Maher (2010), Grace et al. (2015), and Ritchie (2007) all keep the axes separate; none specifies a combination rule, and that reticence is a finding, not an oversight.
- **Declare the reference class in the schema.** Ritchie's inspiring set I is the right abstraction: make it a first-class, versioned artifact of the system, so `R − I` is computable and the meaning of "novel" is inspectable. Expect ~.18 correlation loss when generalizing across reference classes (held-out-prompt result, Organisciak et al.).
- **Budget for human anchors.** With r ≈ .83–.88 as the human-human ceiling, a grader at r = .70 (GPT-4 20-shot, zero training labels) is already ~80% of the way to the ceiling; a few hundred labels buys the rest. Do not spend engineering effort on semantic-distance tuning to climb from .12 to .26.
- **Instrument the length confound from day one.** It is confirmed for LSA (Forthmann et al. 2019), debated in its treatment (term weighting vs. multiplicative composition), and explicitly unmeasured for LLM graders.

### Marked UNANSWERED
- Whether LLM graders inherit the elaboration/length bias, and at what magnitude relative to human judges (open question stated by Organisciak et al. 2023).
- A controlled study varying reference corpus while holding artifacts fixed, reporting rank-order change (§4).
- Any published head-to-head of predictive-surprise metrics (Grace/Maher, Itti-Baldi) against human creativity judgments on the same corpora used for the semantic-distance/LLM benchmarks. I found no such comparison; the surprise literature and the DT-scoring literature do not share a benchmark.
- I did not retrieve the primary text of Silvia et al. (2008) or Plucker, Qian & Wang (2011) in this session (session search limit); their content above is from secondary description and should be verified before being quoted as primary evidence.

**Sources:**
- [Beaty & Johnson 2021, SemDis, Behavior Research Methods](https://link.springer.com/article/10.3758/s13428-020-01453-w)
- [Organisciak et al. 2023, Beyond Semantic Distance (ScienceDirect)](https://www.sciencedirect.com/science/article/abs/pii/S1871187123001256) · [open-access PDF, ERIC ED629879](https://files.eric.ed.gov/fulltext/ED629879.pdf) · [Ocsai tool](https://openscoring.du.edu/scoringllm) · [code](https://github.com/massivetexts/llm_aut_study)
- [Hocevar 1979 (ERIC EJ203979)](https://eric.ed.gov/?id=EJ203979) · [PsycNet record](https://psycnet.apa.org/record/1979-27665-001)
- [Runco & Albert 1985, Educ. & Psych. Measurement](https://journals.sagepub.com/doi/10.1177/001316448504500306)
- [Forthmann et al. 2019, JOCB](https://onlinelibrary.wiley.com/doi/abs/10.1002/jocb.240)
- [Itti & Baldi 2009, Vision Research (PDF)](http://ilab.usc.edu/publications/doc/Itti_Baldi09vr.pdf) · [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC2782645/)
- [Maher 2010, DESIRE (ACM DL)](https://dl.acm.org/doi/10.5555/1854969.1854977) · [Semantic Scholar](https://www.semanticscholar.org/paper/7ba12aad06d79d2051cf885d91e6f88adfbbefda)
- [Ritchie 2007, Minds and Machines (Springer)](https://link.springer.com/article/10.1007/s11023-007-9066-2) · [free PDF](https://users.dcc.uchile.cl/~nbaloian/CapstonePreparation/readings/Creativity.pdf)
- [Colton, Charnley & Pease 2011, FACE/IDEA (ICCC proceedings PDF)](https://computationalcreativity.net/iccc2011/proceedings/the_foundational/colton_1_iccc11.pdf)
- [Jordanous 2012, SPECS, Cognitive Computation](https://link.springer.com/article/10.1007/s12559-012-9156-1) · [open-access PDF](https://kar.kent.ac.uk/42379/1/evaluatingComputationalCreativity2.pdf)
- [Maher 2012, Computational and Collective Creativity, ICCC (PDF)](https://computationalcreativity.net/iccc2012/wp-content/uploads/2012/05/067-Maher.pdf)
