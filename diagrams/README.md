# Diagrams

The README figures ship as committed PNGs, not as ```mermaid blocks.

GitHub renders Mermaid in the viewer's browser, measuring label text in one font
and drawing it in another. Labels clip, and each browser clips differently.
Safari and Chrome both truncate flowcharts differently. Pre-rendering removes
the viewer's browser from the loop.

| Source | Figure |
|---|---|
| `phases.mmd` | the six phases: frame core, map axes, search, gate, execute, score, deliver |
| `lessons-lifecycle.mmd` | how lessons move from findings to candidate to promoted to demoted |
| `candidates.mmd` | candidate flow: raw generation, dedup, gating, execution tests, scoring, picks |

## Changing a diagram

Edit the `.mmd` file, then:

```bash
./regen.sh
```

That renders a light and a dark PNG for every `.mmd` in this directory and
rewrites `.stamp`. Running `mmdc` by hand skips the stamp and the sync check fails.
Needs Node 18+; the renderer is fetched by `npx` on first use.

## Rules learned the hard way

- **No multi-line edge labels.** A `<br/>` inside an edge label makes mermaid
  mis-measure the background box and clip the text. Node labels are fine
  multi-line; edge labels must be short and single-line.
- **Don't hardcode node fills.** Light fills glare in dark mode. Let the theme
  supply fills; mark semantics with stroke or dash instead. Theme variables live
  in `light-config.json` and `dark-config.json`.
- **Top-down for anything with more than five nodes.** A left-to-right chain is
  rendered at page width, so a long one becomes a strip of unreadable text at
  README size. Subgraphs do not help: mermaid ignores a subgraph's direction as
  soon as an edge crosses its boundary.
- **Charts need explicit colors.** The default `xychart` palette is near-white in
  the light theme and draws a grey background in the dark one. Both configs set
  `plotColorPalette` and a transparent `backgroundColor`.
- **The sync check compares hashes, never re-renders.** PNG bytes are not
  reproducible across machines.

## Verifying sync

`.stamp` hashes every source, both configs, and every PNG together. To check that
the committed PNGs match the committed sources:

```bash
[ "$(cat diagrams/.stamp)" = "$(bash diagrams/stamp.sh)" ] && echo in-sync || echo STALE
```
