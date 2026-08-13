Search budget exhausted; writing the report from what's verified. Cross-cultural and domain-generality constants are marked UNANSWERED where I could not fetch primary sources.

---

# Product-Based Creativity Assessment: The Consensual Assessment Technique and Its Descendants

## 1. The original protocol (Amabile, 1982)

Amabile, T. M. (1982). Social psychology of creativity: A consensual assessment technique. *Journal of Personality and Social Psychology, 43*, 997–1013. Eight studies, elementary-school and undergraduate samples, artistic and verbal domains.

The operational definition, verbatim from p. 1001: "A product or response is creative to the extent that appropriate observers independently agree it is creative. Appropriate observers are those familiar with the domain in which the product was created."

This is a deliberately *criterion-free* definition. Amabile's move was to accept the subjectivity of creativity judgment and take a theoretically neutral stance on criteria, rather than lose another decade to the definition debate. The measure's validity is grounded in the claim that domain experts are the final arbiters of what is creative in a domain (Kaufman, Baer, Cole & Sexton, 2008, p. 175).

The protocol conditions, as compiled by Cseh & Jeffries (2019) from Amabile (1982, 1996), Hennessey (1994), and Hennessey, Amabile & Mueller (2011), require that judges:

- have domain experience;
- rate creativity **independently and subjectively**, with no training, no discussion, and no specific guidance;
- rate creativity **relative to the specific sample and context**, not against an absolute or external standard;
- each see the items in a **different random order**, to defeat order effects;
- when a task is used for the first time, also rate **factors other than creativity** (technical execution, aesthetic appeal), with factor analysis run to establish that the creativity measure is discriminantly valid.

The task itself must be open-ended enough to permit varied responses.

Kaufman et al.'s (2013) instruction block for short stories is the cleanest published instance of the rating instructions, and is worth quoting in full because it is close to a drop-in judge prompt: rate creativity 1–10; **"compare the stories to one another, not to some other standard"**; these are not writers, they are experiment subjects; **use the full scale**; **no need to explain or justify your ratings**; judge creativity only, not spelling or punctuation.

Two structural properties follow from this design and both matter enormously downstream:

**There is no absolute scale.** Scores are only meaningful within the judged batch. A CAT score of 7.2 in one study is not comparable to a 7.2 in another. Cseh & Jeffries treat this as an under-discussed weakness; it is also, arguably, the source of the method's reliability.

**Reliability was treated as validity.** In Amabile's framing, inter-rater agreement *is* the construct validity of the CAT. If appropriate judges independently agree, the product is creative by definition. This is a circularity the field has largely tolerated, and it is precisely the assumption that LLM-judge work breaks (see §6).

## 2. Measured reliabilities

### Amabile's originals
Judge counts across the 1982 multi-study work ranged from **3 to 21**. In the collage study, psychologists, art teachers, and artists all rated above the .70 threshold: psychologists lowest at **α = .73**, art teachers highest at **α = .88**. Amabile herself flagged the awkward result on p. 1006 — there was "no clear superiority of artists over nonartists in interjudge reliability."

Typical reported range across the literature is **α = .70 to .90**, with .70 the conventional acceptability floor (Amabile, 1996; Kline, 2000).

### Judge-count guidance and the actual practice
There is no derived formula anywhere in this literature. What exists:

- Amabile (1982) declined to specify an ideal number; use whatever achieves acceptable agreement.
- Silvia et al. (2008, p. 81): "One is clearly not enough; 20 seems like overkill."
- Kaufman, Plucker et al. (2008, p. 74): **"for most purposes 5-10 judges is an adequate number."**
- In practice: Daly et al. (2016) used **2 judges** on 439 engineering student designs and reported agreement of **.70**. Valgeirsdottir et al. (2015) used **134** general-public judges. Karwowski et al. (2016, Study 8) reused Jauk et al. (2013) data in which each judge appears to have made **3,564 ratings** (297 participants × ~12 responses).

Cseh & Jeffries make a point that transfers directly to LLM ensembles: because Cronbach's α is a function of rater count, **increasing judges above ~10 inflates apparent consensus** and raises the risk of a Type I error on inter-rater reliability; going below five raises both Type I and Type II risk. A high α from a large panel is not evidence that the panel is any good.

