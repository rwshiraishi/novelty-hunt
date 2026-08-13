# Semantic-Distance Creativity Instruments: Technical & Validation Report

All primary sources fetched in full (PMC/publisher PDFs). Numbers below are transcribed from the source text.

---

## 1. SemDis (Beaty & Johnson, 2021)

**Citation:** Beaty, R. E., & Johnson, D. R. (2021). Automating creativity assessment with SemDis: An open platform for computing semantic distance. *Behavior Research Methods*, 53(2), 757–780. [doi:10.3758/s13428-020-01453-w](https://link.springer.com/article/10.3758/s13428-020-01453-w) · [PMC8062332](https://pmc.ncbi.nlm.nih.gov/articles/PMC8062332/) · app: `semdis.wlu.psu.edu` · OSF: [osf.io/gz4fc](https://osf.io/gz4fc/)

### Exact pipeline
1. Preprocess: strip special chars + numbers; optional stop-word removal (`tm` R package list). **No spellcheck** — unrecognized/misspelled words → missing (Johnson et al. 2019 reported 4.1% data loss from this). Optionally strip the cue word and its plural from responses.
2. Look up each word's vector in each of 5 spaces.
3. Compose multiword phrases into a single vector: **elementwise multiplication** (multiplicative composition, per Mitchell & Lapata 2008/2010). Additive is offered but explicitly not recommended.
4. Distance = **1 − cosine(prompt_vector, response_vector)**. Computed via `LSAfun` (Günther et al. 2015) in R. Stated range −1 to 1; observed means ~0.74–0.99.
5. Outputs: 5 per-space scores, **SemDis_MEAN** (arithmetic mean of the 5), and an optional **latent factor score** (CFA via `lavaan` `cfa()` + `lavPredict()`, item-level).

### The five spaces (exact specs from the paper)
| Label | Model | Corpus | Window | Dims | Vocab |
|---|---|---|---|---|---|
| `cbowukwac` | CBOW | ukWaC (~2B words) + subtitle corpus (~385M) | 12 (6L/6R) | 300 | 150k |
| `cbowsubtitle` | CBOW | subtitle corpus only | 12 | 300 | 150k |
| `cbowBNC` (`cbw`) | CBOW | BNC + ukWaC + 2009 Wikipedia dump (~800M tokens) | 11 | 400 | 300k |
| `TASA` | LSA (count + SVD) | TASA, 37,000+ docs, 92,393 word types (school textbooks + literary works) | — | 300 | — |
| `GloVe` | count-predict hybrid | Wikipedia 2014 + Gigaword (~6B tokens) | — | 300 | 400k |

Note the paper flags corpus overlap among the three CBOW spaces as a limitation on the latent-variable logic.

### Validation r values (all **person-level** unless noted)

**Study 1** (N=171, AUT box + rope, 2 min, 4 raters):
- Additive-composition latent SemDis × human ratings: **r = −.37** (14% variance) — *negative*.
- **Multiplicative latent SemDis × human ratings: r = .91** (83% variance). Model fit: χ²(132)=185.8, CFI .970, RMSEA .049.
- Individual-space × individual-rater zero-orders (Table 1) are far lower, **r ≈ .10–.49**: e.g. box GloVe × rater1 = .26; rope cbowukwac × rater4 = .54; TASA × rater3 (box) = .23.
- External: metaphor r=.41 (human .49); creative behavior r=.21 (human .43); creative self-efficacy r=.32 (human .36); fluid intelligence r=.10 n.s. (human .36); openness r=.03 n.s. (human .30).

**Study 2** (N=142, Silvia et al. 2017 reanalysis, 3 raters): latent SemDis × human **r = .75** (~56% variance). Zero-orders .13–.55, with TASA occasionally *negative* (b_tasa × b_r1 = −.06). Creative behavior: SemDis r=.03 n.s., human r=.16 n.s. Openness: SemDis r=.24, human r=.48.

**Study 3** (N=133, AUT brick, 10 min, 3 raters): latent SemDis × human **r = .45** (single item — the honest single-task number). Serial-order effect confirmed (b=.05/min). Openness r=.19; Gf r=.24.

**Study 4** (verb generation, Heinen & Johnson 2018 reanalysis; correlations computed on **participant-aggregated means**, n=62 / n=56, though factor scores are trial-level):
- Sample 1: creativity — GloVe .61, cbowukwac .55, TASA .54, cbowsubs .35, cbowBNC .32; **latent factor .49**. Novelty — GloVe .84, latent .73. Appropriateness — GloVe −.75, latent −.81.
- Sample 2: creativity — GloVe .78, cbowukwac .76, TASA .75; **latent .73**. Novelty — GloVe .97, cbowukwac .96, TASA .92, cbowsubs .91, cbowBNC .89; **latent .95**. Appropriateness — latent −.93.

**Study 5** (C-BAND noun association, n=58, 1,856 responses, 3 MTurk raters, α=.88): cbowukwac .82, cbowsubs .81, TASA .78, GloVe .77, cbowBNC .76; **latent × mean originality r = .85**.

**Critical interpretive point:** the headline r = .91/.85 figures are **latent-variable correlations between two disattenuated factors aggregated over persons/items**, not response-level agreement. The single-item, single-rater zero-orders are r ≈ .2–.5, and the response-level figures (below) are far lower still.

### The elaboration confound — as reported *inside* SemDis
- Prior: **Forthmann, Oyebade, Ojo, Günther & Holling (2019)**, *J. Creative Behavior* 53(4):559–575, [doi:10.1002/jocb.240](https://onlinelibrary.wiley.com/doi/abs/10.1002/jocb.240) — LSA-based distance is biased by elaboration (more words → higher cosine similarity → lower distance); demonstrated by simulation; mitigated by stop-word removal + simulation-based bias correction. (Exact r values in that paper: **UNANSWERED** — Wiley paywalled, no OA copy located.)
- Beaty & Johnson replicate it: with **additive** composition, word count × semantic distance **r = −0.25**, while word count × mean human creativity rating **r = +0.41**. That sign clash explains the −.37.
- With **multiplicative** composition the correlation flips to word count × distance **r = +0.47**, which they present as the fix. Note this is a *sign reversal, not decorrelation* — the metric is now confounded with length in the same direction as human raters, which makes the correlation look better without removing length dependence. They concede: "More work will be needed to more systematically investigate the role elaboration bias may still play."

### Known failure modes (SemDis authors' own)
- **Gameable:** "the semantic distance approach with the AUT can be 'hacked' if participants simply respond with random or task-unrelated words." Requires manual screening — a cost human raters don't incur.
- Measures **novelty, not creativity.** Correlations with appropriateness are strongly *negative* (up to −.93). It correlates near-unity with novelty ratings (.95–.97) and only moderately with creativity ratings (.49–.73) whenever raters weight usefulness.
- Single-item correlations drop sharply (r=.45 for brick alone vs .91 for the two-item latent).
- Factor approach needs n > 100 to converge; several reported models had fit above recommended cutoffs (Study 1 additive SRMR .113; Study 2 RMSEA .083).
- No spellchecking; misspellings silently drop.

### Follow-ups, critiques, and the damaging replication

**Beaty, Johnson, Zeitlen & Forthmann (2022)**, "Semantic Distance and the Alternate Uses Task: Recommendations for Reliable Automated Assessment of Originality," *Creativity Research Journal* — [PDF](https://bpb-us-e1.wpmucdn.com/sites.psu.edu/dist/c/122043/files/2022/09/2022-Beaty-CRJ.pdf). Item analysis of 20 AUT objects yields a recommended reliable 10-item set (**book, bottle, brick, candle, clock, comb, knife, lamp, pencil, pillow, purse, sock** appear in the selection text). Reliability estimates around ω ≈ .65 (authors argue this underestimates). They restate the elaboration bias as an open concern and recommend against certain composites.

**Organisciak, Acar, Dumas & Berthiaume (2023)**, "Beyond semantic distance: Automated scoring of divergent thinking greatly improves with large language models," *Thinking Skills and Creativity* 49:101356. [Free full text (ERIC ED629879)](https://files.eric.ed.gov/fulltext/ED629879.pdf). This is the single most important critique. Using SemDis_MEAN with the authors' own recommended settings (stoplist removal + multiplicative composition), on a federated corpus of **27,217 human-judged AUT responses from 2,039 participants across 9 datasets**, at the **response level**:

| Model | ALL | betal18 | bs12 | dod20 | hmsl | motesf | motesp | setal08 | snb17 | snbmo09 |
|---|---|---|---|---|---|---|---|---|---|---|
| **semdis-mean** | **.120** | .210 | .167 | .243 | .155 | .191 | **−.045** | .064 | .157 | **−.020** |
| ocs-main (GloVe, Dumas) | .256 | .319 | .178 | .371 | .364 | .257 | .337 | .328 | .193 | .295 |
| gpt3_emb-ada | .285 | .396 | .284 | .429 | .396 | .382 | .480 | .314 | .369 | .254 |

Headline: **"an average performance of r = .783 with human judges versus r = .188 across the baseline semantic distance systems."** On held-out unseen prompts, semdis-mean averaged **r = .19** (per-prompt: bottle .24, pants .20, paperclip .14, shovel .15, spoon .21) versus GPT-3-curie **r = .66**. Two datasets produced **negative** SemDis correlations. Fine-tuned Ocsai reaches r ≈ .81–.86. They explicitly note the elaboration confound remains unresolved: "the treatment for varying numbers of words has remained in debate, from term weighting (Dumas et al., 2020) to multiplicative composition (Beaty & Johnson 2021)."

The gap between r=.91 (Beaty & Johnson, latent, person-level, 2 items, N=171) and r=.12 (Organisciak et al., response-level, 9 datasets, N=27,217 responses) is the central psychometric fact about SemDis. It is not a contradiction — different units of analysis and disattenuation — but any system consuming SemDis as a per-item novelty signal should plan on **r ≈ .12–.25**, not .9.

**Dumas, Organisciak & Doherty (2020)**, "Measuring divergent thinking originality with human raters and text-mining models: A psychometric comparison of methods," *Psychology of Aesthetics, Creativity, and the Arts*. Compared TASA-LSA, EN_100k_lsa, GloVe 840B, word2vec-skipgram with **additive** composition and term weighting; reported GloVe as best. Exact per-model r values: **UNANSWERED** (paywalled; Beaty & Johnson characterize them as "strongly correlated" and "similarly high" but give no numbers).

**Dumas & Dunbar (2014)**, "Understanding fluency and originality: A latent variable perspective," *Thinking Skills and Creativity* — exact figures **UNANSWERED**.

**Multilingual:** Patterson, Merseal, Johnson, Agnoli, Baas, Baker, Barbot, Benedek, Borhani, Chen, et al. (2023), "Multilingual semantic distance: Automatic verbal creativity assessment in many languages," *Psychology of Aesthetics, Creativity, and the Arts* 17(4):495, [doi:10.1037/aca0000618](https://doi.org/10.1037/aca0000618). 28 researchers, >6,500 participants, **12 languages**: Arabic, Chinese, Dutch, English, French, German, Hebrew, Italian, Persian, Polish, Russian, Spanish. Two deep-learning models compared against human ratings; open dataset + Python code released. **Exact per-language r values: UNANSWERED** (not retrieved; full text not fetched). See [MPIEA release](https://www.aesthetics.mpg.de/en/newsroom/news/news-article/article/creativity-research-in-twelve-languages-1.html). A newer alternative: [S-DAT (arXiv 2505.09068)](https://arxiv.org/html/2505.09068), a multilingual GenAI DAT framework.

**Composite weighting critique:** Forthmann & colleagues, "Semantic Spaces Are Not Created Equal – How Should We Weigh Them in the Sequel? On Composites in Automated Creativity Scoring," *European Journal of Psychological Assessment* 39(6), [doi:10.1027/1015-5759/a000723](https://econtent.hogrefe.com/doi/10.1027/1015-5759/a000723) — directly targets the unweighted SemDis_MEAN. Exact findings: **UNANSWERED** (paywalled).

### Implementation cost
Trivial. Five pretrained static embedding tables (~1–2 GB total for 300–400d × 150k–400k vocab), one cosine per response, CPU-only, milliseconds per item. Or use the hosted app free. The cost is not compute — it's the r≈.12–.25 ceiling and the manual screening for gaming.

---

## 2. DSI — Divergent Semantic Integration (Johnson, Kaufman, Baker et al., 2023)

**Citation:** Johnson, D. R., Kaufman, J. C., Baker, B. S., Patterson, J. D., Barbot, B., Green, A. E., van Hell, J., Kennedy, E., Sullivan, G. F., Taylor, C. L., Ward, T., & Beaty, R. E. (2023). Divergent semantic integration (DSI): Extracting creativity from narratives with distributional semantic modeling. *Behavior Research Methods*, 55(7), 3726–3759. [doi:10.3758/s13428-022-01986-2](https://doi.org/10.3758/s13428-022-01986-2) · [PMC10615993](https://pmc.ncbi.nlm.nih.gov/articles/PMC10615993/) · code+tutorial+web app: **[osf.io/ath2s](https://osf.io/ath2s/)** (no GitHub repo cited in the paper — **UNANSWERED** whether a mirror exists).

**Construct:** "the extent to which a narrative connects divergent ideas." Note this is a **within-text** metric — unlike SemDis it needs no prompt.

### Exact computation
- Model: **BERT-large** (24 layers, 340M params). Cased/uncased variant not stated in the text: **UNANSWERED**.
- Text split into **sentences**; BERT run per sentence so embeddings are context-dependent.
- **Layers 6 and 7** selected (early-to-middle). Selection was empirical (all 24 layers analyzed in Supplement; layers 6–7 correlated highest with human ratings) plus theory (Jawahar et al. 2019: early/middle layers carry syntactic+semantic info).
- **Crucially, layers are not summed, averaged, or concatenated.** DSI = pairwise cosine semantic distance among **all word embeddings from both layers**, treating the two layers' vectors as members of one pooled set.
- Score = sum of all pairwise cosine distances ÷ number of word pairs → one scalar per narrative.
- For the five non-BERT (context-independent) models: strip punctuation **and stop words**, tokenize (word pieces, e.g. "bath"+"ing"), match to vectors, mean pairwise cosine distance.

### The six models compared
BERT (transformer) vs. LSA (count) vs. two/three CBOW variants vs. GloVe (count-predict hybrid), plus `conind_ave` = the mean of the five context-independent models (i.e. the SemDis-style composite).

### Validation — all 34 prompt-level correlations (Table 6, BERT DSI × mean human rating)

| Study | Source | Prompt | Mean words (SD) | N | sr_ave | **r [95% CI]** |
|---|---|---|---|---|---|---|
| 1 | new | stamp-letter-send | 59.8 (20.2) | 179 | .89 | **.77 [.70,.82]** |
| 3A | Taylor+21 | storyboard p1 (3 images) | 52.1 (43.6) | 125 | .86 | **.73 [.62,.81]** |
| 6 | new | stamp-letter-send (L1) | 80.4 (30.7) | 107 | .86 | **.66 [.54,.76]** |
| 2 | new | stamp-letter-send | 73.8 (23.4) | 153 | .84 | **.61 [.50,.70]** |
| 2 | new | petrol-diesel-pump | 73.2 | 153 | .87 | .59 |
| 4A | Ward+13 | shade | 39.8 | 86 | .77 | .59 |
| 2 | new | year-week-embark | 68.1 | 153 | .85 | .57 |
| 4A | Ward+13 | marriage | 38.7 | 86 | .74 | .56 |
| 2 | new | statement-stealth-detect | 68.3 | 153 | .79 | .55 |
| 3A | Taylor+21 | storyboard p4 | 65.0 | 125 | .82 | .55 |
| 4A | Ward+13 | joy | 42.1 | 86 | .70 | .55 |
| 2 | new | belief-faith-sing | 67.4 | 153 | .84 | .54 |
| 4A | Ward+13 | sky | 41.5 | 86 | .71 | .54 |
| 4A | Ward+13 | death | 44.7 | 86 | .69 | .54 |
| 2 | new | organ-empire-comply | 68.3 | 153 | .80 | .52 |
| 4B | Taylor&Kaufman20 | glow | 155.5 (69.3) | 141 | .76 | .52 |
| 6 | new | stamp-letter-send (L2) | 91.8 (33.2) | 119 | .84 | .52 |
| 4A | Ward+13 | delay | 43.7 | 86 | .69 | .52 |
| 3B | Kaufman+13 | execution/2305 (Engl MA) | 194.2 (172.3) | 202 | .67 | .50 |
| 4B | Taylor&Kaufman20 | frame | 158.2 (67.0) | 147 | .71 | .49 |
| 4A | Ward+13 | simplicity | 40.5 | 86 | .70 | .49 |
| 3B | | execution/2305 (Educ MA) | 194.2 | 202 | .67 | .48 |
| 3A | Taylor+21 | storyboard p3 | 61.4 | 125 | .83 | .48 |
| 2 | new | gloom-payment-exist | 69.0 | 153 | .82 | .47 |
| 3B | | (Novice) | 194.2 | 202 | .37 | .46 |
| 3B | | (English teachers) | 194.2 | 202 | .74 | .46 |
| 3B | | (Creativity students) | 194.2 | 202 | .73 | .46 |
| 3B | | (**Expert**) | 194.2 | 202 | .78 | **.43** |
| 4A | Ward+13 | illness | 41.4 | 86 | .72 | .43 |
| 4A | Ward+13 | enemy | 44.7 | 86 | .72 | .39 |
| 3A | Taylor+21 | storyboard p2 | 52.3 | 125 | .87 | .37 |
| 4A | Ward+13 | lie | 44.9 | 86 | .71 | .36 |
| 4C | Zedelius+19 S1 | superpower story | 442.6 (153.3) | 133 | .83 | **.35** |
| 4C | Zedelius+19 S2 | superpower story | 476.2 (158.5) | 128 | .77 | **.09 [−.09,.25]** |

`sr_ave` = average correlation of a single human rater with the rater mean (the human-agreement benchmark).

**Latent / aggregate results:**
- Study 1: BERT **r=.77** vs `conind_ave` **r=.54** (difference z=5.72, p<.001); BERT vs single-rater .84 was *not* reliably different (z=1.88, p=.06).
- Study 2 CFA (N=153, 7 prompts): latent DSI × latent human ratings **r = .85** [.78,.91], **"explaining over 72% of the variance"** — this is the source of the abstract's "72%" claim. RMSEA .058, SRMR .076, CFI .911. Authors caution: small N for a second-order CFA.
- Reliability (ω): Study 2 = **.88** (7 prompts); Study 3A = **.75** (4 prompts); Study 4A = **.88** (10 prompts).
- Incremental validity (Study 1): controlling word count, frequency, prevalence, AoA, readability, MTLD, DSI adds **ΔR² = .132** [.07,.20]. Lexical-only R² = .531. Standardized DSI b* = **.63** vs word count b* = **.32**.
- Convergent (Study 1): verb generation, openness, Gc, Gf, Gr all in the r = .20–.30 band.

### Length dependence — tested properly (Study 5)
Following McCarthy & Jarvis (2010): three corpora (Kaufman 90-word truncations n=60; Zedelius 200-word n=60; **LOB corpus** 2,000-word n=200), random non-replacement subsampling at many lengths. DSI **rises with length up to ~30 words (90-word stories), ~50 words (200-word), ~200 words (2,000-word), then stabilizes** in central tendency, variance, and distribution shape. Rank-order correlation between the stabilization-length score and full-story score **rs > .96** in all three corpora. Conclusion: length is **not** a computational artifact above ~50 words. Implication: DSI is unreliable on texts under ~30–50 content words.

### Known failure modes
- **Genre/length interaction (real, unexplained).** Validity falls monotonically with story length: r=.77 at ~60 words, r=.49–.52 at ~155 words, r=.35 and **r=.09 (n.s.)** at ~440–480 words. Study 5 rules out the computational artifact; the authors' explanation is substantive — longer stories let raters weight voice, imagery, character, world-building, and DSI captures only one component. For long-form text DSI is close to useless.
- **Explicitly gameable, and the authors say so:** "it seems unlikely that the current DSI algorithm could distinguish between highly creative texts and a collection of nonsense random words. If a random word generator produced a collection of words... would exhibit a high DSI score, despite not making any sense."
- **Construct gap:** "rooted in the semantic distance between words, not ideas."
- Expert raters give *lower* DSI correlations (.43) than novices (.46) or English MA students (.50) — DSI tracks the crowd, not expertise.
- Prompt sensitivity is large: within Study 2's 7 prompts, r ranged .47–.61. Low-semantic-distance prompts outperformed high-distance ones.
- Bias check (Study 6): L1-White r=.66 vs L2-Hispanic r=.52, difference n.s. (z=1.60, p=.109). L2-Hispanic scored slightly *higher* on DSI (d=.30, p=.029) with no human-rating difference — small directional bias, flagged as preliminary.
- Overall validity coefficient range across all prompts: **r = .09 to .77**. "Researchers should select the creative story task and prompt carefully."

### Implementation cost
BERT-large inference (340M params), sentence-level, two layers retained; pairwise cosine over all word tokens is O(n²) in tokens (for a 500-word story, ~250k pairs × 2 layers — trivial matrix op). GPU optional, CPU feasible. HuggingFace `bert-large` + ~1.3 GB weights. Or use the OSF web app. Dependencies: Python + transformers; R for the psychometrics.

---

## 3. Forward Flow (Gray, Anderson, Chen et al., 2019)

**Citation:** Gray, K., Anderson, S., Chen, E. E., Kelly, J. M., Christian, M. S., Patrick, J., Huang, L., Kenett, Y. N., & Lewis, K. (2019). "Forward flow": A new measure to quantify free thought and predict creativity. *American Psychologist*, 74(5), 539–554. [doi:10.1037/amp0000391](https://doi.org/10.1037/amp0000391) · [full PDF](https://mikechristian.web.unc.edu/wp-content/uploads/sites/13307/2019/01/Gray-et-al.-2019.pdf) · tools: `forwardflow.org`

### Exact computation
Instantaneous forward flow of thought *n* = its **average semantic distance from all preceding thoughts**:

FF(n) = ( Σ_{i=1}^{n−1} D(n,i) ) / (n−1)

Whole-sequence forward flow = the average of that across the sequence:

FF = [ Σ_{i=2}^{n} ( Σ_{j=1}^{i−1} D(i,j) / (i−1) ) ] / (n−1)

- D = LSA cosine distance, range 0–1, symmetric matrix with zero diagonal.
- Engine: **custom LSA, 300-dimensional** (per Deerwester et al. 1990). Training corpus documented only at forwardflow.org and in supplement — **corpus identity UNANSWERED** from the article body; the worked example ("bear…cake" = .66) is computed at lsa.colorado.edu.
- Task: participants get a **seed word + 19 blank lines** → a **20-word chain**, producing a symmetric **20×20** matrix per participant. Instruction: "starting with the word '[seed]', write down the next word that follows in your mind from the previous word. Please put down only single words, and do not use proper nouns." **Participants are not told to be creative.**
- Seed words varied within and across studies.
- Exclusions: ≥2 unprocessable words (e.g., "Nike", "Schnizzle"), or FF > 3 SD from mean. Exclusion counts are substantial (Study 1: 60 of 277; Study 4: 48).
- Key design property: it is *not* adjacent-pair distance; it is cumulative-history distance, so it is invariant to sequence length by construction.

### Validation and effect sizes (all reported values)

| Study | Sample | N (post-exclusion) | Result |
|---|---|---|---|
| Pilot | amateur actors | 35 (3 excl.) | FF × DT creativity **r(30)=.46, p=.008** |
| 1 | UNC undergrads | 277 (60 excl.) | FF × creativity **r(215)=.24, p<.001**; controlling cognitive ability **β=.24, p<.001**; FF × convergent thinking (RAT) **r(215)=−.01, p=.96** (discriminant) |
| 2 | representative US adults (Qualtrics panel) | 581 (62 excl.) | FF × creativity **r(517)=.12, p=.009**; controlling cognitive ability **β=.09, p=.036** |
| 3 | drama performance vs theater-history majors | 254 (29 excl.) | FF .79 (.04) vs .76 (.05), **t(223)=4.93, d=0.66**; creativity d=.91; groups equal on cognitive ability (p=.24). Logistic: FF B=−10.10, SE=3.44, p=.003 **controlling rated creativity**. Test–retest over a semester **r(167)=.40, p<.001** |
| 4 | professional actors vs MTurk | 52 vs 100 (48 excl.) | FF .83 (.04) vs .79 (.04), **t(104)=4.34, d=0.88**; creativity d=1.08. Pros vs amateur actors: **t(68)=4.38, d=1.05** while DT creativity was identical (**d=0.01, p=.96**) |
| 5 | entrepreneurs vs accountants (UNC B-school alumni) | 159 vs 169 (28 excl.) | FF .81 (.04) vs .79 (.05), **t(296)=3.70, d=0.43**; creativity only d=0.28. Logistic: FF B=9.21, SE=3.04, p<.003; rated DT creativity **not** predictive (p=.36) |
| Superanalysis | pooled Studies 1–5 + pilot | **N=1,397** | **r(1,395)=.19, p<.001**; controlling cognitive ability (Studies 1–3) **β=.20, t(958)=6.49**; mixed-effects with study random intercept **b=1.89, SE=.30, p<.001** |
| 6 | 100 most-followed Twitter accounts, ≤3,200 tweets each (summer 2016) | 97 (5 excl.) | FF of tweet stream × third-party-coded CAQ **r(95)=.22, p=.035** (3 coders, α=.77) |

Rater reliability across all creativity tasks: **α > .81**. Creativity tasks used: novel uses, draw-an-alien, caption, similarity, charity ideas.

### Known failure modes and limits
- **Small effects.** The honest pooled figure is **r ≈ .19** — about 3.6% of variance. Study 2 (the only representative sample) gives r=.12. The impressive numbers are all *group-difference* d's from known-groups designs, not prediction of individual creativity.
- **Confounded known-groups.** The authors concede "actors and MTurk participants likely differ on many characteristics." Entrepreneur/accountant and drama/history comparisons are correlational occupation contrasts, not creativity manipulations.
- **Gameability acknowledged but only indirectly tested.** Study 6 (Twitter) exists specifically to address "the (unlikely) possibility that participants may have been trying to generate outlandish associations to seem creative." A participant told to maximize FF trivially can (emit unrelated words) — the design defense is that they aren't told to.
- **High exclusion rates** driven by proper nouns and out-of-vocabulary words (LSA has no OOV handling). ~10–30% of participants dropped in several studies.
- **LSA-specific fragility**, acknowledged: "dependency on the specific text corpora and the specific number of dimensions."
- Modest test–retest (r=.40 over a semester) — it's partly a state measure.
- No convergent relation to convergent thinking (r=−.01) — this is presented as discriminant validity, but it also means FF is not measuring general ability.
- Twitter data collection predates the 2016 election and the API restriction to 6 days; **Study 6 is not reproducible today** as specified.

### Implementation cost
Lowest of the three. One 300-d LSA space, a 20×20 cosine matrix per participant, pure arithmetic. Task takes "a minute or two." The binding constraints are the LSA vocabulary (proper nouns fail) and the 20-word chain protocol.

---

## Cross-cutting synthesis

1. **Unit of analysis is the whole game.** Every headline number here is a latent, aggregated, disattenuated correlation. At the level a generator would actually use — score this one response / this one story — SemDis is r ≈ .12–.25 (Organisciak et al., 27k responses), DSI is r ≈ .35–.77 depending on length, forward flow is r ≈ .19.
2. **All three are gameable in the same way, and all three papers admit it in print.** Random unrelated words maximize every one of these metrics. None of the three has a nonsense detector. Any system using them as a reward signal will be Goodharted immediately.
3. **All three measure novelty/remoteness, not creativity.** SemDis's own data is the cleanest proof: r = .95 with novelty ratings, r = −.93 with appropriateness ratings, r = .73 with creativity. Distance is one axis of a two-axis construct.
4. **Length dependence is unresolved for SemDis, resolved-but-substantive for DSI.** SemDis's multiplicative "fix" flips the sign of the word-count correlation (−.25 → +.47) rather than removing it. DSI has a genuine floor (~30–50 words) and a genuine validity decay with length that is not artifactual.
5. **Fine-tuned LLM scoring dominates all of them on AUT** (r = .783–.86 vs .188), at the cost of needing labeled training data and losing explainability.

### UNANSWERED
- Exact r values inside Forthmann et al. (2019) — paywalled, no OA copy found.
- Exact per-model r values in Dumas, Organisciak & Doherty (2020) — paywalled.
- Dumas & Dunbar (2014) figures.
- Per-language r values in Patterson et al. (2023) multilingual paper.
- Forthmann's EJPA composite-weighting findings.
- BERT-large cased vs uncased in DSI.
- Forward flow's LSA training corpus identity (in supplement/website only).
- Whether DSI has a GitHub mirror beyond osf.io/ath2s.
