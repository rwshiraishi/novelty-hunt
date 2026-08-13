# Boden & the 4C Model: Theory, Measurement, and What a Blind LLM Grader Can Actually Score

---

## PART A — MARGARET BODEN

### A.1 The primary texts

- Boden, M. A. (1990/2004). *The Creative Mind: Myths and Mechanisms*. Weidenfeld & Nicolson / Routledge (2nd ed. 2004).
- Boden, M. A. (1998). "Creativity and artificial intelligence." *Artificial Intelligence*, 103(1–2), 347–356.
- Boden, M. A. (2009). "Computer models of creativity." *AI Magazine* 30(3) — the compact restatement most secondary sources quote.

### A.2 P-creativity vs. H-creativity

**P-creativity** (psychological): an idea is novel *relative to the mind that produced it* — the person has not had it before, and could not have simply retrieved it. **H-creativity** (historical): novel relative to the whole of recorded human history — nobody has had it before.

Two properties matter enormously for measurement, and are routinely missed:

1. **H-creativity is a strict subset of P-creativity, and it is parasitic on it.** Boden's position is that H-creativity is *P-creativity plus a historical fact about the rest of the world*. There is no extra psychological mechanism. This means H-creativity is not a property of the artifact-generating process at all; it is a property of the artifact's relation to a corpus.
2. **P-creativity is defined over the creator's prior state, not the artifact.** It is therefore *unobservable from the artifact alone*. Two identical artifacts, one recombined from the creator's habitual repertoire and one arrived at by genuine restructuring, are P-creatively different and physically indistinguishable.

This is the first hard wall for a blind grader (see verdict, §C).

### A.3 The three types

