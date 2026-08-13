# Computational Creativity Evaluation: Literature Map and FIT/COST Verdict

**Provenance note.** Ritchie (2007), Lamb/Brown/Clarke (2018), and Jordanous (2011) were read in **full primary source** (PDFs fetched and text-extracted). Wiggins' CSF, Colton's FACE/IDEA internals, and the quality-diversity/generative-art metrics literature are from **search-level secondary sources only** — two subagents assigned to those died on rate limits, and the session's web-search budget (200 calls) is now exhausted. Those sections are marked with confidence levels, and specific gaps are listed as UNANSWERED at the end. Do not treat the Wiggins formal notation or the FACE tuple structure below as verified from source.

---

## 1. The formal vocabulary (adopt these terms; do not reinvent)

| Term | Origin | Meaning |
|---|---|---|
| **Inspiring set** (*I*) | Ritchie 2001/2007 | The subset of known artifacts that guided the system's construction — training corpus, exemplars, KB. The reference class against which "novelty" is defined. |
| **Result set** (*R*) | Ritchie 2007 | The outputs of one run (or aggregated runs) of the system. |
| **Basic items** (*B*) | Ritchie 2007 | The datatype the system emits (strings of words, pixel arrays). *Not* a success criterion — just the type. |
| **Typicality** (*typ*) | Ritchie 2007 | Mapping *B* → [0,1]: the degree to which an output is a member of the intended artifact class. "Is this a poem?" |
| **Value / quality** (*val*) | Ritchie 2007 | Mapping *B* → [0,1]: how *good* an instance it is. "Is this a good poem?" Deliberately separate from typicality. |
| **Rating scheme** | Ritchie 2007 | The (unspecified) procedure producing *typ* and *val*. Ritchie explicitly abstracts over it — "we have no firm proposals on what this information should be." |
| **P-creativity / H-creativity** | Boden 1990 | Novel to the creator vs. novel to anyone in history. |
| **A-creativity / C-creativity** | Dasgupta 2011 | Antecedent (never seen before) vs. consequence (influenced others). C-creativity requires historical hindsight. |
| **Mere novelty** | Boden 1990 | Novelty that is trivial/random. The central failure mode of naive novelty metrics. |
| **Conceptual space** | Boden 1990, formalized Wiggins 2006 | The set of artifacts reachable under a given rule set. |
| **Exploratory vs. transformational creativity** | Boden 1990 | Search within the space vs. changing the rules defining the space. |
| **Uninspiration** (hopeless / conceptual / generative) | Wiggins 2006 | Three diagnosable failure modes — see §3. |
| **Aberration** | Wiggins 2006 | Traversal reaching concepts *outside* the declared conceptual space; the trigger condition for transformation. |
| **Creative tripod** (skill / appreciation / imagination) | Colton 2008 | Three properties an observer must *perceive* for creativity to be attributed. |
| **Framing** | Colton/Charnley/Pease 2011–2012 | Narrative context the system supplies about how/why it made the artifact. Argued to be part of the creative act, not decoration. |
| **The four Ps** (Person, Process, Product, Press) | Rhodes 1961, imported by Jordanous/Lamb | The standard decomposition of *what* is being evaluated. "Press" = the receiving culture. |
| **Consensual Assessment Technique (CAT)** | Amabile 1983 | Criteria-*free* expert rating; judges given no rubric, validated by inter-rater reliability. |
| **Impressiveness** | Lehman & Stanley 2012 | Easy to appreciate, hard to recreate. |

Also standard: **fine-tuning** (Colton et al. 2001 — degree to which output merely reinvents *I*), **modified Turing test** (static-artifact discrimination, distinct from Turing's interactive test), **SPECS** (Jordanous 2012), **novelty–typicality tradeoff** (Ritchie's core structural claim).

---

## 2. Ritchie's empirical criteria — the only fully formalized product framework

