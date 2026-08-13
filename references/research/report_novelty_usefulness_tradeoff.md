I have enough verified primary material. Closing out with the report.

---

# Novelty vs. Usefulness in Evaluation: What the Empirical Record Actually Says

**Epistemic status marking used throughout:** ✅ = I read the number in the primary full text. ⚠️ = I have the finding from a reliable secondary/abstract source but not the primary numbers. ❌ **UNANSWERED** = I could not retrieve it and will not reconstruct it from memory.

Four papers I obtained in **full text** and mined directly: Mueller/Melwani/Goncalo 2012, Boudreau et al. 2016, Blair & Mumford 2007, Berg 2016. The Rietzschel papers, Dean et al. 2006, Diehl & Stroebe 1987, and Licuanan et al. 2007 were blocked by Cloudflare/AWS-WAF/paywall after repeated attempts (including browser automation, which rendered the Dean PDF but did not yield extractable text at acceptable cost). Their numbers are marked accordingly.

---

## 1. The headline finding you asked me to verify or refute

**Claim under test:** "Rated novelty and rated usefulness/feasibility of the same ideas are often negatively or near-zero correlated."

**Verdict: PARTIALLY SUPPORTED, and weaker than the folklore.** The negative relationship is robust in *evaluator preference and selection behavior*, but the *raw correlation between novelty ratings and quality ratings of the same artifacts* is, in the data I could actually verify, **near zero or mildly positive** — not negative. These are different quantities and the literature routinely conflates them. This distinction is the single most important thing in this report for your grader design.

| Source | Quantity | Value | N | Domain |
|---|---|---|---|---|
| Boudreau et al. 2016 ✅ | raw r(proposal novelty, evaluation score) | **−0.03** | 2,130 pairs | biomedical grant proposals |
| Boudreau et al. 2016 ✅ | *controlled* β on novelty | **−2.067** (SE 0.64), p<.01 | 689 pairs | same |
| Berg 2016 ✅ | r(novelty, audience liking) | **+.25**, p<.01 | 100 videos | circus arts |
| Berg 2016 ✅ | r(novelty, audience sharing = "quality") | **+.12**, n.s. | 100 | same |
| Berg 2016 ✅ | r(novelty, audience financial support) | **+.18**, p<.05 | 100 | same |
| Blair & Mumford 2007 ✅ | r(implementation difficulty, risk) | **+.15** | ~161 raters | foundation grant ideas |
| Blair & Mumford 2007 ✅ | r(implementation difficulty, prob. of success) | **−.29** | ~161 | same |
| Blair & Mumford 2007 ✅ | r(ease of implementation, prob. of success) | **+.58** | ~161 | same |
| Mueller et al. 2012 ✅ | r(explicit creativity-bias, implicit creativity-bias) | **+.29**, p<.05 | 73 | undergrad IAT |
| Mueller et al. 2012 ✅ | r(implicit bias, creativity rating of a target idea) | **−.33**, p<.01 | 140 | undergrad |

**The reconciliation.** Boudreau is decisive here and it is the cleanest design in the entire literature. The raw novelty–score correlation is **−0.03 — indistinguishable from zero.** The large negative penalty (−2.067) appears **only after conditioning on proposal quality covariates.** The bivariate near-zero is a *cancellation*: novel proposals are genuinely somewhat better on other observed dimensions, and evaluators dock them for novelty, and the two effects wash out. A grader that only looks at the marginal correlation will conclude "no tension exists." That conclusion is wrong. **The anti-novelty effect is a partial effect, and it is invisible marginally.**

---

## 2. Boudreau, Guinan, Lakhani & Riedl (2016), *Management Science* 62(10):2765–2783 ✅ FULL TEXT

The best-identified evidence in this literature, because assignment was randomized.

**Design.** A real grant competition at a leading research university (Harvard Medical School, endocrine-related disease). Evaluators and proposals randomly assigned → **2,130 evaluator–proposal pairs.** Dependent variable `EVALUATION_SCORE` is an **integer 1–10**; mean 5.69, **SD 2.58** (text elsewhere states s.d. 2.6, and 1.7 after removing proposal and evaluator fixed effects).

