All four sub-agents stalled permanently after resuming (the shared web-search budget for the session is exhausted at 200/200, and they hit repeated 429s). I recovered what they had retrieved and pulled the load-bearing primary sources myself. Report follows.

---

# Theories and Definitional Frameworks of Creativity — and What Each One Commits a Measurement Instrument To

## 0. Verification status (read this first)

This session's web-search budget was exhausted and four parallel sub-agents died mid-task. I therefore separate claims into three tiers, and you should treat them differently:

- **[V]** — verified in this session against the primary source full text, which I fetched and extracted. Quotes and numbers are transcribed from the actual document.
- **[S]** — from a search-engine abstract/summary retrieved this session. Citation is reliable; exact numbers are second-hand.
- **[B]** — background knowledge, **not verified this session**. Citations are almost certainly correct as bibliography; any specific number attached to them should be re-checked before you rely on it.

Anything I could not establish at all is listed in Section 11 as **UNANSWERED**. I have not upgraded a remembered number into a reported finding.

---

## 1. The standard definition and its actual lineage

**[V] Runco, M. A., & Jaeger, G. J. (2012). The Standard Definition of Creativity. *Creativity Research Journal*, 24(1), 92–96.** DOI 10.1080/10400419.2012.650092. I extracted the full text; the following are transcribed.

The definition is bipartite: "Creativity requires both originality and effectiveness." Their argument for two criteria rather than one is the anti-noise argument, and it is sharper than it usually gets quoted:

> "Originality can be found in the word salad of a psychotic and can be produced by monkeys on word processors. A truly random process will often generate something that is merely original."

This is the load-bearing justification for the second criterion, and it is worth noticing what it actually is: a *demarcation* argument, not a positive theory of value. Effectiveness is introduced to exclude noise, not because anyone has a theory of what effectiveness is. Runco and Jaeger themselves list its labels as interchangeable: "usefulness, fit, or appropriateness," and separately "value."

**Provenance.** They credit **Stein, M. I. (1953). Creativity and culture. *Journal of Psychology*, 36, 311–322** with the first unambiguous statement, quoting him at pp. 311–312:

> "The creative work is a novel work that is accepted as tenable or useful or satisfying by a group in some point in time … By 'novel' I mean that the creative product did not exist previously in precisely the same form … The extent to which a work is novel depends on the extent to which it deviates from the traditional or the status quo."

Two things in Stein are routinely dropped when people cite the standard definition, and both matter for automated grading. First, **"accepted … by a group in some point in time"** — the definition is *indexed to a judging population and a date*. Second, Stein already distinguished **internal and external frames of reference**, which is the P/H distinction forty years before Boden.

**Precursors Runco and Jaeger examine and reject** (all [V]):
- Barron, F. (1955). The disposition towards originality. *J. Abnormal and Social Psychology*, 51, 478–485. He states two criteria — uncommonness, and that a response "must be to some extent adaptive to reality … to exclude uncommon responses which are merely random, or which proceed from ignorance or delusion" (p. 479). Rejected because both criteria are criteria *for originality*, not for creativity.
- Guilford, J. P. (1950). Creativity. *American Psychologist*, 5, 444–454. Guilford operationalized originality as "the frequency of uncommon, yet acceptable, responses to items" (p. 452) — **this single sentence is the origin of frequency-based originality scoring and it already contains the appropriateness constraint that later uniqueness-scoring practice dropped.** Rejected as a definition because his framing is largely tautological ("creativity refers to the abilities that are most characteristic of creative people," p. 444).
- Also examined: Bethune (1839), Royce (1898), Hutchinson (1931), Bruner (1962, "effective surprise," p. 18), Cropley (1967), Jackson & Messick (1965, "appropriate"), Kneller (1965, "relevant"), Cattell & Butcher (1968) and Heinelt (1974) on *pseudocreativity* and *quasicreativity* — products original but not worthwhile.

**The authors' own concession, which is the crux for your use case [V]:**

> "…only pinpoints which criteria must be used; it does not say anything about who is to judge each, and who is to judge the judges."

They attribute the question to **Murray (1958)**: "Who is to judge the judges? And the judges of the judges?" The standard definition is, by its own authors' admission, **silent on the judge**. Every operationalization must therefore supply a judge specification that the definition does not license. An LLM grader panel is not deviating from the standard definition by being an arbitrary judge — the definition never constrained the judge at all. That cuts both ways, and I return to it in Section 10.

---

## 2. Challenges to the standard definition

### 2.1 Simonton's three-criterion definition (novelty × utility × surprise)

**[S] Simonton, D. K. (2012). Taking the U.S. Patent Office Criteria Seriously: A Quantitative Three-Criterion Creativity Definition and Its Implications. *Creativity Research Journal*, 24(2–3), 97–106.**

The move is to import the USPTO's three statutory criteria — new, useful, nonobvious — and note that "nonobvious" is the inverse of "surprising." This makes Boden's *novel/valuable/surprising* and patent law's triad the same triad. Simonton's claim is that the definition thereby becomes **necessary and sufficient**, where the bipartite version is only necessary.

**[S] Simonton, D. K. (2018). Defining Creativity: Don't We Also Need to Define What Is Not Creative? *Journal of Creative Behavior*, 52(1), 80–90.** Formalizes it with three parameters: initial probability *p*, final utility *u*, and the creator's prior knowledge of that utility *v*, giving

  **c = (1 − p) · u · (1 − v)**

where (1 − p) is originality and (1 − v) is surprise. **The multiplicative form is the substantive claim, not the decoration**: it means creativity is zero if *any* factor is zero, and it generates "seven different ways creativity can minimize."

**[V] Runco & Jaeger's contemporaneous framing:** they note Simonton "made a strong case for three criteria — surprise being the third — and Runco (in press) raised the possibility that only **one** criterion is needed … Runco felt that parsimony was the best guide." So in the same 2012 issue of the same journal, the field's own summary paper records live proposals for one, two, and three criteria. **There is no consensus definition to operationalize; there is a consensus *slogan*.**

Note also that the third criterion is *doubly indexed*: (1 − v) is surprise **to the creator**, which a blind artifact grader cannot observe even in principle. Simonton's own formula contains a term that is unmeasurable from the product. This is not a flaw in the theory; it is a flaw in the fit between the theory and product-only assessment.

### 2.2 Corazza's dynamic definition

**[S] Corazza, G. E. (2016). Potential Originality and Effectiveness: The Dynamic Definition of Creativity. *Creativity Research Journal*, 28(3), 258–267.** DOI 10.1080/10400419.2016.1195627. (Full text is paywalled; tandfonline returned 403.)

The definition: **"Creativity requires *potential* originality and effectiveness."** One inserted qualifier. His stated motivations are precisely the three holes in the standard definition: who is entitled to assess originality and effectiveness; what happens when experts do not agree or the assessment changes across time and culture; and what to do when a genuine creative process yields no product meeting the criteria. That third case gets the label **"inconclusive outcome"** rather than failure.