**Sources:** Ritchie, G. (2001) "Assessing creativity," AISB Symposium on AI and Creativity, pp. 3–11; Ritchie, G. (2007) "Some Empirical Criteria for Attributing Creativity to a Computer Program," *Minds and Machines* 17(1):67–99, [Springer](https://link.springer.com/article/10.1007/s11023-007-9066-2), [free PDF](https://users.dcc.uchile.cl/~nbaloian/CapstonePreparation/readings/Creativity.pdf).

### The apparatus

A **value-based artefact class** is a triple (*B*, *typ*, *val*). Given inspiring set *I* ⊆ *B* and result set *R* ⊆ *B*, define:

- *T<sub>a,b</sub>*(X) = {x ∈ X | a ≤ typ(x) ≤ b} — subset in a typicality band
- *V<sub>a,b</sub>*(X) = {x ∈ X | a ≤ val(x) ≤ b} — subset in a quality band
- AV(F, X) = (Σ<sub>x∈X</sub> F(x)) / |X| — average
- ratio(X, Y) = |X| / |Y|

Four thresholds: **a** (typicality high-water mark), **b** (untypicality limit), **c** (quality bar), **h** (the comparison level in every criterion).

Critically, Ritchie's decomposition of novelty is **two-factor and non-obvious**: novelty = *untypicality* (low *typ*, i.e. dissimilarity from genre norms) **plus** *innovation* (set-difference from *I*). These are separate axes. He does not treat novelty as primitive.

### All 18 criteria (verbatim structure)

*Baseline — is it even the right kind of thing:*
1. AV(typ, R) > h
2. ratio(*T<sub>a,1</sub>*(R), R) > h

*Quality without regard to typicality:*
3. AV(val, R) > h
4. ratio(*V<sub>c,1</sub>*(R), R) > h

*Conventional skill (typical AND good) — maps loosely to exploratory creativity:*
5. ratio(*V<sub>c,1</sub>*(R) ∩ *T<sub>a,1</sub>*(R), *T<sub>a,1</sub>*(R)) > h

*Unconventional skill (atypical AND good) — maps loosely to transformational creativity:*
6. ratio(*V<sub>c,1</sub>*(R) ∩ *T<sub>0,b</sub>*(R), R) > h
7. ratio(*V<sub>c,1</sub>*(R) ∩ *T<sub>0,b</sub>*(R), *T<sub>0,b</sub>*(R)) > h
8. ratio(*V<sub>c,1</sub>*(R) ∩ *T<sub>0,b</sub>*(R), *V<sub>c,1</sub>*(R) ∩ *T<sub>a,1</sub>*(R)) > h
8a. ratio(*V<sub>c,1</sub>*(R) ∩ *T<sub>0,b</sub>*(R), *V<sub>c,1</sub>*(R)) > h — **the 2007 repair.** Criterion 8 as published in 2001 was unbounded (not in [0,1]) and divided by zero whenever there were no high-value highly-typical items. 8a fixes both.

*Relation to the inspiring set:*
9. ratio(I ∩ R, I) > h — can it at least replicate its exemplars
10. ratio(R, I ∩ R) > h — divides by zero when nothing is replicated
10a. (1 − ratio(I ∩ R, R)) > h — **the repair**

*The same battery restricted to novel outputs (R − I):*
11. AV(typ, R−I) > h
12. AV(val, R−I) > h
13. ratio(*T<sub>a,1</sub>*(R−I), R) > h
14. ratio(*V<sub>c,1</sub>*(R−I), R) > h
15. ratio(*T<sub>a,1</sub>*(R−I), R−I) > h
16. ratio(*V<sub>c,1</sub>*(R−I), R−I) > h
17. ratio(*V<sub>c,1</sub>*(R−I) ∩ *T<sub>a,1</sub>*(R−I), R−I) > h
18. ratio(*V<sub>c,1</sub>*(R−I) ∩ *T<sub>0,b</sub>*(R−I), R−I) > h — **the maximal demand**: novel w.r.t. *I*, atypical, and high quality

Ritchie's own grouping: *Basic success* {1,2,9}; *Unrestrained quality* {3,4}; *Conventional skill* {5}; *Unconventional skill* {6,7,8a}; *Avoiding replication* {10a}; then {11,13,15}, {12,14,16}, {17}, {18} as the same facets restricted to novel output.

### What Ritchie actually says about how to use them — and this matters

He is explicit that everyone has used them wrong:

> "Those who have applied them... have treated all the 14 original criteria as having equal status, and as providing a 14-point checklist or yardstick... **This is not how they were intended.**"

The intended use is as a **repertoire to select from**, and the fact that criteria "lead in different directions" is by design, not a defect. He also endorses the practice that emerged in the wild: drop the boolean threshold *h* entirely and report the criteria as **a vector of values in [0,1] — a behavioral profile**, not a pass/fail. This is the single most important design lesson in the paper for your purposes.

On thresholds, he proposes calibrating *a, b, c* from the distribution of **human-created** artifacts' *typ*/*val* judgments (mean, SD, max, min) — i.e., set machine thresholds from a human reference distribution rather than picking them.

### The empirical record — the criteria were run, and they behaved badly

**Gervás (2002), WASP poetry generator.** Inspiring set = one 16th-c. Spanish sonnet. 14 initializations × 12 runs = 168 outputs, human-rated on syntactic correctness (0–5) and aesthetic quality. Criteria 1–8 under three typ-weightings:

| Crit | 50/50 | 70/30 | 30/70 |
|---|---|---|---|
| 1 avg typicality | 0.71 | 0.67 | 0.75 |
| 2 typical/results | 0.54 | 0.48 | 0.79 |
| 3 avg quality | 0.47 | 0.47 | 0.47 |
| 4 good/results | 0.24 | 0.24 | 0.24 |
| 5 good typical/typical | 0.36 | 0.34 | 0.29 |
| 6 good atypical/results | 0.05 | 0.08 | 0.01 |
| 7 good atypical/atypical | 0.12 | 0.16 | 0.06 |
| 8 good atyp/good typ | 0.28 | 0.52 | 0.05 |

Because no element of *I* appeared in *R*: criteria 11–14 **exactly replicated** 1–4, criterion 9 = 0.00, criterion 10 = division by zero. Four of fourteen criteria were degenerate and four more were redundant.

**Threshold sensitivity is catastrophic.** Same data, varying a/b/c:

| Crit | high (.7) | med (.5) | low (.3) | a,b high c low | a,b low c high |
|---|---|---|---|---|---|
| 2 | 0.54 | 0.88 | 0.89 | 0.54 | 0.89 |
| 4 | 0.24 | 0.50 | 0.68 | 0.68 | 0.24 |
| 5 | 0.36 | 0.57 | 0.77 | 0.89 | 0.28 |
| 6 | 0.05 | 0.00 | 0.00 | 0.21 | 0.00 |
| 7 | 0.12 | 0.00 | 0.00 | 0.45 | 0.00 |
| 8 | 0.28 | 0.00 | 0.00 | 0.44 | 0.00 |

Criteria 6, 7, 8 swing from nonzero to **identically zero** on a threshold move of 0.2. The "unconventional skill" block — the part that supposedly measures the interesting kind of creativity — is the most threshold-fragile. Gervás's own conclusion was that thresholds must be fixed a priori per domain.

**Pereira, Mendes, Gervás & Cardoso (2005)**, "Experiments with assessment of creative systems: an application of Ritchie's criteria," IJCAI-05 Workshop on Computational Creativity — extended this to Divago (conceptual blending) and Dupond (paraphrase). Divago's authors **computed *typ* and *val* automatically** rather than using human judges: *I* = the concept KB, typicality = edit-distance closeness to *I*, value = fit to the input goal. Notably criterion 8 for Divago's Creature domain reports **2.0** — outside [0,1], the exact unboundedness bug 8a was meant to fix.

**Ritchie's own verdict on these applications is damning, and it is the most transferable warning in the literature:**

> "In the Divago study, typicality (*typ*) is measured as closeness to *I*, which means that what were two separate factors in the original criteria are now very closely related, both in effect measuring avoidance of replication."

He calls this out across three systems (Divago, Dupond, Haenen & Rauchas's melody work): researchers "changed the meaning of the formulae while retaining the notation... **Such redefinitions disguise what is being measured.**" If you define typicality as distance-to-corpus, the novelty axis and the typicality axis collapse into one, and every composite criterion silently becomes a function of a single quantity.

### Published critiques

- **Ventura (2008), RASTER** — thought experiment: generate pixels at random, output an image if a similar one is found online. Ventura argues RASTER satisfies the tripod (imagination = random search; appreciation = a fitness function; skill = it produces images) and renders Ritchie's inspiring-set criteria inapplicable. RASTER also defines typicality and quality identically, invalidating further criteria. It is the field's standard reductio.
- **Jordanous (2012b)** — Ritchie's model is "cumbersome to implement" (requires rating a large number of outputs) and its results are "abstract and need to be rephrased to be useful." Her 2011 survey adds: "relatively impractical to use in evaluation."
- **Lamb, Brown & Clarke (2018)** — "Ritchie's model has fallen out of favor in recent years."
- **Non-independence of criteria.** Pereira et al. (2005) found typicality and quality ratings under Ritchie's model are *not* independent; Lamb et al. (2015) replicated this for Ritchie's model, the IDEA model, and the Creative Tripod with non-expert raters. Lamb et al.'s summary judgment: "A model in which criteria are demonstrably orthogonal to each other has yet to emerge, meaning that **we do not have a sense of any 'dimensionality' that might underlie creativity.**"
- **Ritchie himself** names the framework's biggest weakness: it compares *R* to *I* only by **set membership overlap**, with no similarity metric. An output one word away from a training example counts as fully novel. Gervás showed Colton's "fine-tuning" measure is always exactly 0 for WASP for precisely this reason.

---

## 3. Wiggins' Creative Systems Framework — *(secondary sources only; medium confidence)*

**Sources:** Wiggins, G. (2006) "A preliminary framework for description, analysis and comparison of creative systems," *Knowledge-Based Systems* 19(7):449–458; Wiggins (2006a) — the aberration material is cited to p. 456.

The CSF is the first formal, logic-based rendering of Boden. It posits a **universe of possibilities** *U*, a **language** *L* for describing concepts, and three rule sets:

- **ℛ** — rules constraining the conceptual space (which subset of *U* counts as in-space)
- **𝒯** — the traversal strategy (how the agent moves through the space)
- **ℰ** — evaluation rules (what counts as valued)

Boden's **transformational creativity is then precisely the modification of ℛ**, while exploratory creativity is movement under 𝒯 with ℛ fixed. This is the framework's single genuine contribution: it makes "changing the rules" a well-typed operation rather than a metaphor.

**The diagnostic vocabulary is the operationalizable part.** Three **uninspiration** failure modes:
- *hopeless uninspiration* — no valued concepts exist anywhere in the universe (the task is impossible)
- *conceptual uninspiration* — no valued concepts exist within the agent's conceptual space (ℛ is wrong)
- *generative uninspiration* — valued concepts are in-space but 𝒯 cannot reach them (search is wrong)

And **aberration**: 𝒯 reaches a set of concepts lying *outside* the conceptual space defined by ℛ. Aberrations are the trigger/guide for transformation — described as *perfect* (fully acceptable to the user) or *productive* (partly acceptable).

**Why this matters to you:** the uninspiration triad is a **failure-attribution schema**, and it is directly portable to LLM evaluation. When a model fails to produce a valued artifact, it separates "the prompt is unsatisfiable" from "the model's learned notion of the genre excludes good answers" from "good answers are representable but decoding/sampling didn't find them." That distinction is exactly what a grader-based eval usually collapses into a single low score. The Creative Problem Solving in LLVMs paper ([arXiv:2405.01453](https://arxiv.org/pdf/2405.01453)) explicitly proposes using the three modes this way. A formal mapping of CSF onto Markov Decision Processes exists ([arXiv:2405.14966](https://arxiv.org/pdf/2405.14966), ICCC 2024), where aberration maps to unlikely trajectories given π and *T*.

**Known critique:** the CSF uses **crisp sets**, so conceptual-space membership, uninspiration, and aberration are all binary — which contradicts Boden's own gradualist framing. Ritchie (2012) reportedly redefined hopeless uninspiration in a way later authors call contradictory. *(Secondary; I did not read Ritchie 2012.)*

---

## 4. Colton — tripod, FACE/IDEA, framing *(mixed confidence; FACE internals UNVERIFIED)*

### The creative tripod (Colton 2008)

**Source:** Colton, S. (2008) "Creativity Versus the Perception of Creativity in Computational Systems," AAAI Spring Symposium, [PDF](https://classes.cc.gatech.edu/AY2013/cs7601_spring/papers/Colton-PerceptionofCreativity.pdf).

Three legs — **skill** (can produce the artifact), **appreciation** (knows why the output is good), **imagination** (can generate diverse output from one input) — all of which "must be extended to some degree for the tripod to stand."

The decisive subtlety, which Lamb et al. flag and which most citations get wrong: **Colton's claim is not that a creative system must possess these qualities, but that it must *appear* to possess them.** The tripod is a **Press-perspective** instrument (about audience attribution), not a Person-perspective one, despite its personal vocabulary. Its motivating argument is that product-only evaluation fails, via a thought experiment where the same artifact reached by different processes warrants different creativity attributions — but only if the observer knows the process.

**Critiques (strong, and I'd weight them heavily):**
- **Bown (2014):** the tripod's terms have no clear definitions and therefore "cannot be distinguished from trivial pseudo-versions of themselves." A programmer can assert imagination exists because they put in something they believe relates to imagination. This bypasses falsifiability. Lamb et al.: "Many papers perform exactly this kind of trivial evaluation, and even obviously uncreative systems can appear to pass the Tripod with the right argument." RASTER is the demonstration.
- **Colton et al. (2014) themselves conceded insufficiency**, adding Learning, Intentionality, Accountability, Innovation, Subjectivity, and Reflection — with, per Lamb et al., "not much justification... except a statement that they addressed the most common objections."
- **Smith et al. (2014)** rescued it by supplying working definitions (skill = produce something useful; imagination = search the conceptual space and produce something novel; appreciation = self-assess) — which, note, collapses the tripod back into Product/Process terms.
- **Jordanous (2016b)** maps the three legs onto three SPECS components: skill → *domain competence*, imagination → *variety/divergence/experimentation*, appreciation → *thinking and evaluation*.

Lamb et al.'s verdict, which generalizes past the tripod: "for any criteria, Press or otherwise, if we have not specified just what we mean by each of our criteria, our evaluation becomes meaningless."

Despite all this, **the tripod was the most-adopted framework in the field** — 6 of 75 systems in Jordanous's survey, vs. 4 for Ritchie. It was used "to describe why a given system should be considered creative, rather than for any comparison between systems." That is a rhetorical instrument, not a measurement instrument.

### FACE and IDEA (Colton, Charnley & Pease 2011)

**Source:** ICCC 2011. **I could not verify the formal internals** — the subagent assigned to this died and my search budget is exhausted. What is established from the secondary literature: FACE describes creative acts as tuples over **F**raming information, **A**esthetic measures, **C**oncepts, and **E**xpressions of concepts, with a ground/process distinction (generating an aesthetic measure vs. generating the method that generates it). IDEA is the companion model covering (Iterative) Development–Execution–Appreciation, concerned with audience impact. Both are explicitly **descriptive models**, proposed in Pease & Colton (2011) as the alternative to the Turing test.

**Framing** (Charnley, Pease & Colton 2012, "On the Notion of Framing in Computational Creativity," ICCC 2012): the system's own narrative about how and why it made the artifact. Colton's position is that framing is *part of* the creative act and withholding it from evaluators is a methodological error. Lamb et al. note this is deliberately persuasive — "taking advantage of contextual effects that can sway human judgment." That cuts both ways: framing improves attributed creativity partly by exploiting known context effects on art appraisal (price, title, artist name, setting — Lauring et al. 2016).

**UNVERIFIED:** the F/A/C/E tuple formalism, the F<sup>g</sup>/F<sup>p</sup> ground/process superscript notation, IDEA's well-being/impact measures and "ideal audience" construct, and whether framing has ever been shown experimentally to shift audience judgments.

---

## 5. Jordanous — SPECS, the 14 components, and the field's self-diagnosis

**Sources:** Jordanous, A. (2012) "A Standardised Procedure for Evaluating Creative Systems: Computational Creativity Evaluation Based on What it is to be Creative," *Cognitive Computation* 4(3):246–279, [DOI](https://link.springer.com/article/10.1007/s12559-012-9156-1); Jordanous (2011) ICCC-11 pp. 102–107 [PDF](https://kar.kent.ac.uk/42374/1/jordanous-2011a.pdf); Jordanous (2014) ICCC-14 meta-evaluation [PDF](https://computationalcreativity.net/iccc2014/wp-content/uploads/2014/06/8.3_Jordanous.pdf).

### SPECS is a three-step meta-procedure, not a metric

1. State a definition of what it means for *this* system to be creative.
2. Derive testable standards from that definition.
3. Test against them.

That is the whole thing. SPECS supplies no measurement. Its contribution is forcing the definition to be **stated before the test is built**, which prevents the post-hoc criterion selection that Lamb et al. identify as endemic.

### The 14 components (empirically derived, and this is the useful part)

Derived by clustering analysis over **694 creativity words** from a corpus of academic writing about creativity across disciplines — i.e., these are what the literature *means* by creativity, not what one author thinks it should mean:

active involvement and persistence · dealing with uncertainty · domain competence · general intellect · generation of results · independence and freedom · intention and emotional involvement · originality · progression and development · social interaction and communication · spontaneity/subconscious processing · thinking and evaluation · value · variety, divergence, experimentation

SPECS does **not** require using all 14; domain-specific components may be added. This is the field's best available answer to "what dimensions should a creativity rubric have," and it is the one artifact here with an empirical derivation behind it.

### The survey — verified numbers from the primary source

75 journal/conference papers presenting systems as creative. Table 1 of Jordanous (2011):

| Measure | % |
|---|---|
| Paper makes at least a mention of evaluation | 77% |
| Paper gives details of what evaluation was done | 55% |
| Paper contains section(s) on Evaluation | 51% |
| Paper states evaluation criteria | 69% |
| **Main aim of evaluation: Creativity** | **35%** |
| Main aim of evaluation: Quality/Accuracy/Other | 43% |
| **Mention of creativity evaluation methodology** | **27%** |
| **Application of creativity evaluation methodology** | **24%** |
| System compared to other systems | 15% |
| System compared to systems by other researchers | 11% |
| Systems evaluated by independent judges | 33% |

Her stated key finding: "evaluation of computational creativity is **not being performed in a systematic or standard way**." Of the 18 papers that did apply a creativity evaluation methodology, the tripod was used 6 times, Ritchie 4 times, and **no other methodology more than once**. Only 11% compared against another researcher's system.

### The five meta-evaluation criteria (Jordanous 2014)

A methodology for evaluating evaluation methodologies. Good evaluations are:
1. **Correct** — accurately and comprehensively portray the system's creativity (she explicitly denies a ground truth exists; "correct" means appropriate and realistic for the system)
2. **Useful** — actionable for understanding and improving the system
3. **Faithful** — capture creativity rather than something correlated with it
4. **Usable** — easily applied
5. **General** — transfer across system types

She concedes no single methodology satisfies all five; the criteria are for comparing tradeoffs. Note criterion 5 directly contradicts Baer's domain-specificity thesis.

### Pearce, Meredith & Wiggins (2002) — the purpose-first argument

Meta-analysis of music generation found a "**methodological malaise**." Their prescription is the sharpest thing in this literature: **evaluation method must follow from stated purpose.** Four purposes, four regimes:
- *Making art* — legitimate, but it's art, not science; evaluate via Press (acclaim, exhibition, appeal), don't publish it as science absent a technical advance.
- *Engineering a general creative system* — normal engineering: requirements, specification, testing. Researchers "often fail to specify their engineering goals — in particular, to list practical scenarios in which their system would be useful, and to state the conditions under which they will deem the system successful."
- *Investigating a theory of artistic style* or *of human creative cognition* — scientific method; state a hypothesis, minimize confounds, attempt disconfirmation, and **do not hand-tweak the system until it sounds better**.

---

## 6. Lovelace tests and the discrimination-test failure

**Original Lovelace Test** — Bringsjord, Bello & Ferrucci (2001), "Creativity, the Turing Test, and the (better) Lovelace Test," *Minds and Machines* 11:3–27. Agent *a* passes if it produces output *o* via repeatable (non-fluke) processes such that its architect *h* **cannot account for how *o* was produced**.

**Riedl's critique and Lovelace 2.0** — [arXiv:1410.6142](https://arxiv.org/pdf/1410.6142) (verified from full text). Riedl's objections:
- The Turing Test "relies on deception" — systems succeed by evading rather than demonstrating, and it was never specified in enough methodological detail to administer.
- The original Lovelace Test is **effectively unbeatable**: anyone with the resources to build the agent can explain its outputs, and even for learning systems "one can deduce the data necessary to produce" the output.

**Lovelace 2.0 formulation:** agent *a* must create artifact *o* of type *t* conforming to constraints *C*, where each constraint is any criterion expressible in natural language; a human **evaluator** *h* (who chose *t* and *C*) must be satisfied *o* is a valid instance of *t* meeting *C*; and a human **referee** *r*, expert in *t*, must confirm *t*+*C* is "not unrealistic for an average human."

Design properties worth stealing:
- Natural-language constraints make it "**Google-proof and resistant to Chinese Room arguments**" — you cannot retrieve your way past a novel constraint conjunction.
- **Aesthetic quality is explicitly not required.** The bar is what an unskilled average human could do.
- The evaluator is instructed to be **adversarial** — to craft constraints they expect the agent to fail.
- **Quantification via escalation**: run repeated challenges with increasing constraint counts until failure; score = mean number of challenges passed across evaluators.

Riedl's own acknowledged weaknesses: **no halting condition** (it ranks agents, it never declares one intelligent), and **constraint difficulty is unmeasured** — there's no calibration of how hard a given *C* is.

### Why discrimination tests reward typicality, not creativity

The **modified Turing test** (static artifacts, forced choice human-vs-machine) is distinct from Turing's interactive test and is used constantly in CC. **Pease & Colton (2011)**, "On impact and evaluation in computational creativity: a discussion of the Turing Test and an alternative proposal," AISB'11, pp. 15–22, give four objections:
1. **It encourages pastiche and superficial imitation.** ← this is the argument you were looking for
2. Absence of interaction undermines validity relative to the original test.
3. It withholds framing information from the evaluator.
4. No current system passes it, so it cannot discriminate among partially creative systems — it yields no gradient.

Lamb et al. concur: "The modified Turing test is not appropriate unless your specific goal is to imitate existing human work."

**The empirical version of this argument is stronger than the philosophical one.** Llano et al. (2014), Riedl & Young (2006), and Veale (2015) independently found that **human assessments of novelty/surprise are negatively correlated with most other desired criteria**. In Llano et al.'s study, **randomly generated statements were judged more surprising than either human or computational output**, while scoring poorly on everything else. Diedrich et al. (2015) found human judges' creativity ratings on divergent-thinking tasks correlated with novelty but **not** usefulness. This is Boden's "mere novelty" appearing as a measured effect, and it is the empirical vindication of Ritchie's structural decision to gate novelty behind typicality.

---

## 7. Product metrics: what has actually been computed

*(Partial — the subagent covering quality-diversity and generative-art aesthetics died. What follows is from Lamb et al.'s survey, read in full.)*

**Novelty operationalizations catalogued by Pease, Winterstein & Colton (2001):** difference relative to an inspiring set; degree of conceptual-space transformation; relative complexity; fuzzy-set membership against an archetype; Bayesian improbability; subjective human novelty judgment.

**Value operationalizations:** human emotional response; degree of goal achievement.

**Specific implementations:**
- **França et al. (2016)** — novelty as **Bayesian surprise**; value as **synergy**, a metric over expert judgments of the value of component *pairs*.
- **Bhattacharjya (2016)** — quality via a preference model mathematically combining aspects of subjective judgment.
- **Elgammal & Saleh (2015)** — novelty in art history via a **directed influence graph** built from semantic and perceptual traits, operationalizing Bartel's "originality = being an origin." Reported good correspondence with art historians' opinions. This is the most convincing product-novelty result in the survey, and note that it measures *influence*, not distance.
- **Lehman & Stanley (2012)** — **impressiveness**: easy to appreciate, hard to recreate.
- **Cropley & Cropley (2005)**, functional creativity for engineering — a **hierarchical, gated** model: Effectiveness → Novelty → Elegance → Generalizability, where each criterion only becomes relevant once the prior is met. "Without novelty, an engineering product is not creative; but if a product is not effective, its novelty does not matter to an engineer." **This is the closest thing in the literature to a defensible rubric for code and solution grading**, and its gating structure is the design pattern to copy.

**Computational aesthetics has largely failed as a creativity proxy.** Galanter (2012) surveys visual-art aesthetic measures: complexity, Zipf's law, JPEG compressibility, prototypicality — these "do correlate at least somewhat with human preferences," which is a weak claim. Art-school rules like color theory are hard to encode perceptually. Theories with more explanatory power exist (**representational fit**, Sammartino & Palmer 2012; **appraisal theory**, Silvia 2005; **Leder et al. 2004**'s stage model of aesthetic judgment ending in "cognitive mastery"), but Lamb et al. state flatly: "**None of these theories can yet be encoded precisely enough for a computer.**" Augustin et al. (2012) find humans value different qualities in visual art, film, and music — no cross-domain aesthetic measure.

**Rater expertise is a validated, not speculative, concern.** Amabile's CAT requires domain experts specifically because **non-expert judges lack inter-rater reliability, and even when they agree their judgments fail to correlate with expert judgment**. Gervás and Veale each independently worried their systems' output would be rated *too highly* by people who didn't understand it.

**Construct validity: essentially absent.** Lamb et al.: "The only model we are aware of that has been validity tested in this way is Carroll and Latulipe's (2009) Creativity Support Index" — which applies to co-creative tools, not autonomous generation. Every other framework in this report, including Ritchie's, the tripod, FACE/IDEA, and SPECS, **has never been construct-validity tested.**

**UNANSWERED:** Novelty search (Lehman & Stanley 2011) and MAP-Elites/quality-diversity — behavior characterization, novelty as mean distance to *k* nearest neighbors in behavior space, archive maintenance. I could not retrieve the formulas. On the strength of Lamb et al.'s framing and the field's structure, **I judge this the single most promising unexplored branch for your purposes** and recommend it as the first target of any follow-up: it is the only line of work that made novelty computable *and* demonstrably productive (it outperformed objective-driven search on deceptive problems), and it sidesteps the threshold-calibration problem entirely by defining novelty relative to a dynamically maintained archive rather than a fixed inspiring set.

---

## 8. Is evaluation the field's acknowledged weak point? Yes — with receipts

This is not a contested claim. Explicit statements:

- **Jordanous (2011):** "evaluation of computational creativity is not being performed in a systematic or standard way"; only 35% of papers presenting creative systems evaluated for creativity at all.
- **Lamb, Brown & Clarke (2018)**, opening sentence of the motivation: "**many attempts at computational creativity lack rigor, especially in evaluation.**"
- **Pearce, Meredith & Wiggins (2002):** "methodological malaise."
- **Jordanous (2012)**: creativity evaluation "has not kept pace with system development."
- No framework has become standard in 20+ years; the most-used one (tripod) was applied to 6 of 75 systems and functions as rhetoric rather than measurement.
- **No demonstrated orthogonal criterion set exists** — the dimensionality of creativity is unknown (Lamb et al. 2018, citing Pereira et al. 2005, Tapscott et al. 2016, Lamb et al. 2015).

Lamb et al. also fairly report **arguments against evaluating creativity at all** (their §7): domain specificity (Baer's position, that creativity is not a general trait and cross-domain measures are incoherent) and others I did not read in detail.

---

## 9. FIT / COST verdict for LLM-artifact grading

### Blunt classification

| Framework | Computable? | Verdict |
|---|---|---|
| **Ritchie's criteria** | Yes — fully formal, given *typ*/*val* raters and a defined *I* | **ADOPT THE STRUCTURE, NOT THE CHECKLIST** |
| **Wiggins CSF** | The *diagnostics* are; the formalism is not a metric | **ADOPT AS FAILURE-ATTRIBUTION VOCABULARY** |
| **Colton's tripod** | No — Bown's objection is fatal without stipulated definitions | **REJECT as a metric; the framing insight is real** |
| **FACE / IDEA** | Descriptive tuples; not comparable across systems | **REJECT** (and unverified here) |
| **SPECS** | Not a metric — a procedure | **ADOPT THE PROCEDURE + THE 14 COMPONENTS AS A RUBRIC PRIOR** |
| **Lovelace 2.0** | Yes, as a *protocol* | **ADOPT THE CONSTRAINT-ESCALATION PROTOCOL** |
| **Modified Turing test** | Yes, trivially | **REJECT — it measures typicality and rewards pastiche** |
| **CAT** | Yes | **ADOPT — the only method with a real validity pedigree** |
| **Computational aesthetics** | Partially | **REJECT as a creativity proxy** |
| **Cropley & Cropley functional creativity** | Yes, gated | **ADOPT for code/solution grading** |

### What implementing Ritchie's criteria on a batch of LLM outputs actually requires

Concretely, for a batch of *N* LLM artifacts:

1. **Define *I* explicitly.** This is the hard part and the one everyone gets wrong. For an LLM, the honest *I* is the pretraining corpus — unknowable. You must substitute a **stipulated *I***: the few-shot exemplars, the retrieved context, the prompt's referenced works, or a curated reference corpus for the genre. **Whichever you choose, the resulting novelty numbers are only interpretable relative to that choice**, and you must say so. Ritchie already flags this: where *I* is "a wide variety of examples with which the designer was acquainted, it may be very difficult to define its exact extent."
2. **Two independent grader passes**, not one: a *typ* pass ("is this a member of the intended class?") and a *val* pass ("is it good?"). These must be **separately prompted and separately scored**, or you reproduce the Divago collapse. Ritchie's warning generalizes exactly: if your grader's typicality judgment is implicitly "how similar is this to good examples," typicality and novelty are the same variable wearing two hats, and criteria 5–8 and 17–18 become algebraically degenerate.
3. **Do not use set membership for R ∩ I.** Ritchie names this as the framework's key weakness; with LLM outputs, exact-match overlap with *I* is essentially always empty, which makes criteria 9, 10, and 11–14 degenerate exactly as they did for WASP. **You need a similarity threshold** (embedding distance, n-gram overlap) to define "effectively replicated." This is the necessary repair and Ritchie explicitly invites it ("If we could define some form of distance metric between basic items, then a more subtle approach would be possible").
4. **Report a vector, not a verdict.** Drop *h* entirely. Ritchie endorses this. Report the criteria as a profile in [0,1] and use his facet grouping (basic success / conventional skill / unconventional skill / avoiding replication).
5. **Calibrate a, b, c from a human reference distribution**, per Ritchie's own suggestion — grade a matched set of human-authored artifacts, take the mean/SD of their *typ* and *val*, set thresholds from those. **Do not pick 0.5.** The WASP threshold table is the evidence for why: criteria 6, 7, 8 go to identically zero on a 0.2 threshold move.

**COST estimate:** two grader passes over *N* artifacts plus a human-reference calibration batch, plus an embedding index over *I*. The criteria arithmetic itself is trivial. The genuine cost is entirely in (a) defining *I* defensibly and (b) keeping *typ* and *val* graders independent.

**Is it worth it?** For a *profile* comparing model/prompt variants on the same task with the same *I* — yes, the untypical-and-good facet {6, 7, 8a, 18} measures something no standard eval captures, and it is the direct operationalization of "creative rather than merely competent." For an absolute creativity claim — no. Nobody in 20 years has made that work, and the criteria's non-independence means the 18 numbers are not 18 pieces of information.

### The four highest-value transfers, ranked

1. **Ritchie's two-axis separation of typicality from value, with novelty decomposed into untypicality + innovation.** This is the structural insight, and it is empirically supported from outside CC (Hekkert et al. 2003 found novelty and typicality contribute *separately* to human preference in design). Most LLM creative-writing rubrics collapse these into one "creativity" score and thereby measure neither. A grader that scores "is this a competent instance of the genre" and "is this good" and "how far is it from the reference set" as three separate quantities is strictly better instrumented, and the interesting cell — high value, low typicality — is the one that a single blended score makes invisible.

2. **Cropley & Cropley's gating.** Effectiveness → Novelty → Elegance → Generalizability, each gate only assessed if the prior passes. For code and solution grading this is obviously correct and directly implementable: a novel solution that doesn't work scores zero on creativity, not partial credit. It also solves the "mere novelty" problem structurally rather than by measurement, which matches your stated preference for constructions over checks.

3. **Lovelace 2.0's adversarial constraint escalation.** An evaluator who picks constraints they *expect* to fail, escalating constraint count until failure, scoring by mean challenges survived. This is a well-posed, Google-proof, gradient-producing protocol, and it is far more suited to LLM grading than anything else in the CC literature — it produces a continuous difficulty measure rather than a pass/fail, and natural-language constraint conjunctions are exactly what LLM graders can verify reliably. Its known gap (constraint difficulty is uncalibrated) is fixable with an item-response model, which is squarely in a statistician's wheelhouse.

4. **The CAT's criteria-free expert design as a validity check on your rubric.** Run a subset of artifacts through blind expert raters given *no rubric* and instructed only to use the full scale. If your rubric-driven grader scores don't correlate with criteria-free expert consensus, the rubric is measuring something else. This is the only validity-testing method in the literature with a real pedigree, and it is the check that would have caught the non-independence problems that took CC fifteen years to notice.

**Two things to steal from the failures rather than the successes:** Ritchie's "redefinitions disguise what is being measured" — freeze the operational definition of every rubric dimension before grading and version it, because silently redefining typicality as distance-to-corpus is the single most common way this goes wrong. And Pearce et al.'s purpose-first rule — decide whether a given eval is art, engineering, or science *before* choosing the method, because the three demand different evidence and mixing them produces the malaise.

**One caution against over-adopting this field.** Ritchie's criteria are the *only* fully formal product framework here, they have been applied to roughly four systems total, they behaved badly on all of them, and the field abandoned them. SPECS's real contribution is the 14 empirically-derived components and the discipline of stating a definition first. The tripod and FACE/IDEA are descriptive vocabulary with no measurement content — Bown's "cannot be distinguished from trivial pseudo-versions of themselves" is the correct verdict and RASTER is the proof. **The vocabulary is worth adopting to avoid reinvention; most of the frameworks are not worth implementing.** The instinct that CC "contains formal frameworks he would otherwise reinvent" is right about *inspiring set*, *typicality/novelty separation*, *uninspiration triad*, and *framing* — and wrong if it extends to expecting a working measurement instrument.

---

## 10. UNANSWERED / unverified

- **Wiggins' exact CSF formal notation** — the septuple/tuple structure, the interpreter operator, and the precise definitions of ℛ, 𝒯, ℰ. Secondary sources only. Primary: Wiggins 2006, *Knowledge-Based Systems* 19(7):449–458.
- **FACE/IDEA formal internals** — tuple composition, ground/process distinction, IDEA's impact/well-being measures. Not verified.
- **Whether framing information has ever been experimentally shown to change audience creativity judgments** — Charnley/Pease/Colton 2012 asserts it; I found no measurement.
- **Novelty search and quality-diversity formulas** (Lehman & Stanley 2011; Mouret & Clune MAP-Elites) — not retrieved. Flagged above as the highest-value follow-up.
- **Generative-art aesthetic measures in detail** — Machado & Cardoso's NEvAr measure, Birkhoff's M = O/C, Ralph's bell-curve, global contrast factor. Only Galanter's summary judgment obtained.
- **Ritchie (2012)'s redefinition of hopeless uninspiration** and the claimed contradiction.
- **Any post-2018 replication of Jordanous's evaluation-practice survey** — whether the 35% figure has improved in the LLM era. The music-generation evaluation survey ([arXiv:2506.05104](https://arxiv.org/pdf/2506.05104)) surfaced in search but was not read.
- **Lamb et al. §7 (arguments against evaluating creativity) and §3 (Person perspective / psychometric tests like TTCT)** — not read; the full text is at (local session cache, not included) if you want them without re-fetching.

**Local extracted full texts** (usable without further network calls):
- Ritchie 2007: (local session cache, not included)
- Lamb/Brown/Clarke 2018: (local session cache, not included)
- Jordanous 2011: (local session cache, not included)

**Sources:**
- [Ritchie 2007, Minds and Machines](https://link.springer.com/article/10.1007/s11023-007-9066-2) · [free PDF](https://users.dcc.uchile.cl/~nbaloian/CapstonePreparation/readings/Creativity.pdf)
- [Lamb, Brown & Clarke 2018, ACM Comput. Surv. 51(2):28](https://dl.acm.org/doi/10.1145/3167476) · [free PDF](https://cs.uwaterloo.ca/~jhoey/teaching/cogsci600/papers/Lamb2019.pdf)
- [Jordanous 2011, ICCC-11](https://kar.kent.ac.uk/42374/1/jordanous-2011a.pdf)
- [Jordanous 2012, Cognitive Computation 4(3)](https://link.springer.com/article/10.1007/s12559-012-9156-1) · [Kent repository PDF](https://kar.kent.ac.uk/42379/1/evaluatingComputationalCreativity2.pdf)
- [Jordanous 2014, meta-evaluation, ICCC-14](https://computationalcreativity.net/iccc2014/wp-content/uploads/2014/06/8.3_Jordanous.pdf)
- [Jordanous 2019, "Evaluating Evaluation" book chapter](https://link.springer.com/content/pdf/10.1007/978-3-319-43610-4_10)
- [Colton 2008, creative tripod PDF](https://classes.cc.gatech.edu/AY2013/cs7601_spring/papers/Colton-PerceptionofCreativity.pdf)
- [Pease & Colton 2011, Turing test critique](https://www.researchgate.net/publication/230855719_On_Impact_and_Evaluation_in_Computational_Creativity_A_Discussion_of_the_Turing_Test_and_an_Alternative_Proposal) · [Dundee record](https://discovery.dundee.ac.uk/en/publications/on-impact-and-evaluation-in-computational-creativity-a-discussion/)
- [Colton, Charnley & Pease 2011, FACE and IDEA](https://www.researchgate.net/publication/267954245_Computational_creativity_theory_The_FACE_and_IDEA_descriptive_models)
- [Riedl 2014, Lovelace 2.0, arXiv:1410.6142](https://arxiv.org/pdf/1410.6142) · [ar5iv HTML](https://ar5iv.labs.arxiv.org/html/1410.6142)
- [Wiggins CSF, extension paper ICCC 2019](https://computationalcreativity.net/iccc2019/papers/iccc19-paper-20.pdf) · [Action Selection in the CSF, ICCC 2020](https://computationalcreativity.net/iccc20/papers/011-iccc20.pdf) · [Creativity and MDPs, arXiv:2405.14966](https://arxiv.org/pdf/2405.14966) · [Creative Problem Solving in LLVMs, arXiv:2405.01453](https://arxiv.org/pdf/2405.01453)
- [Creative Zombie Apocalypse: A Critique of Computer Creativity Evaluation](https://www.academia.edu/26471515/Creative_Zombie_Apocalypse_A_Critique_of_Computer_Creativity_Evaluation)
- [Creativity and Machine Learning: A Survey, ACM Comput. Surv.](https://dl.acm.org/doi/abs/10.1145/3664595) · [arXiv version](https://arxiv.org/pdf/2104.02726)
- [Human Evaluation of Creative NLG Systems, arXiv:2108.00308](https://arxiv.org/pdf/2108.00308)
- [Computational Creativity and Music Generation Systems, Frontiers in AI](https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2020.00014/full)
- [Evaluating Creativity in Computational Co-Creative Systems, ICCC 2018](https://computationalcreativity.net/iccc2018/sites/default/files/papers/ICCC_2018_paper_26.pdf)