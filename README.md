# Novelty Hunt

A Claude Code skill that searches a solution space for genuinely original candidates and scores them without killing them.

Most "brainstorm then pick the best" workflows fail twice. The generation step produces paraphrases of the obvious answer, and the scoring step buries whatever originality survived. This skill is built against both failures, and every mechanism in it traces to a measured result from the creativity-assessment literature.

## The two problems this solves

**Aligned models are mode-collapsed.** Preference tuning sharpens output toward the familiar. Measured on a Tulu-70B: the base model retains 45.4% semantic diversity, 20.8% after SFT, 10.8% after DPO. Frontier models produce fewer than 4 functionally distinct answers in 10 samples, and bigger models collapse harder. Temperature does not fix it. Specific prompting structures do, and the skill uses the ones with measured recovery.

**Judges are biased against the thing you asked for.** Selecting on quality is selecting against novelty (r = -0.27 to -0.48 between novelty and capability benchmarks). Zero-shot LLM judging of originality agrees with experts at roughly chance (Cohen's kappa 0.02 to 0.04). LLM judges rate AI-typical polish up and semantic surprise down, the exact inversion of expert judgment. And the "pick the single best" framing itself induces anti-novelty bias in the judge. The scoring protocol counters each of these explicitly.

## How it works

Six phases, lightweight by default:

- **Phase 0**: route the domain, then actually generate the 2 to 4 obvious answers first. This "conventional core" is the baseline novelty gets measured against, and it stays in the running, because the obvious answer sometimes wins. The effectiveness rubric is frozen here, before any candidate exists.
- **Phase 1**: name the structural axes solutions can differ on. These form the archive grid.
- **Phase 2**: search rounds using validated diversity moves: verbalized sampling with explicit low-probability-tail requests (1.6 to 2.1x measured diversity gain), in-context regeneration, atypical injection, inversion, distant analogy, persona shifts. Candidates are archived best-per-cell so novel ones cannot be crowded out by a quality filter. Rounds stop when nothing new appears.
- **Phase 3**: cheap gates before any scoring. Sense first (91% of maximally-rare output is junk), then distance-from-core, then degeneracy. A gate is not a rating.
- **Phase 4**: scoring. Novelty is judged holistically after a forced "what exactly is original here" analysis; effectiveness is judged by the decomposed Phase 0 anchors. Separate passes, because LLM judges collapse any multi-dimension rubric into one latent score (inter-dimension r = .92 to .99). Batch-relative, blind, multiple-valid-solutions framing, persona repeats with dispersion reporting, gate-then-sum and never multiply.
- **Phase 5**: three named picks. **Best Conventional** (top of the core), **Frontier** (novel and effective and atypical, usually the answer), and a **Wildcard** that is never auto-dropped, because judges are structurally blind to the transformative tail. Each pick carries the deliverable in the form you asked for.

The hit-shape target comes from a 17.9M-paper analysis: winning work is a highly conventional core plus one injected atypical element, peaking at the 85th to 95th percentile of conventionality. The skill hunts for the atypical injection, not for wall-to-wall weirdness.

## What's in the box

```
SKILL.md                          the skill: calibration, phases, output contract
references/generation-moves.md    search playbook with prompt templates
references/scoring-protocol.md    judge blocks, panel design, bias counters
references/evidence.md            every constant above, with citations
references/research/              the 17 underlying deep-research reports
```

The evidence file is the part most repos skip. Every rule in the skill maps to a measured number, and open questions are listed as UNANSWERED instead of being filled with invented constants.

## Installation

### Claude Code

Personal (all projects):

```bash
git clone https://github.com/rwshiraishi/novelty-hunt.git ~/.claude/skills/novelty-hunt
```

Project-scoped (shared with your team via the repo):

```bash
git clone https://github.com/rwshiraishi/novelty-hunt.git .claude/skills/novelty-hunt
```

Claude Code discovers skills automatically. Trigger it by asking for "truly unique ideas", "original solutions", "unconventional options", or invoke it directly with the Skill tool. No restart needed.

### OpenAI Codex

Codex has no native skill loader, but the skill is plain markdown and works as an on-demand instruction file.

Option A, slash command: copy `SKILL.md` to `~/.codex/prompts/novelty-hunt.md`, then run `/novelty-hunt <your problem>` in the Codex CLI.

Option B, AGENTS.md pointer: clone the repo anywhere and add a line to your `AGENTS.md`:

```
When asked for truly original or unconventional ideas, read and follow
~/tools/novelty-hunt/SKILL.md (phases 0-5), including its references/ files.
```

### Cursor and other agents

Any agent that can read files can run the protocol. Add a rule (for Cursor: `.cursor/rules/novelty-hunt.mdc`) that points at `SKILL.md` and instructs the agent to follow its phases when originality is requested. The scoring protocol in `references/scoring-protocol.md` also stands alone as a judge-prompt spec for eval harnesses.

## Works with: solution-tournament

[solution-tournament](https://github.com/rwshiraishi/solution-tournament) is this skill's counterpart on the selection side. The tournament ranks concrete implementations for production fitness; its one structural blind spot is that it can only rank candidates somebody generated. Novelty-hunt is the generation front-end for that gap.

The seam runs both directions:

- **Hunt then tournament**: when the hunted artifact is an implementation choice, hand the Phase 2 archive to the tournament as its Phase 1 field (frozen candidate mechanisms, named axes). Declare novelty as a weighted rubric dimension in the tournament's Phase 0 if originality should count in the final ranking.
- **Tournament calls the hunt**: when the tournament's red-team answers its field-ceiling question ("name one credible approach absent from the field") with a real gap, or generation keeps producing paraphrases, run novelty-hunt before reopening Phase 1.

Division of labor in one line: novelty-hunt scores originality, the tournament scores production fitness.

## Cost and when not to run it

Lightweight mode runs in one agent at roughly 10 to 25k tokens. Full mode (subagent generators and judges, cross-model scoring) costs 40 to 90k and needs an explicit trigger. The skill includes a skip floor: for trivial asks, or when a conventional answer is plainly sufficient, it refuses to run and says why.

Honesty notes the skill prints about itself: scores are one synthetic panel's attribution, not a measurement of creativity; ranks are valid within a batch only; in lightweight mode the judge personas share one context, so their agreement is not independent evidence. Where a cheap execution test exists, it outranks every judge score.

## License

MIT
