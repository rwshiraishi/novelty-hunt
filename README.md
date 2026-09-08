# Novelty Hunt

**Explore original ideas and bring back three useful directions: conventional, frontier, and wildcard.**

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE) · Built by [Neon Peach, LLC](https://neonpeach.co)

Novelty Hunt helps an AI agent move beyond familiar answers. It maps the space of possibilities, searches through different creative moves, and preserves distinct ideas long enough to compare originality and usefulness separately.

Use it for product concepts, positioning, naming, technical designs, or creative text when exploring a wider range of options matters.

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

The agent first generates the obvious answers and defines what makes an answer useful for your request. It maps structural differences, explores several generation moves, and merges equivalent ideas into a candidate archive. Coherence and hard constraints narrow the field; inexpensive experiments inform the comparison where feasible. Novelty and effectiveness are then considered separately.

| Mode | Shape | Useful for |
|---|---|---|
| Lightweight | One agent, one or two search rounds, a target archive of 6–12 candidates | Focused exploration |
| Full | Separate generator and judge agents, with cross-model scoring where configured | Deeper searches, exhausted conventional options, or an explicit full hunt |

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