**Novelty measure.** Proposals coded by a professional NLM-trained librarian into MeSH terms (mean 12.42 terms per proposal, SD 5.42, from a 26,579-term vocabulary). `PROPOSAL_NOVELTY` = share of all MeSH-term *pairs* in a proposal that had never co-occurred in the prior PubMed literature (~185M term combinations across ~21M articles, 1855–2010), expressed as a percentile 1%–100%.

**The coefficient you asked for:**

| Model | PROPOSAL_NOVELTY coefficient | SE |
|---|---|---|
| 1 — evaluator dummies + proposal control vector | **−2.067*** | 0.64 |
| 2 — extended proposal controls | **−3.010*** | 0.89 |
| 3 — + EVALUATOR_DISTANCE | **−2.080*** | 0.64 |

(*** = p<.01. n = 689 pairs for these models — the within-university subsample for which proposal covariates exist. Adj. R² = .423/.459/.428.)

**Magnitude.** Because novelty is a 0–1 percentile, −2.067 means moving from the least to the most novel proposal costs **~2.07 points on a 10-point scale ≈ 0.80 SD of raw scores, or ~1.2 SD of the fixed-effect-purged score distribution.** This is a very large penalty.

**Three things that make this hard to explain away, all verified:**
1. **Adding controls makes it more negative, not less.** The authors explicitly checked: "progressively adding more controls to the model generally produces more negative estimates, not less. For example, dropping control variables altogether produces a far less negative coefficient (−0.25; s.e. = 0.20)." Omitted-variable bias runs *against* their finding.
2. **The penalty is concentrated in the right tail.** A quintile-dummy and second-order-polynomial specification both show the relationship is **non-monotonic**: at low novelty, scores *increase* with novelty; the negative relationship "is largely driven by the most novel proposals," i.e. the **fifth quintile**. This is an inverted-U with a sharp cliff at the top, not a linear discount.
3. **It is not agency/self-interest.** No significant DISTANCE × NOVELTY interaction (−0.38, SE 1.06); no interaction with seniority, years since graduation, or gender. The authors rule in bounded rationality, not incentives.

**Companion finding (separate mechanism, same paper).** `EVALUATOR_DISTANCE` (cosine distance between evaluator's and proposal's MeSH vectors, percentiled) has a **positive** coefficient: 1.10 (SE 0.19) with proposal fixed effects; 0.86 (SE 0.33) with both proposal and evaluator fixed effects; 1.48 (SE 0.59) alongside novelty. Evaluators score proposals **closer to their own expertise more harshly** — roughly a full point across the range, plus 0.37 (SE 0.12) for being outside the disease domain entirely. This relationship is **linear** with no nonlinearity. Random-coefficient estimate: mean 1.48 (SE 0.41), between-evaluator SD 0.61.

**Noise baseline, which matters enormously for grader design.** Proposal dummies explain only **26%** of score variance (R²); evaluator dummies explain **19%.** The authors: "The patterns are consistent with considerable noise in the evaluation process." **Over half the variance in expert evaluation of grant proposals is neither the proposal nor a stable evaluator trait.**

---

## 3. Mueller, Melwani & Goncalo (2012), *Psychological Science* 23(1):13–17 ✅ FULL TEXT

**Mechanism: uncertainty, not distaste.** The bias is *state-activated*, not a stable trait.

**Experiment 1.** N = 73 (uncertainty n = 28, baseline n = 45; 51% male, mean age 22.74). Uncertainty manipulated by telling participants they *might* receive extra payment via lottery. Pilot N = 82 confirmed the manipulation raised uncertainty.

IAT: creativity words (novel, creative, inventive, original) vs. practicality words (practical, functional, constructive, useful), crossed with good (rainbow, cake, sunshine, laughter, peace, heaven) / bad (vomit, hell, agony, rotten, poison, ugly). Scored with Greenwald's D statistic; higher = more anti-creativity. Block order counterbalanced.

- **Explicit bias: no effect.** Uncertainty M = 0.02 (SD 0.83) vs. baseline M = −0.11 (SD 0.96), **F(1,70) = 0.07, p = .78.**
- **Implicit bias: large effect.** Uncertainty M = **+0.15** (SD 0.54) vs. baseline M = **−0.23** (SD 0.47), **F(1,70) = 13.13, p = .001.** **Condition accounted for 11% of the variance in implicit bias.**
- Explicit ratings were near-identical for creativity (M = 5.37, SD 0.75, α = .77) and practicality (M = 5.43, SD 0.91, α = .88) on 7-point scales. Explicit-bias score M = 0.06, SD 0.91.
- Table 1 correlations (N=73): openness × explicit bias **−.32**, openness × implicit bias **−.25**, condition × implicit bias **+.35**, explicit × implicit bias **+.29**.

