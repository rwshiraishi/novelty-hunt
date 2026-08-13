Research complete. Delivering the report.

---

# Ideation & Creativity Measurement in Engineering Design: Formulas, Validity, and Automation Fit

## 1. The canonical framework: Shah, Vargas-Hernandez & Smith (SVS, 2003)

**Citation:** Shah, J.J., Vargas-Hernandez, N., Smith, S.M. (2003). "Metrics for measuring ideation effectiveness." *Design Studies* 24(2), 111-134. doi:10.1016/S0142-694X(02)00034-0. Precursor: Shah, Kulkarni & Vargas-Hernandez (2000), *J. Mech. Des.* 122(4), 377-384.

The framework proposes four outcome metrics answering two questions: does the ideation method **explore** the design space (novelty, variety) and does it **expand** it (quantity, quality)? Two experimental strategies are offered: a *direct* method treating the idea as a whole, and an *indirect* method decomposing the idea into components and studying their individual and interactive effects. The indirect/decomposed route is what everything below rests on.

### 1.1 Novelty

Overall novelty of one idea:

```
M = Σ_{j=1..n} p_j · Σ_{i=1..m} f_i · S_ij
```

where `m` = number of functions/attributes, `n` = number of design stages, `f_i` = importance weight of attribute *i*, `p_j` = importance weight of stage *j*, and `S_ij` = the "unusualness/unexpectedness" score of the solution implementing attribute *i* at stage *j*. Weights are normalized so the composite lands on a 0-10 scale.

`S_ij` is obtained one of two ways:

- **A priori (historical novelty):** judges score against a subjectively defined universe of existing products, per attribute.
- **A posteriori (psychological novelty / uncommonness), the formula almost everyone actually uses:**

```
S_ij = [ (T_ij − C_ij) / T_ij ] × 10
```

`T_ij` = total number of solutions generated for attribute *i* at stage *j*; `C_ij` = count of the current solution among them. This is **pure within-sample infrequency**. It is not a measure of novelty in any absolute sense, and Shah et al. say so: it gives "presence and chance of novelty," not novelty.

### 1.2 Variety (genealogy tree)

```
V = Σ_{j=1..m} f_j · [ Σ_{k=1..4} S_k · b_k ] / N
```

`f_j` = importance weight of function *j*; `S_k` = score for hierarchical level *k*; `b_k` = number of branches at level *k*; `N` = number of ideas in the set. The four levels of the genealogy tree are **physical principle, working principle, embodiment, detail**, weighted by descending scores. Reported weightings differ across sources: SVS's original is commonly cited as 10/6/3/1, while the Nelson-refined convention (and the Penn State reference implementation) uses **10/5/2/1**. This discrepancy is itself a symptom of the reporting problem documented in §1.5.

Variety is a **set-level** quantity only. An individual idea has no variety score.

### 1.3 Quality

```
Q = Σ_j Σ_k f_j · p_k · S_jk / (normalizer)
```

`S_jk` = quality score for function *j* at stage *k*, denominator normalizes to 0-10. Quality is defined as "the measure of the feasibility of an idea and how close it comes to meeting the design specifications" — a weighted-objectives / Pugh-style scoring, not a creativity construct. In practice most implementations collapse this into a 4-point qualitative rubric per function.

### 1.4 Quantity

Raw count of ideas per person/team per unit time. No deduplication in the original.

### 1.5 What the literature actually did with it — and why you should discount most of it

**The definitive audit:** Fiorineschi & Rotini (2023), "Uses of the novelty metrics proposed by Shah et al.: what emerges from the literature?", *Design Science* 9, e11. They took all 739 Scopus citations of SVS (as of March 2021), analyzed the 686 retrievable ones, and found:

| Finding | Value |
|---|---|
| Papers actually using the original SVS novelty metric | **61 (~9%)** |
| Papers using a modified/adapted version, usually with no stated justification | 72 |
| Of the 61: a posteriori | 63.9% |
| Of the 61: a priori | 8.2% |
| Of the 61: both | 3.3% |
| Of the 61: **insufficient information to tell which** | 24.6% |
| Of the 61: reported any interrater reliability test | **18 (29.5%)** |
| Papers reporting enough to replicate the assessment (idea sets, attribute identification, weights, handling of missing attributes) | **0** |
| Papers explicitly specifying historical vs psychological novelty | 3 |
| Papers exploiting the multi-design-stage capability of Eq. 1 | 0 |
| Industrial (non-academic) applications | 0 |

In several years (2006, 2007, 2011, 2014, 2020) not a single reviewed paper applied an IRR check. **This is the single most important fact in this literature for your purposes: the canonical engineering-design creativity metric has a twenty-year citation record and a near-empty validation record.** The metric's fame is citation fame, not measurement fame.

### 1.6 Documented failure modes of the SVS novelty metric

- **Within-sample relativity.** A posteriori novelty is defined only against the generated pool. Add or remove ideas and every score changes. Srivathsavai et al. (2010): it cannot assess against existing products at all. Sluis-Thiescheffer et al. (2016): it can return misleadingly high novelty even when similar solutions recur frequently, because the score depends on the partition of attributes, not on semantic distance.
- **Undefined attribute space.** SVS never defined what counts as an "attribute" or "function"; it is problem-dependent and rater-dependent. Brown (2014) identifies this plus weight subjectivity plus difficulty separating ideas by design stage as the core practical failures.
- **Heterogeneous attribute sets ("missing"/"extra" attributes).** When ideas implement different numbers of attributes, `T_ij` differs per attribute and the composite is not comparable across ideas. Fiorineschi et al. (2020a, 2021) call this unsolved in the original and propose a fix (Fiorineschi, Frillici & Rotini, "Refined metric for a-posteriori novelty assessments," *Journal of Engineering Design*, 2021, doi:10.1080/09544828.2021.1976397). **UNANSWERED: I could not retrieve the exact refined formula — Taylor & Francis returned 403 on both the 2021 JED paper and the 2018 *Journal of Design Creativity and Innovation* paper.**
- **Abstraction-level floor.** SVS and Vargas-Hernandez variety both require all ideas be described at the lowest (embodiment/detail) level, excluding ideas expressed at higher abstraction. Peeters et al. (2010) built a variant (used with the PAnDA design-by-analogy tool) scoring higher novelty when the difference appears at higher abstraction levels, using three hierarchical levels and Nelson's standardized weights. Because it preserves SVS's frequency logic it inherits the partial-representation problem. **UNANSWERED: exact Peeters formula not retrieved.**
- **Unbalanced trees.** Verhaegen et al. show both SVS and Nelson variety break when the genealogy tree is unbalanced.