Follow-on: **[S] Corazza & Lubart's Dynamic Creativity Framework (2020, and Corazza, Agnoli & Mastria 2022)**, adding the Dynamic Universal Creativity Process with concatenation, dynamic estimation, and exaptation.

**Blunt assessment.** The dynamic definition is a real fix for a real problem in *process* research — it decouples creative achievement from creative process, so a lab can study creativity without conditioning on success and thereby selecting on the dependent variable. **For product grading it is close to useless and possibly harmful.** "Potential" originality is unobservable from a finished artifact; adopting it in a grader rubric converts a scoreable criterion into an unscoreable counterfactual, and in practice graders will silently substitute their own guess about what the artifact could have become. Take Corazza seriously as a warning about what your score *is not* (it is an achievement attribution, not a process measure), and do not put "potential" in a rubric.

### 2.3 Does "usefulness" fail for the arts?

This is the most consequential challenge for artifact grading and the one I could least verify.

**[V]** The tension is visible inside Runco & Jaeger themselves: their list of labels for effectiveness slides from "usefulness, fit, or appropriateness" to "value," and they quote **Bethune (1839)** on "mingl[ing] the useful with the beautiful" — i.e., the historical usage treats aesthetic value and utility as *different things* that both count. Their fallback to "value" is the tell: when the domain is art, "useful" is quietly swapped for "valuable," which is a different criterion wearing the same slot.

**[V]** They also record that Runco's own position at the time was that **one criterion (originality) suffices**, on parsimony grounds — which is the strongest available form of "usefulness fails."

**[B, unverified]** The canonical explicit argument is **Weisberg, R. W. (2015). On the usefulness of "value" in the definition of creativity. *Creativity Research Journal*, 27(2), 111–124**, arguing value should be dropped from the definition (partly because it makes creativity retrospective and makes malevolent or failed-but-brilliant work definitionally uncreative). I could not fetch it; treat the characterization as UNANSWERED in detail.

**What this means operationally.** In a rubric, the second criterion is not one construct. It is at least four that come apart in the arts:
1. **Appropriateness / task-fit** — does it satisfy the brief? (Scoreable, high agreement, low discriminating power.)
2. **Craft quality** — is it well made? (Scoreable, high agreement; *this is what graders default to*.)
3. **Aesthetic value** — is it good? (Scoreable, low agreement, high variance across judges.)
4. **Utility** — does it work for something? (Often undefined in art; graders will confabulate a purpose if forced to score it.)

Collapsing these into one "effectiveness" column is the single most common rubric design error the theory literature predicts, because the standard definition licenses the collapse and nothing in it warns you against it.

---

## 3. Boden: what is measurable and what is not

**Sources.** Boden, M. A. (1990/2004). *The Creative Mind: Myths and Mechanisms*. Boden, M. A. (1998). Creativity and artificial intelligence. *Artificial Intelligence*, 103(1–2), 347–356. **[B]** for Boden's own text — I did not obtain it. **[V]** for Ritchie's exposition and critique, below, which I did obtain in full.

**Tri-criterion:** novel, surprising, valuable.

**P- vs H-creativity [V, as stated by Ritchie 2007, p. 72]:** H-creativity is "wholly novel within the culture"; P-creativity is "original as far as the creator is concerned, even though it might have been proposed or generated elsewhere in the culture, perhaps much earlier in history." Ritchie's gloss on why it matters is the useful part: "it clarifies what evidence is or is not relevant."