### The judge-count vs reliability curve, measured
The best empirical curve in this literature is Kaufman et al.'s (2013) resampling design: draw 100 random subgroups of 10 novices, compute α in each, to strip out the group-size confound.

**Short stories (205 stories; Kaufman et al., 2013, Study 1):**

| Rater group | n | α |
|---|---|---|
| Experts (professional writers, 5 MFA, 3 PhD, all published) | 10 | .93 |
| Quasi-experts (all) | 41 | .97 |
| Quasi-expert subgroups | 9–12 | .86–.92 |
| Novices (all) | 106 | .99 |
| Novices, random samples of 10 | 10 | **.35–.75, mean .53** |

**Engineering (mousetrap designs; Study 2):**

| Scale | Experts (15) | Quasi-experts (31) | Novices (274) | Novices, random-10 (min/max/mean) |
|---|---|---|---|---|
| Overall creativity | .86 | .93 | .98 | .06 / .88 / **.66** |
| Elegance | .96 | .79 | .96 | .15 / .89 / .53 |
| Genesis | .85 | .87 | .96 | .06 / .78 / .53 |
| Novelty | .93 | .90 | .94 | .06 / .86 / .48 |
| Relevance/effectiveness | .83 | .97 | .97 | .06 / .89 / .59 |

The headline constant: **10 experts reach α ≈ .86–.93; 10 novices reach α ≈ .48–.66 on average and can land as low as .06.** Novice panels only reach expert-level internal consistency at panel sizes of ~100, and even then they are measuring something else (§3).

## 3. Expert vs quasi-expert vs novice: the correlations

This is the most decision-relevant body of numbers, because it is the human analogue of "can a cheap judge stand in for an expensive one."

**Poetry (Kaufman, Baer, Cole & Sexton, 2008; >200 poems, 10 expert poets, >100 novices):**
- Expert–novice correlation: **r = .22**
- Expert α = .83; novice groups of 10 α ≈ .58; full 106-novice α = .94
- Note the trap Cseh & Jeffries flag: novices *appeared* more reliable than experts (**.94 vs .83**) purely as an artifact of panel size. Equalized at n = 10, novice agreement drops to **.53**.

**Short stories (Kaufman, Baer & Cole, 2009; same design):**
- Expert–novice correlation: **r = .71**
- Expert α = .92; mean α of random 10-novice groups = .53; all 106 novices = .93
- The .71 required **more than 100 novice raters** to obtain.

**Short stories, full correlation matrix (Kaufman et al., 2013, Study 1, Table 1):**
- Experts × quasi-experts (all 41): **r = .89**
- Experts × novices (all 106): **r = .72**
- Experts × quasi-expert English teachers .86; × English majors .77; × education majors .80; × creativity students .91
- Experts × random 10-novice samples: **r = .22 to .76, mean .56**
- Quasi-experts × random 10-novice samples: r = .32 to .85, mean .66

**Engineering (Kaufman et al., 2013, Study 2, Table 3) — where it falls apart:**

| Scale | Expert × quasi-expert | Expert × novice (full) | Expert × novice (random-10 mean) |
|---|---|---|---|
| Overall creativity | **.52** | **.29** | .35 (min −.45) |
| Elegance | .60 | .38 | .42 |
| Genesis | **.02** | **−.16** | −.05 |
| Novelty | .71 | .08 | .15 |
| Relevance/effectiveness | .78 | .79 | .65 |

Quasi-experts correlated with **novices at .85** and with **experts at .52** — they had joined the wrong group. On "genesis" (does the design open up new perspectives), expert–quasi-expert agreement was **.02** and expert–novice was **−.16**: nonexistent, then negative.

