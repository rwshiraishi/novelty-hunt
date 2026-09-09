# Novelty Hunt

**Explore original ideas and bring back three useful directions: conventional, frontier, and wildcard.**

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE) · Built by [Neon Peach, LLC](https://neonpeach.co)

[Why use it?](#why-use-it) · [How it works](#how-it-works) · [The three picks](#what-the-three-picks-let-you-decide) · [Pair with Tournament](#why-use-novelty-hunt-with-solution-tournament) · [Larger builds](#taking-the-selected-idea-into-a-larger-build) · [Install](#install)

Novelty Hunt turns “give me something more original” into a repeatable search process. It helps your AI agent explore different mechanisms and perspectives, preserve promising alternatives, and return usable ideas with a clear explanation of what makes them distinctive.

Use it for product concepts, positioning, naming, technical designs, or creative text when exploring a wider range of options matters. You get a conventional baseline, a practical frontier candidate, and a wildcard with a reason to investigate it—each in the form you requested.

## Why use it?

A longer list of answers does not necessarily give you a wider choice. Ten product concepts can share the same business model. Ten proposed fixes can move the same logic between files. Ten positioning lines can repeat the same promise with different adjectives.

Novelty Hunt asks what would make an option functionally different: a different actor, timing, mechanism, assumption, or combination of components. It establishes the familiar answers first, then searches beyond them. Distinct ideas are preserved in an archive before they have to compete on overall usefulness, giving less familiar approaches a chance to develop.

The value is a more useful range of choices. You can stay with the conventional answer for a clear reason, select a distinctive option that still fits your constraints, or reserve an ambitious direction for a focused experiment. A strong conventional core with one unusual element can be a more attractive result than an idea that changes everything at once.

## Where it brings the most value

| Situation | What Hunt helps you explore |
|---|---|
| Product concepts feel interchangeable | Different mechanisms, audiences, or ways of delivering value |
| Positioning sounds like every competitor | Distinct promises and perspectives expressed as usable copy |
| A technical problem keeps producing the same fixes | Alternatives that change an underlying assumption |
| An existing approach is good but needs differentiation | One unusual addition that preserves the useful core |
| You want an ambitious option without betting the project on it | A wildcard and a concrete next experiment |

For a routine question or a task where speed matters more than originality, a direct answer is usually sufficient. Hunt is most useful when a broader search could change your decision.

## What you can do

- Establish the conventional answers so originality has a clear reference point.
- Search through analogy, inversion, unusual combinations, and changes in perspective.
- Keep an archive of functionally distinct ideas instead of repeated variations in wording.
- Compare novelty and effectiveness as separate dimensions.
- Receive finished picks in the form you asked for: names, positioning lines, draft copy, or mechanism sketches.

## Try it

```text
Use novelty-hunt to find a positioning direction for our data visualization tool.
Our audience is small research teams. Go beyond “powerful, flexible,
collaborative.” Give me a positioning line and short explanation for each pick.
```

For a deeper technical search:

```text
Run a full novelty hunt for alternatives to our current report-delivery flow.
Use subagents to explore distinct mechanisms. Preserve a conventional option,
a practical frontier candidate, and a wildcard worth investigating.
```

## How it works

### 1. Make the familiar answers explicit

The agent generates two to four conventional answers and defines what an effective answer must accomplish. This baseline gives originality a reference point. An option is not novel just because its wording differs from another option in the list.

Your constraints also shape the search from the beginning: who the output is for, what it must do, and what would make it unusable. That keeps exploration connected to the problem you actually want to solve.

### 2. Map where ideas can differ

Hunt identifies structural axes such as who acts, when work happens, where logic lives, or which assumption changes. These form an archive that records how candidates differ. Equivalent ideas are merged so repeated wording does not inflate the apparent range of options.

### 3. Search through several creative moves

The agent chooses a mix of moves rather than repeatedly asking the same question:

| Move | What it does | Illustrative report-delivery example |
|---|---|---|
| Verbalized sampling | Asks for familiar and unlikely answers across a range of possibilities | Include approaches a team would rarely propose first |
| Inversion | Reverses an assumption shared by the existing answers | What if the report did not have to be generated when requested? |
| Distant analogy | Transfers a concrete mechanism from another field | Could publishing or logistics suggest a different delivery pattern? |
| Atypical injection | Adds one unusual element to a conventional approach | Keep standard reports but add a change-only update mechanism |
| Lens shift | Searches from different disciplines or incentives | Compare the operator's, analyst's, and customer's perspectives |
| Regeneration | Uses the existing archive to demand an additional distinct idea | Find a mechanism absent from every option already listed |

In full mode, separate generators explore different moves without seeing each other's intermediate output. The lead merges their results before the next round. This gives the search multiple starting points while retaining a coherent archive.

### 4. Connect possibilities to practical constraints

Candidates need a coherent mechanism and must respect hard constraints. Where a cheap prototype, dry run, render, or artifact check can answer an important question, Hunt uses it before scoring. The purpose is to improve the ideas and the comparison while changing direction is still inexpensive.

A failed experiment can also reveal an overlooked approach. The agent investigates the failure and can add a newly discovered mechanism to the candidate field.

### 5. Compare originality and usefulness separately

Hunt asks two different questions: what is original relative to the baseline, and how well does the candidate meet the task's effectiveness criteria? Keeping those dimensions separate lets you see why an unusual idea deserves attention even when it is less mature than a familiar one.

The comparison includes qualitative reasoning alongside within-hunt ratings. Multiple perspectives can expose different tradeoffs; single-agent personas remain perspectives from one model, not independent experts.

### Choose the depth of the hunt

| Mode | Shape | Useful for |
|---|---|---|
| Lightweight | One agent, one or two search rounds, a target archive of 6–12 candidates | Focused exploration |
| Full | Separate generators and judges; up to three search rounds under the full-mode playbook, stopping earlier when exhausted or budget-limited | Deeper searches, exhausted conventional options, or an explicit full hunt |

Full mode can use different model families where configured. Set a budget appropriate to the decision; extra rounds are useful only when they add meaningful alternatives. The [generation playbook](references/generation-moves.md) explains the detailed search mechanics.

## What you get

| Pick | Purpose |
|---|---|
| **Best Conventional** | A strong familiar answer and a useful baseline |
| **Frontier** | An unusual approach with a credible path to usefulness |
| **Wildcard** | The most novel surviving direction, with its uncertainty and a practical next experiment |

The result includes the candidate archive, a comparison table, and the requested deliverable for each available pick. A narrow or exhausted field is reported honestly rather than padded with duplicates.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="diagrams/phases-dark.png">
  <img alt="Novelty Hunt workflow" src="diagrams/phases-light.png" width="640">
</picture>

## What the three picks let you decide

The picks serve different purposes. **Best Conventional** gives you a familiar option to compare against. **Frontier** shows how far you can move toward originality while retaining a credible path to usefulness. **Wildcard** keeps a potentially valuable but less certain idea visible, with a next experiment that can clarify its promise.

For a naming task, each available pick contains an actual name and positioning line. For a technical task, it contains a mechanism sketch and relevant experiment results. You receive something you can discuss, develop, or test, rather than just labels such as “innovative platform.”

You are free to choose the conventional option. Hunt's contribution is showing the alternatives and the reasons for choosing, not requiring novelty for its own sake.

## Why use Novelty Hunt with Solution Tournament?

**Hunt helps you discover a wider field. Tournament helps you choose an implementation from that field.** The combination is especially useful when a technical decision needs both imagination and a disciplined comparison of engineering tradeoffs.

A hunt's Frontier pick balances originality and effectiveness for the stated request. An implementation decision may also depend on migration complexity, operating burden, resilience, maintainability, or cost. [Solution Tournament](https://github.com/rwshiraishi/solution-tournament) makes those priorities explicit in a weighted rubric, compares structurally different candidates, challenges the finalists, and can carry the selected approach into code.

Together, the skills address two distinct opportunities: discovering an option you would otherwise miss, and understanding whether that option is the right one to build. Tournament includes its own candidate generation, so Hunt is optional. Add it when the obvious field feels exhausted or when differentiation is part of the objective.

### Example: make reporting faster without assuming more compute

Imagine a reporting product where users wait too long for updated results. A hunt might preserve conventional query optimization, explore maintaining results incrementally, and consider changing the user experience so expensive detail is generated on demand. A wildcard could challenge an even more fundamental assumption about when a report needs to exist.

These are illustrative candidates, not recorded outcomes. Hunt explains their distinct mechanisms and checks cheap factual questions. Tournament then compares the viable approaches against the real system's freshness requirements, correctness needs, maintenance capacity, and migration budget. It might favor a conventional fix or a carefully chosen hybrid. The novelty scores do not predetermine that choice.

### Choose the right handoff

| Your next need | Recommended workflow |
|---|---|
| Original names, copy, positioning, or concepts ready to consider | Finish with Hunt's three picks |
| A challenge to the leading idea or wildcard | Reuse the hunt for a focused adversarial review or experiment |
| A production implementation decision with competing engineering priorities | Start Tournament at Phase 0 and reuse the viable hunt candidates |
| A tournament whose candidates share the same assumptions | Use Hunt's generation moves to search for missing alternatives |

The handoff carries the raw candidate text, conventional baseline, structural differences, constraints, and experiment results. Keep the original ratings labeled as hunt ratings. A full Tournament defines its own rubric before scoring; novelty becomes one of its dimensions only when agreed up front.

This distinction prevents duplicated work. You do not need to run two full scoring exercises just to ask whether Hunt's top idea holds up. A focused challenge can reuse the existing analysis. A separate tournament is worthwhile when the new question is which approach to implement under a different set of engineering priorities.

```text
Use novelty-hunt to explore alternatives to our current report-delivery design.
Preserve the conventional baseline, a frontier option, and a wildcard.
Then run solution-tournament on the viable implementation approaches.
Reuse the candidate descriptions and experiments, and establish a production
rubric before scoring. Prioritize freshness, correctness, and operating simplicity.
Use subagents for full-mode work. Recommend before editing.
```

For a lighter follow-up:

```text
Use the hunt's existing archive. Challenge the Frontier and Wildcard picks
with a focused adversarial review and the cheapest useful experiment.
Do not run another full scoring process. Explain what the findings change.
```

## Taking the selected idea into a larger build

When implementation has several independent parts, [Foreman](https://github.com/rwshiraishi/foreman) can coordinate the execution stage. The lead turns the selected approach into task cards, assigns workers, and integrates their outputs. Foreman discovers dispatchable models and matches model bands to the work: frontier reasoning for the lead's consequential decisions, standard models for substantial implementation, and economy models for suitable mechanical tasks.

```text
Novelty Hunt: explore possibilities
        ↓ candidate archive and experiment results
Solution Tournament: choose an implementation approach
        ↓ selected mechanism, tradeoffs, and decision record
Foreman: decompose, assign models, and build
```

This is an explicit handoff between installed skills. Hunt does not automatically invoke Tournament, and Tournament does not automatically select or upgrade the Foreman's lead model. The lead normally uses the current session model; worker overrides depend on the host's capabilities and permissions. See [Tournament's model-role explanation](https://github.com/rwshiraishi/solution-tournament#model-roles-tournament-and-foreman) for the division of responsibilities.

## Getting value from the extra model work

A useful hunt should broaden the choice enough to justify the exploration. Start lightweight for focused questions. Use full mode when independent generation could reveal a materially different approach, and reuse the archive when moving into review or implementation.

For larger work, the combination offers several opportunities for efficiency: settle a consequential design choice before many implementation tasks depend on it, send workers compact relevant context, assign suitable tasks to less expensive models, and keep retries focused on the affected task. Independent work can also run in parallel.

These benefits affect different measures. A cheaper model can lower cost per token; a smaller task packet can reduce repeated input; parallelism can shorten elapsed time. Running generators, judges, and checkers adds tokens, so none of these mechanisms guarantees lower total token usage. The aim is useful delivered work for the available budget, including the rework a better decision may avoid.

Hunt itself prioritizes generation, cheap execution tests, and an adversarial challenge when the budget is constrained; repeated persona scoring is reduced first. Foreman's implementation report separately records models, retries, escalations, and estimated cost. This gives you a practical basis for deciding whether a larger workflow was worth it.

## Install

This repository provides an Agent Skill: instructions and supporting resources loaded by a compatible AI agent. It is a separately installed community skill.

Clone once:

```bash
mkdir -p ~/dev
git clone https://github.com/rwshiraishi/novelty-hunt.git ~/dev/novelty-hunt
```

For Claude Code:

```bash
mkdir -p ~/.claude/skills
ln -s ~/dev/novelty-hunt ~/.claude/skills/novelty-hunt
```

Invoke with `/novelty-hunt` or ask for the skill by name.

For Codex:

```bash
mkdir -p ~/.agents/skills
ln -s ~/dev/novelty-hunt ~/.agents/skills/novelty-hunt
```

Invoke with `$novelty-hunt`. For delegated work, explicitly request subagents. Host tool availability and permissions apply.

For a project-scoped installation, use `.claude/skills/novelty-hunt` or `.agents/skills/novelty-hunt` inside your project. If the destination already exists, inspect it before replacing it. Pull updates with `git -C ~/dev/novelty-hunt pull --ff-only`; the symlink follows the checkout.

Installation references: [Claude Code skills](https://code.claude.com/docs/en/skills) and [Codex skills](https://learn.chatgpt.com/docs/build-skills).

## Scope and practical notes

Originality is relative to the stated reference set; this workflow does not establish that an idea has never been tried anywhere. Ratings compare candidates within one hunt. Single-agent personas provide different perspectives, not independent judges. The [research index](references/evidence.md) and [run lessons](references/lessons.md) document the method's foundations and limitations.

## Explore the repository

- [SKILL.md](SKILL.md): framing, search, comparison, and delivery workflow.
- [Generation moves](references/generation-moves.md): search techniques and prompt templates.
- [Scoring protocol](references/scoring-protocol.md): full-mode judgment procedure.
- [Research reports](references/research/): supporting literature notes.
- [Diagram guide](diagrams/README.md): sources and regeneration instructions.

Contributions should preserve functional distinctness and keep novelty separate from usefulness. Record concrete observations in the lessons ledger and qualify findings from individual studies or runs.

## Related skills

Use each skill on its own, or combine them when the task calls for it:

- [Solution Tournament](https://github.com/rwshiraishi/solution-tournament): compare implementation approaches.
- [App Blueprint](https://github.com/rwshiraishi/blueprint): prepare a product build package.
- [Foreman](https://github.com/rwshiraishi/foreman): coordinate implementation with a team of agents.

These are optional handoffs between skills, not an automatically connected pipeline.

## Maintainer and license

Created by [Ray Shiraishi, Ph.D.](https://www.linkedin.com/in/ray-w-shiraishi-ph-d-780276331), [Neon Peach, LLC](https://neonpeach.co). Feedback, examples, and contributions are welcome.

Released under the [MIT License](LICENSE).

Views expressed here are personal and do not represent the views of any employer or institution.