**Three types [B]:** *combinational* (novel juxtaposition of familiar ideas), *exploratory* (generating structures within an existing conceptual space by following its rules), *transformational* (altering or dropping one of the space's defining dimensions or constraints, so that structures previously impossible become possible). The conceptual space is a generative rule system; transformational creativity is second-order — an edit to the generator, not to the output.

### 3.1 Ritchie's formalization, and the fact that it bottoms out in human ratings

**[V] Ritchie, G. (2007). Some Empirical Criteria for Attributing Creativity to a Computer Program. *Minds and Machines*, 17(1), 67–99.** DOI 10.1007/s11023-007-9066-2. I fetched and extracted the full text; the following is transcribed.

Ritchie's key structural move — and it is the most useful single idea in this entire literature for your purposes — is that **novelty is not a primitive judgment**. He takes two properties as primitive, adds a third prerequisite, and *derives* novelty:

- **Typicality**: "To what extent is the produced item an example of the artefact class in question?"
- **Quality**: "To what extent is the produced item a high quality example of its genre?"
- **Novelty** (derived): "To what extent is the produced item dissimilar to existing examples of its genre?"

> "In our formulation below, we have chosen to make typicality and quality primitive elements, describing the (very important) concept of novelty in terms of other constructs, including typicality."

He decomposes novelty into **two** factors: (a) low typicality — untypicality relative to the genre; and (b) dissimilarity from an explicit **inspiring set** *I* ⊆ *B*, the subset of basic items the program was built from or exposed to. Formally the framework is a tuple ⟨*B*, typ, val⟩ with an inspiring set *I* and a result set *R*, and the criteria are ratios and thresholds over those.

Ritchie also notes explicitly that **typicality and novelty trade off**: "high novelty may raise questions about, or suggest a low value for, typicality."

**And here is the honest part, verbatim [V, p. 74 region]:**

> "Both typicality and quality would normally be determined by human assessment of basic items (potential artefacts)."

> "The intention is that both typicality and quality will usually be assessed by human judgement and may therefore be partly or wholly subjective."

So the most rigorous formalization of Boden in the literature is **a calculus over human ratings, not a replacement for them**. Its contribution is that it tells you *which* ratings you need (typicality, quality) and *what else* you must specify (the inspiring set), and it derives the rest. It does not eliminate the judge.

Ritchie's further honesty about why he includes the inspiring set: it is needed because a program's output can only be called novel *relative to something*, and the analyst must state what. He even treats *I* = ∅ as a distinct meaningful case (a program built with no exemplars at all), separate from "the analyst does not know what the inspiring set was."

**[V, incidental but useful]** He validates the scheme against a real system: the JAPE joke generator was evaluated by human judges on exactly two questions — "is this item a joke?" and "how funny is this item?" (Binsted et al. 1997) — which Ritchie notes "correspond directly to the typicality rating and the value rating of our framework."

**[B]** He also leaves the *rating scheme* deliberately abstract — "a checklist of properties, perhaps with weightings attached" — and states plainly "At present, we have no firm proposals on what this information should be." **[V]** That is the honest admission that the formalism is a frame, not an instrument.

### 3.2 Wiggins' Creative Systems Framework

**[B, unverified in detail] Wiggins, G. A. (2006). A preliminary framework for description, analysis and comparison of creative systems. *Knowledge-Based Systems*, 19(7), 449–458.** Formalizes Boden with a universe *U*, a rule set *R* defining the conceptual space, a traversal strategy *T*, and an evaluation function *E*; exploratory creativity is search under ⟨*R*, *T*, *E*⟩, transformational creativity is search in the *space of* ⟨*R*, *T*, *E*⟩ triples — a meta-level move. The framework is widely cited as the cleanest statement of *why* transformational creativity is categorically different.

**[V, indirect]** Ritchie explicitly positions his own work as *not* doing what Wiggins does: "this is not intended as a model of creativity (unlike Wiggins (2001, 2006a, b), for example)." Ritchie's aim is empirical criteria applicable to an existing program's outputs; Wiggins' aim is a descriptive model of the system. **The distinction is exactly the one you need**: Wiggins describes the generator, Ritchie scores the products. A blind artifact grader is structurally in Ritchie's world and structurally locked out of Wiggins'.

### 3.3 Blunt verdict on Boden

**Measurable:** the P/H distinction, *provided you make it a reference-corpus decision* rather than a mental-state claim. "Novel relative to corpus C" is exactly Boden's P/H axis with C as the free parameter, and Ritchie's inspiring set is the machinery for it.

**Not measurable from products, and I would not try:** the combinational/exploratory/transformational trichotomy. It is a claim about **the generative process and its rule system**, not about the artifact. Two artifacts can be pixel-identical with one produced exploratorily and one transformationally. A blind grader asked "is this transformational?" will answer a different question — usually "is this unusual?" — and you will get a number that looks like a transformationality score and is actually a novelty score with extra noise and a misleading label. **I found no evidence of any reliable measurement of transformational vs exploratory creativity from artifacts, and I looked; see UNANSWERED.**

**Beautiful but unmeasurable, ranked:** transformational creativity (worst offender — the most cited and least operationalized idea in the field); conceptual spaces as literal rule systems (no procedure exists for recovering a domain's rule system empirically); surprise-to-the-creator.

---

## 4. The 4C model

**[B] Kaufman, J. C., & Beghetto, R. A. (2009). Beyond big and little: The Four C Model of Creativity. *Review of General Psychology*, 13(1), 1–12.**

- **mini-c** — novel and personally meaningful interpretation of experience; intrapersonal, learning-centered, invisible to any external observer.
- **little-c** — everyday creativity, recognizable to others.
- **Pro-c** — professional-level expertise, not eminent.
- **Big-C** — eminent, historically durable contribution.

**Instrument-to-level mapping** (this is the model's real contribution and it is [B] as a synthesis, though each pairing is standard):

| C level | Instruments that target it | What the score is |
|---|---|---|
| mini-c | self-report, think-aloud, learning journals, insight self-report | subjective process report; no artifact |
| little-c | divergent thinking tests (TTCT, AUT), CAT on non-expert products, self-report inventories (e.g., CBI, K-DOCS) | rated attributes of everyday products/responses |
| Pro-c | CAT with domain experts, professional output records, peer nomination | expert consensus on domain-competent work |
| Big-C | historiometry, eminence counts, citation/canonical inclusion | field-level retrospective attribution |

**The measurement implication is the whole point:** these instruments do not measure the same construct at different intensities. They measure different things, and the correlations between them are modest. A divergent-thinking score is not a weak proxy for eminence; it is a measure of a different C.

**Critiques [B, unverified]:** The most common serious criticism is that 4C is a **taxonomy, not a theory** — it labels regions of a continuum without specifying transition mechanisms or providing a measurement model with discriminant validity. The mini-c/little-c boundary in particular is defined by whether anyone else has noticed, which makes it a social fact rather than a property of the creator. Kaufman & Beghetto have revised and extended it (including the "In praise of Clark Kent" treatment of little-c/Pro-c, 2013 [B]).

**Domain specificity — Amusement Park Theoretical model [B]. Baer, J., & Kaufman, J. C. (2005). Bridging generality and specificity: The Amusement Park Theoretical (APT) model of creativity. *Roeper Review*, 27(3), 158–163.** Four hierarchical levels: initial requirements (general), general thematic areas, domains, micro-domains. The claim is that creativity is general at the bottom and increasingly specific as you ascend.

**Why APT matters more than 4C for your problem:** it predicts that **a single grader rubric will not generalize across domains**, and that the amount of shared variance shrinks as you move to finer domains. If you use one "creativity" rubric across poetry, code, and experimental design, APT says you are measuring the general thematic level at best, and any domain-level signal you claim is unlicensed.

**Empirical backup [V, from the Kaufman & Baer "Gold Standard" chapter]:** they argue against *total* domain specificity on the grounds that if creativity were fully domain-specific, cross-domain creativity would be uncorrelated rather than negatively correlated, and abilities would be randomly distributed across domains. So the field's own position is "substantially but not totally domain-specific" — which for a rubric designer means *domain-specific rubrics, with a small general component you should not overclaim*.

---

## 5. Systems and confluence models — where the judge becomes part of the construct

### 5.1 Csikszentmihalyi's systems model

**[B] Csikszentmihalyi, M. (1988). Society, culture, and person: a systems view of creativity.** In R. J. Sternberg (Ed.), *The Nature of Creativity*, 325–339. Also **(1996) *Creativity: Flow and the Psychology of Discovery and Invention*** and **(1999) Implications of a systems perspective for the study of creativity**, in Sternberg (Ed.), *Handbook of Creativity*, 313–335.

Three interacting subsystems: the **domain** (a symbolic system of rules and content), the **field** (the gatekeepers who select which variations enter the domain), and the **person** (who produces variation). The strong claim is that **creativity is not a property of a person or a product; it is an attribution made by the field.** The often-quoted reformulation of the research question is from "what is creativity?" to "where is creativity?"

I could not retrieve the exact 1988/1999 wording (the source PDF 404'd), so **the verbatim quotes are UNANSWERED**; the substance above is standard and safe.

**The measurement implication, stated precisely:** if creativity is a field attribution, then a creativity score is a **function of the judge population**, and the judge population is **part of the construct, not measurement error**. You cannot average judges toward a true score, because there is no judge-independent true score to converge on. Changing the judge panel does not add noise; it changes what is being measured.

**Who says this explicitly:** Amabile's CAT is the operationalization that *accepts* this rather than fighting it.

### 5.2 Amabile's CAT — the systems model turned into a procedure

**[B] Amabile, T. M. (1982). Social psychology of creativity: A consensual assessment technique. *JPSP*, 43(5), 997–1013.**

The definitional core: a product is creative to the extent that **appropriate observers independently agree it is creative**. This is a deliberately *operational* definition — it declines to specify the features that make something creative and instead specifies the procedure. Judges must be **appropriate observers** (familiar with the domain), must work **independently**, must **not** be given a definition of creativity, and must rate products **relative to one another** rather than against an absolute standard.

**[V, from the Kaufman & Baer "Gold Standard" chapter, which I fetched]:**
- Expert inter-rater reliabilities in CAT studies "typically exceed .70, and often range as high as the .90s"; one cited range is **.72 to .93**, another **.70 to .89**; the general claim is reliabilities "typically in the .70-to-.90 range."
- **[V]** From Amabile's original collage work, correlations of specific judged attributes with creativity ratings: **novel use of materials .81**, **complexity .76**, **aesthetic appeal** (positive, high), versus **technical goodness .13**, **neatness −.26**, **expression −.05**. This is a genuinely useful result: expert creativity judgments are driven by novelty-flavored attributes and are essentially *orthogonal or mildly negatively related to craft-tidiness attributes*. If your rubric's "quality" column is really measuring neatness, it is measuring something that correlates **−.26** with expert creativity judgment.
- **[V]** The chapter also states the CAT's epistemic position plainly: "the experts in a given domain" are the "valid yardstick," and concedes "experts don't always agree and expert opinion may change over time."

### 5.3 Can novices replace experts? The single most important empirical result for your use case

**[V] Kaufman, J. C., Baer, J., Cole, J. C., & Sexton, J. D. (2008). A Comparison of Expert and Nonexpert Raters Using the Consensual Assessment Technique. *Creativity Research Journal*, 20(2), 171–178.** DOI 10.1080/10400410802059929. **I fetched and extracted the full text. All numbers below are transcribed from it.**

Design: **10 expert raters** (poets), **106 nonexpert raters** (students), **205 poems**.

Results:
- **Expert–novice correlation of mean ratings: r = .21**, which the authors immediately gloss as "**far too low (4.41% shared variance**) to suggest that nonexpert ratings can be substituted for those of experts without changing the outcome."
- **Expert inter-rater reliability: coefficient alpha = .935** (95% CI .926–.944) — "excellent."
- **Spearman-Brown standardized to 10 raters per group**: experts drop to **.804** (95% CI .771–.835); students drop **"massively" to .575** (95% CI .562–.588).
- Experts rated systematically **lower**: expert mean **3.09** (SD .90, range 1.25–5.21) vs novice mean **4.47** (SD .87, range 2.56–6.88) on a 1–6 scale.
- The authors' conclusion, verbatim: "**The results of this study clearly do not suggest that experts are easily replaceable.**" And: "these results also suggest a need for caution in using nonexperts to rate other kinds of creative products."
- **[V]** They also cite a prior finding (Kaufman, Gentile & Baer 2005) where gifted-novice ratings *did* correlate significantly with experts, and note gifted novices "may fall somewhere in between the two groups."
- **[V]** A referenced related finding: two groups' scores "were significantly correlated for **originality**, but there was no relationship for **craftsmanship** and **quality**." Novices and experts converge on originality and diverge on quality — which is precisely the decomposition Section 2.3 argued for.
- **[V]** Intergroup correlations in a cited collage study "ranged from **.44**" (art-expert vs psychologist raters), with the authors noting the psychologists "cannot be considered true nonexperts."

**This result is the empirical heart of the FIT verdict.** Two rater panels, both rating the same 205 artifacts, both nominally measuring "creativity," share 4.4% of variance. High internal reliability within a panel (α = .935) tells you nothing about whether the panel is measuring what another panel measures. **Reliability is not validity, and in this literature the gap between them is enormous.**

### 5.4 Sternberg & Lubart's investment theory

**[B] Sternberg, R. J., & Lubart, T. I. (1991). An investment theory of creativity and its development. *Human Development*, 34(1), 1–31; (1995) *Defying the Crowd*.** "Buy low, sell high": creative people pursue ideas that are unknown or out of favor but have growth potential. Six convergent resources: intellectual abilities, knowledge, thinking styles, personality, motivation, environment. The confluence claim is that resources interact — deficiency in one cannot always be compensated by surplus in another, and there may be thresholds.

**Measurability:** the six resources are individually measurable with existing instruments; the *confluence* claim (multiplicative/threshold interaction) is what is hard, because detecting interactions needs far more power than main effects. Specific empirical results from Lubart & Sternberg (1995) are **UNANSWERED**.

**Relevance to you: low.** This is a person-level theory. It has almost nothing to say about scoring an artifact.

### 5.5 Amabile's componential model

**[B] Amabile, T. M. (1983). The social psychology of creativity: A componential conceptualization. *JPSP*, 45(2), 357–376; (1996) *Creativity in Context*.** Three components: **domain-relevant skills**, **creativity-relevant processes**, **task motivation**. The famous corollary is the **intrinsic motivation principle**: intrinsic motivation is conducive to creativity, extrinsic controlling motivation detrimental. Amabile later softened this — informational or enabling extrinsic motivators can be **synergistic** with intrinsic motivation (1993, 1996).

**[B] Amabile, T. M., & Pratt, M. G. (2016). The dynamic componential model of creativity and innovation in organizations. *Research in Organizational Behavior*, 36, 157–183.** Adds meaningfulness of the work and progress/affect cycles.

**Relevance to you: indirect but real.** Amabile is the same person who built the CAT, and the componential model is why: the CAT deliberately does *not* try to measure the components, because they are not visible in the product. **Amabile's own architecture separates the process theory (componential) from the product measure (CAT) and does not let either contaminate the other.** That is the design discipline to copy.

### 5.6 Gruber, Simonton, and the propulsion model

- **[B] Gruber's evolving systems approach** (Gruber 1974, *Darwin on Man*; Gruber & Davis 1988): case-study method, the unit of analysis is the individual's evolving network of enterprise over decades. Deliberately idiographic and non-psychometric. Measurement contribution: essentially none, by design; its value is as a standing objection to snapshot measurement.
- **[B] Simonton's historiometry and chance-configuration theory** (Simonton 1988, 1997): quantitative analysis of eminent careers, productivity distributions, the equal-odds rule (the ratio of hits to total attempts is roughly constant within a career). Measures Big-C at the population level from archival records. **The equal-odds rule is directly relevant to automated evaluation**: it implies that if you want to compare generators, you should compare *quality-adjusted total output*, and that picking the best-of-N is not evidence of a better generator unless N is controlled.
- **[B] Sternberg, R. J., Kaufman, J. C., & Pretz, J. E. (2002). *The Creativity Conundrum*** — the **propulsion model**, eight types of creative contribution: replication, redefinition, forward incrementation, advance forward incrementation, redirection, reconstruction/redirection, reinitiation, integration. **Whether anyone has operationalized this as a coding scheme with published inter-rater reliability: UNANSWERED.** I did not find such evidence, and my prior is that it exists only as illustrative case coding. Do not build on it without checking.

---

## 6. The 4P framework — the taxonomy of what is being measured

**[B] Rhodes, M. (1961). An analysis of creativity. *Phi Delta Kappan*, 42(7), 305–310.** Rhodes collected 40-plus definitions of creativity and 16 of imagination, and found they sorted into four overlapping strands:

- **Person** — traits, dispositions, biography.
- **Process** — the cognitive operations of creating.
- **Product** — the artifact and its properties.
- **Press** — the environment and its influence on the person.

**[V]** Runco & Jaeger confirm the framing and note that Stein (1953) already anticipated *press* as a trait × state interaction: environments "never have a completely predictable impact. Their influence is always dependent on the individual's perception." They explicitly identify press as "one of the four strands of research identified by Rhodes, 1961."

**Why this is the most practically useful framework in the entire literature for you:** it is the diagnostic that catches the most common category error. An LLM grader scoring an artifact is doing **Product** measurement, full stop. It has no access to Process (it did not watch the creating), no access to Person (blind), and no access to Press. Therefore:

- Any rubric item phrased as "the author took a risk," "this shows original thinking," "the writer avoided the obvious approach," or "this demonstrates insight" is a **Process or Person item smuggled into a Product instrument**. The grader will answer it, confidently, by inferring process from product — which is precisely the inference the 4P taxonomy exists to forbid.
- **This is the single highest-yield audit you can run on an existing rubric**: classify every item as P/P/P/P and delete or rewrite everything that is not Product.

Later extensions **[B]**: Simonton's 5th P (*persuasion*); Runco's *potential*; **Glăveanu's Five A's** (actor, action, artifact, audience, affordances), which recasts the 4Ps in sociocultural terms and, notably, makes **audience** an explicit first-class element — the same move the systems model makes.

---

## 7. Novelty decomposition: relative to what, and for whom

### 7.1 The reference class is a free parameter and it dominates the score

Every novelty operationalization is of the form "distance from *C*" or "infrequency in *C*" for some reference class *C*. The theory literature offers four canonical choices:

| *C* | Construct measured | Boden/Stein label | Observable from artifact alone? |
|---|---|---|---|
| Creator's prior knowledge | P-creativity | Stein's "internal frame of reference" | **No** |
| Judging population's knowledge | field-relative novelty | Stein's "external frame"; C's field | Only via the judges themselves |
| An explicit reference corpus | corpus-relative novelty | Ritchie's **inspiring set** | **Yes** — if you state the corpus |
| All of human history | H-creativity | Boden's H | Not practically |

**[V] Ritchie makes the inspiring set a mandatory declared component of the framework**, which is the correct discipline: he treats "the analyst does not know what the inspiring set was" as a *defect in the analysis*, distinct from the substantive case of there being no inspiring set. **Applied to your system: a novelty score without a declared corpus is not underspecified, it is uninterpretable.**

### 7.2 Frequency-based vs distance-based

**Frequency-based** descends directly from **[V] Guilford (1950, p. 452)**: "the frequency of uncommon, yet acceptable, responses to items." Operationalized as uniqueness scoring — an idea is original if fewer than 5% (or 1%) of the sample produced it.

Known problems **[B, unverified numerically]**:
- **Sample-size dependence.** Uniqueness is defined against the sample, so originality scores inflate as samples shrink. Scores are not comparable across studies with different N.
- **Fluency confound.** **Hocevar, D. (1979). Ideational fluency as a confounding factor in the measurement of originality. *Journal of Educational Psychology*, 71(2), 191–196.** Producing more responses mechanically produces more unique ones; uniqueness totals correlate heavily with fluency, and the "originality" factor may be fluency in disguise.
- **Infrequency ≠ creativity.** Guilford's own "yet acceptable" qualifier was the guard, and standard uniqueness scoring **drops it**, so nonsense scores maximally. This is Runco & Jaeger's word-salad objection realized as a scoring artifact.
- **[B] Silvia et al. (2008), "Assessing creativity with divergent thinking tasks: Exploring the reliability and validity of new subjective scoring methods," *Psychology of Aesthetics, Creativity, and the Arts*, 2(2), 68–85**, is the standard reference for these problems and for subjective (rated) scoring of top-2 responses as the remedy. Specific numbers UNANSWERED.

**Distance-based** replaces frequency with semantic distance in an embedding space.
- **[B] Beaty, R. E., & Johnson, D. R. (2021). Automating creativity assessment with SemDis: An open platform for computing semantic distance. *Behavior Research Methods*, 53, 757–780.**
- **[B] Organisciak, P., Acar, S., Dumas, D., & Berthiaume, K. (2023). Beyond semantic distance: Open GPT-based scoring of creativity. *Thinking Skills and Creativity*** — LLM-based scoring reported to outperform semantic distance against human ratings.
- **[B] Forthmann et al. (2019)** on LSA-based divergent-thinking scoring being **biased by elaboration** — longer responses score as more distant regardless of idea quality. This is the distance-based analogue of the fluency confound and it is the one that will bite an LLM grader hardest, because verbose artifacts look novel.
- **Specific r values for all of the above: UNANSWERED.** My sub-agents were fetching exactly these numbers when they died. Do not quote correlations from me here.

### 7.3 Surprise is not infrequency, and the distinction is formal

This is a genuine conceptual advance and it is cleanly statable:

- **Surprisal** is −log p(x) under a model. It is a property of a single outcome under a fixed model. **Low probability alone.**
- **Bayesian surprise** **[B] (Itti, L., & Baldi, P. (2009). Bayesian surprise attracts human attention. *Vision Research*, 49(10), 1295–1306)** is **KL(posterior ‖ prior)** — the divergence between your beliefs before and after observing the datum. It measures **how much the observation changed your model**.

The two come apart exactly where it matters. A very rare event that your model already assigns to a known tail is high-surprisal but **zero Bayesian surprise** — it teaches you nothing. A moderately probable event that forces you to revise your generative model is low-surprisal but **high Bayesian surprise**. **Novelty is distance in a feature space; surprise is the derivative of your model with respect to the observation.** A random-noise artifact maximizes surprisal and minimizes Bayesian surprise, which is precisely the word-salad failure mode expressed information-theoretically. **This is why "surprise" is a better third criterion than "unusualness," and why an infrequency-based novelty score cannot be relabeled as surprise.**

**[B]** Computational-creativity work on surprise: **Maher, M. L. (2010). Evaluating creativity in humans, computers, and collectively intelligent systems, DESIRE'10**; **Maher & Fisher (2012)**; **Grace & Maher** on expectation-based surprise models, which formalize surprise as violation of *predicted attribute values* derived from the historical trend of a design domain — i.e., surprise relative to an extrapolated trajectory, not relative to a static corpus. That distinction (trend-violation vs corpus-distance) is the most implementable version of surprise I encountered.

### 7.4 Computational creativity evaluation frameworks: numbers vs checklists

| Framework | Output | Notes |
|---|---|---|
| **[V] Ritchie (2007)**, criteria over ⟨*B*, typ, val⟩, *I*, *R* | **Numbers** — ratios and threshold-parameterized criteria | But typ and val are **human ratings**; the framework computes over them, it does not produce them |
| **[B] Colton's creative tripod** (skill, appreciation, imagination) | **Checklist** — a perception-of-creativity argument | Explicitly about *perceived* creativity of the system, not artifact quality |
| **[B] FACE/IDEA** (Colton, Charnley & Pease 2011, ICCC) | **Partly numeric** — IDEA defines audience-impact measures in [0,1] | I saw retrieved text defining measures like instant appeal, shock, opinion splitting, acquired taste, opinion forming, subversion, and **triviality(A) = (1 + ind(a) − prov(A))/2**, each returning [0,1] and parameterized by a provocation level. Requires audience response data, which a blind grader does not have |
| **[B] Jordanous SPECS** (2012, *Cognitive Computation*, 4(3), 246–279) + fourteen components of creativity | **Procedure, then numbers** — you state your definition, derive standards, then test against them | Its real contribution is meta-methodological: it forces the evaluator to *declare the definition being operationalized* before scoring |

**SPECS is the right meta-level answer to your problem** even though it produces no number by itself: its whole thesis is that a creativity evaluation is invalid unless the definition it operationalizes is stated in advance. That is exactly the FIT/COST demand below.

---

## 8. Creativity vs adjacent constructs

- **Originality** — one component, not the whole. **[V]** Barron (1955) and Guilford (1950) were both defining *originality*, and Runco & Jaeger's central historiographic point is that the field spent decades mistaking definitions of originality for definitions of creativity. The same error recurs in modern automated scoring, where "creativity score" almost always means a novelty proxy.
- **Surprise** — Section 7.3. Distinct from infrequency; formally a property of model-updating.
- **Appropriateness / task-fit** — the deflationary reading of "effectiveness." High grader agreement, low variance, near-useless for discrimination once the population clears the bar.
- **Aesthetic value** — correlates with creativity judgments but is not identical. **[V]** In Amabile's collage data, aesthetic appeal correlated strongly with creativity ratings while technical goodness (.13) and neatness (−.26) did not — so "aesthetic value" itself splits into an appeal component that tracks creativity and a craft component that does not.
- **Quality/craft** — **[V]** the expert-novice study's related finding that groups agreed on **originality** but showed **no relationship** on **craftsmanship** and **quality** means these are separable dimensions with different judge-dependence profiles. Originality judgments are comparatively robust across judge populations; quality judgments are not.

### The novelty–usefulness correlation

**Status: partially UNANSWERED, and I will not fake it.** My dedicated sub-agent on this question died before retrieving the numbers. What I can report honestly:

**[S, retrieved]** Rietzschel, Nijstad & Stroebe's theoretical position is confirmed from retrieved text: "the underlying problem in effective idea evaluation and selection is the tension between originality and feasibility, and … highly original ideas tend to be disliked or rejected because they are perceived to be risky and unfeasible. Situational or personal factors that make implementation or feasibility concerns more salient will therefore hinder creative idea [selection]."

**[B] Citations, correct as bibliography, numbers UNVERIFIED:**
- Rietzschel, E. F., Nijstad, B. A., & Stroebe, W. (2010). The selection of creative ideas after individual idea generation: Choosing between creativity and impact. *British Journal of Psychology*, 101(1), 47–68. (Core finding: people are poor at selecting their most creative ideas and favor feasible over original ones; creativity instructions at *selection* do not reliably fix it.)
- Rietzschel, Nijstad & Stroebe (2006), *JESP*; (2014), *Creativity Research Journal* (problem scope and creativity instructions).
- Diehl, M., & Stroebe, W. (1987). Productivity loss in brainstorming groups. *JPSP*, 53(3), 497–509.
- **Mueller, J. S., Melwani, S., & Goncalo, J. A. (2012). The bias against creativity: Why people desire but reject creative ideas. *Psychological Science*, 23(1), 13–17.** Mechanism: **uncertainty**. Induced uncertainty produced more negative implicit associations with creativity (IAT) and lower explicit ratings of a creative idea, even among people who explicitly endorsed creativity as a goal. **Effect sizes UNANSWERED.**
- Blair, C. S., & Mumford, M. D. (2007). Errors in idea evaluation: Preference for the unoriginal? *Journal of Creative Behavior*, 41(3), 197–222.
- Licuanan, B. F., Dailey, L. R., & Mumford, M. D. (2007). Idea evaluation: Error in evaluating highly original ideas. *Journal of Creative Behavior*, 41(1), 1–27.
- Berg, J. M. (2016). Balancing on the creative highwire: Forecasting the success of novel ideas in organizations. *ASQ*, 61(3), 433–468. (Creators vs managers differ in forecasting accuracy for novel ideas.)
- Boudreau, K. J., Guinan, E. C., Lakhani, K. R., & Riedl, C. (2016). Looking across and looking beyond the knowledge frontier: Intellectual distance, novelty, and resource allocation in science. *Management Science*, 62(10), 2765–2783. (Novel grant proposals systematically penalized; **coefficient UNANSWERED**.)
- Dean, D. L., Hender, J. M., Rodgers, T. L., & Santanen, E. L. (2006). Identifying quality, novel, and creative ideas: Constructs and scales for idea evaluation. *JAIS*, 7(10), 646–699. Four dimensions: **novelty** (originality, paradigm relatedness), **workability** (acceptability, implementability), **relevance** (applicability, effectiveness), **specificity** (implicational explicitness, completeness, clarity). **Inter-dimension correlations UNANSWERED** — I attempted the AIS PDF and got 403.
- Diedrich, J., Benedek, M., Jauk, E., & Neubauer, A. C. (2015). Are creative ideas novel and useful? *Psychology of Aesthetics, Creativity, and the Arts*, 9(1), 35–40. (Relative weighting of novelty vs usefulness in creativity judgments; **weights UNANSWERED**.)

**What I will assert on verified grounds:** the *directional* claim that novelty and usefulness/feasibility judgments trade off is supported by the retrieved Rietzschel/Nijstad text and is consistent with **[V]** Amabile's collage correlations (creativity tracked novel use of materials at .81 and complexity at .76, while tracking neatness at −.26). **The claim that the raw novelty–usefulness correlation is reliably negative across domains is NOT established by anything I verified.** My honest read is that it is often near zero to modestly negative and is heavily moderated by domain and by whether raters are forced to trade the dimensions off, but that is a hypothesis, not a finding I can hand you.

---

## 9. The map: which theory each measurement tradition operationalizes

| Tradition / instrument | Theory it operationalizes | Reference class for novelty | Who judges | 4P slot | 4C level |
|---|---|---|---|---|---|
| Divergent thinking, uniqueness scoring (Guilford, Torrance) | Guilford's 1950 structure-of-intellect; standard definition with "acceptable" dropped | the response sample | the scoring key | Process (claimed), Product (actual) | little-c |
| DT with subjective/snapshot scoring (Silvia et al.) | standard definition, both criteria retained | rater's implicit norms | trained raters | Product | little-c |
| Semantic-distance scoring (SemDis) | novelty-as-distance; effectiveness silently dropped | the embedding corpus | the embedding model | Product | little-c |
| **CAT (Amabile 1982)** | **Csikszentmihalyi's systems model, made procedural** | judges' internalized domain | **appropriate observers = a miniature field** | Product | little-c / Pro-c |
| Ritchie's criteria (2007) | Boden, formalized | **declared inspiring set *I*** | humans, for typ and val | Product | n/a (systems) |
| FACE/IDEA (Colton et al.) | Boden + audience impact | domain history | an audience with measured response | Product + Press | n/a |
| SPECS (Jordanous) | whatever you declare — that is the point | you declare it | you declare them | declared | declared |
| Historiometry (Simonton) | systems model at population scale | the historical record | the actual historical field | Person + Product | Big-C |
| Self-report inventories (CBI, K-DOCS) | 4C, person side | self | self | Person | mini-c / little-c |
| Propulsion-model coding (Sternberg et al.) | contribution-type taxonomy | the domain's prior paradigm | expert coder | Product | Pro-c / Big-C |
| Patent/citation novelty measures | Simonton's three-criterion, institutionalized | prior art | patent examiners / citing authors | Product | Pro-c / Big-C |

**The pattern worth seeing:** every instrument that produces a defensible creativity number either (a) declares an explicit reference corpus, or (b) declares an explicit judge population. **None does neither.** The instruments that appear to do neither — uniqueness scoring, raw semantic distance — have merely hidden the choice (in the response sample; in the training corpus) rather than escaped it.

---

## 10. FIT/COST verdict for blind LLM-grader scoring

### 10.1 What the theory forces you to decide explicitly

**Decision 1: Novelty relative to what corpus?**

You must declare an inspiring set. **[V] Ritchie treats an undeclared inspiring set as an analytic defect, not a simplification.** Options and costs:

- *Relative to the other artifacts in the batch* — this is uniqueness scoring, and it inherits **sample-size dependence**: the same artifact scores differently in a batch of 10 and a batch of 1000, and cross-batch comparisons are invalid. Cheap; defensible only within a batch.
- *Relative to a fixed declared corpus* — comparable across batches, interpretable, auditable. **This is the recommended default.** Cost: you must build and version the corpus, and your novelty scores are now claims about that corpus and nothing else. Say so in the output.
- *Relative to the grader model's training distribution* — this is what you get by default if you ask an LLM "is this original?" and declare nothing. It is **unversioned, undocumented, changes when you change models, and is not inspectable**. This is the worst option and it is also the one you get by accident.
- *Relative to the creator's prior state* — P-creativity proper. **Unavailable to a blind grader. Do not put it in the rubric.**

**Decision 2: Effectiveness judged by whom, and which effectiveness?**

**[V] The standard definition does not answer this** — "it does not say anything about who is to judge each, and who is to judge the judges." So there is no appeal to authority available; you must legislate. And per Section 2.3, **do not use a single "effectiveness" column**. Split at minimum into task-fit and quality, and if the domain is artistic, name whether "quality" means craft or aesthetic value — **[V]** they correlate .13 and (strongly positive) respectively with expert creativity judgments in Amabile's data, so conflating them is not a rounding error.

**Decision 3: Surprise, or mere infrequency?**

If you want Simonton's or Boden's third criterion, implement it as **model-update** (how much does this artifact change the expected distribution?) or as **trend-violation** (Grace & Maher), **not** as low frequency. Infrequency-as-surprise reduces to novelty and gives you a correlated third column that adds no information while looking like it does. Cost of doing it properly: you need a predictive model of the domain, which is a real engineering commitment. **Cost of skipping it: you have a two-criterion instrument, which is the field's majority position anyway. This is an acceptable skip; the fake version is not.**

**Decision 4: Additive or multiplicative composition?**

**[S] Simonton's c = (1 − p)·u·(1 − v) is explicitly multiplicative**, and the substantive content of that choice is that **zero on any dimension means zero creativity**. An additive/mean composite lets a maximally original nonsense artifact score mid-range by averaging high novelty against low effectiveness — which reintroduces exactly the word-salad case **[V]** the second criterion was invented to exclude. **Recommendation: report dimensions separately, always; if you must produce a scalar, use a multiplicative or minimum-style composite, and state the rule.** Whether multiplicative beats additive against an external criterion empirically is **UNANSWERED** — I found no head-to-head test.

**Decision 5: Which C, and which domain?**

State the target C level. A rubric tuned to little-c everyday products applied to professional work will compress at ceiling. And per APT **[B]**, use **domain-specific rubrics**; a single cross-domain creativity rubric measures at best a general thematic level, and any domain-level claim from it is unlicensed.

### 10.2 The construct claim an LLM-grader panel can and cannot defensibly make

**The systems model says a blind LLM grader is not measuring creativity — it is constituting a synthetic field.** That framing is correct and it is more useful than it is discouraging, because Amabile's CAT is *also* a constituted field, just made of humans. The CAT's validity was never "the judges detect the creativity signal"; it was "creativity **is** what appropriate observers agree on." That is an operational definition, and an LLM panel can occupy the same logical slot — **if and only if it meets the same conditions the CAT imposes.**

**What the CAT requires, and what it costs you:**

| CAT requirement | LLM-panel analogue | Status |
|---|---|---|
| Judges are **appropriate observers** (domain experts) | model must have domain competence; you must argue for it | **This is the load-bearing, unverified assumption.** See below |
| Judges work **independently** | separate calls, no shared context, no seeing each other's scores | Easy and cheap. **Do it.** |
| Judges are **not given a definition** of creativity | tension: LLMs need instruction, but a supplied definition makes the panel measure *your* definition, not a field's consensus | Unresolvable; pick a side and declare it. I recommend supplying the definition and giving up the "consensus" claim, because the alternative is an undeclared definition |
| Ratings are **relative to one another**, not absolute | rank or compare within a batch rather than absolute Likert | **Do this.** Absolute scoring on an unanchored scale is the weakest available design |
| Report inter-rater reliability | inter-run and inter-model agreement | Necessary but, per below, **radically insufficient** |

**The killer empirical constraint [V]:** Kaufman et al. (2008) found expert and nonexpert panels rating the same 205 poems correlated at **r = .21 (4.4% shared variance)**, while the expert panel's own internal α was **.935**. **A panel can be nearly perfectly self-consistent and still share almost no variance with the panel whose judgment you actually care about.** Therefore:

- **High inter-run or inter-model agreement among LLM graders is not evidence of validity.** It is evidence that you have built a consistent field, which is exactly what the novice panel also had — internally coherent, and measuring something else. If anything, LLM graders should be *expected* to agree with each other more than human novices do (shared training distribution, shared instruction-tuning) while sharing that same 4%-style relationship with expert judgment. **Agreement among correlated judges is the failure mode this study documents, not the success criterion.**
- **The only thing that establishes your grader's standing is a measured correlation against an expert human panel, in your domain.** Not a general-purpose LLM-judge benchmark; not agreement with another LLM; not face validity of the rubric. **Until you have that number, you do not know which panel you have built.**
- **[V] Encouraging nuance:** in the same literature, novice and expert groups agreed on **originality** but showed **no relationship on craftsmanship and quality**. If that generalizes, an LLM panel's **novelty** ratings are the more trustworthy half and its **quality** ratings the more suspect half — the opposite of most people's intuition, since quality feels more objective. **Prioritize expert-validation effort on the quality dimension.**

**The defensible claim:** *"Score X is the consensus judgment of a declared panel P, applying a declared rubric R, assessing novelty relative to a declared corpus C, on domain-D artifacts."* Every element is stated; nothing is claimed about creativity in general. This is a real, useful, auditable measurement.

**The indefensible claims:** that the score measures creativity as such; that it would replicate with an expert human field (without measuring that); that high inter-grader agreement demonstrates validity; that a novelty column without a declared corpus means anything; that transformational creativity has been assessed; that the score reflects the creator's process, risk-taking, or insight.

### 10.3 Concrete recommendations

1. **Audit every rubric item against the 4Ps and delete every non-Product item.** Highest yield per unit effort of anything here.
2. **Adopt Ritchie's decomposition rather than asking for novelty directly.** Rate **typicality** ("is this an instance of the class?") and **quality** ("is this a good instance?") as primitives, and compute novelty as untypicality plus distance from a **declared inspiring set**. This is the field's most rigorous formalization and it maps cleanly onto what a blind grader can actually see. **[V] Ritchie's own acknowledgment that typicality and quality "would normally be determined by human assessment" is the honest statement of what you are substituting an LLM for** — and it tells you exactly which two judgments need human validation.
3. **Never emit a single scalar without also emitting the components.** Multiplicative composition if you must compose.
4. **Version and publish the reference corpus.** A novelty score is a two-argument function; shipping it with one argument hidden is the defect Ritchie names.
5. **Declare the definition before scoring** (SPECS's core methodological demand). "We operationalize the standard definition (Runco & Jaeger 2012), splitting effectiveness into task-fit and craft quality, with novelty relative to corpus C" is a sentence your instrument should be able to print.
6. **Budget for a human expert calibration panel in each domain.** Given Kaufman et al., this is not optional polish; it is the only thing standing between you and an r = .21 instrument that looks excellent on every internal metric.
7. **Use relative/comparative judgments within batch rather than absolute Likert ratings**, per CAT practice.
8. **Expect and test for the anti-novelty bias.** The human literature (Mueller et al. 2012; Blair & Mumford 2007; Boudreau et al. 2016) says human judges penalize novelty under uncertainty. Whether LLM graders inherit this — plausibly amplified, since RLHF selects for the unobjectionable — is **UNANSWERED and directly testable**: score a set of artifacts with known-high novelty and known-high expert-rated quality, and check whether the grader's novelty and quality columns move together or against each other. If your grader's quality score falls as novelty rises, you have reproduced the bias and your composite is systematically penalizing the artifacts you most want to find.

---

## 11. UNANSWERED

Claims I could not verify and that you should not rely on from this report:

1. Verbatim Csikszentmihalyi formulations from the 1988 and 1999 chapters (source PDF unavailable).
2. Boden's own text on combinational/exploratory/transformational — I have only Ritchie's secondary exposition.
3. Whether **any** reliable measurement of transformational vs exploratory creativity from artifacts exists. I found none; absence of evidence here, though my prior is strongly that it does not exist.
4. Specific critiques of Wiggins' Creative Systems Framework as unimplementable — I could establish only that Ritchie explicitly declines to build a model of the kind Wiggins builds.
5. All specific correlations for SemDis and GPT-based originality scoring vs human ratings (Beaty & Johnson 2021; Organisciak et al. 2023).
6. Rietzschel et al. (2010) numeric results: the originality–feasibility correlation, sample sizes, selection-accuracy statistics.
7. Mueller, Melwani & Goncalo (2012) effect sizes and IAT statistics.
8. Boudreau et al. (2016) novelty-penalty coefficient.
9. Dean et al. (2006) inter-dimension correlations and inter-rater reliabilities.
10. Diedrich et al. (2015) relative weights of novelty and usefulness.
11. Whether the propulsion model has ever been operationalized as a coding scheme with published inter-rater reliability.
12. Lubart & Sternberg (1995) empirical results testing investment theory.
13. Weisberg (2015) on dropping "value" — argument details.
14. Any head-to-head empirical comparison of multiplicative vs additive creativity composites against an external criterion.
15. Whether LLM graders reproduce the human anti-novelty bias — no evidence either way retrieved.

**Primary sources I did fully verify and can re-supply on request:** Runco & Jaeger (2012) complete text; Ritchie (2007) complete text; Kaufman, Baer, Cole & Sexton (2008) complete text; Kaufman & Baer "Gold Standard" chapter. Extracted text is at (local session cache, not included) (Ritchie 2007), `.../goldstd.txt` (Gold Standard chapter), (local session cache, not included) (Kaufman et al. 2008), and (local session cache, not included) (Runco & Jaeger 2012).

**Sources:** [Runco & Jaeger 2012 PDF](https://ead.ufpa.br/pluginfile.php/542154/mod_folder/content/0/Leituras%20Complementares%20n%C3%A3o%20acess%C3%ADveis/RUNCO,%20Mark%20A.;%20JAEGER,%20Garrett%20J.%20The%20standard%20definition%20of%20creativity.%20.pdf) · [Simonton 2012, CRJ](https://www.tandfonline.com/doi/abs/10.1080/10400419.2012.676974) · [Simonton 2018, JOCB](https://onlinelibrary.wiley.com/doi/abs/10.1002/jocb.137) · [Simonton 2022, BVSR status](https://www.tandfonline.com/doi/full/10.1080/10400419.2022.2059919) · [Corazza 2016, CRJ](https://www.tandfonline.com/doi/full/10.1080/10400419.2016.1195627) · [Corazza et al. 2022, Dynamic Creativity Framework](https://arts.units.it/bitstream/11368/3016771/4/Corazza,+Agnoli,+Mastria+2022_The+Dynamic+Creativity+Framework-Post_print.pdf) · [Ritchie 2007, Minds and Machines](https://users.dcc.uchile.cl/~nbaloian/CapstonePreparation/readings/Creativity.pdf) · [Kaufman, Baer, Cole & Sexton 2008](https://jck_articles.s3.amazonaws.com/KaufmanBaerColeSexton.pdf) · [Kaufman & Baer, "Gold Standard"](https://www.rider.edu/sites/default/files/2024-05/GoldStandard.pdf) · [Ritchie ICCC 2012, creativity formulae](https://computationalcreativity.net/iccc2012/wp-content/uploads/2012/05/041-Ritchie.pdf) · [Colton, Charnley & Pease 2011, FACE/IDEA](https://computationalcreativity.net/iccc2011/proceedings/the_foundational/colton_1_iccc11.pdf)