**Other rater-expertise findings:**
- Gifted novice writers vs experts: **r = .78 (poetry), r = .77 (short stories)** (Kaufman, Gentile & Baer, 2005).
- Movies (Plucker, Kaufman, Temple & Qian, 2009): IMDb users × critics **.72**; IMDb users × students **.65**; **critics × students only .43**. Experts gave substantially lower mean scores. Agreement was highest between critics and the students with most film exposure — the expertise cutoff is a continuum, not a category.
- Fashion illustrations (Freeman, Son & McRoberts, 2015): 3 experts vs 3 novices, **no difference in agreement**.
- Children's musical compositions (Hickey, 2001): the three composer-judges **did not agree with each other at all** and their ratings had to be discarded. Theorists, three teacher types, and children each agreed internally; the children's ratings **did not correlate** with theorists' or teachers'.
- Flower designs (Lee, Lee & Young, 2005): novice inter-rater reliability low; rater-attributable variance much lower for experts; **product-based variance twice as large in experts as in novices** — experts discriminate between products, novices flatten them.

**The synthesis Kaufman et al. reach:** quasi-experts are acceptable substitutes in creative writing but not in engineering. Domains higher in Simonton's consensus hierarchy (physics, chemistry > biology > psychology > sociology > arts) have sharper expertise boundaries and tolerate substitution less. And a decomposition finding worth carrying forward: **novelty and relevance/effectiveness are the criteria least dependent on rater expertise** (relevance E×N = .79); **elegance and genesis are the most dependent** (genesis E×QE = .02).

## 4. Known limitations

**No absolute scale.** Ratings are explicitly relative-within-batch. Cross-study comparison is invalid. Change the batch and you change every score.

**Cost.** Cseh & Jeffries and Kaufman et al. both name expense as the binding constraint that has "largely limited the CAT to research use." Experts are scarce, busy, and want to be paid.

**Judge fatigue and item count.** Items per judge in the literature range from single digits to **3,564**. Amabile's own Study 1 suggested a link between time-on-task and inter-judge agreement: more items and longer sessions degrade the ability to compare relatively. Cseh & Jeffries state flatly that **no CAT study has established how many items a judge can hold in relative comparison at once** — an open question with a direct bearing on context-window design for LLM judges.

**Internal consistency ≠ absolute agreement.** Stefanic & Randles (2015, p. 281): if one judge is always two points above another, they are perfectly consistent and in complete disagreement about what a given level of creativity is. Most CAT studies report Cronbach's α, which is blind to this. Shrout & Fleiss (1979) identified six ICC variants, later expanded to ten (McGraw & Wong, 1996); Cseh & Jeffries note most researchers fail to declare which they used.

**Methodological scatter.** This is the central charge of Cseh & Jeffries (2019, *PACA* 13(2), 159–166). Practice varies on: judge expertise criteria, judge count (2 to 134), rating scale (3-point to 10-point, with "very little or no justification" typically offered), whether judges are trained (Friedrich & Mumford, 2009 permitted prior training and discussion — a direct protocol violation), whether creativity is separated from technical execution and aesthetic appeal or collapsed to one factor, whether judges are explicitly told to judge relative-to-sample (often unreported), and which reliability statistic is used. Their conclusion: inconsistency of method may be producing inconsistency in ratings, and the omissions in procedural reporting are "scientifically problematic." Note the honest limit — this is a narrative review, **not a coded audit**; they do not report the percentage of studies deviating from protocol. Anyone citing a deviation *rate* from this paper is citing something that isn't in it.

