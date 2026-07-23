# aidenfox.dev — "Site as a PCB" theme design (2026-07-23)

Approved concept: the page IS one continuous PCB; content blocks are mounted
components; the 3D viewers are display modules wired to the board.

Revert point: git tag `pre-pcb-theme`.

## D1 — Copper trace spine
Inline SVG drawn by ~40 lines of JS measuring section designator positions.
Runs down the left margin hero→footer, vias at section junctions, 45° branch
stubs into each section designator pad. 2-3 animated signal pulses (copper +
cyan, same language as hero canvas). Hidden <900px. Static under
prefers-reduced-motion.

## D2 — Display-module chrome
Video + PCB viewers get: corner mounting-hole dots, FPC/ribbon stub on the
trace-side edge, silk designators DS1 (RL demo), DS2 (power board),
DS3 (logic board). Captions folded into silk labels.

## D3 — IC treatment
BetaRush/Motif cards + toolbox get subtle CSS pin stubs on edges.

## D4 — OG image
assets/og.png 1200x630: dark soldermask, power-board render right,
silkscreen-style name/title/domain left. og:image + twitter:card meta.

## D5 — Favicon
Crafted SVG: dark chip package, copper edge pads, fox-orange fox-head
center. SVG + 32px PNG + 180px apple-touch-icon + theme-color meta.

## D6 — Hardware anchor
Viewer grid id="hardware"; nav gains DS·Hardware link.

## Constraints
No new libraries. Total new JS ≈60 lines. Mobile: spine hidden, layout
unchanged. SEO/load untouched. Existing content intact.

Future (out of scope): DS4 leg-assembly viewer (leg_v6 STLs exist), hero 3D.