**Experiment 2** — this is the one that matters for grading. N = 140 (70/70; 55% female, mean age 20.66).

Manipulation: write an essay supporting either *"For every problem, there is more than one correct solution"* (high tolerance) or ***"For every problem, there is only one correct solution"*** (low tolerance). **This is precisely the framing of a single-score rubric grader.**

- Manipulation check: low-tolerance M = 4.36 (SD 1.23) vs. high M = 3.87 (SD 1.33), **F(1,133) = 5.14, p = .025.**
- **Explicit bias: again nothing.** 0.20 (SD 0.81) vs. 0.22 (SD 0.94), **F(1,133) = 0.14, p = .71.**
- **Implicit bias: +0.07 (SD 0.43) vs. −0.16 (SD 0.46), F(1,133) = 7.87, p = .007.**
- **The behavioral payoff.** Target stimulus: a running shoe with nanotechnology that adjusts fabric thickness to cool the foot and reduce blisters. Pretested (separate N = 36) as **creative M = 5.82 (SD 0.80), novel M = 5.62 (SD 1.02), practical M = 5.85 (SD 0.92)** on 7-point scales — i.e. **high on both dimensions simultaneously.** Low-tolerance participants rated it **M = 5.06 (SD 1.06)** vs. high-tolerance **M = 5.76 (SD 0.93)**, **F(1,137) = 15.48, p < .001.**
- **Mediation.** Condition → creativity rating β = −0.64, t(134) = −3.81, p < .001; attenuated to β = −0.56, t(134) = −3.30, p < .01 with implicit bias in the model. Bootstrapped indirect effect 95% CI **[−.24, −.02]**, excluding zero → partial mediation. Holds controlling for explicit bias and openness.
- Table 2 correlations (N=140): condition × implicit bias **+.25**, condition × creativity rating **−.33**, implicit bias × creativity rating **−.33**, explicit bias × creativity rating **−.24**, openness × creativity rating **+.20**.

**Critical detail for your use case.** 95% of participants in *both* high-uncertainty conditions rated creativity positively on the explicit measure, statistically equivalent to practicality. **Asking an evaluator whether it values novelty tells you nothing.** The bias is only visible in behavior. Corollary: asking an LLM grader "do you penalize novelty?" is worthless as a diagnostic.

---

## 4. Blair & Mumford (2007), *Journal of Creative Behavior* 41(3):197–222 ✅ FULL TEXT

**Design.** Stage 1: 210 University of Oklahoma undergraduates generated ideas for a foundation grant. Stage 2: 165 undergraduates evaluated ideas systematically varied on 12 attributes (analysis dfs run 1,157 and 11,147, so effective n ≈ 161). Crossed manipulations: time pressure, stringency level, social evaluative pressure.

**Main result — enormous effect.** Attribute main effect **F(11,147) = 90.356, p < .001, η² = .871.** For comparison: time pressure F = .006 (p = .938, η² = .000); stringency F = .403 (p = .526); social evaluative pressure F = 7.452 (p = .007, η² = .045). **The attribute profile dwarfs every situational manipulation by more than an order of magnitude in variance explained.** People's preferences over idea attributes are close to a fixed function.

**Preference task** (overall mean X = .48, SE .007). Preferred, in order: consistent with extant social norms **.57** (SE .006); provides desired outcomes quickly **.55** (SE .008); complex to implement **.54** (SE .008); easy to understand **.53** (SE .007); benefits many people **.52** (SE .007).
**Rejected: original X = .40 (SE .008); risky X = .41 (SE .011); complete description X = .33 (SE .009).**

**Choice task** (overall mean .486). Preferred: complicated **.585** (SE .009); quick desired outcomes **.566** (SE .012); complex to implement **.545** (SE .011); easy to understand **.532** (SE .009). **Rejected: complete descriptions .321 (SE .013); original .412 (SE .011).**

Authors' own words: **"the undeniable disdain for risky and original ideas."** Original and risky ideas were neither preferred nor selected, across *both* preference and choice tasks.