### 1.7 Nelson et al. (2009) refinement

Nelson, B.A., Wilson, J.O., Rosen, D., Yen, J. (2009). "Refined metrics for measuring ideation effectiveness." *Design Studies* 30(6), 737-743.

Fixes two specific variety flaws:
1. **Double counting.** SVS counted the number of *design ideas* at each level rather than the number of *differentiations*. Nelson's version counts `d_l`, the number of differentiations at node *l*.
2. **Improper normalization.** Dividing by `N` penalizes larger sets. Nelson removes the normalization, on the argument that a non-normalized score better represents the actual design space explored.

Level scores `S_k` = 10, 5, 2, 1. Nelson also proposes a single composite metric for design-space-exploration quality so ideation methods can be compared on one number. **UNANSWERED: I could not retrieve the exact refined variety equation (ScienceDirect paywall); the Penn State reference page renders equations as images.**

### 1.8 Aggregation: the axiom-based repair

Sorensen et al., "Axiom-based aggregation functions for calculating variety, novelty, quality and quantity of ideation results," *Design Science* (2026). This is the most rigorous treatment of the aggregation problem and it is worth your attention because the pathologies it names are exactly the ones an automated grader would hit.

**Pathologies proven with SVS's own published numbers:**
- Averaging quality or novelty **rewards idea filtering**. A team that discards its weak ideas scores higher than one that keeps them, even though the weak ideas did not reduce design-space exploration. As they put it, low-novelty ideas "do not decrease the exploration of the design space; they just do not increase the exploration."
- SVS variety divided by idea count: removing a detail-level idea *raised* the score from 6.54 to 7.1.
- SVS quality (weighted average): removing a low-quality idea *raised* the score from 5.37 to 6.05.
- Quantity as raw count inflates on near-duplicates.
- SVS provides **no set-level novelty aggregation at all**.

**Their axioms** (abbreviated):
- *Variety:* any unique idea at any level increases total variety; higher-abstraction ideas add more than lower-level ones; ideas in large families add less than ideas in small families.
- *Novelty:* higher idea novelty adds more at a given tree location; novel ideas at higher levels add more than equally novel ideas at lower levels; small families contribute more; adding a novel idea increases set novelty, zero-novelty ideas leave it unchanged, removal never increases it.
- *Quantity:* duplicates not recounted; level-independent; evaluator-added organizational nodes do not count.
- *Quality:* high-quality ideas add more than moderate; adding/removing low-quality ideas never lowers/raises set quality; every added high-quality idea increases set quality regardless of how many already exist.

**Their construction.** Ideas are placed in an **OPED tree** (objective / principle / embodiment / detail). Each idea at level *l* explores design space `Ω_l = α^(1−l)` with `Ω_1 = 1`, `α ∈ (0,1)` the asymptotic minimum space fraction.

```
member fraction:        f_i = α + (1−α)·β^(i−1)
total family fraction:  f_t = mα + (1−α)(β^m − 1)/(β − 1)
element variety:        V_E = f_a·Ω_l = [ mα + (1−α)(β^m − 1)/(β − 1) ] / ( α^(l−1) · m )
branch variety:         V_B = V_E + Σ V_B(children)
total variety:          Σ over top-level branches
```

Chosen parameters: **α = 0.46, β = 0.32**. Element novelty is the product of idea novelty and the idea's explored design-space fraction, summed over the set; ideas are ordered by *decreasing* novelty for the novelty roll-up (arbitrary order for variety, since variety is order-invariant). Idea novelty itself is scored by scarcity in the current context `S_c` and in the foreign context of origin `S_f`, combined into composite points `P`. Quality uses a threshold `q_th` to separate low-quality ideas; quantity counts unique ideas `U` via an uniqueness measure `u`.

**Validation is mathematical proof against the axioms, not empirical.** They prove TV-1 (all element varieties positive, total is a sum), TV-2 (min space at level *l*+1 equals max at level *l*, strict for finite trees), TV-3 (`ΔV = (1−α)(β^{m2} − β^{m1})·Ω_l > 0` when `m2 < m1`), and note the axioms hold for any valid α, β though numeric scores shift. **There is no reported human-agreement study.** Note the parameter values α=0.46, β=0.32 appear to be design choices, not measured constants; treat them as EXTRAPOLATED unless you can find their derivation.

---

## 2. Alternative design-creativity metrics

### 2.1 Sarkar & Chakrabarti (2011): creativity = novelty × usefulness

**Citation:** Sarkar, P. & Chakrabarti, A. (2011). "Assessing design creativity." *Design Studies* 32(4), 348-383. doi:10.1016/j.destud.2011.01.002.

**Novelty** is assessed structurally, not by frequency. Products are modeled with FBS, and the *degree* of novelty is read off the **SAPPhIRE** causal hierarchy (State change, Action, Parts, Phenomena, Inputs, oRgans, Effects). The rule: a product has "very high" novelty if it fulfills a function no existing product fulfills; lower levels of the hierarchy give lower novelty. Qualitative bands convert to points:

```
Very high = 4, High = 3, Medium = 2, Low = 1
```

Ties within a band are broken by "a larger number of differences at higher levels of the SAPPhIRE hierarchy."

**Usefulness** (Eq. 1) is the one genuinely unusual formula in this literature. It refuses to score usefulness by inspection and instead scores it by *actual use*:

```
U = L × R × F × D
```

- `L` = level of importance (5 bands, e.g. 2.5/5 for shoes vs 2.1/5 for slippers)
- `R` = rate of popularity of use = users / potential users
- `F × D` = rate of use = hours of use / hours available

Worked example from the paper: leather shoes `U = (2.5/5)(210/1000)(5/24) = 0.0218`; leather slippers `U = (2.1/5)(332/1000)(9/24) = 0.0523`, ratio 1 : 2.4.

**Creativity** (Eq. 2):

```
C = N × U
```

