# Sun / LA — figure shot list (Figs. 1–3 in the revision patch)

The previews in `figure_references/` are low-resolution captures from the Claude browser pane (about 740 px wide). Use them **only** to see the intended state. Recapture every figure yourselves at full resolution (method below); the grader deducted points for an unreadable Fig. 1 last time.

## How to capture at print quality (Chrome)
1. Open https://dsci-554.github.io/project-20263-team-4/ in Chrome, full screen.
2. Press F12 to open DevTools, then Ctrl+Shift+M (device toolbar). Choose "Responsive", set **1600 × 1000** and **DPR 2** (⋮ menu → "Add device pixel ratio" if the field is hidden).
3. Set up the state (below). Then Ctrl+Shift+P → type "Capture screenshot" → Enter. You get a 3200 × 2000 PNG.
   - For the side panel only: right-click it in the Elements tab → "Capture node screenshot".
4. Crop in any editor. Keep the legend in the shot, and save as PNG (not JPG).
5. In LaTeX, use `width=\textwidth`. For a/b/c panels, use `\usepackage{subcaption}` (it works with LNCS).
6. Check the compiled PDF at 100% zoom: can you read the legend numbers and the side-panel text? If not, crop tighter.

## Fig. 1 — whole-app overview (dashboard integration)
- (a) **Sun exposure** tab, default state: Summer · 06-21, Leave at 08:00, "Total time in direct sun", city view. Header (tabs + search), control row, legend and side panel all visible.
- (b) **Reachability** tab: "Include signal waits", Leave at 08:00, city overview. Side panel reads "52 communities need over 30 minutes…".
- Caption idea: shared header search and community selection; view-specific time controls.

## Fig. 2 — primary challenge: Hollywood (Tract 1902.01)
- (a) Sun tab, Summer 08:00, **city overview zoomed to Hollywood**, *before* selecting the tract. Shows that the tract's fill is light (about 1.5 min). If hovering the tract shows a tooltip with its value, capture with the tooltip.
- (b) Search "1902.01" → select. Zoom so A (origin), the bus stop (Yucca / Cahuenga) and the short orange/teal walk are all visible. In the side panel, expand **Details** so the walk/wait breakdown shows 1.3 min walk (0.8 min sun) and 52.7 min wait; headline **53.5 min**, bus at 08:54.
- Caption must say: the fill uses the stop with the earliest boarding; the selected journey goes to the nearest stop.

## Fig. 3 — reachability challenge
- (a) Reachability, Include signal waits, **08:00**, city overview. The `!` markers, default routes (3 closest stops per difficult tract) and the 8-class legend are visible. → reference: `figure_references/fig3a_reachability_overview_0800.jpg`
- (b) **Tujunga (Tract 1011.22)**: search "1011.22", or the side panel's "Show an example". Zoom in to where the route crosses the light-blue 20–30-minute fill, so the dark casing + red edge is clearly visible. Side panel: **39.0 min**, fastest walk 29:54, "Stops within 60 minutes (55)". → reference: `figure_references/fig3b_tujunga_0800.jpg`
- (c) **Palms (Tract 2699.03)** at **03:00**. Set the slider with the keyboard: click the slider handle, then press ← until it reads 03:00. Take two captures with the same zoom:
  - **No signal waits**: earliest boarding at Venice / Motor, board **03:03**.
  - **Include signal waits**: boarding moves to Venice / Helms, board **03:47** (47.0 min, flagged).
  - Expand the walk-and-wait breakdown in both, and put them side by side.
- Optional (d), if space allows: the 03:00 city overview next to (a), to show 52 vs. **375** difficult tracts.

## Checklist before inserting
- Same zoom level and window size within each a/b/c set.
- No mouse cursor or hover artifacts (except the intended tooltip in 2a).
- Legend visible in at least one panel of each figure.
- Every number quoted in a caption matches what is on screen.