**Cross-scale correlations (construct-validity check, ~161 raters)** — these are the closest thing to a novelty×usefulness correlation matrix in this paper:
- ease of implementation × understandability **r = .29**
- ease of implementation × consistency with social norms **r = .42**
- ease of implementation × probability of success **r = .58**
- implementation difficulty × complication **r = .40**
- implementation difficulty × risk **r = .15**
- implementation difficulty × probability of success **r = −.29**

**A finding that should worry you specifically.** "Complete description" — the *most complete, fully specified* ideas — was the **single most rejected attribute** in both tasks (.33 and .32). And "complicated" was **preferred** (.585). Evaluators liked ideas that were complex-to-implement and *incompletely* specified. This is not a coherent quality preference; it looks like a legibility/plausibility heuristic that runs orthogonal to actual merit. Note this directly contradicts Dean et al.'s "specificity" dimension being treated as a quality virtue.

---

## 5. Berg (2016), *ASQ* 61(3):433–468 ✅ FULL TEXT

**Why this matters: it is the counterexample.** In a domain where novelty is the product (circus arts), **novelty correlates positively with market value.**

**Design.** Field study, 489 individuals total: 339 circus professionals (177 creators, 44.1% female; 120 managers, 42.5% female; 42 hybrids, 55.0% female) plus 150 laypeople (50.0% female). 161 videos collected via a circus video contest, 100 selected by pretest for forecasting. Each participant forecast 10 randomly assigned videos. Accuracy validated against **13,248 audience members**, each rating one randomly assigned video.

**Table 2 correlations, N = 100 videos:**
- audience liking M 5.15 (SD 0.50); audience sharing M 49.82% (SD 6.85%); audience financial M 4.75 cents of 10 (SD 0.48); **novelty M 4.78 (SD 1.13)**
- liking × sharing **.66***; liking × financial **.55***; sharing × financial **.46***
- **novelty × liking +.25** (p<.01); **novelty × sharing +.12** (n.s.); **novelty × financial +.18** (p<.05)
- novelty × past success **.03**; novelty × future success rate **.09**

Novelty measured by consensual assessment technique (Amabile-style), defined as uniqueness of video content.