Multiplicative "to embody the notion that absence of either will lead to a lack of creativity." Both factors are first converted to relative grades (rank *k* of *n* → *k/n*) before multiplying, so C is explicitly a **relative, within-set** score.

**Validation — and it is the best in this literature.** Three product sets, ranked by two independent teams, benchmarked against the collective intuitive judgment of experienced designers, compared by Spearman rank correlation:

| Method vs. experienced designers | Set 1 | Set 2 | Set 3 | Avg |
|---|---|---|---|---|
| Shah's method (novelty) | 0.8 | 0.667 | 0.5 | 0.656 |
| Chakrabarti & Khadilkar | 0.316 | 0.820 | 1.0 | — |
| **Proposed (SAPPhIRE) method** | **0.8** | **1.0** | **1.0** | **~0.93** |

For usefulness, the proposed method beat the weighted-objectives method against designer intuition (e.g. 0.8/0.8/0.9 vs 0.4/0.8/0.6). Significance thresholds reported in the paper: p<0.1 for r>0.62, p<0.05 for 0.63-0.70, p<0.02 for 0.71-0.79, p<0.01 for r>0.83. **Sample sizes are tiny (product sets of 4-5, two teams), so these correlations are suggestive rather than conclusive.**

**Refinements:** a 2016 DESIGN conference paper identified deficiencies and proposed four modifications; a 2018 *Journal of Design Creativity and Innovation* paper reports a refined novelty method in closer agreement with experienced designers than the 2011 original. **UNANSWERED: exact refinements not retrieved (T&F 403).**

### 2.2 Oman, Tumer, Wood & Seepersad (2013): CCA and MPCA

**Citation:** *Research in Engineering Design* 24(1), 65-92. doi:10.1007/s00163-012-0138-9.

Two methods, both built as modifications of SVS: **Comparative Creativity Assessment (CCA)** and **Multi-Point Creativity Assessment (MPCA)**. The paper's more durable contribution is a critical survey comparing person-, product-, and process-oriented creativity assessments and validating against in-class design projects. Follow-up: Oman, Tumer, Stone & Doolen, "Reducing the Subjectivity in the Comparative Creativity Assessment Method by Including Function Combinations." **UNANSWERED: exact CCA/MPCA formulas and reliability coefficients not retrieved (paywall).** Given the Fiorineschi audit's base rate, assume validation is thin until proven otherwise.

### 2.3 Besemer & O'Quin: CPAM / Creative Product Semantic Scale (CPSS)

**Citations:** Besemer & Treffinger (1981) CPAM model; Besemer & O'Quin (1986) CPSS; O'Quin & Besemer (1989), "The development, reliability, and validity of the revised creative product semantic scale," *Creativity Research Journal* 2(4); Besemer (1998) *CRJ* 11(4), 333-346; Besemer & O'Quin (1999) *CRJ* 12(4), 287-296; O'Quin & Besemer (2006), *Creativity and Innovation Management* 15(1).

**Structure:** 55 items, 7-point **bipolar semantic differential** (adjective pairs), 11 subscales nesting under 3 dimensions:

- **Novelty** — originality and newness in materials, ideas, processes, concepts
- **Resolution** — how well the product does what it was made for
- **Style** (originally "Elaboration and Synthesis," renamed because the original label confused raters) — presentation, appearance, aesthetic quality, complexity

**Validity evidence, honestly reported by the authors themselves:**
- Factor analysis across four subject groups extracted 11 factors and supported the Elaboration & Synthesis dimension, but it proved hard to isolate from the others.
- **Novelty validity supported.** Study 1 detected differences in novelty among three commercial household products varying in unusualness. Study 2 showed naive judges' novelty ratings were consistent with experts'.
- **Resolution validity NOT established.** No significant differences among products were observed. This is a 35-year-old unresolved gap in the most-used product-creativity scale.
- Internal consistency of subscales "generally good" (specific coefficients UNANSWERED — I could not retrieve the per-subscale alphas).

**The finding most relevant to you:** O'Quin & Besemer (2006) report that **novelty is a poor predictor of stated willingness to buy**; perceived *Value* and demonstrated *Elegance* in Style are much stronger. If your grading system's downstream purpose is "will this artifact be adopted/used," novelty is close to the wrong signal to weight heavily.

A 15-item advertising adaptation exists retaining the three dimensions.

### 2.4 Cropley & Cropley: Creative Solution Diagnosis Scale (CSDS)

**Citations:** Cropley, D.H. & Kaufman, J.C. (2012), "Measuring Functional Creativity: Non-Expert Raters and the Creative Solution Diagnosis Scale," *Journal of Creative Behavior* 46(2), doi:10.1002/jocb.9. Cropley, Kaufman & Cropley (2011), "Measuring Creativity for Innovation Management," *J. Technology Management & Innovation* 6(3), doi:10.4067/S0718-27242011000300002.

**Original theoretical model:** 30 items on four a priori criteria — **Relevance & Effectiveness, Novelty, Elegance, Genesis**. Five-point Likert, max total 100.

**Empirical revision 1 (Cropley & Kaufman 2012):** non-expert judges rated a series of mousetrap designs; CFA produced a simple structure closely matching the a priori model and a **revised 24-item CSDS**. Non-experts used it with high reliability and internal consistency. The stated purpose is to substitute cheap non-expert raters for expensive experts in a CAT-style protocol.