**Cross-cultural agreement: UNANSWERED.** Hennessey, Kim, Guomin & Weiwei (2008), "A multi-cultural application of the consensual assessment technique," *International Journal of Creativity and Problem Solving, 18*, 87–100, and Kaufman, Niu, Sexton & Cole (2010), "In the eye of the beholder: Differences across ethnicity and gender," *Journal of Applied Social Psychology, 40*, 496–511, are the two directly relevant papers. I confirmed both exist (they appear in Kaufman et al.'s 2013 reference list) but exhausted the session's web-search budget before retrieving their numbers. **Do not treat cross-cultural CAT agreement as established in either direction on the basis of this report.**

**Domain generality of judged creativity: UNANSWERED.** Baer's within-person cross-task correlation work (Baer, 1994, *CRJ* 7, 35–46 and related) is the relevant literature and I could not verify its coefficients. What I *can* verify is adjacent and weaker: reliability and expertise requirements differ sharply by domain (Kaufman et al., 2013), and Kaufman, Baer & Cole (2009) found novice–expert agreement higher for short stories than poetry — a domain × expertise interaction within a single broad domain.

## 5. Modern extensions

**Subscales.** Amabile's own guidance requires rating technical execution and aesthetic appeal alongside creativity on first use of a task, to demonstrate discriminant validity. In the 1982 collage field study this worked: a clean separation appeared between creativity judgments and judgments of technical goodness/aesthetic appeal. Practice since has fragmented — Baer (1993) instructs judges to *discount* technical execution; others rate it alongside; others measure a single creativity factor. Cropley & Cropley's functional-creativity model (used in Kaufman et al., 2013) decomposes to relevance/effectiveness, novelty, elegance, genesis. Cropley & Kaufman's (2012) **Creative Solution Diagnosis Scale** is a 27-item instrument in the same family.

**Scale granularity.** Amabile favored 5-point. Published CAT studies span 3-point to 10-point. Preston & Colman (2000) find optimal granularity at **5 to 7 points**, with reliability, validity, and discriminatory power all degrading outside that band. Kwon et al. (2017) justified a 3-point scale explicitly on rater-fatigue grounds — reviewers had to rate hundreds of ideas.

**Rank vs rating.** Amabile explored ranking, continuous scales, and low/medium/high categorization alongside the 5-point scale. Kaufman et al. (2008) and Jeffries et al. (2017) used a two-stage procedure: **first sort into low/medium/high, then apply a 6-point scale**. This is a coarse-to-fine ranking scaffold and is the most interesting under-cited design in this literature.

**Statistical machinery.** Myszkowski & Storme (same 2019 *PACA* special issue) argue for **Judge Response Theory** and McDonald's ω over Cronbach's α, with judge characteristics modeled explicitly rather than averaged away. For a statistician this is the most promising thread: it treats judge severity and discrimination as estimable parameters instead of noise.

**Whether judges must see the whole sample.** Amabile's protocol says relative-to-sample. Whether judges are actually *told* this is frequently unreported. Cseh & Jeffries name the "how many items can be held in relative comparison" question as entirely unstudied. **UNANSWERED, and it is the single most important unanswered question for porting the CAT to LLM judges.**

## 6. The bridge: CAT protocols with LLM judges

Every study below either invokes the CAT explicitly or is a direct methodological descendant. The pattern across all of them is consistent enough to state as a finding.

### Kränzle & Sharratt (2025), *Creativity and Innovation Management* 34(4), 991–1012
66 ideas from 144 Dutch entrepreneurs; 22 human experts using Amabile's CAT as the base framework; GPT-4 Turbo as judge in 7 configurations crossing zero-shot / spectrum-few-shot / random-few-shot × base CAT vs 27-item CSDS × temperature 0 vs 1. ([open-access PDF at CBS](https://research-api.cbs.dk/ws/portalfiles/portal/117889119/Creat_Innov_Manage_-_2025_-_Kranzle_-_Evaluating_Creative_Output_With_Generative_Artificial_Intelligence_Comparing_GPT.pdf))

- Human expert **ICC = 0.404**. GPT-4 self-consistency **ICC = 0.760–0.968, mean 0.914**.
- GPT-4 × expert alignment: **ρ = 0.24** (zero-shot, n.s. at p = .056) to **ρ = 0.31** (spectrum few-shot). After removing 4 alignment outliers, best config reached **ρ = 0.41**.
- **The granular rubric made it worse**: 27-item CSDS gave ρ = 0.13–0.16 vs 0.24–0.41 for holistic CAT. The difference was not itself significant (Steiger Z = 1.65, p = .10), so state it as *no evidence the rubric helps*, not as proof it hurts.
- Leniency: every configuration rated higher than experts (best config M = 4.10 vs expert M = 3.28).
- Range restriction: LLM SDs **0.19–0.34** vs expert SD **0.50**. One config's entire range was 3.5–4.3 on a 5-point scale.
- GPT-4 aligned better on usefulness (ρ = .31) than novelty (ρ = .14).

### Chakrabarty, Laban, Agarwal, Muresan & Wu (2024), CHI '24, "Art or Artifice?" ([arXiv 2309.14556](https://arxiv.org/abs/2309.14556))
Explicitly a CAT, operationalized as the Torrance Test of Creative Writing: 14 binary tests, 10 professional writers, 48 stories, 3 independent expert evaluations each.

- Expert IRR: **Fleiss κ = 0.41 mean** (range 0.27–0.66); at the aggregate 0–14 level, expert **ρ = 0.69**.
- New Yorker stories passed **84.7%** of tests; GPT-3.5 **<10%**; GPT-4 and Claude v1.3 **≈30%**.
- **LLMs given the identical 14 tests agreed with experts at Cohen's κ = 0.016 (GPT-3.5), 0.035 (GPT-4), −0.006 (Claude)**, with individual tests as low as −0.23. GPT-4 exceeded κ = 0.2 on 2 of 14 tests. This is the strongest negative result in the literature and it used a *binary* rubric, so binarizing does not rescue LLM judging.

### Ismayilzada, Stevenson & van der Plas (2025), ICCC ([arXiv 2411.02316](https://arxiv.org/abs/2411.02316))
431 five-sentence stories, human and LLM authors; 2 trained experts, 96 non-experts, 3 LLM judges; framed explicitly against Amabile (1982).

- Reliability: experts **ICC .62–.90**; non-experts **.43–.71**; **LLM judges .86–.94**.
- **Dimension collapse**: within-judge-group correlations among creativity/originality/surprise/effectiveness were **.83–.89 for experts, .64–.74 for non-experts, and .92–.99 for LLM judges**. LLM judges score one latent thing regardless of what you name the dimensions.
- Direction of preference: experts rated human stories **1.25 points higher**; non-experts rated AI stories **1.19 higher**; **LLM judges rated AI stories 1.85 higher** (t = 28.75, p < .001).
- The mechanism, from regressing ratings on automated metrics: expert creativity ratings rise with semantic diversity, surprise, and lexical diversity. **LLM-judge creativity ratings rise with lexical complexity and fall with semantic diversity, novelty, and surprise.** The sign is flipped on exactly the constructs that define creativity.

### Harel-Canada et al. (2024), EMNLP, Psychological Depth Scale ([arXiv 2406.12680](https://arxiv.org/abs/2406.12680))
Human Krippendorff **α = 0.72**; LLM judges **α = 0.88–0.96**; judge–human validity only **ρ = 0.51** (GPT-4o + Mixture-of-Personas), up to ρ = 0.68 on the best single dimension.

### Peer, Vinkers et al. (2025) ([arXiv 2502.03253](https://arxiv.org/html/2502.03253v1))
72 STEM experts on Design Problem Task solutions. **Human expert accuracy against true originality was r = 0.44 (no example) / 0.47 (with example)** — a sobering ceiling on the human side. Expert facet intercorrelations r = .45–.67. Under few-shot prompting, LLM facet intercorrelations rose **to above 0.99** — the same collapse Ismayilzada measured, found independently in a different domain. Inter-model agreement (GPT-4o vs Claude-3.5-Sonnet) on rating rationale: Cramér's V = 0.549–0.798.

### Organisciak, Acar, Dumas & Berthiaume (2023), *Thinking Skills and Creativity* 49:101356 ([ERIC full text](https://files.eric.ed.gov/fulltext/ED629879.pdf))
27,217 AUT responses. This is the one clear success, and the reason matters.

- **Fine-tuned** GPT-3 davinci: **r = .813** with human ratings.
- **Prompted** GPT-4: **r = .53 zero-shot, .66 at 5-shot, .70 at 20-shot**.
- Embeddings alone: r = .17–.29. Semantic-distance baseline: r = .120.
- **The human ceiling, measured on duplicated items: human–human r = .83; single judgment vs less-noisy mean r = .88.** So fine-tuned scoring sits at 92–98% of the human ceiling and prompting does not.

### Ranking vs rating for LLM judges
Liu et al. (2024), COLM, PAIRS ([arXiv 2403.16950](https://arxiv.org/abs/2403.16950)): on Newsroom, Spearman correlations went from direct scoring **.13–.17** → G-Eval **.26–.35** → **PAIRS-greedy .56–.65**. On SummEval, .11–.31 → .43–.54. But on **HANNA**, the creative-story benchmark, scoring ρ ≈ .19–.25 and PAIRS reached only **.24–.28**. Pairwise comparison is a large win on factual-quality judgment and a much smaller one on creative-quality judgment. Calibration methods were reported insufficient to fix direct scoring.

### Inter-run reliability
Haldar & Hockenmaier (2025), EMNLP Findings, "Rating Roulette" ([ACL 2025.findings-emnlp.1361](https://aclanthology.org/2025.findings-emnlp.1361.pdf)): same prompt, same settings, 3 runs, intra-rater Krippendorff α — MTBench: **Llama 3.1 = 0.265, DeepSeek-R1 = 0.507, Qwen-3 = 0.563**, against a human inter-annotator κ of 0.713. Extending to 5 and 10 runs produced **no significant change**: self-inconsistency is a property of model × task, not sampling noise you can average away. Temperature 0 raises self-consistency but degrades performance.

**Provenance caveat:** one source in this bundle, "Reliability without Validity" (arXiv 2606.19544), carries a 2026-06 identifier and cites model names I cannot independently corroborate. Its numbers (test–retest α ≈ 0.94, κ-deflation of 33.8–41.3 pp against raw agreement, position-bias flip rates 0.002–0.192) are directionally consistent with everything else here but should be treated as **UNVERIFIED provenance**. Its one genuinely useful contribution if it holds: raw agreement overstates chance-corrected κ by ~39 points, so an "85% agreement" LLM judge is a κ ≈ 0.48 judge.

## 7. FIT / COST verdict for LLM-graded evaluation

**Overall FIT: high, but as a source of *design constraints*, not as a validated method.** The CAT is the right ancestor to cite. It is not a warrant that LLM judges work.

### The one finding that should change how you build
**LLM judges are far more reliable than human experts and far less valid.** Every study with both numbers shows it:

| Study | Human reliability | LLM self-consistency | LLM × human validity |
|---|---|---|---|
| Kränzle 2025 | ICC .404 | ICC .914 | ρ .24–.41 |
| Ismayilzada 2025 | ICC .62–.90 | ICC .86–.94 | pro-AI bias +1.85 |
| Harel-Canada 2024 | α .72 | α .88–.96 | ρ .51 |
| Chakrabarty 2024 | κ .41 | — | **κ ≈ 0.00–0.04** |

Under Amabile's original epistemology, reliability *is* validity — agreement among appropriate observers constitutes creativity. **That equation breaks for LLM judges**, because they agree with each other and with themselves for reasons unrelated to the construct. Any eval that reports an ensemble's ICC as evidence of quality is making exactly the error this literature refutes. Reliability is a precondition you check and then set aside; validity has to be measured against held-out human ratings, separately.

### Protocol elements to port, with cost

**Blindness — port it, free.** Amabile's judges were blind to hypothesis and condition. For an LLM grader: strip model identity, provenance, and ordering signals from the artifact. This is nearly costless and the pro-machine bias findings (+1.85 points, Ismayilzada) make it mandatory.

**Relative-within-batch judgment — port it, moderate cost.** This is the highest-value transfer. Kaufman et al.'s explicit instruction — compare these to one another, not to some other standard — plus PAIRS's .13→.65 gain on direct scoring, plus the leniency and range-restriction findings (LLM SD 0.19 vs expert 0.50) all point the same way. Absolute Likert scoring by an LLM produces a compressed, inflated, near-constant score. Cost: pairwise is O(n²); the Kaufman/Jeffries **two-stage coarse-then-fine design** (sort into low/medium/high, then rate on 6 points within tier) is the cheap approximation and is already validated in the human literature. Caveat: the PAIRS gains on *creative* content (HANNA, .25→.28) are far smaller than on summarization. Do not budget for the summarization-sized win.

**Multiple independent judges — port with modification, and don't expect much.** In humans, independence buys real error cancellation: 10 experts reach α .86–.93. In LLMs, "independent" runs of the same model reach intra-rater α of only .27–.56 (Haldar & Hockenmaier) *and* more runs don't help. Ensembling across *different model families* is the defensible analogue — Peer et al.'s cross-model Cramér's V of .55–.80 suggests genuine partial independence there. Ensembling across seeds of one model is closer to asking one judge the same question twice.

**No rubric vs rubric — the evidence favors gestalt, contrary to standard eval practice.** Amabile deliberately refused to define criteria. The LLM evidence supports her: Kränzle's 27-item CSDS scored ρ = .13–.16 vs .24–.41 for holistic CAT; Chakrabarty's 14 binary criteria produced κ ≈ 0. The mechanism is visible in the dimension-collapse numbers — LLM sub-scores intercorrelate at .92–.99 (vs .83–.89 for experts), so a 27-item rubric is 27 restatements of one latent judgment plus 27 opportunities for the model to anchor on surface features. **A detailed rubric buys the appearance of rigor and, in the one CAT-anchored test, costs validity.** If you keep dimensions, verify empirically that they decorrelate; if they don't, you have one score wearing a costume.

**Decompose selectively, not exhaustively.** Kaufman et al. (2013) give the rule: **novelty and relevance/effectiveness survive low rater expertise** (relevance E×N = .79); **elegance and genesis do not** (genesis E×QE = .02, E×N = −.16). If you must have sub-scores, ask for the two robust ones. Do not ask an LLM whether a design "opens new perspectives" — human experts can't agree on that with each other.

**Domain matching — port it, and treat it as the expertise dial.** Simonton's hierarchy predicts where substitute judges fail, and Kaufman et al. confirmed it: quasi-experts worked for creative writing (r = .89 with experts) and failed for engineering (r = .52). Expect a general LLM judge to behave like a *novice* in codified domains (code correctness, engineering, statistics) and like a *quasi-expert* in writing. The Ismayilzada bias-direction result supports this directly: LLM judges pattern with non-experts, not experts, on every measure.

**Fine-tune if the domain matters.** Organisciak's numbers are the sharpest cost/benefit in this whole report: prompted GPT-4 r = .53–.70; fine-tuned on human labels r = .813 against a human ceiling of .83–.88. No prompting strategy in any paper here closed that gap. If you have or can collect a few thousand human-rated artifacts in your domain, fine-tuning is the only intervention with demonstrated ceiling-level performance.

### What the CAT cannot give you
**Absolute scores.** The CAT has no absolute scale by construction, and LLM judges inherit the problem in a worse form: they show both batch-relativity *and* fixed leniency (Kränzle: every configuration inflated). A CAT-derived LLM eval produces **within-batch rankings only**. Do not build a dashboard that tracks "creativity score" over time across different artifact batches; it is measuring batch composition. Judge fixed anchor items alongside each new batch if you need longitudinal comparison.

**A validity criterion.** The CAT is used as gold standard throughout — Chakrabarty, Ismayilzada, and Kränzle all validate against expert CAT ratings, and Organisciak validates against human ratings with an explicitly measured human–human ceiling. That last move is the one to copy: **measure your human–human agreement first**, and report LLM–human agreement as a fraction of it. Peer et al.'s r = .44–.47 for expert accuracy against true originality is a reminder that the ceiling may be much lower than you assume, and an LLM judge at ρ = .40 against a ceiling of .45 is a very different result than one at ρ = .40 against a ceiling of .88.

### Cheapest defensible design, given all of the above
Blind the artifacts. Batch them and instruct explicit within-batch comparison. Two-stage: coarse tier sort, then fine rating within tier. Holistic creativity judgment plus at most novelty and effectiveness as separate asks — and check that those two decorrelate before believing them. Ensemble across model families, not seeds. Hold out a human-rated subset, measure human–human agreement on it, and report LLM–human agreement against that ceiling rather than in absolute terms. Never report ensemble self-consistency as a quality metric.

### Open questions I could not close
- **Cross-cultural CAT agreement** (Hennessey et al., 2008; Kaufman, Niu et al., 2010) — sources identified, numbers not retrieved. UNANSWERED.
- **Within-person cross-task correlations** establishing domain specificity (Baer, 1994 and successors) — UNANSWERED.
- **How many items a judge can hold in relative comparison.** Unstudied in humans (Cseh & Jeffries state this explicitly) and unstudied in LLMs. This directly determines optimal batch size for any CAT-style LLM eval and nobody has an answer.
- **No study anywhere runs a full, unmodified CAT protocol** — domain-expert-defined task, no rubric, independent raters, within-judge normalization — with LLMs substituted for judges *and* reports those judges' inter-run reliability on the same items. Kränzle is closest but uses a CAT-derived rubric. That experiment is missing from the literature and would be cheap to run.