From Boden 1998/2009, with the standard glosses reproduced in the computational-creativity literature ([Thornton, "A Quantitative Reconstruction of Boden's Creativity Theory"](https://users.sussex.ac.uk/~christ/papers/boden-reconstruction.pdf)):

| Type | Boden's gloss | Mechanism |
|---|---|---|
| **Combinational** | "novel (improbable) combinations of familiar ideas" | association, analogy, bisociation; no space required |
| **Exploratory** | generation of novel ideas by exploring a structured conceptual space | search within fixed generative rules; "minimal tweaking of fairly superficial constraints" |
| **Transformational** | transformation of one or more *dimensions* of the space, so that structures can be generated that could not have arisen before | alter the rules that define the space |

**Conceptual spaces** are, in Boden's own framing, *structured styles of thought* — ways of writing poetry, styles of sculpture or music, theoretical frameworks in chemistry or biology. They are culturally acquired and they determine what is *thinkable*. The definitional move that carries all the weight: transformational creativity produces things that were **impossible**, not merely improbable, in the prior space. Impossibility is the criterion; that is what separates transformation from exploration.

The three types are usually read as a continuum of increasing surprise — combinational surprises by improbability, transformational surprises by *impossibility*.

### A.4 The tri-criterion: novel, surprising, valuable

Boden requires all three. Note the internal structure:
- **Novel** — indexed to a reference class (self → P; history → H).
- **Surprising** — Boden explicitly distinguishes *statistical* surprise (improbable, combinational), *unexpected-but-possible* surprise (exploratory: "I knew the space had that, I just hadn't seen it"), and *impossibilist* surprise ("that couldn't have happened"). The three surprise types map one-to-one onto the three creativity types. This is the most measurement-relevant part of Boden and the most consistently mangled in secondary use.
- **Valuable** — Boden is candid that value is culture-relative, contested, and shifting; she does not supply a criterion. This is a deliberate deferral, not an oversight.

### A.5 Wiggins' formalization (CSF)

Wiggins, G. A. (2006). "A preliminary framework for description, analysis and comparison of creative systems." *Knowledge-Based Systems*, 19(7), 449–458. [ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0950705106000645) · [Goldsmiths repository](https://research.gold.ac.uk/1000/). Companion: "Searching for computational creativity," *New Generation Computing* 24(3). Updated as a 2016 Springer chapter, ["A Framework for Description, Analysis and Comparison of Creative Systems"](https://link.springer.com/chapter/10.1007/978-3-319-43610-4_2).

The CSF represents an exploratory creative system as a septuple. Ritchie's 2012 summary enumerates the seven components verbatim as:

> (i) the universal set of concepts; (ii) the language for expressing the relevant mappings; (iii) a symbolic representation of the acceptability mapping; (iv) a symbolic representation of the quality mappings; (v) a symbolic representation of the search mechanism; (vi) an interpreter for expressions like (iii) and (iv); (vii) an interpreter for expressions like (v).

In Wiggins' own notation: universe **U**, language **L**, rule-set **R** (defines the conceptual space), traversal strategy **T**, evaluation **E**, interpretation functions **[[.]]** and **⟨⟨R,T,E⟩⟩**. **[[R]]** and **[[E]]** are fuzzy memberships in [0,1]; Wiggins then **thresholds them at 0.5** to recover crisp sets — the conceptual space *C* is {c : [[R]](c) ≥ 0.5}.

**The central result, and it is a genuine one:** transformational creativity is *exploratory creativity at the meta-level*. The meta-level has exactly the same seven-part structure, but its universe is the set of expressions describing object-level systems. Searching that meta-space for a better ⟨R,T,E⟩ *is* transforming the conceptual space. Boden's dichotomy collapses into a level distinction.

The CSF also yields named failure modes, which are useful diagnostic vocabulary:
- **hopeless uninspiration** — no valued concepts exist in the universe at all;
- **conceptual uninspiration** — valued concepts exist but none lie in the agent's conceptual space;
- **generative uninspiration** — valued concepts lie in the space but this agent's traversal strategy cannot reach them.

Plus *aberration* (the system produces valued concepts that fall outside its own R — it is breaking its own rules) and *perversity* (the system's traversal actively avoids the valued region).

### A.6 The critique of Wiggins — and it is severe

The strongest source is **Ritchie, G. (2012), "A closer look at creativity as search," ICCC 2012, pp. 41–48** ([PDF](https://computationalcreativity.net/iccc2012/wp-content/uploads/2012/05/041-Ritchie.pdf)). Direct quotes from the paper:

> "the published versions of the CSF are at best a preliminary sketch: some details are unspecified, some natural extensions are undeveloped, and there are some formal errors or infelicities."

> "There is a minor slip in his definition, in that repeated applications of ⟨⟨R,T,E⟩⟩ ... will compute sequences (tuples) of concepts, not individual concepts."

The most damaging finding — the CSF **does not actually define transformation**:

> "the metalevel is assigning (potential) object level systems to these three categories. What the metalevel does not do is characterise a single object level system, or even a unique set of systems. This means that we do not, from the published papers, have a definition of how one object level system is a transformation of another, or how a computation at the metalevel will yield a new object level system — all that the metalevel provides is this tripartite classification."

That is: **the headline claim of the CSF (transformation = meta-level exploration) was, as published, not formally cashed out.** Ritchie supplies the missing definition himself, reformulating an exploratory creative system as a 4-tuple ⟨P, N, V, Q⟩ (possibilities, norms, value, quest/search), with N ≅ [[R]], V ≅ [[E]], Q(N,V) ≅ ⟨⟨R,T,E⟩⟩. His Definition 7:

> "(iii) S′obj is a **transformation** of Sobj using S^meta if S′obj is a revision of Sobj using S^meta and also N ≠ N′."

And then he immediately concedes the definition is nearly vacuous:

> "As this could be true even if N and N′ differ only on one element, proponents of transformation as a form of radical change might wish to enhance this definition."

**This is the crux.** Once formalized, "transformational" reduces to *any change whatsoever to the acceptability mapping* — a one-element difference in a fuzzy set counts. Every attempt to make it mean something stronger ("radical" change) requires an unspecified magnitude threshold on a space nobody can enumerate. Ritchie names the gap and does not fill it. **UNANSWERED in the literature: nobody has supplied a principled, domain-general threshold separating "revision" from "transformation."**

Ritchie also strips out the symbolic language L, noting the abstraction "states the essential relations within a creative system without regard for representational issues," and flags that Wiggins is inconsistent about whether R is a single expression or a set of expressions, and never separates the language from the particular expressions a given system uses.

Wiggins himself concedes divergence: he notes Ritchie (2007) "presents a completely different account of what is going on in 'transformational' creativity, in which the notion of transformation is not so clearly present."

**On implementability**: I did not find a source using the exact phrase "unimplementable" about the CSF. What exists is (a) Ritchie's formal-gap critique above; (b) the sociological evidence in **Jordanous, A. (2012), "A Standardised Procedure for Evaluating Creative Systems (SPECS)," *Cognitive Computation* 4(3)** ([Springer](https://link.springer.com/article/10.1007/s12559-012-9156-1)), which observes that Colton's informal "creative tripod" was adopted far more often than the formally stated predecessors (Ritchie's criteria, Wiggins' CSF) — a field voting with its feet against the formalisms; and (c) Pereira, Mendes, Gervás & Cardoso, ["Experiments With Assessment of Creative Systems: An Application of Ritchie's Criteria"](https://old.cisuc.uc.pt/publication/show/1043), who applied the criteria to three real systems (WASP poetry generator, Divago conceptual blender, Dupond paraphraser) and reported the **main difficulties of applying the methodology**. **Mark as UNANSWERED**: I found no published implementation of the *full* CSF (object + meta level, with an actual meta-level search over object-level systems) applied to a real generative system. The CSF is used descriptively, essentially always.

### A.7 Ritchie's criteria (2007) — the closest thing to a measurable Boden

Ritchie, G. D. (2007). "Some empirical criteria for attributing creativity to a computer program." *Minds and Machines*, 17(1), 67–99. [Springer](https://link.springer.com/article/10.1007/s11023-007-9066-2) · [full PDF](https://users.dcc.uchile.cl/~nbaloian/CapstonePreparation/readings/Creativity.pdf).

The apparatus (verified against the PDF text):
- **B** = the basic item set; **typ** and **val** ∈ RAT(B) = the **typicality** and **quality/value** ratings, each mapping items to numeric scores.
- **I ⊆ B** = the **inspiring set** — the items known to / used by the *program's designer*. This is the key construct: it stands in for "what was already available," making novelty computable as set difference.
- **R** = the **result set** the program produced.
- Threshold parameters: **α** (threshold to achieve high typicality), **β** (limit of untypicality), **γ** (threshold for good quality), **θ** (general comparison level used in every criterion).
- Notation: T_{α,1}(R) = high-typicality results; V_{γ,1}(R) = high-quality results; R − I = the genuinely novel results.

**18 criteria** (14 from Ritchie 2001, 4 added in 2007). Representative forms:

- **Criterion 1**: AV(typ, R) > θ — average typicality above threshold.
- **Criterion 2**: ratio(T_{α,1}(R), R) > θ — enough of the output is recognizably in-genre.
- **Criteria 3–4**: the same two shapes for quality.
- **Criterion 5**: quality *conditional on* typicality (good instances among genuine instances).
- **Criterion 6**: items that score *low* on typicality but high on quality — the untypical-but-good quadrant, the closest operational proxy Ritchie offers for Boden-style boundary-breaking.
- **Criteria 9–10/10a**: replication — what proportion of the inspiring set I does the program reproduce.
- **Criteria 11–18**: all restricted to **R − I** (novel output). E.g. **Criterion 17**: ratio(V_{γ,1}(R−I) ∩ T_{α,1}(R−I), R−I) > θ — novel, good, *and* typical (creativity within norms). **Criterion 18**: ratio(V_{γ,1}(R−I) ∩ T_{0,β}(R−I), R−I) > θ — novel, good, and *untypical* (original deviation from past practice).

Ritchie's own limitations, stated in the paper:
- The paper is **methodological, not empirical** — it says where to look for evidence, not what any program scored.
- Thresholds are frankly arbitrary: on Criterion 1, "It is not obvious or trivial to define such a threshold."
- The framework **deliberately disregards process** and analyzes artifacts post-hoc. Ritchie sets it up so that typicality and quality are primitive constructs, and novelty is *derived* from them plus the inspiring set — it is not measured directly.
- Criteria 11–18 are undefined when R − I = ∅; criteria 9–10 are undefined when I = ∅.
- Both typ and val "would normally be determined by human assessment."
- He is dismissive of process-based extensions: on Pease/Winterstein/Colton's randomness, surprise, complexity and meta-level formulae, he writes that "on closer inspection it is not clear where they would fit into, or alongside, our formalisation," and that they "do not explain why complexity and novelty are synonymous."

Colton, Pease & Ritchie's fine-tuning measures (reproduced in Ritchie 2007) are worth noting for eval-system design: knowledge subset K′ is *creatively irrelevant* if V_K = V_{(K−K′)}, *creatively useful* if V_{(K−K′)} ⊂ V_K, *creatively destructive* if V_K ⊂ V_{(K−K′)}; fine-tuning degree ft(K′) = |I ∩ (V_K − V_{(K−K′)})| / |(V_K − V_{(K−K′)}) − I|. **This is an ablation protocol** — it detects whether a system's apparent creativity is baked into its inputs. It requires running the generator with knowledge removed, which a *blind artifact grader* cannot do.

### A.8 Other critiques of Boden worth naming

- **The conceptual space is never formally defined.** Thornton's [quantitative reconstruction](https://users.sussex.ac.uk/~christ/papers/boden-reconstruction.pdf) exists precisely because Boden's central construct has no formal definition; the field has settled for reading it literally as "a space of concept representations," which is a gloss, not a definition.
- **Combinational creativity is undersold.** [Integrational creativity (Inquiry, 2024)](https://www.tandfonline.com/doi/full/10.1080/0020174X.2024.2389992) argues Boden is "so underwhelmed by the bathwater of combination that the baby of bisociation is thrown away as well" — drawing on conceptual blending, even simple combination has implicit-space structure Boden's taxonomy hides. Practical consequence: **the combinational/exploratory boundary is as leaky as the exploratory/transformational one**, because "mere combination" often presupposes an unstated space.
- **The three types are a continuum, not a partition.** Widely acknowledged; the boundary "remains an active area of discussion." A rubric that forces a categorical label is imposing structure the theory does not license.
- **Bundy's objection** (cited by Wiggins as motivating the formalization) that Boden's account fails to capture aspects of real creative situations — **UNANSWERED**: I did not retrieve Bundy (1994) directly and cannot state his argument precisely.

### A.9 Blunt verdict — which parts of Boden are beautiful but unmeasurable

**Measurable from artifacts alone (with a corpus):**
- H-novelty, approximately — it is a corpus-relative set-difference question, exactly what Ritchie's R − I formalizes. The approximation error is entirely the gap between your corpus and "recorded human history."
- Typicality — distance from genre norms. Directly computable, and LLM graders are decent at it.
- Value — only in the weak sense of an inter-rater-reliable rubric score. Boden herself refuses to define it, so you are supplying the definition, not measuring hers.
- Statistical (combinational) surprise — improbability under a distribution you fit. Legitimately measurable.

**Not measurable from artifacts, even in principle:**
- **P-creativity.** It is a two-place relation between artifact and *creator's prior mental repertoire*. No artifact carries that information. Any blind estimate of P-creativity is estimating H-creativity and mislabeling it.
- **The exploratory/transformational distinction.** This is the big one. The distinction is defined over *the generative rule system used*, not over outputs. Wiggins' CSF says so explicitly by placing it at the meta-level; Ritchie's Definition 7 confirms it by defining transformation as N ≠ N′ — a property of two *systems*, not two artifacts. **An artifact that lies outside space C is observationally identical whether it was produced by transforming C, by a lucky exploratory jump, by a generator that never had C, or by copying something from outside C.** Nobody has measured this reliably. I found no reliability study, no inter-rater agreement statistic, no validated instrument for the exploratory/transformational distinction in either the psychology or the computational-creativity literature. **Mark UNANSWERED with high confidence that the answer is "no."**
- **Impossibilist surprise.** Requires knowing the prior space's boundary. If you could enumerate the space's boundary you would not need the concept.
- **"Radical" transformation vs. trivial revision.** Ritchie explicitly leaves the magnitude threshold open.

**The honest summary:** Boden's taxonomy is a taxonomy *of processes*, dressed in language that sounds like it is about products. Every attempt at operationalization (Wiggins, Ritchie 2007, Ritchie 2012) either (a) relocates the distinction to a meta-level that requires access to the generator, or (b) reduces it to a threshold on a norm-conformity score, at which point "transformational" just means "atypical and good" — Ritchie's Criterion 18. **Criterion 18 is the entire operationalizable residue of transformational creativity.** That is a much smaller thing than Boden's concept, and pretending otherwise is where this literature usually goes wrong.

---

## PART B — THE 4C MODEL

### B.1 Primary text and definitions

Kaufman, J. C., & Beghetto, R. A. (2009). "Beyond big and little: The Four C Model of Creativity." *Review of General Psychology*, 13(1), 1–12. [SAGE/APA](https://journals.sagepub.com/doi/10.1037/a0013688) · [PDF](https://scispace.com/pdf/beyond-big-and-little-the-four-c-model-of-creativity-1ev858l3g5.pdf).

| Level | Definition | Distinguishing feature |
|---|---|---|
| **mini-c** | "novel and personally meaningful interpretation of experiences, actions, and events" | **intrapersonal**; creativity inherent in learning; typically not shared or externalized |
| **little-c** | everyday creativity — observable creative acts and products of hobbyists, amateurs, children, emerging experts | **social**: the product exists and is judged by others. This is the key differentiator from mini-c |
| **Pro-c** | professional-level expertise in a creative domain, reached by "developmental and effortful progression beyond little-c" | domain expertise; ~10-year rule; vocational |
| **Big-C** | eminent, legacy-level contribution | **posthumous / historical**; requires the field's verdict over time |

Theoretical lineage: mini-c draws on Vygotsky (personal-meaning construction in learning) and on Runco's *personal creativity* (1996); little-c aligns with the Plucker/Beghetto/Dow standard definition (novel + useful, *within a social context*); Big-C aligns with Csikszentmihalyi's systems model, where the *field* confers creativity.

**The developmental trajectory claim**: creativity is a lifespan developmental quality, not a fixed trait. mini-c is "the genesis of later forms of C." The motivating argument is corrective: judging creative potential by eminence criteria too early "unnecessarily limits who is considered creative," so "the creative potential of the many can get overshadowed by the accomplishments of the few."

Later development: Kaufman & Beghetto, ["Where Is the When of Creativity? Specifying the Temporal Dimension of the Four Cs"](https://journals.sagepub.com/doi/abs/10.1177/10892680221142803), *Review of General Psychology* (2023) — adds an explicit time axis, tacitly conceding that the original model under-specified when a judgment is made. Helfand, Kaufman & Beghetto, ["The Four-C Model of Creativity: Culture and Context"](https://link.springer.com/chapter/10.1057/978-1-137-46344-9_2) in Glăveanu's *Palgrave Handbook* (2016/2017) — concedes the culture gap.

### B.2 Instrument-to-C-level mapping

This is the most decision-relevant part for an eval system.

| C level | Instruments that target it | Reliability / validity status |
|---|---|---|
| **mini-c** | Self-report of subjective insight and personal meaning; think-aloud; interview; creative *metacognition* measures (Kaufman & Beghetto 2013b, "In praise of Clark Kent," *Roeper Review* 35(3), 155–165, [PDF](https://s3.amazonaws.com/jck_articles/KaufmanBeghettoRoeperInPress.pdf)); Kaufman, Beghetto & Watson (2015), "Creative metacognition and self-ratings of creative performance: A 4-C perspective," *Learning and Individual Differences* | **Requires first-person access by construction.** No product-only instrument exists or could exist |
| **little-c** | Divergent-thinking tests (TTCT, alternate uses); CAT on everyday products; K-DOCS self-report (Kaufman 2012, *PACA* 6(4), 298–308 — 50 items, 5 domains: Everyday, Scholarly, Performance, Scientific, Artistic); Biographical Inventory of Creative Behaviors, Creative Behavior Inventory | DT predictive validity **contested and probably poor** (see B.4). CAT reliable but expert-dependent. Self-report is self-report |
| **Pro-c** | CAT with genuine domain experts; Creative Achievement Questionnaire (CAQ); productivity counts; professional recognition | CAT α typically **.80–.90** with experts |
| **Big-C** | Historiometry (Simonton); eminence ratings; citation and canonization counts; posthumous survival | Only productivity/eminence measures reach genius level (Simonton, ["Quantifying creativity: can measures span the spectrum?"](https://www.tandfonline.com/doi/full/10.31887/DCNS.2012.14.1/dsimonton), *Dialogues in Clinical Neuroscience* 14(1), 2012) |

Simonton's mapping is the cleanest statement of the problem: creativity instruments, unlike intelligence tests, **do not span the range**. Each is calibrated to a band. He nominates the CAQ as the most promising single scale because it "scales from an effective zero point through degrees of little-c creativity up to domain-specific accomplishments of a very high order."

### B.3 Critiques of 4C

**(a) Runco's attack, and the authors' concession.** Runco (2014, *Creativity Research Journal* Comment) argued that dichotomous and sub-dichotomous representations — Big-C/little-c, and the Pro-c and mini-c subdistinctions — lack value, on the grounds that "reality is not categorical." Beghetto & Kaufman replied in ["Promise and Pitfalls in Differentiating Amongst the C's of Creativity," *CRJ* 27(2), 1–2 (2015)](https://reflectingcreatively.wordpress.com/wp-content/uploads/2018/03/24d8e-beghetto26kaufman28crj29.pdf). Their concessions are unusually direct and are the single most useful passage for answering "is it a taxonomy or a theory":

> "we have seen instances where the labels have been employed too rigidly and the distinction made too sharply. A number of our students, in first learning the model, sometimes ask many questions about whether particular creators or actions would be Pro-c or little-c or mini-c, and we have to emphasize with them that **the goal of the Four C model was not to invent a taxonomy upon which all creative actions could be sorted.**"

> "We also acknowledge that our descriptions of these levels of creativity can sometimes reinforce the very limitations we endeavored to address..."

> "Our intent has never been to argue in favor of different creativities or establish rigidly distinct categories."

> "we are the first to say that to the extent that these distinctions are not helpful in making sense of creativity, one should abandon them."

**Read that carefully.** The authors deny it is a sorting taxonomy, and they do not claim it is a predictive theory either. What they defend is its **heuristic and communicative utility** — it disambiguates which creativity a study is about (their worked example: the creativity/mental-illness literature is contradictory largely because studies conflate Big-C with little-c, and the relationship genuinely differs by level). **So: it is neither a validated taxonomy nor a falsifiable theory. It is a conceptual disambiguator with strong face validity, and its own authors say so.** Any eval system that treats a 4C label as a measurement is over-claiming beyond what the originators claim.

**(b) Discriminant validity.** **UNANSWERED.** I found no study establishing discriminant validity *among the four Cs* — i.e., no factor-analytic or MTMM evidence that mini-c, little-c, Pro-c and Big-C are empirically separable constructs rather than points on a single magnitude continuum. The closest empirical work is Kaufman & Beghetto (2013a), "Do people recognize the four Cs? Examining layperson conceptions of creativity," *Psychology of Aesthetics, Creativity, and the Arts* 7(3), 229–236 — which tests whether **laypeople's implicit theories** map onto the model, i.e. face/consensual validity, not discriminant validity of the constructs. Also relevant: ["Evaluation is Creation: Self and Social Judgments of Creativity Across the Four-C Model," *Educational Psychology Review* (2024)](https://link.springer.com/article/10.1007/s10648-024-09947-1). Given that the authors explicitly disclaim rigid categories, it is unclear that discriminant validity is even the right test — but the absence should be stated, not glossed.

**(c) The boundaries are acknowledged as fuzzy in the original paper.** Big-C is "hard to identify" because creators often arrive posthumously (Picasso, Mendel); "an important consideration is whether contributions stand the test of time." The little-c/Big-C boundary was the original problem the model was invented to fix, and mini-c/little-c turns on whether the product was externalized and socially judged — which is a fact about the *social event*, not the artifact.

**(d) Domain generality.** The model's levels are not domain-general: "what is true of Big-C or Pro-C creativity in one domain might not apply in other domains."

**(e) Culture and context.** Glăveanu's Five A's (actor, action, artifact, audience, affordances), ["Rewriting the Language of Creativity: The Five A's Framework," *Review of General Psychology* (2013)](https://journals.sagepub.com/doi/abs/10.1037/a0029528), attacks the Rhodes 4P tradition for an "individualistic, static, and oftentimes disjointed vision of creativity" written "largely from the perspective of the individual, and within individuals, from the perspective of cognitive functioning." Kaufman & Beghetto conceded enough to co-author the culture chapter in Glăveanu's own handbook. The sociocultural claim that bites hardest for an eval system: **context establishes the criteria for what counts as original and task-appropriate; creativity and context are inseparable.** A fixed rubric is a fixed context assumption.

**(f) Proposed extensions**, indicating the model is not settled: an ["ed-c" contextual domain](https://www.academia.edu/41974773/Exploring_and_Extending_the_4C_Model_of_Creativity_Recognising_the_value_of_an_ed_c_contextual_domain) for education; the 2023 temporal dimension.

### B.4 Divergent thinking — the instrument most likely to be borrowed, and the one with the worst record

Baer, J., ["How Divergent Thinking Tests Mislead Us: Are the Torrance Tests Still Relevant in the 21st Century?"](https://www.rider.edu/sites/default/files/2024-05/BaerTTCTDebate.pdf). The conclusion he draws, echoing Kogan (1983), Wallach (1970), Anastasi (1982), Crockenberg (1972), and more recently Sawyer, Simonton and Sternberg: **the predictive validity of no domain-general divergent-thinking test has been established.** DT scores were not significantly correlated with actual creative performance even when both were measured concurrently. There is a rebuttal literature (Kim, ["Proven Reliability and Validity of the TTCT"](https://www.researchgate.net/publication/254735578_Proven_Reliability_and_Validity_of_the_Torrance_Tests_of_Creative_Thinking_TTCT)) arguing critics generalize from single subtests, so treat this as **contested but with the burden of proof on DT**.

Hocevar's classic measurement review (1979/1981) identified the field's core problem as **lack of convergent validity across methods**, and judged a simple inventory of creative achievement the best measure "because it is direct, observable, and the best-validated predictor of future creative behavior." Forty-five years later, this is still roughly the state of play.

### B.5 The CAT — the instrument a blind LLM grader is actually imitating

Amabile's Consensual Assessment Technique: domain-appropriate experts independently rate products for creativity, with no rubric, using their own internalized domain sense; the score is the aggregate. Often called the gold standard.

- Inter-rater α typically **.80–.90** with experts.
- **Expert vs. novice** — Kaufman, Baer & Cole (2009), ["Expertise, Domains, and the Consensual Assessment Technique," *Journal of Creative Behavior*](https://onlinelibrary.wiley.com/doi/abs/10.1002/j.2162-6057.2009.tb01316.x): on short fiction, expert–novice correlation was only **r = .71**, mean differences small but significant, and **novice inter-rater reliability was potentially problematic**. Novices are not valid judges of the creativity of poems by college students. Novice–expert convergence **varies by domain** — better for captions than for poetry.
- **Temporal stability** — Barth et al. (2021), ["Creativity Assessment over Time: Examining the Reliability of CAT Ratings," *Journal of Creative Behavior*](https://onlinelibrary.wiley.com/doi/full/10.1002/jocb.462): aggregate product means are highly stable (ICCs > .90), but **individual raters drift over time**. The instrument is the judge, and the judge is not stationary.
- Domain-specific caveats: ["A CAT with caveats: is the Consensual Assessment Technique a reliable measure of graphic design creativity?"](https://www.tandfonline.com/doi/full/10.1080/21650349.2015.1084893).

**The load-bearing point:** the CAT's *entire* validity claim rests on the raters being domain experts, and its reliability is an aggregate property that individual raters do not possess.

### B.6 The Amusement Park Theoretical model — why one rubric cannot generalize

Baer, J., & Kaufman, J. C. (2005). "Bridging generality and specificity: The Amusement Park Theoretical (APT) model of creativity." *Roeper Review*, 27(3), 158–163. [T&F](https://www.tandfonline.com/doi/abs/10.1080/02783190509554310) · [ERIC](https://eric.ed.gov/?id=EJ698845). Updated: [Cambridge Handbook of Creativity Across Domains, ch. 2 (2017)](https://www.cambridge.org/core/books/abs/cambridge-handbook-of-creativity-across-domains/amusement-park-theoretical-model-of-creativity/E473C41B703E0C88EEB4E3C04AE91DB1).

Four hierarchical levels of increasing domain specificity:

1. **Initial requirements** — domain-general prerequisites (intelligence, motivation, suitable environment). Necessary, not sufficient, and shared across everything.
2. **General thematic areas** — broad clusters (e.g. artistic/verbal, scientific/math, interpersonal, everyday/problem-solving).
3. **Domains** — poetry, physics, jazz composition.
4. **Microdomains** — sonnets specifically; free jazz specifically.

The empirical backbone is Baer's domain-specificity work (Baer 1994 multi-domain training; Baer 1998 "The case for domain specificity in creativity"): **correlations between the same subjects' creative performance across different tasks are trivially small.** Diakidoy's parallel-instrument study (112 ninth graders, verbal TTCT vs. a content-specific Creativity in History Test, [*JCB* 2002](https://onlinelibrary.wiley.com/doi/10.1002/j.2162-6057.2002.tb01055.x)) found **both content-specific and task-specific factors have significant effects.**

**Direct implication for a single grader rubric: only APT Level 1 generalizes.** A rubric written at Level 1 (novelty, coherence, effort) will transfer across domains and will be nearly uninformative, because it discriminates on things all competent artifacts share. A rubric written at Level 3 or 4 will be informative and will not transfer — and worse, if applied off-domain it will produce *confidently wrong* scores rather than obvious failures, because the criteria are still linguistically applicable ("is it original?") while being substantively mis-calibrated. There is no free lunch at Level 2; general thematic areas are where the model says the specificity gradient is steepest and least characterized.

---

## PART C — FIT / COST VERDICT

**The use case:** an automated evaluation system in which LLM graders score artifacts **blind** — no access to the creator's process, no field of human experts, only the artifact and possibly a reference corpus.

### C.1 Boden

**ADOPT — genuine fit, real cost, worth paying**

| Construct | How to operationalize blind | Cost / what you lose |
|---|---|---|
| **H-novelty** (approximate) | Ritchie's R − I made concrete: your reference corpus **is** the inspiring set I. Score novelty as distance-to-nearest-neighbor / non-membership relative to the corpus. | Your novelty claim is only as good as corpus coverage. State the corpus explicitly as the reference class in every score. Do not call the result "novelty" unqualified. |
| **Typicality** (Ritchie's `typ`) | Blind LLM graders are genuinely good at genre-conformity judgments; this is a recognition task, exactly what pretraining supplies. | Cheap and reliable. Take it. |
| **Value** (`val`) | A written rubric. Boden supplies no criterion, so you must and should own the definition explicitly. | You are not measuring "Boden value." You are measuring rubric compliance. Label it that way. |
| **Combinational / statistical surprise** | Improbability of the combination under a fitted or LLM-estimated distribution. | Legitimate. The only one of Boden's three surprise types that survives blind measurement. |
| **Criterion 18** — `ratio(V_γ(R−I) ∩ T_{0,β}(R−I), R−I)` | The **novel + high-quality + atypical** cell. This is the single best artifact-only proxy for "boundary-pushing." | It is a proxy, not the thing. It cannot distinguish transformation from a lucky exploratory jump from off-corpus copying. |
| **CSF failure-mode vocabulary** | *conceptual* vs *generative uninspiration* is a useful diagnostic when your generator produces nothing good: is the target outside its space, or merely unreachable by its search? | Requires generator access, so it is a **debugging** frame, not a grading frame. |
| **Fine-tuning / ablation measures** (Colton-Pease-Ritchie) | ft(K′) detects creativity baked into inputs. | Requires re-running the generator with knowledge ablated — **out of scope for a blind artifact grader.** If your system ever gains generator access, this is the highest-value addition available. |

**REJECT — cannot be operationalized blind, even in principle**

- **P-creativity.** Two-place relation to the creator's prior repertoire. No artifact carries it. Cost of pretending otherwise: you will report H-novelty under a P-creativity label and your scores will systematically punish creators with unusual backgrounds and reward creators whose prior work is absent from your corpus.
- **The exploratory/transformational distinction as a label on artifacts.** Defined over generative rule systems (Wiggins: meta-level; Ritchie Def. 7: N ≠ N′). Blind graders will nonetheless *produce* a confident label if asked — this is the specific failure mode to design against. **If you put "is this transformational?" in a rubric you will get high inter-rater agreement on a construct with no referent in the data.** That is the worst possible outcome: reliable and meaningless.
- **Impossibilist surprise.** Requires the prior space's boundary.
- **"Radical" vs. trivial transformation.** Ritchie leaves the magnitude threshold explicitly open; nobody has closed it. Any threshold you pick is invented, and per the house rule on fabricated constants it must be marked as such in source.

**Recommended framing:** use Boden as **vocabulary and as a novelty-decomposition scheme**, not as a scoring dimension. Concretely: score `typ`, `val`, and corpus-novelty separately; report the **2×2 of (novel × typical)** with Criterion 17 and 18 as named cells; never emit a combinational/exploratory/transformational label.

### C.2 4C

**ADOPT (narrowly)**

- **The reference-class discipline.** 4C's real contribution is forcing "creative *compared to what*" to be answered before scoring. Your grader should carry an explicit reference class per artifact — the corpus, and the expected competence band — and the score should be meaningless without it. This is the disambiguation function Beghetto & Kaufman themselves defend.
- **Pro-c-band CAT emulation, with honest error bars.** The CAT is what a blind LLM grader most resembles. Adopt its protocol shape (multiple independent raters, aggregate, no leakage between raters) and inherit its known properties: aggregate ratings are stable, individual raters drift, and expert–novice correlation is only ~.71 in verbal domains. **Budget for rater ensembles, not single calls.** An LLM grader with unknown domain expertise is, by default, a *novice* rater — the literature says novice inter-rater reliability is "potentially problematic" and novices are not valid judges in some domains.

**REJECT**

- **mini-c.** Definitionally requires first-person access to personal meaning-making. There is no product-only instrument, and there could not be. Blind grading of mini-c is a category error. Cost of forcing it: you would be scoring "does this look like it was personally meaningful," which is a style detection task wearing a construct's name.
- **Big-C.** Requires the field's verdict, and Kaufman & Beghetto note it typically arrives *posthumously* and turns on standing "the test of time." A grader operating at generation time has none of the required information. Any Big-C score is a prediction, not a measurement, and should be labeled as such or omitted.
- **4C level as an output label.** The authors deny the model is a sorting taxonomy. Discriminant validity among the four Cs is **UNANSWERED** in the literature. Emitting a C-level per artifact claims more than the model's own authors claim.
- **Divergent-thinking scoring (fluency/flexibility/originality) as a general creativity score.** Predictive validity for domain-general DT is unestablished (Baer; Kogan; Wallach; Sawyer; Simonton; Sternberg), contested by Kim. If you use DT-style counts, use them as *ideation-volume* diagnostics, never as a creativity score.

### C.3 The domain-specificity cost (APT) — the constraint that dominates rubric design

This is the finding with the largest practical consequence and the one most likely to be ignored.

- **A single grader rubric can generalize across domains only at APT Level 1** — and Level 1 criteria are, by construction, the necessary-not-sufficient prerequisites shared by everything. Expect low discriminative power.
- **Level 3/4 rubrics are informative and non-transferable.** Empirically: cross-task creativity correlations are trivially small (Baer); task-specific and content-specific factors both matter (Diakidoy); CAT novice–expert agreement varies by domain (captions ≠ poetry).
- **Therefore: a per-domain rubric family with a shared Level-1 spine is the only design the evidence supports.** The cost is real — you must author and validate a rubric per domain, and validate the *domain router* too, because a misrouted artifact gets scored confidently by the wrong instrument.
- Additional cost from the sociocultural critique (Glăveanu; Helfand/Kaufman/Beghetto): **your rubric is a frozen context.** Judgments of originality and appropriateness are determined by a sociocultural-historical context; a fixed rubric silently asserts one. This is not fixable, only disclosable.

### C.4 Compact decision table

| Construct | Blind-scorable? | Cost if adopted | Cost if faked |
|---|---|---|---|
| Boden H-novelty (corpus-relative) | **Yes** | corpus curation + explicit reference class | — |
| Boden typicality | **Yes** | ~free | — |
| Boden value | **As rubric only** | you own the definition | claiming it is "Boden value" |
| Combinational surprise | **Yes** | distribution estimation | — |
| Criterion 17/18 quadrants | **Yes** | threshold params α, β, γ, θ are arbitrary — mark EXTRAPOLATED | — |
| Exploratory vs transformational | **No, in principle** | — | **reliable-but-meaningless labels**; worst failure mode |
| Impossibilist surprise | **No, in principle** | — | invented space boundary |
| Boden P-creativity | **No, in principle** | — | systematic bias against unusual creators |
| 4C reference-class discipline | **Yes** | per-artifact reference class metadata | scores that mean nothing |
| CAT-style Pro-c emulation | **Yes, with ensembles** | multiple rater calls; drift monitoring | single-rater scores with false precision |
| 4C mini-c | **No, in principle** | — | style detection mislabeled as insight |
| 4C Big-C | **No** (prediction, not measurement) | — | fabricated eminence forecast |
| 4C level label | **No** (validity UNANSWERED) | — | over-claiming past the authors' own claims |
| Single cross-domain rubric | **Only at APT L1** | low discrimination | confident off-domain miscalibration |
| Fine-tuning / ablation (CPR) | **No** (needs generator) | — | — |

### C.5 Explicit UNANSWERED list

1. No reliable, validated instrument for the **exploratory vs. transformational** distinction exists in either literature. I searched the computational-creativity and psychometric sides and found none. High confidence the answer is "nobody has done this," but recorded as UNANSWERED rather than a proven negative.
2. No published **full implementation of the CSF** including an operating meta-level search over object-level systems. The CSF is used descriptively.
3. No **discriminant-validity** study separating the four Cs as constructs. Kaufman & Beghetto (2013a) tests layperson recognition, which is a different question.
4. No principled **magnitude threshold** distinguishing "revision" from "transformation" — Ritchie names the gap explicitly and leaves it open.
5. **Bundy's (1994) objection to Boden** — cited by Wiggins as motivating the formalization; I did not retrieve the primary source and cannot characterize the argument.
6. Whether the phrase "**unimplementable**" is applied to the CSF by any named critic — not verified. What is verified is Ritchie's "at best a preliminary sketch ... some formal errors or infelicities," his finding that transformation is undefined as published, Jordanous's observation that informal frameworks out-adopted formal ones, and Pereira et al.'s report of practical difficulty applying Ritchie's own criteria.

---

**Sources:**
- [Boden — Four C / Boden overview via ScienceDirect topics](https://www.sciencedirect.com/topics/psychology/four-c-model)
- [Wiggins 2006, Knowledge-Based Systems](https://www.sciencedirect.com/science/article/abs/pii/S0950705106000645) · [Goldsmiths repo](https://research.gold.ac.uk/1000/) · [2016 Springer chapter](https://link.springer.com/chapter/10.1007/978-3-319-43610-4_2)
- [Ritchie 2012, "A closer look at creativity as search," ICCC](https://computationalcreativity.net/iccc2012/wp-content/uploads/2012/05/041-Ritchie.pdf)
- [Ritchie 2007, Minds and Machines 17(1)](https://link.springer.com/article/10.1007/s11023-007-9066-2) · [full PDF](https://users.dcc.uchile.cl/~nbaloian/CapstonePreparation/readings/Creativity.pdf)
- [Thornton, "A Quantitative Reconstruction of Boden's Creativity Theory"](https://users.sussex.ac.uk/~christ/papers/boden-reconstruction.pdf)
- [Integrational creativity (Inquiry, 2024)](https://www.tandfonline.com/doi/full/10.1080/0020174X.2024.2389992)
- [Jordanous, SPECS, Cognitive Computation 2012](https://link.springer.com/article/10.1007/s12559-012-9156-1)
- [Pereira et al., "Experiments With Assessment of Creative Systems"](https://old.cisuc.uc.pt/publication/show/1043)
- [Kaufman & Beghetto 2009, Review of General Psychology](https://journals.sagepub.com/doi/10.1037/a0013688) · [PDF](https://scispace.com/pdf/beyond-big-and-little-the-four-c-model-of-creativity-1ev858l3g5.pdf)
- [Beghetto & Kaufman 2015, "Promise and Pitfalls in Differentiating Amongst the C's," CRJ 27(2)](https://reflectingcreatively.wordpress.com/wp-content/uploads/2018/03/24d8e-beghetto26kaufman28crj29.pdf)
- [Kaufman & Beghetto 2013b, "In praise of Clark Kent," Roeper Review](https://s3.amazonaws.com/jck_articles/KaufmanBeghettoRoeperInPress.pdf)
- [Kaufman & Beghetto 2023, "Where Is the When of Creativity?"](https://journals.sagepub.com/doi/abs/10.1177/10892680221142803)
- [Helfand, Kaufman & Beghetto, "Four-C Model: Culture and Context"](https://link.springer.com/chapter/10.1057/978-1-137-46344-9_2) · [PDF](https://firstuniv.com/pluginfile.php/3161/mod_resource/content/1/HelfandKaufmanBeghetto2017.pdf)
- [Glăveanu 2013, "Rewriting the Language of Creativity: The Five A's"](https://journals.sagepub.com/doi/abs/10.1037/a0029528)
- [Baer & Kaufman 2005, APT model, Roeper Review](https://www.tandfonline.com/doi/abs/10.1080/02783190509554310) · [ERIC](https://eric.ed.gov/?id=EJ698845) · [Cambridge Handbook ch. 2](https://www.cambridge.org/core/books/abs/cambridge-handbook-of-creativity-across-domains/amusement-park-theoretical-model-of-creativity/E473C41B703E0C88EEB4E3C04AE91DB1)
- [Baer, "How Divergent Thinking Tests Mislead Us"](https://www.rider.edu/sites/default/files/2024-05/BaerTTCTDebate.pdf) · [Kim rebuttal](https://www.researchgate.net/publication/254735578_Proven_Reliability_and_Validity_of_the_Torrance_Tests_of_Creative_Thinking_TTCT)
- [Diakidoy 2002, "Domain Specificity in Creativity Testing," JCB](https://onlinelibrary.wiley.com/doi/10.1002/j.2162-6057.2002.tb01055.x)
- [Kaufman, Baer & Cole 2009, "Expertise, Domains, and the CAT," JCB](https://onlinelibrary.wiley.com/doi/abs/10.1002/j.2162-6057.2009.tb01316.x)
- [Barth et al. 2021, "Creativity Assessment over Time," JCB](https://onlinelibrary.wiley.com/doi/full/10.1002/jocb.462)
- ["A CAT with caveats," Journal of Design Creativity](https://www.tandfonline.com/doi/full/10.1080/21650349.2015.1084893)
- [Simonton 2012, "Quantifying creativity: can measures span the spectrum?"](https://www.tandfonline.com/doi/full/10.31887/DCNS.2012.14.1/dsimonton)
- [Hocevar, "Measurement of Creativity: Review and Critique" (ERIC)](https://eric.ed.gov/?id=ED175916)
- ["Evaluation is Creation," Educational Psychology Review 2024](https://link.springer.com/article/10.1007/s10648-024-09947-1)
- ["Exploring and Extending the 4C Model: ed-c contextual domain"](https://www.academia.edu/41974773/Exploring_and_Extending_the_4C_Model_of_Creativity_Recognising_the_value_of_an_ed_c_contextual_domain)