**Empirical revision 2 (Cropley, Kaufman & Cropley 2011):** 323 college students rating the same five mousetraps produced a **27-item, five-factor** solution: **Relevance & Effectiveness, Problematization, Propulsion, Elegance, Genesis**. The theoretically-unitary *Novelty* construct **split into two empirically distinct factors** — Problematization (does the solution reframe/expose the problem) and Propulsion (Sternberg's model: does it replicate, redefine, forward-increment, redirect, reinitiate the field). Three original indicators were dropped as redundant. Both studies converged on the same five-factor solution.

That split is the most operationally interesting result in this section: **novelty is not one thing even inside a validated instrument.** Two raters can both be "right" about novelty while scoring different constructs.

**UNANSWERED: exact per-factor Cronbach alphas, CFA fit indices, factor loadings, and the stepwise-regression betas predicting Overall Creativity.** Both the SciELO and JOTMI full texts returned 403/abstract-only. The 2011 paper reports rater consistency "uniformly high" and "in the excellent range (Nunnally & Bernstein, 1994)" across individual stimuli and the whole dataset, which is a verbal claim I could not resolve to numbers.

---

## 3. The novelty-vs-quality tension: how design research actually handles it

**Answer: mostly independently, and the field has evidence that this is not a safe choice.**

**Three aggregation regimes exist in the literature:**

1. **Independent scoring, reported separately (SVS, Nelson, Oman, and the overwhelming majority of empirical papers).** Four numbers, no combination rule. Validated only in the weak sense that each is defined; nothing validates the *absence* of a combination rule.
2. **Multiplicative (Sarkar & Chakrabarti, `C = N × U`).** Justified by the argument that either factor at zero should zero out creativity. This is the only combination rule in the design literature with reported human-agreement validation (Spearman ~0.93 avg against experienced designers, small samples).
3. **Axiom-constrained set-level roll-up (Sorensen et al.).** Handles the *within-metric* aggregation problem (how do ideas combine into a set score) rather than the *across-metric* problem. Proven against axioms, not against humans.

**The empirical result that should govern your design.** Miller, Hunter, Starkey, Ramachandran, Ahmed & Fuge (2021), "How Should We Measure Creativity in Engineering Design? A Comparison Between Social Science and Engineering Approaches," *J. Mech. Des.* 143(3), 031404, doi:10.1115/1.4049061 (conference version DETC2020-18968). **N = 932 engineering design ideas**, rated by both CAT experts and SVS metrics:

| Relationship | r | Note |
|---|---|---|
| Expert (CAT) novelty vs **SVS novelty** | **−.10 to −.11** | p = 0.002. Significant and **negative** |
| Expert novelty vs Johnson et al. extended SVS novelty | +.14 | p < .001, small |
| Expert **quality** vs SVS quality | **+.31** | p < .001, medium |
| Expert novelty vs expert quality | −.29 | novelty and quality trade off *within* expert judgment |
| Expert novelty vs quasi-expert novelty | +.74 | p < .001 |
| Expert quality vs quasi-expert quality | +.50 | p < .001 |

Interrater reliability (ICC2, in brackets in their Table 2):

| Measure | ICC2 |
|---|---|
| Expert novelty | .71 |
| Expert quality | .75 |
| SVS novelty | .85 |
| SVS quality | **.62** (below the .70 threshold) |
| Quasi-expert novelty | .78 |
| Quasi-expert quality | **.56** (below threshold) |
| Johnson et al. novelty | .85 |

Coding IRR for the genealogy-tree construction itself: Cohen's κ = 0.88.

**Three conclusions from this paper you should treat as load-bearing:**
1. **CAT novelty and SVS novelty measure different constructs, and are negatively related.** Studies using one cannot be compared to studies using the other. Their qualitative concept-mapping study traced this to raters and the SVS decomposition placing weight on different design features.
2. **Quality is where structured decomposition helps.** The 4-point qualitative SVS quality rubric guided raters toward the same construct experts used (r = .31), while both quality ICCs on the human side fell below threshold. Structured rubrics rescue the noisiest human dimension.
3. **Trained novices are an acceptable proxy for experts on novelty (r = .74, ICC2 .78) but not on quality (r = .50, ICC2 .56).** Prior CAT work shows the same pattern by domain complexity: expert-novice correlation r = .22 on poems (Kaufman et al. 2008) vs r = .89 on short stories (Kaufman et al.). The gap widens with product complexity.

Also cited in that paper: applying different creativity metrics to the same design problem can produce rankings that are "not only vastly different, but often negatively correlated." **Metric choice is a confound in this literature, not a detail.**

---

## 4. Brainstorming / group ideation: scoring protocols and metric-dependent findings

**The headline claim:** Osborn's two principles were "defer judgment" and "reach for quantity," resting on the assumption that **quantity breeds quality** — decomposed into (a) higher total output means higher absolute output of creative ideas, and (b) a larger pool of high-quality ideas raises the chance that a creative one is selected.

**The critique:** Diehl & Stroebe reviewed 22 studies of group brainstorming; **18 of 22** found real interacting groups produce fewer ideas than nominal groups (individuals working separately whose non-redundant output is pooled). Three mechanisms were proposed — free riding, evaluation apprehension, and **production blocking** — and Diehl & Stroebe (1991) identified production blocking as the dominant cause: only one person can speak at a time, and ideas held in working memory while waiting are forgotten. This directly contradicts Osborn's premise that hearing others' ideas stimulates new ones.

**Why this is a metric story, not just a psychology story.** The nominal-group comparison *requires* a redundancy rule: pooled individual output must be deduplicated before it can be compared to group output, and the deduplication threshold determines the effect size. The literature's counter-critique lands exactly here — Isaksen's review of 50 studies argues that misunderstanding of the technique, weak facilitation, and artificial problems/groups undermine most of these conclusions. Separately, Group Support Systems (simultaneous anonymous electronic submission) remove blocking and can flip the result, and work on separating generation from selection as distinct tasks shows the *selection* stage, not the generation stage, is often where quality is lost.

**Practical protocol conventions in this tradition** (as used, not as formalized): idea segmentation into non-redundant units by trained coders; originality scored as statistical infrequency within the sample (the same relativity trap as SVS a posteriori); feasibility/quality scored on separate short Likert scales; IRR reported as Cohen's κ or ICC. **UNANSWERED: I did not locate a canonical, formalized segmentation/redundancy protocol by name; the field appears to rely on per-study coding manuals rather than a shared standard.** That absence is itself a finding.

---

## 5. Patents: novelty and inventive step as operationalized distance

**The legal frame is already distance-shaped.** Non-obviousness asks whether the invention is "an adequate distance beyond or above the state of the art." US practice runs the Graham factors: scope and content of prior art, differences between prior art and the claims, level of ordinary skill, plus secondary considerations. The EPO produces machine-readable labels in search reports: **X** = novelty-destroying alone, **Y** = relevant in combination with another document, **A** = technological background. Those labels are the closest thing this whole report has to ground truth for novelty.

**Computational implementations:**

- **SEARCHFORMER** (*World Patent Information*, 2023): siamese transformers fine-tuned so that embeddings of an application's claims and the prior-art passages that *negate* novelty or inventive step land close in semantic space. Trained on examiner citation categories.
- **PatentMatch** (arXiv:2012.13919): dataset pairing claims with examiner-cited prior-art paragraphs, labeled by relevance category.
- **FiNE-Patents** (SIGIR 2026, arXiv:2605.02392): **3,658 first claims** annotated with feature-level prior-art references from European Search Opinions. Shifts the task from binary novelty classification to joint retrieval plus feature-level reasoning. **LLM workflows that decompose a claim into features and analyze each against retrieved prior art outperform embedding-based baselines.** This is the single most directly transferable computational result in this report.
- **Text-embedding patent-to-patent similarity** (Sentence-BERT on claims; *Technological Forecasting and Social Change*, 2022) for technological relatedness at scale. Hain et al. (2022) at scale; Shibayama et al. (2021) define *recombinant* novelty via how unusually distant a paper's cited references are from each other.
- **The "optimal distance" framing:** too close to prior art fails novelty; too far yields a non-enabling or incoherent disclosure. High-value inventions sit in a band of moderate dissimilarity.

**The honest gap:** researchers explicitly flag that operationalizing non-obviousness remains open, and that retrieval methods find *known* content rather than identifying what is *missing*. Non-obviousness is the most subjective of the three patentability standards and no computational method has been validated as matching examiner judgment on it.

---

## 6. Applying these metrics to AI-generated designs (2023-2026)

### 6.1 Ma, Grandi, McComb & Goucher-Lambert (2023) — the cleanest engineering-design comparison

"Conceptual Design Generation Using Large Language Models," ASME IDETC-CIE 2023 / *J. Comput. Inf. Sci. Eng.* 23(5) 051012. arXiv:2306.01779. Code: github.com/kevinma1515/gpt_IDETC.

**Design:** 12 design problems, 100 GPT-3 solutions each, compared against 100 crowdsourced (MTurk) solutions per problem from Goucher-Lambert et al. **600 solutions** rated.

**Rating protocol — note what they dropped and why.** Two experts in design theory and methodology, trained, rating on **anchored 0-2 scales**:
- *Feasibility:* 0 = technology does not exist, 2 = implementable as suggested
- *Novelty:* 0 = copied from a common/pre-existing solution, 2 = new and unique (explicitly relative to both the existing design space *and* the generated set)
- *Usefulness:* 0 = completely off-topic, 2 = helpful given the prompt context

**They deliberately dropped the quality measure** because it drew frequent expert disagreement and had the lowest IRR in prior work on the same data. Consistency checked with Cohen's κ on 120 designs (20 per prompt per generation mode), targeting fair-to-moderate agreement. One-way ANOVA across conditions.

**Results:** GPT-3 solutions scored higher on **feasibility and usefulness**; crowdsourced solutions scored higher on **novelty** and showed **substantially higher variance** on all three. Mechanism named explicitly: GPT-3 repeats itself semantically with adjective/feature-level variation until coherence degrades, so its idea pool collapses; crowdworkers come from heterogeneous backgrounds so their solutions are mutually distinct. Generated solutions were also **shorter and carried less information**, which the authors identify as "at the core of why generated solutions did not perform as well."

**Computational metrics used instead of expensive human raters:** SentenceBERT `all-MiniLM-L6-v2` (384-d) embeddings, then (a) **mean nearest generated sample** (design-space coverage) and (b) **convex hull hypervolume** (diversity/breadth). Few-shot prompting gave the highest similarity to the crowd set (mean cosine 0.804, SD 0.06) but **at the cost of diversity**; the zero-shot "unique" adjective prompt gave the lowest (0.746, SD 0.06). Prompt sensitivity was extreme and non-monotonic: adding "diverse" raised similarity from 0.627 to 0.805 on prompt 2 but *lowered* it from 0.738 to 0.675 on prompt 11.

**Their own stated gap:** "there are no computational methods to evaluate usefulness and feasibility, which currently can only be done through expensive expert evaluations." That gap is precisely what an LLM grader would close, and it was written before LLM-as-judge was established practice.

### 6.2 Girotra, Meincke, Terwiesch & Ulrich (Wharton) — GPT-4 vs elite students

Working paper, Mack Institute / arXiv:2402.01727 for the follow-up. GPT-4 generated ideas **faster and cheaper** than students at an elite university, with **higher average quality** (measured by purchase-intent surveys) and **higher variance in quality**; the vast majority of the best ideas in the pooled sample came from ChatGPT. **Students retained a slight advantage on novelty.** Seeding the LLM with high-quality example ideas did **not** improve quality or novelty. The follow-up work targets dispersion directly: limited dispersion caps both novelty and the quality of the pooled best idea, and they study prompting effects on cosine similarity, unique-idea count, and speed of idea-space exhaustion.

Related: a study generating **4,000 GPT-4 design solutions** across five topics, eight parameter combinations, and eight prompting techniques found human-generated solutions had **consistently greater diversity scores** on all five topics.

**The converged finding across all of these: LLMs win on quantity, speed, average quality, feasibility, and usefulness; humans win on novelty and, decisively, on diversity.** Note this is a mirror image of the research-ideation literature, where blind review by 100+ NLP researchers rated LLM ideas *more* novel but less feasible than expert ideas — and a follow-up execution study found AI ideas scored significantly lower after actual execution. The difference is probably that engineering-design novelty is judged against a physical/functional design space where the LLM's training distribution is dense, while research novelty is judged against a literature frontier where fluent recombination reads as novel.

### 6.3 LLMs as graders of design creativity

**Zhang, Han & Ahmed-Kristensen, "Exploring the use of LLMs to evaluate design creativity," ICED25 / *Proceedings of the Design Society*.** This is the most directly relevant paper in the entire report for your use case.

- **Task:** 78 text design ideas (37 human, 41 ChatGPT) for "design a container for hot drinking liquids."
- **Graders:** **10 LLMs** including GPT-4, Gemini, Llama 3.1 variants, Gemma 2, OpenChat 3.6.
- **Metrics:** novelty and usefulness, 7-point scale (0-6).
- **The key design move:** rather than asking for a holistic score, each dimension is **decomposed into four yes/no questions** — novelty via function and structure questions inspired by SAPPhIRE, usefulness via effectiveness and feasibility questions — with **rule tables mapping the answer pattern to a score.** This converts a judgment task into a feature-extraction task plus a deterministic lookup.
- **Human baseline:** two experts (10+ years), independent then consensus. Cronbach α = **.80** (novelty, "good") and **.92** (usefulness, "excellent").
- **Result:** cross-model score SD fell from 0.970 → **0.822** (novelty) and 0.946 → **0.881** (usefulness) versus the baseline prompt. Human alignment measured by binning scores into four bands (0 / 1-2 / 3-4 / 5-6) and counting exact-band matches; the structured method improved alignment for most models.
- **Reported honestly:** **no correlations or kappas against humans were reported**, only binned match counts and cross-model dispersion. Cross-model agreement is not accuracy. Authors flag prompt-author style bias, model randomness, and lack of current product knowledge for the comparison-to-existing-products step.

**Corroborating and cautioning evidence from adjacent domains:**

| Study | Finding |
|---|---|
| Hackathon creativity at scale (arXiv:2503.04290) | **Human** raters agree poorly on novelty (κ = 0.18, dropping to 0.04 at finer granularity) but reasonably on usefulness (κ = 0.53). Best inter-LLM κ: 0.59 novelty (Prometheus/Llama), 0.46 usefulness (Mistral/Llama). Fleiss κ stronger for usefulness than novelty across all settings |
| Scientific ideation, *Nature Communications* (arXiv:2412.17596) | Expert ICC = .823 originality, .782 clarity, **.453 feasibility**. Strong human IRR on originality supported high human-LLM correlation |
| NLP research ideas | LLM-human ρ = 0.42 vs human-human ρ = 0.41; MAD 1.70 vs 1.73. **LLM judges disagree with individual experts no more than experts disagree with each other** |
| "Novelty mirage" (arXiv:2606.12071) | LLM judges rate model-generated research questions as highly novel and **amplify this in comparative settings**; domain experts sharply disagree and prefer author-anchored questions. Recommends multi-judge panels, agreement checks, and scope/narrowness as a first-class dimension |
| Marketing creativity benchmark (arXiv:2509.09702) | Three LLM-as-judge setups vs human rankings: **weak, inconsistent correlations and judge-specific biases** |
| Broad LLM-as-judge evaluation (arXiv:2606.19544) | "Reliability without validity" — consistently sub-human agreement with subject-matter experts on domain tasks |
| Rating Roulette (EMNLP 2025 Findings) | Self-inconsistency in LLM-as-judge across repeated runs |

**The pattern is stable across every domain measured: LLM graders track human judgment on usefulness/effectiveness/feasibility, and are unreliable on novelty — because humans are also unreliable on novelty, and because LLMs carry a systematic pro-AI-output novelty bias on top of that noise.**

---

## 7. FIT / COST verdict for automated blind LLM grading

### 7.1 Transfers well — adopt

**(a) Decomposition into independent dimensions with anchored scales.** The single most transferable idea. Ma et al.'s **0-2 anchored scales** with concrete verbal anchors at each point, and their willingness to *delete* a dimension (quality) that failed IRR, is the right pattern. Cost: near zero. Evidence: Miller et al. show the SVS 4-point *structured quality rubric* pulled raters toward the expert construct (r = .31) while unstructured human quality ICC was .56-.75. Structure helps most where holistic judgment is weakest.

**(b) The Zhang/Han/Ahmed-Kristensen construction: yes/no feature questions + deterministic score lookup.** Do not ask the grader "how novel is this, 0-6." Ask four binary feature questions with objective referents, then map the answer vector to a score via a fixed table. This makes the score reproducible, auditable, and diffable across runs, and it measurably reduced cross-model dispersion (novelty 0.970 → 0.822). Cost: one prompt-engineering pass plus a lookup table. This is the highest FIT/COST ratio item in the report.

**(c) Sarkar & Chakrabarti's `C = N × U` multiplicative combination — when a combined score is required.** It has the best human-agreement validation of any combination rule here (Spearman ~0.93 vs experienced designers, small n) and it encodes the correct semantics: a useless novelty and a banal solution should both score near zero. Cost: trivial. Caveat: it is defined on *relative ranks within a set*, so it inherits set-dependence — see 7.3.

**(d) Cropley's Elegance and Genesis facets, and CPSS Style.** These are the dimensions nobody automates and they are the ones with commercial evidence behind them: O'Quin & Besemer found **novelty is a poor predictor of willingness to buy while Value and Elegance are much stronger.** For grading code or designs, "elegance" (economy, harmony, sufficiency of the solution given the problem) is a dimension LLMs are unusually good at articulating and one that correlates with what practitioners actually want. Cost: low. Evidence: the CSDS facet has survived two independent factor analyses; exact reliabilities UNANSWERED.

**(e) The Sorensen aggregation axioms, as constraints on your scoring function.** Do not implement their OPED tree. Do implement their axioms as unit tests on whatever aggregation you use:
- adding a low-scoring artifact must never raise a set score
- adding a high-scoring artifact must always raise it
- near-duplicates must not inflate quantity
- a rater/system that filters weak outputs must not thereby score higher

Averaging violates the first two and is the default in every naive implementation. Cost: a test file. This catches a real class of gaming.

**(f) Patent-style feature-level decomposition against retrieved prior art (FiNE-Patents).** If your artifacts have a retrievable "prior art" corpus (existing solutions, the codebase, prior submissions), decompose the artifact into features and check each feature against retrieved evidence. LLM workflows doing this beat embedding-similarity baselines. This is the only *validated-against-ground-truth* novelty method in the report, because EPO X/Y/A examiner labels are real labels. Cost: moderate — needs a retrieval index. FIT is high and rising.

**(g) Diversity/coverage metrics on embeddings, at the set level.** Ma et al.'s two metrics are cheap and directly measure the thing LLMs are demonstrably worst at:
- **mean nearest generated sample** — coverage of a reference set
- **convex hull hypervolume** — breadth of the generated set

Both run on `all-MiniLM-L6-v2` embeddings. Cost: minutes. These are the correct replacement for the genealogy-tree variety metric (see 7.2). Every study that measured it found LLM output diversity below human: Ma et al., the 4,000-solution GPT-4 study across all five topics, and the Wharton dispersion follow-up.

### 7.2 Does not transfer — do not build

**(a) The SVS genealogy tree (variety, and the tree-based novelty variants).** Requires a human to construct a four-level physical-principle / working-principle / embodiment / detail hierarchy over the whole idea pool, and to assign function weights `f_j` and stage weights `p_k` per problem. Miller et al. report Cohen's κ = 0.88 for two trained coders building the tree, so it *is* reliably codable, but the cost is a trained-coder pass over the entire pool per problem. Worse, it breaks on unbalanced trees (Verhaegen et al.), requires all ideas at the lowest abstraction level, and its normalization is provably wrong (Nelson; Sorensen's counterexample 6.54 → 7.1). **An LLM could plausibly build the tree, but you would be automating a construct with a negative correlation to expert novelty judgment (r = −.10, p = .002, N = 932). Automating it makes the wrong answer cheap.** Use embedding-based diversity instead.

