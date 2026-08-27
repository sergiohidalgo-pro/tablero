---
name: tablero
description: "Trigger: tablero, estado del proyecto, dónde vamos, avance, project status, status board. Publica el estado de un proyecto como artefacto: métricas, backlog por fases, costos y Gantt simulable."
license: MIT
metadata:
  author: "sergiohidalgo-pro"
  version: "1.0"
---

## Activation Contract

Load when the user asks for the state of a project or sub-project: "tablero", "dónde vamos", "estado", "avance", "cómo vamos", "status board", or asks to update a tablero published earlier.

Do NOT load for a one-line status answer, a commit log summary, or a plain checklist.

## Hard Rules

- Every number comes from a primary source read in THIS session (git, pipeline runs, cloud CLI, tickets, transcripts). Never from memory or a summary. If a figure cannot be sourced, mark it as an estimate and say what would confirm it.
- Hours are session hours with a human present, cutting pauses > 20 min — never pipeline wall-clock. State the cutoff rule in the artifact.
- `Σ act` across `PHASES` must match total `SESSIONS` hours within 0,5 h. Reconcile the data before publishing; never adjust one side to hide the gap.
- Costs are incremental: only what is new. Name the price source and date, and list what is excluded because it was already paid.
- Never invent qualitative claims about rhythm or productivity. `EVAL_EXTRA` entries need a time and a result.
- Do not edit the CSS in `assets/template.html`: the palette, typography and motion ARE the format. Extend by adding sections, not by restyling.
- If you add movement, follow the performance rules in `references/design-system.md`: nothing animates off-screen, nothing that moves carries a `drop-shadow`, and a re-render is not a re-animation.
- Keep the footer `.brand` block: logo slot + credit line + MIT link.
- Default output is a LOCAL html file. Publish to claude.ai only when the user asks for it in this session — publishing is an outward action.
- One artifact per project. To update, republish the same file path (or pass its `url`) — never create a second URL.
- Before republishing over an existing URL, read the LIVE version and merge onto it. Never rebuild from the copy you downloaded when the session started: another session may have published in between, and its version may be fresher than yours. If the live version and your evidence disagree, re-check the primary source before overwriting — the tablero published last is not automatically the one that is right, and a wrong figure inherited silently is worse than a stale one.
- The artifact is written in Spanish (neutral, `tú`); code identifiers stay in English.
- Load the `artifact-design` skill before writing the file, as the Artifact tool requires.

## Decision Gates

| Invocation | Output |
|---|---|
| `/tablero` (default) | Write the self-contained html file, report its path, offer to publish in one line. Do NOT publish. |
| `/tablero publicar` | Write the file and publish it as an Artifact in the same turn. |
| `/tablero <url>` | Update that published tablero: read it, rebuild `DATA` from current state, republish to the SAME url. |

| Situation | Section to use |
|---|---|
| Infrastructure, services, data flow | `#arq` as SVG diagram with `data-s` states + evidence table |
| No topology (research, migration, audit) | Replace the diagram with a coverage or detection-layer table |
| Work is time-boxed in sessions | Keep `#ritmo` (session blocks) |
| Work is calendar-driven, several people | Replace `#ritmo` with `Riesgos vivos` (risk · impact · trigger · mitigation) |
| Nothing is blocked | Drop the `col bad` column, keep 3-column grid balanced |
| No money involved | Keep `#costos` with hours only; delete the money table |

## Execution Steps

1. Gather evidence first. Read the primary sources; list what you could NOT verify.
2. Copy `assets/template.html` to the working file (scratchpad, or where the user asks). For a local file, wrap it with `assets/local-wrapper.html` — the template is a fragment and only the Artifact tool supplies the html/head/body shell.
3. When updating an existing tablero: read the live version FIRST, diff it against your evidence, and keep every fact it has that you did not re-verify. Publish only additions and corrections you can source.
4. Fill the `// ---------- data ----------` block only: `PHASES`, `SESSIONS`, `COSTS`, `DONE_BARS`, `TODAY`, `PHASE_ORDER`, `COST_NOTE`, `EVAL_RECO`, `EVAL_EXTRA`. See `references/data-contract.md`.
5. Replace every `{{...}}` placeholder in the HTML. Search for `{{` and confirm zero matches before publishing.
6. Adjust the diagram per `references/sections.md`; the LED, packets and power-on view are injected by JS — only author `<g data-s>`, `<path class="edge">`, `<rect class="zone">`. Give every component a `data-title` and a `data-info` explaining what it is and why it sits in that state: that is what the reader gets when tapping the box.
7. Open the local file to verify it renders before handing it over. Fonts come from Google Fonts and need network; the fallback stack must keep it readable offline.
8. Only when publishing: use the Artifact tool with a `favicon` stable across redeploys, `title` = short project name, `description` = one line.
9. Report the file path (or URL), what is estimated and what is verified.

## Output Contract

Return: the file path (or the artifact URL when published), the three headline numbers (effort %, hours done, hours left with buffer), the current blocker, and an explicit list of figures that are estimates.

## References

- `references/data-contract.md` — shape of every data structure and how each metric is derived.
- `references/sections.md` — section catalogue, diagram rules, when to swap a section.
- `references/design-system.md` — tokens, components, motion and accessibility rules.
- `assets/template.html` — the format itself.
- `assets/local-wrapper.html` — html/head/body shell for local files.
- `assets/logo.svg` — brand slot for the footer.