**Role effects (the paper's actual thesis).** Forecasting accuracy, role × novelty interactions: **creators vs. managers β = −6.23*** (estimation) and **−0.45*** (placement); creators vs. laypeople **−3.02**, **−0.24***; creators vs. hybrids −0.73, +0.08 (n.s.). Novelty main effects **+2.60*** and **−0.30***. For high-novelty videos (+1 SD) creators outperformed managers; at low novelty (−1 SD) creators mean = −6.55 vs. managers reported adjacently. **The advantage is specifically at high novelty.**

Moderator: past success × quality interaction on placement accuracy **β = 0.16**, p<.01 (past success β = −1.45 n.s., quality β = 0.04 n.s.). Creators whose *low-quality* ideas succeeded anyway became *worse* forecasters.

**Berg's conclusion, verbatim-adjacent:** having managers specialize in selection may ironically hinder their ability to select the best ideas.

**Also verified and important:** quality (audience sharing) was **not significantly correlated with past success** (view counts), though it was correlated with future success *rate*. Berg cites Salganik/Dodds/Watts (2006): quality drives success, but most variance in success is driven by other, less predictable factors. **Ground truth in creative domains is itself a noisy, path-dependent variable.**

---

## 6. Rietzschel, Nijstad & Stroebe — the selection-vs-generation program

⚠️ **I could not obtain the primary full texts.** Groningen's repository is behind Cloudflare; Elsevier and Wiley are paywalled. Everything below is from abstracts and reliable secondary summaries. **The specific correlation coefficients you asked me to confirm are ❌ UNANSWERED and I will not invent them.**

**Rietzschel, Nijstad & Stroebe (2010), *BJP* 101(1):47–68.** ⚠️
- Verified qualitative finding: the authors identify participants' **strong tendency to select feasible and desirable ideas at the cost of originality** as the primary cause of poor selection performance.
- Two manipulations of how participants processed the idea pool — **exclusion instructions** and **quality ratings** — had **no effect** on selection effectiveness. This is a null result and it is a significant one for your design: forcing an explicit dimension-rating step did not fix the bias.
- Instruction framing does matter: selecting "the most creative" idea rather than "the best" idea yields more novel selections.
- ❌ **UNANSWERED:** exact Ns, the originality×feasibility correlation, mean originality of generated vs. selected ideas, all F/t/p values.

**Rietzschel, Nijstad & Stroebe (2006), *JESP* 42(2):244–251, "Productivity is not enough."** ⚠️
- Widely and consistently reported finding: when asked to select the best ideas from a generated pool, groups **did not** select the ideas independent judges rated highly creative — selection was **"hardly more effective than taking a random sample of ideas."**
- I could verify this characterization across multiple independent secondary sources but **not** the primary statistics. ❌ **UNANSWERED:** the exact comparison against the random-selection baseline, N, and effect sizes. This is the most-cited number in the whole area and I could not confirm it. Treat "no better than random" as a strong claim awaiting your own verification.
- Structural implication verified: **generation quantity does not propagate to selection quality.** More ideas in the pool does not raise the quality of what comes out. The bottleneck is selection, not generation.

**Rietzschel, Nijstad & Stroebe (2014), *Creativity Research Journal* 26(2):185–191, problem scope and creativity instructions.** ⚠️ OA copy exists at `research.rug.nl/files/16113815/CRJ_problem_scope.pdf` (Cloudflare-blocked in this session). ❌ **UNANSWERED** for all numbers.

**One primary-source anchor I did verify:** Mueller et al. (2012), in their own literature review, write that evaluators "have a hard time viewing novelty and practicality as attributes that go hand in hand, often viewing them as **inversely related**," citing Rietzschel, Nijstad & Stroebe. ✅ (verified in Mueller full text). That is a characterization of *perception*, not of the objective correlation.

**Later theoretical work by Rietzschel & Nijstad** ⚠️: the core problem in idea evaluation is the **tension between originality and feasibility**; highly original ideas are disliked because they are perceived as risky and unfeasible; any situational or personal factor that makes feasibility salient will suppress creative selection.

---

## 7. Licuanan, Dailey & Mumford (2007), *JCB* 41(1):1–27

⚠️ Paywalled; no OA copy exists (confirmed via Unpaywall and Semantic Scholar — both return CLOSED).

**Verified qualitative content:** 181 undergraduates evaluated the originality of marketing campaigns developed by six teams, with idea originality experimentally varied. The documented error is **systematic underestimation of the originality of truly novel ideas.** Two interventions reduced the error: (a) **active analysis of product originality**, and (b) **appraisal of the interactional processes that generated the idea**.

**This is the most directly actionable finding in the literature for a grader, and I could not verify its numbers.** ❌ **UNANSWERED:** effect sizes, the magnitude of the underestimation, and the size of the correction. If you plan to build on the "force explicit originality analysis" intervention, get this paper.

**Note the tension:** Licuanan et al. say forcing explicit originality analysis helps. Rietzschel et al. (2010) ⚠️ report that adding a quality-rating step did *not* help. These are not the same intervention (originality-specific analysis vs. general quality rating), but the boundary between them is unresolved in what I could verify.

---

## 8. Dean, Hender, Rodgers & Santanen (2006), *JAIS* 7(10):646–699

⚠️ Open access at AISeL but behind AWS WAF. I loaded the 55-page PDF in a real browser and confirmed it renders, but could not extract text at reasonable cost.

**Verified structure.** A review of **90 studies** on creativity and idea generation. Conclusions: (1) studies claiming to measure "idea quality" or "idea creativity" are measuring **different constructs**, making cross-study comparison unsound; (2) all of the measures used map onto **four primary dimensions from MacCrimmon & Wagner (1994)**; (3) each dimension has two measurable sub-dimensions.

| Dimension | Sub-dimensions | Scale |
|---|---|---|
| **Novelty** | originality, paradigm relatedness | 1–4 |
| **Workability** | acceptability, implementability | 1–4 |
| **Relevance** | applicability, effectiveness | 1–4 |
| **Specificity** | completeness, implicational explicitness, clarity | 1–3 |

⚠️ Confirmatory factor analysis reported **high loadings among the sub-dimensions within each dimension and high discriminant validity between dimensions.**

**Their explicit recommendation, and it is the one you asked about** ⚠️: *"to systematically sample each dimension, it is better to score each dimension separately rather than assess a unitary, overall score."* They argue this from the finding that different studies using ostensibly the same "creativity" label were measuring incommensurable things.

❌ **UNANSWERED: the inter-dimension correlation matrix.** This was a specific ask and I did not get it. The paper reports discriminant validity between dimensions, which implies the correlations are low-to-moderate rather than negative — but I did not read the values and will not guess them.

---

## 9. Diehl & Stroebe (1987), *JPSP* 53(3):497–509

❌ **UNANSWERED.** No OA copy (confirmed via Unpaywall, OpenAlex, Semantic Scholar — all CLOSED; the two CiteSeerX mirrors are dead). I retrieved no numbers from this paper and will not report remembered ones.

What I can say with confidence about its *role* in the literature: it is the origin of the production-blocking explanation for brainstorming productivity loss and of the nominal-vs-interactive-group quality-scoring apparatus that Rietzschel et al. (2006) later extended from generation to selection. Its idea-quality measurement approach (independent judges rating on separate scales, aggregated) is the direct ancestor of the Dean et al. framework. **Any novelty–quality correlation it reports is ❌ UNANSWERED.**

---

## 10. Multiplicative vs. additive composition

**Simonton's position** (BVSR / the three-parameter model): creativity is the **product** of initial probability *p*, final utility *u*, and prior knowledge of utility *v*, with creativity ∝ *(1−p) × u × (1−v)*. A multiplicative rule makes creativity **zero if either factor is zero** — a useless novelty scores nothing, and a valuable non-novelty scores nothing. An additive rule lets a high score on one dimension buy off a zero on the other.

**Empirical comparison of composite scoring rules against a criterion: ❌ UNANSWERED.** I found no study that (a) scored the same artifact set additively and multiplicatively and (b) compared both against an external outcome. This is a genuine gap, not a retrieval failure on my part — though I cannot rule out that one exists.

**What the verified data implies anyway, and it is enough to decide:**

1. **The dimensions are not near-orthogonal, so the rules genuinely diverge.** Berg's r = +.12 to +.25 and Boudreau's r = −.03 mean novelty and value are close to independent in the raw. When two near-independent dimensions are combined, additive and multiplicative rankings differ sharply at the extremes — exactly where your interesting artifacts live.

2. **Boudreau's non-monotonicity is decisive against a plain linear-additive rule.** Scores *rise* with novelty up to the fourth quintile and *collapse* in the fifth. Neither a sum nor a product reproduces that shape. If you want your grader to *not* replicate the human curve, you need to know the curve exists; if you want it to track true value, note that Boudreau's design cannot tell you whether the fifth-quintile collapse is an evaluator error or a real quality drop. **That identification problem is unsolved in the literature.**

3. **Multiplicative composition amplifies noise catastrophically.** With Boudreau's numbers — 26% proposal variance, 19% evaluator variance, >50% unexplained — each dimension is measured with large error. A product of two noisy scores has *relative* variance approximately the sum of the relative variances of its factors, and its expectation is biased. **Under measurement error this severe, a multiplicative rule is worse-behaved than an additive one even if multiplicative is the true generative model.** This is the strongest single argument in the report and it comes straight from verified variance decompositions.

4. **The honest reading of Simonton:** multiplicativity is right as a *theory of what creativity is* and wrong as a *scoring rule for noisy ratings*. Keep the zero-property (an artifact useless on one dimension should not win) as a **gate**, not as a multiplication.

---

## FIT/COST verdict for an automated rubric grader

### (a) Score novelty and usefulness separately, or as one "creativity" score?

**Separately. This is the one recommendation with real evidential weight behind it, from three independent directions.**

- Dean et al.'s explicit conclusion from reviewing 90 studies ⚠️: score each dimension separately; a unitary score conflates incommensurable constructs and destroys cross-study comparability. In your setting, "cross-study" means cross-run and cross-model — you lose the ability to tell whether a score change reflects a novelty change or a usefulness change.
- Boudreau ✅ proves the effects are **separable and opposite-signed**: novelty −2.07, evaluator distance +1.10, with **no interaction** between them. Collapsing to one number mathematically destroys recoverable signal — the marginal correlation is −0.03 and tells you nothing.
- Mueller ✅ shows a **single-best-answer framing is itself the bias trigger.** The Experiment 2 manipulation was literally "For every problem, there is only one correct solution," and it produced d ≈ 0.5 on implicit bias and a 0.70-point drop in creativity ratings of a genuinely creative artifact. **A single-score rubric is not a neutral aggregation of a two-dimensional judgment; it is an experimental manipulation known to induce anti-novelty bias.**

**Cost:** roughly 2× the rating calls, or one call with structured multi-field output. Trivially worth it.

**Additional design note from Blair & Mumford ✅:** do not assume your "specificity/completeness" dimension is a quality proxy. Human evaluators *rejected* the most completely described ideas (.33 and .32, the lowest of all 12 attributes) and *preferred* complicated ones (.585). Whatever that heuristic is, it is not tracking merit.

### (b) Will LLM graders replicate the human anti-novelty bias?

**Substantially UNANSWERED, and I want to be blunt that this is the weakest part of the report.** I found no study I could verify that tests LLM graders for the anti-novelty effect specifically. One lead worth chasing: **Kern et al., "Assessing novelty, feasibility and value of creative ideas with an unsupervised approach using GPT-4," *British Journal of Psychology*** — surfaced in search, **not read, ⚠️ unverified**, but it is squarely on point and separates exactly the three dimensions at issue.

**What the mechanism evidence predicts, offered as a hypothesis and not a finding:**

- Mueller's mechanism is **uncertainty-driven**, not preference-driven ✅. The relevant question is therefore not "does the model dislike novelty" but "does the model's scoring behavior change under uncertainty." An LLM asked to produce a **single confident score** is arguably in a permanent low-tolerance-for-uncertainty state — the exact Experiment 2 condition. This predicts LLM graders *should* show the effect, possibly more strongly than humans, because they cannot decline to answer.
- Boudreau's mechanism is **bounded rationality / evaluation noise** ✅ — the evaluator cannot assess what falls outside their sampled knowledge. An LLM's "knowledge frontier" is its training distribution. A genuinely novel artifact is, by construction, off-distribution. **The structural analogy is close enough that I would expect the penalty to transfer, and to be concentrated in the same right tail.**
- **The one thing you can be confident about ✅:** self-report will not detect it. 95% of Mueller's biased participants explicitly endorsed creativity, statistically indistinguishable from practicality, while behaving with an implicit anti-creativity bias. **Do not ask your grader whether it penalizes novelty. Measure it.**

**Concrete, cheap test you can run — and you should, because the literature will not answer this for you.** Replicate Mueller Experiment 2 on your grader. Build stimuli pretested to be *simultaneously* high-novelty and high-usefulness (Mueller's running shoe: creative 5.82, novel 5.62, practical 5.85 on 7-point scales). Score them under two prompt framings: "identify the single best" vs. "multiple valid solutions exist." A significant score drop under the single-best framing is the bias, measured directly. Mueller found F(1,137) = 15.48 with N=140; with deterministic sampling you need far fewer items. **This is a few hours of work and it converts your largest UNANSWERED into a measured quantity.**

Second test, from Boudreau: bin your artifacts into novelty quintiles and check whether the usefulness score is **non-monotonic** with a fifth-quintile collapse. If your grader reproduces that shape, it has the human bias.

### (c) How to combine dimensions

**Recommended: do not combine by default. Emit the vector. If a scalar is required, use a gated weighted sum, never a product.**

1. **Store novelty and usefulness separately and permanently.** Boudreau's −0.03 marginal vs. −2.07 partial ✅ is the proof: the composite is not merely lossy, it is *actively misleading* — it will show you no relationship where a large one exists.
2. **If a scalar is unavoidable: gate, then sum.** Apply a minimum threshold on each dimension (this preserves Simonton's zero-property — a useless novelty and a valueless conventionality both fail), then a weighted sum above the gate. Do **not** multiply: with per-dimension measurement error at Boudreau's levels (>50% of variance unexplained ✅), a product compounds relative variance and biases the expectation. **Multiplicative theory, additive arithmetic.**
3. **Report the novelty-conditional usefulness curve, not just the mean.** Boudreau's inverted-U with a fifth-quintile cliff ✅ is the diagnostic shape. A grader whose usefulness scores fall off in the top novelty quintile is telling you something — either about the artifacts or about itself, and you need both dimensions retained to tell which.
4. **Budget for noise honestly.** Boudreau ✅: proposal identity explains 26% of expert score variance; evaluator identity 19%. **If your grader is more self-consistent than that, it is not more accurate than expert humans — it is differently biased, with the variance moved from noise into systematic error.** High inter-run agreement in an LLM grader should be treated as a warning sign, not a validation.
5. **Ensemble across roles, not just across samples.** Berg ✅ found role-dependent accuracy specifically at high novelty (creators beat managers, β = −6.23 and −0.45 for the interaction). Selection specialists were *worse* at judging novel work. A grader prompted purely as an evaluator may be the "manager" role. Ensembling a generator-perspective grader with an evaluator-perspective grader has a real empirical rationale — **though whether the role effect transfers to LLMs is ❌ UNANSWERED.**

### What is genuinely not known — be honest about this in any downstream use

- **Whether the anti-novelty penalty is an error at all.** Boudreau ✅ establishes that highly novel proposals score lower, all else equal. It does **not** establish that those proposals were better. No study I verified tracks novel-but-rejected ideas to realized outcomes. **The entire "bias against creativity" framing assumes a counterfactual nobody has measured.**
- Whether LLM graders show the effect. Untested in anything I could verify.
- Whether additive or multiplicative composites better predict any external criterion. No comparison found.
- The Dean et al. inter-dimension correlation matrix.
- Every number in Rietzschel 2006/2010/2014, Licuanan 2007, and Diehl & Stroebe 1987.

**The most load-bearing unverified claim in this whole area** is Rietzschel et al. (2006)'s "selection is no better than random." It is cited everywhere, including as the premise of the 2010 follow-up. I could not confirm it from a primary source. If your grader design leans on it, verify it first.

---

### Sources

- [Mueller, Melwani & Goncalo (2012), *Psychological Science* — full text PDF](https://static1.squarespace.com/static/5cb9e53451f4d45cb568b991/t/5cbf25030d92973c33db85ab/1556030724203/Creativity_Bias_Psych_Science_Final.pdf) · [SAGE](https://journals.sagepub.com/doi/10.1177/0956797611421018)
- [Boudreau, Guinan, Lakhani & Riedl (2016), *Management Science* — full text PDF](https://gwern.net/doc/statistics/bias/2016-boudreau.pdf) · [INFORMS](https://pubsonline.informs.org/doi/10.1287/mnsc.2015.2285)
- [Blair & Mumford (2007), *J. Creative Behavior* — full text PDF](https://gwern.net/doc/psychology/novelty/2007-blair.pdf) · [Wiley](https://onlinelibrary.wiley.com/doi/10.1002/j.2162-6057.2007.tb01288.x)
- [Berg (2016), *ASQ* — full text PDF](https://justinmberg.com/wp-content/uploads/Berg_2016_ASQ.pdf) · [SAGE](https://journals.sagepub.com/doi/abs/10.1177/0001839216642211)
- [Rietzschel, Nijstad & Stroebe (2010), *British Journal of Psychology*](https://bpspsychub.onlinelibrary.wiley.com/doi/abs/10.1348/000712609X414204) · [PubMed 19267959](https://pubmed.ncbi.nlm.nih.gov/19267959/)
- [Rietzschel, Nijstad & Stroebe (2014), *Creativity Research Journal* — OA copy](https://research.rug.nl/files/16113815/CRJ_problem_scope.pdf)
- [Dean, Hender, Rodgers & Santanen (2006), *JAIS* 7(10)](https://aisel.aisnet.org/jais/vol7/iss10/30/)
- [Licuanan, Dailey & Mumford (2007), *J. Creative Behavior*](https://onlinelibrary.wiley.com/doi/10.1002/j.2162-6057.2007.tb01279.x) · [ERIC EJ901318](https://eric.ed.gov/?id=EJ901318)
- [Kern et al., GPT-4 assessment of novelty/feasibility/value, *BJP* — lead, unread](https://bpspsychub.onlinelibrary.wiley.com/doi/full/10.1111/bjop.12720)
- [Rietzschel & Nijstad, "Why Great Ideas Are Often Overlooked" — review](https://research.rug.nl/en/publications/why-great-ideas-are-often-overlooked-a-review-and-theoretical-ana/)
- [Zhou, Wang, Bavato, Tasselli & Wu (2019), "Receiving Side of Creativity" review, *J. Management*](https://journals.sagepub.com/doi/abs/10.1177/0149206319827088)