**(b) A-posteriori SVS novelty (`S = (T−C)/T × 10`) as your novelty measure.** Fatal for blind automated grading on three counts: (i) it is defined only relative to the current batch, so the same artifact scores differently depending on what else was submitted, which breaks any longitudinal or cross-cohort comparison; (ii) it depends entirely on an undefined attribute decomposition, so the score is a function of the rater's ontology rather than the artifact; (iii) it can score a frequently-recurring solution as highly novel (Sluis-Thiescheffer et al.). Combined with the Miller negative correlation, this metric is worse than not measuring novelty. **If you want frequency-based novelty, compute it explicitly and call it "uncommonness within batch," never "novelty."**

**(c) Sarkar & Chakrabarti's usefulness formula `U = L × R × F × D` as literally specified.** It requires field data — population using the product, hours per day of use. For a design artifact that does not exist yet this is unavailable. **But the underlying idea is worth porting:** usefulness = importance × reach × frequency × duration is a defensible decomposition for an LLM to *estimate* with explicit uncertainty, and it beats a holistic "rate usefulness 1-7." Mark estimates as estimates.

**(d) Full CPSS (55 items × 7-point bipolar semantic differential).** Item cost is high and the Resolution dimension's validity has been unestablished since 1989. Take the three-dimension *structure* (Novelty / Resolution / Style) and the finding that Style predicts adoption; leave the instrument.

**(e) Full CSDS (24 or 27 items).** Same reasoning. The *facet taxonomy* is the value; the item battery is not worth the tokens unless you need a psychometrically-defensible published instrument for a paper. Note the theoretically-clean **Novelty construct empirically split into Problematization and Propulsion** in both studies. If you use one novelty score, you are averaging two constructs.

### 7.3 The three failure modes to design against

**(1) Within-sample relativity contaminates every frequency-based metric.** SVS a posteriori, brainstorming originality-as-infrequency, and Sarkar's relative-grade conversion all define scores against the current pool. An automated grader that scores batch-relative cannot answer "is this submission better than last month's." **Fix:** score every artifact against a *fixed external reference set* (a frozen corpus, a retrieval index, a rubric with absolute anchors), and compute batch-relative statistics separately as a distinct, differently-named quantity.

**(2) Novelty is the least reliable dimension for humans and LLMs alike, and LLMs have a directional bias on top of the noise.** Human novelty κ = 0.18 → 0.04 (hackathons); expert-vs-SVS novelty r = −.10; expert novelty vs expert quality r = −.29; LLM judges systematically inflate novelty of model-generated content and amplify it in comparative settings ("novelty mirage"). Meanwhile usefulness/effectiveness is reliable everywhere: human κ = 0.53, expert α = .92, expert-quasi-expert agreement highest, SVS-CAT quality r = +.31.
**Fix:** weight usefulness/effectiveness heavily, treat novelty as a low-confidence signal with wide error bars, never let a single-judge novelty score be decisive, and *never* run comparative novelty judging where one candidate is known-or-inferable to be AI-generated. Prefer novelty grounded in retrieval against a corpus over novelty-by-vibes. Add **scope/narrowness as a first-class dimension** (the novelty-mirage recommendation) since inflated novelty usually comes with vagueness.

**(3) Aggregation rules silently reward filtering.** Averaging novelty or quality across a set means discarding weak artifacts raises the score. If your system scores a *portfolio* or a *batch*, this is a live gaming vector. Sorensen et al. demonstrate it with SVS's own published numbers (quality 5.37 → 6.05 by deleting one bad idea). **Fix:** use monotone, non-averaging aggregation and unit-test the axioms.

### 7.4 Where LLM feature extraction makes a previously-manual metric cheap

Three specific opportunities, in descending confidence:

1. **Usefulness and feasibility scoring.** Ma et al. (2023) state flatly that no computational method existed for these and only expensive expert evaluation would do. This is now solved-ish: it is the dimension where LLM-human agreement is consistently strongest (α = .92 expert baseline, κ = 0.53 human, best inter-LLM κ = 0.46, ICC = .83/.75 after calibration in one study). **Highest-confidence automation target in this entire literature.**
2. **Feature-level prior-art decomposition for novelty.** FiNE-Patents shows LLM claim-decomposition-plus-per-feature-retrieval beats embedding baselines against real examiner labels. If you have a corpus, this converts novelty from an unreliable holistic judgment into a set of checkable retrieval questions.
3. **SAPPhIRE-style structural novelty (Sarkar & Chakrabarti via Zhang et al.).** Asking an LLM "does this fulfill a function no existing solution fulfills? does it use a different physical phenomenon? different parts?" is exactly the kind of feature extraction LLMs do well, and Sarkar's version had the best human agreement of the classical methods (~0.93 vs 0.66 for Shah's). Zhang et al. already demonstrated the port. **This is the best available novelty construction: structural difference against named referents, not frequency within a batch.**

### 7.5 Concrete recommended stack

```
Per artifact, blind:
  effectiveness/usefulness  — 4 binary feature questions → lookup table → 0-6
  feasibility               — anchored 0-2 scale, verbal anchors per point
  elegance                  — CSDS/CPSS-Style facet, anchored scale
  structural novelty        — SAPPhIRE-style: differs at function / phenomenon /
                              organ / part level vs named referents from retrieval
                              (report the level, not just a number)
Per artifact, combined (only if a single number is required):
  C = (novelty_rel) × (usefulness_rel)        [Sarkar & Chakrabarti Eq. 2]
Per set:
  coverage   = mean nearest generated sample (SBERT)
  breadth    = convex hull hypervolume (SBERT)
  quantity   = unique-idea count after dedup, not raw count
  aggregation must satisfy the Sorensen axioms (unit-tested)
Reliability discipline (the thing 71% of this literature skipped):
  multi-judge panel, report Krippendorff α or ICC2 per dimension,
  drop any dimension that fails threshold — as Ma et al. dropped quality
```

Report novelty and usefulness separately as the primary output. The multiplicative composite is for ranking only, never for reporting.

---

## 8. Marked UNANSWERED

- Exact refined variety equation from Nelson et al. (2009) — ScienceDirect paywall; Penn State reference page renders equations as images. I have the variable definitions (`d_l` = differentiations at node *l*, `S_k` = 10/5/2/1, non-normalized) but not the assembled formula.
- Exact formula from Fiorineschi, Frillici & Rotini (2021) refined a-posteriori novelty metric, and its handling of missing/extra attributes — T&F 403.
- Exact Peeters et al. (2010) PAnDA-based three-level novelty formula.
- Exact per-subscale Cronbach alphas and CFA fit indices for CSDS (both the 24-item and 27-item revisions), factor loadings, and the stepwise-regression betas predicting Overall Creativity — SciELO and JOTMI full texts returned 403/abstract-only. The 2011 paper's reliability claim is verbal ("uniformly high," "excellent range per Nunnally & Bernstein 1994") and I could not resolve it to numbers.
- Exact per-subscale internal-consistency coefficients for CPSS (described only as "generally good" in the retrieved summaries of O'Quin & Besemer 1989).
- Exact CCA and MPCA formulas and reliability statistics from Oman et al. (2013) — Springer paywall.
- Exact numeric means, SDs, ANOVA F/p values, and Cohen's κ values from Ma et al. (2023) expert evaluation — the paper reports these in Figure 2 (a plot) rather than a table, and the figure did not survive text extraction. The *direction* of every result is confirmed from the body text.
- Whether the SVS original variety level weights are 10/6/3/1 or 10/5/2/1. Sources disagree; the 10/5/2/1 convention is what Nelson-derived implementations use.
- Derivation or measurement basis for Sorensen et al.'s α = 0.46, β = 0.32. Presented as chosen values; treat as EXTRAPOLATED.
- A canonical, named idea-segmentation and redundancy protocol for group-brainstorming scoring. I found none; the field appears to use per-study coding manuals. Treat as a real gap rather than a retrieval failure, but not conclusively.

**Sources:**
- [Fiorineschi & Rotini (2023), Design Science — SVS novelty metric literature audit](https://www.cambridge.org/core/journals/design-science/article/uses-of-the-novelty-metrics-proposed-by-shah-et-al-what-emerges-from-the-literature/28EAA999E457B6A56C3B36AFC181890E)
- [Sorensen et al., Design Science — axiom-based aggregation functions](https://www.cambridge.org/core/journals/design-science/article/axiombased-aggregation-functions-for-calculating-variety-novelty-quality-and-quantity-of-ideation-results/FCE7D4AAC014E42A684D6756D22EBC44)
- [Sarkar & Chakrabarti (2011), Assessing design creativity — full PDF](https://dm.iisc.ac.in/cpdm/ideaslab/publications/UID_45.pdf)
- [Miller et al. (2021), J. Mech. Des. 143(3) 031404 — CAT vs SVS](https://doi.org/10.1115/1.4049061) / [conference PDF](https://decode.mit.edu/assets/papers/2020_miller_creat.pdf)
- [Ma et al. (2023), Conceptual Design Generation Using LLMs](https://arxiv.org/pdf/2306.01779)
- [Zhang, Han & Ahmed-Kristensen (ICED25), Exploring the use of LLMs to evaluate design creativity](https://www.cambridge.org/core/journals/proceedings-of-the-design-society/article/exploring-the-use-of-llms-to-evaluate-design-creativity/B7D16E2612A33C0A9DE1B1922E054639)
- [Nelson et al. (2009), Refined metrics for measuring ideation effectiveness](https://www.sciencedirect.com/science/article/abs/pii/S0142694X0900060X)
- [Penn State creativity metrics reference](https://sites.psu.edu/creativitymetrics/creativity-rating-resources/)
- [Cropley & Kaufman (2012), JOCB — CSDS](https://onlinelibrary.wiley.com/doi/abs/10.1002/jocb.9) / [Cropley, Kaufman & Cropley (2011), JOTMI](https://www.jotmi.org/index.php/GT/article/view/art204)
- [O'Quin & Besemer (1989), Creativity Research Journal — revised CPSS](https://www.tandfonline.com/doi/abs/10.1080/10400418909534323) / [O'Quin & Besemer (2006), CIM](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.1467-8691.2006.00367.x)
- [Oman, Tumer, Wood & Seepersad (2013), Research in Engineering Design](https://link.springer.com/article/10.1007/s00163-012-0138-9)
- [Fiorineschi et al. (2021), Refined metric for a-posteriori novelty assessments](https://dx.doi.org/10.1080/09544828.2021.1976397)
- [Girotra et al., Wharton LLM ideation working paper](https://mackinstitute.wharton.upenn.edu/wp-content/uploads/2023/08/LLM-Ideas-Working-Paper.pdf) / [dispersion follow-up](https://arxiv.org/pdf/2402.01727)
- [FiNE-Patents, SIGIR 2026 — fine-grained patent novelty prediction](https://arxiv.org/pdf/2605.02392)
- [SEARCHFORMER — siamese transformers for prior art search](https://www.sciencedirect.com/science/article/abs/pii/S0172219023000224)
- [Hackathon creativity evaluation at scale](https://arxiv.org/pdf/2503.04290)
- [On the Limits of LLM-as-Judge for Scientific Novelty Assessment](https://arxiv.org/pdf/2606.12071)
- [Reliability without Validity: LLM-as-a-Judge evaluation](https://arxiv.org/pdf/2606.19544)
- [Evaluating LLMs' divergent thinking, Nature Communications](https://www.nature.com/articles/s41467-026-70245-1)
- [Diehl & Stroebe / brainstorming productivity loss review](https://www.sciencedirect.com/science/article/abs/pii/S006526011043004X)