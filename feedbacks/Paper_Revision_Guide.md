# Sun / LA — paper revision guide (pre-submission → final)

**Baseline:** `paper.pdf` in the repo root (pre-submission, last changed in commit `3294ce2`, Nov 9). Not `docs/paper/` or `docs/paper-revised/`.
**Checked against:** `FEEDBACK.md` (graded commit `c346714`), the Nov 16–17 code changes, and the live site on Nov 19.

These are directions, not text to paste. The paper must be written in your own words (ASSIGNMENT.md).

**Correction (Nov 19):** the earlier claim that tract 1902.01's *fill* is in the darkest class was wrong; see §1.3 and §5.1.

**New in this version:** concrete examples (marked **Example:**) in every section, and a rebuilt future-work part in §6. The numbers come from the app's own data (`public/data/reachability/index.json.gz`) and match the live site.

Tags: **[FB]** = fixes a feedback deduction · **[IMPL]** = the paper no longer matches the app · **[CLASS]** = the professor's in-class points (justify encoding, color, scope/unit, route design).

---

## 0. Before you start: page budget and figure plan

- The limit is still **5 pages**, and you need to add 2–3 readable figures, a longer Related Work section and a contributions paragraph. Plan the cuts first:
  - Shorten the abstract's numerical detail.
  - Merge 1.1–1.3 into fewer paragraphs (the grader liked that 1.x isn't over-split).
  - Cut repeated caveats ("modeled, not observed" appears 4–5 times; say it once in 4.1 and once in the Conclusion).
  - Remove the 29-vs-30 high-no-vehicle numbers (see §5.2).
- **Figure plan** (each figure full `\textwidth`, so legends and labels are readable at print size **[FB]**):
  - **Fig. 1: whole-app overview.** Header with the *Sun exposure / Reachability* tabs and the shared search, the control row and the side panel. Show the dashboard integration that was missing **[FB]**. Two half-width panels (a) Sun and (b) Reachability may be enough.
  - **Fig. 2: primary challenge.** A larger crop of one tract's route with orange/teal/gray segments, the legend, and the opened "walk and wait breakdown". Tract 1902.01 still works.
  - **Fig. 3: reachability challenge.** (a) The city overview with `!` markers and the default 3-closest routes. (b) A zoomed difficult community (the site's "Show an example" opens **Tujunga, Tract 1011.22**: 39.0 min at 08:00, fastest walk 29:54, 55 stops within 60 min) showing the casing and red edge. Optionally (c) the same tract with and without signal waits. The feedback asked for that before/after view **[FB]**.
  - If space is tight, combine Figs. 2 and 3 into one figure with panels (a)–(d), and crop the basemap out of each panel.
- Every caption should say what to look at (e.g., "light fill = short walk, but the `!` = long wait"), not just name the view.

---

## Title, authors, abstract, keywords

- **Title:** "Signal-Aware Reachability" undersells the new focus. Consider naming the **underserved / difficult-to-board** framing the professor asked for. Keep it short.
- **Authors and emails:** fine; keep them.
- **Abstract** (now ~172 words; keep within 150–250):
  - **[IMPL]** Add the *difficulty* definition: a tract is difficult when walk **plus** scheduled wait to the first boarding is **more than 30 minutes**.
  - **[IMPL]** Say what the reachability map now shows: a walking-time choropleth with routes shown **only for underserved tracts** by default.
  - **[FB]** Name *who* it is for in one clause (see 1.1).
  - Name both challenges **as visualization challenges** (what is hard to *show*), and state one contribution for each.
  - Keep at most one or two headline numbers. 113,395 → 112,201 is still valid; a difficulty count is more on-message (e.g., 52 of 1,120 origins at 08:00 with signal waits, 4.6%).
- **Keywords:** consider adding *flow/route maps*, *choropleth*, or *underserved areas*. "Time-dependent networks" is more of a modeling term than a visualization one.

---

## 1. Introduction

The feedback here is worth 4 points (purpose −1, contributions −2, reachability challenge −1).

### 1.1 Motivation → also purpose and users **[FB]**
- Add the **users and the decision each makes**, which the grader said is all it takes:
  - *Transit planners:* where walk-plus-wait access exceeds 30 minutes, so stops, service or crossings need attention.
  - *Shade advocates / city staff:* which walks or waits are sun-dominated, to prioritize shelters and trees.
  - *Riders and community groups:* how their own neighborhood compares.
  - **Example** (pick one per user, from cases already in the app):
    - A planner opens Reachability at 08:00 and sees Tujunga (Tract 1011.22) flagged: 39 minutes to board, and its fastest walk alone is 29:54. That suggests a stop or service gap, not just a long wait.
    - A shade advocate selects the Hollywood tract 1902.01 and sees that, at its **nearest** stop (Yucca/Cahuenga), 52.7 of 53.5 sun minutes happen *waiting*. That makes that stop a shelter candidate rather than a street-tree candidate. (The tract's citywide fill is light because a different stop boards sooner.)
    - A rider searches their neighborhood in the header search and compares their tract's color with nearby tracts.
- Keep the LA motivation (24% shelters [6], UCLA shade report [11]), but move it so it supports the purpose, not the other way round.
- **[CLASS] Scope:** add one or two sentences on why the **City of Los Angeles** (consistent data: city signal inventory, lidar coverage, shelter inventory) and why origins are city-only while stops may lie outside (people walk across city borders).

### 1.2 Reachability challenge: rewrite as a visualization challenge **[FB]**
- The grader's main point: 1.2 currently describes a *modeling* task ("we compute routes…"). Restate it as **what is hard to draw**, and give the reasons it is hard.
- Pick one or two named challenges from PAPER_WRITING_GUIDE.md that match what you built:
  - **"Keeping a fine-unit choropleth and a route layer legible together."** This matches the in-class discussion best: routes share the fill's color scale, so a route vanishes over an area of the same class.
  - **"Comparing two reachability sets"**: with vs. without signal waits.
- **Why it is hard** (include numbers):
  - About 113k origin–stop walking pairs within 30 minutes; routes converge on the same streets and stops.
  - 1,120 tract fills under the routes.
  - Two different measures (walking time vs. walk + wait) on one map.
  - Underserved tracts are few (about 4–5%) and easily lost in a citywide view.
  - **Example of the legibility problem:** Tujunga's fastest walk (29:54) falls in the 20–30-minute class, and so does the tract's own fill. The route is drawn in exactly the same color as the area it crosses. Without the casing it would disappear. This is a strong, concrete way to state the challenge.
  - **Example of the two-measures problem:** at 08:00 with signal waits, 23 of the 52 difficult tracts have a fastest walk under 30 minutes. They look light on the walking-time fill, yet fail on walk + wait because of the scheduled wait.
- Optionally rename the subsection so the heading itself says "visualization challenge".

### 1.3 Primary challenge **[FB minor]**
- The grader accepted it. Strengthen it by adding the **scale** (1,120 origins × 4 seasons × 25 departure times from 06:00–18:00) and **why a standard choropleth fails**: one fill per tract cannot say *where* along the trip the sun occurs, or whether walking or waiting dominates.
- **Example (corrected Nov 19):** in summer at 08:00, tract 1902.01's citywide fill is only about **1.5 min** of sun (lightest class), because the fill uses the stop with the earliest boarding. Selecting the tract shows the journey to its **nearest** stop, Yucca/Cahuenga: a 1.3-minute walk, then a 52.7-minute wait, **53.5 min** of sun in total. The fill alone hides this. That makes the challenge concrete: the paper must say that the fill and the selected journey can refer to different stops.

### Add a contributions paragraph (new; −2 pts) **[FB]**
- Put it at the end of the Introduction, before the roadmap. Write it as prose, because bullets cost points elsewhere, e.g., "Our contributions are (1)…, (2)…, (3)…".
- **Tie each contribution to its challenge**, as the feedback requires. A suggested split:
  1. *Primary:* the walk/wait sun-exposure encoding. Orange/teal/gray route segments, plus a walk-vs-wait breakdown, over a fixed-bin choropleth, with unknown kept separate from zero.
  2. *Reachability:* an **underserved-focused travel-time map**. One 8-class walking-time scale shared by fills and routes; a dark casing plus red edge so routes stay legible over fills; routes shown by default only from difficult tracts (3 closest stops), with 60-minute routes on click.
  3. *Integration:* one coordinated dashboard with shared search and selection across both views (this addresses the dashboard deduction).
- Keep the roadmap paragraph (full credit), and update it if the section names change.

---

## 2. Related Work (−1 pt; currently one short paragraph) **[FB]**

- Expand to **three short themed paragraphs**. In each, say what prior work does **and what your design adds**; the grader said that comparison was missing.
  1. **Visualization of accessibility / isochrones / route and flow maps** (*reachability challenge*). Required: visualization work, not just transport modeling. Your current [2] and [3] are modeling papers, so keep them only as support.
  2. **Visualization and modeling of urban sun and shade** (*primary challenge*). [4] and [9] fit; add at least one more visualization paper.
  3. **Topic research:** bus-stop heat and shade exposure, walking distance to transit, pedestrian signal delay. [6], [8] and [11] belong here and need to be *discussed*, not passed over in a clause.
- **Candidate references to check** (look each one up on Google Scholar, read the abstract, and confirm it says what you cite it for before using it):
  - Flow and route maps: Phan et al., "Flow Map Layout" (IEEE InfoVis 2005). Jenny et al., "Design principles for origin–destination flow maps" (Cartography and Geographic Information Science, 2018). Holten & van Wijk, "Force-directed edge bundling for graph visualization" (Computer Graphics Forum, 2009). These support your overplotting discussion: deduplicated shared edges and casing, compared with bundling or width-encoded flows.
  - Transit isochrones and accessibility: O'Sullivan, Morrison & Shearer, "Using desktop GIS for the investigation of accessibility by public transport: an isochrone approach" (IJGIS, 2000). Geurs & van Wee, "Accessibility evaluation of land-use and transport strategies" (J. Transport Geography, 2004).
  - Walking to transit (supports the 30-minute and walking-mode justification): El-Geneidy et al., "New evidence on walking distances to transit stops" (Transportation, 2014).
  - Heat or shade and transit riders: Fraser & Chester, "Transit system design and vulnerability of riders to heat" (J. Transport & Health, 2017). Lanza & Durand on bus-stop shelters, tree shade and ridership (IJERPH, 2021).
  - Color and classes (for §4): Harrower & Brewer, "ColorBrewer.org" (The Cartographic Journal, 2003), which is the source of your YlGnBu scale. Brewer & Pickle (2002) on choropleth classification.
  - Uncertainty / unknown encoding (optional, for the gray ≠ zero choice): MacEachren et al., "Visual semiotics & uncertainty visualization" (IEEE TVCG, 2012).
  - Coordinated views (optional, for the dashboard): Roberts, "State of the art: coordinated & multiple views in exploratory visualization" (CMV 2007).
- End each paragraph by **stating the gap** (the assignment requires this for the reachability literature). Example direction: existing isochrone and flow maps show *how far* places are reachable, but not *which neighborhoods fail on walk + wait*, nor routes legible over a same-scale choropleth.
- **Example gap statements** (one per paragraph; put them in your own words):
  - *Accessibility / isochrones:* isochrones are usually drawn **from a destination outward** (e.g., "everything within 10 minutes of this stop"). Your map is **origin-first**: each tract's own walk, with walk + wait flagged separately.
  - *Flow maps:* flow-map work aggregates many OD pairs into width-encoded flows. Your routes keep individual street paths, because the task is "which stops can this tract actually reach on foot".
  - *Shade:* shadow-accrual maps [9] summarize shade over a whole *area and day*. Your design follows one *trip* (a moving walk, then a stationary wait) at a chosen departure time.
- Use LNCS citations (`\cite{}` with splncs04). Any website you cite as a source needs a `@misc` entry with an access date.

---

## 3. Data

- **[IMPL] Remove or re-scope the ACS no-vehicle role in Reachability.** The Reachability view no longer shows a vehicle threshold, the high-share outlines or a vehicle badge. Difficulty is now walk + wait only. ACS still drives the Sun view's "Who spends more time in the sun?" scatterplot, so say that instead.
  - Update **Table 1, ACS row**: "No-vehicle household shares for tract outlines" is outdated.
- **[IMPL] Add the new inputs:**
  - **LA Times *Mapping L.A.* neighborhoods (via LADOT)** for community names. Mention that 206 tracts share a name, so the tract number stays visible.
  - **OpenFreeMap (Positron) basemap**. A citation or footnote is enough.
- **[IMPL] Add the route-data facts that support your overplotting and performance discussion:**
  - The default difficult-route catalog: **3 closest stops per difficult origin**, 1,506/1,520 paths (0 s / 30 s scenarios), **396 KB** compressed.
  - The 60-minute display routes: 82,055/82,902 routes added for origins difficult at some departure (504/509 origins). All per-tract route files together total **55.7 MB**. They are kept out of git (fetched from a GitHub Release at build time) and deployed as one small file per tract. The browser downloads **only the clicked tract's file** (e.g., ~9 KB for Tract 1011.22).
- **[CLASS] Unit justification:** add a sentence on why the **census tract** is used. ACS is published by tract, 1,120 units is fine enough to show structure inside the city, and tracts are a familiar planning unit. Name the cost: one representative origin per tract hides variation within it (MAUP). Cite Openshaw if you mention MAUP by name.
  - **Example of the cost:** hillside tracts such as Tujunga's are large, and their single origin can sit far from where most homes are. That's why 57 origins were replaced with audited in-tract street proxies (`origin-audit.json`). Mention this as both a fix and a sign of the unit's limits.
  - **Example of the benefit:** tracts are the unit of the ACS data used in the Sun view's demographic scatterplot, so the two views line up on the same units.
- Keep the 4 m lidar, the frozen GTFS date and the 57 street proxies. They earned full credit.

---

## 4. Design and Implementation

The grader asked for alternatives, class breaks, colorblind safety, overplotting and legibility, all of which the professor also stressed in class.

### 4.1 Computing reachability and sun exposure
- Equations (1)–(3) are still valid.
- **[IMPL] Add the difficulty definition formally**, right after Eq. (2):
  - Difficult if B_o(t, δ) > 1,800 s.
  - If the shortest walk alone is over 30 minutes, the tract is difficult with the walk as a **lower bound** ("at least"), even without a full boarding result.
  - "No scheduled bus" and "data incomplete" stay separate states. They are not counted as difficult unless the walk proves it.
- **[IMPL] Separate the two route limits:**
  - Boarding, difficulty, counts and reverse catchments still use the **30-minute** walk search.
  - The **60-minute** routes are **display-only**, for difficult tracts.
  - Readers (and graders) will otherwise assume the 60-minute routes change the numbers.

### 4.2 Reachability map design: rewrite **[FB −3] [IMPL] [CLASS]**
Go through each element the rubric lists, one or two sentences each.
- **Mode and threshold:**
  - Why **walking** (access to the first boarding; stops are local destinations).
  - Why **30 minutes** walk + wait as "difficult". Find a citation (walk-to-transit or wait-tolerance literature, or a planning standard) rather than asserting it.
  - Why **60 minutes** for display (it shows the realistic alternatives a difficult tract has).
- **Choropleth color map and classes:**
  - **8 classes:** 0–5, 5–10, 10–15, 15–20, 20–30, 30–45, 45–60, 60+ minutes (upper bounds inclusive).
  - **Why these breaks:** fine steps where most tracts lie; a break exactly at **30** so the classes line up with the difficulty threshold; coarser classes for the long tail.
  - **Example (from the app's data, 30 s signal scenario):** the median fastest walk is **4.9 min**. Tract counts per class are 579 / 366 / 80 / 33 / 33 / 20 / 7 / 2, so **84% of tracts are under 10 minutes** (hence two 5-minute classes at the low end). The three classes above 30 minutes hold exactly the **29 tracts whose walk alone already makes them difficult**. A small table or a one-line histogram of these counts would justify the breaks better than any argument.
  - **Why YlGnBu** (ColorBrewer, sequential, classes 2–9; the near-white class was dropped for the light basemap): darker = longer walk, single hue progression, designed for print and colorblind use.
  - **Evidence of ordering:** I computed CIE L\* for your hex values; lightness falls monotonically 95 → 13 across the eight classes, so the order survives grayscale and color-vision deficiency. Say this in your words, and if you can, verify with a CVD simulator (Chrome DevTools → Rendering → Emulate vision deficiencies).
- **Fill = walk only; `!` = walk + wait. Explain this split explicitly.** A light tract can still carry a `!` because its wait is long. This is your "walking ≠ boarding" contribution made visible, so put it in a figure caption too.
- **Route layer and its encoding (the in-class focus):**
  - Routes use the **same 8-class scale** as the fills, so color always means "walking minutes". This replaced an earlier design with two scales on the same bins (blue fills, red routes), which read as two different measures; mention it as an iteration.
  - **The legibility problem this creates:** a route over an area of its own class disappears.
  - **The solution:** solid route lines drawn over a **thin dark casing** (`#26323a`), plus a **red outer edge** (`#a52d46`, the same red as `!`) on routes from difficult tracts, so the line has a border on both sides. Fills are translucent and lines solid.
  - **Iteration worth reporting:** the difficult edge was purple first, but was too close in hue (~43°) to the 45–60-minute class. It was changed to red, more than 60° from every class, and a unit test enforces this. That is exactly the kind of design-process evidence the grader asked for.
  - Selected routes are **wider**, but keep their time color.
- **Overplotting strategy:**
  - **Default:** routes only from difficult tracts, to their 3 closest stops. Ordinary tracts get no default route.
  - **On click:** a difficult tract shows every stop within 60 minutes; an ordinary tract shows stops within 30 minutes plus the nearest stop.
  - **Shared candidate edges are deduplicated**, colored by the shortest contributing walk.
  - Layer toggles.
  - Say why you did *not* use bundling or width-encoded flows. For example: individual walking paths must stay on real streets, and the task is "which stops can this tract reach", not aggregate volume. Back this with the flow-map references from §2.
- **Alternatives considered: give 2–3 concrete ones with why each was rejected** (the grader asked for alternatives; only claim ones the team actually considered):
  - *Straight-line buffers or circles around stops:* already rejected in your pre-submission §4.2, because they ignore street connectivity (freeways, hills, dead ends). Reuse that point.
  - *Showing every route citywide:* about 113k origin–stop pairs within 30 minutes. Unreadable, so routes appear only for difficult tracts.
  - *A separate color scale for routes* (the earlier blue fills + red routes): read as two measures, so it was replaced by the shared scale (a real iteration, commit `650d8b6`).
  - *Width-encoded or bundled flows:* would hide the actual street path; see the overplotting bullet above.
  - *Isochrone bands around a selected stop:* this partly exists as the reverse catchment, but a stop-first view cannot answer "is *my* tract underserved", so the origin-first choropleth stays the main view.
- **Basemap:** full-strength OpenFreeMap with translucent fills, because street names are needed to read routes. Mention the trade-off that labels sit under the fills.
- **Reverse catchment:** clicking a stop shows which origins can reach it (the asymmetry). One sentence.

### 4.3 Primary challenge (sun exposure) design **[FB −2]**
- **Class breaks:** state the 5 fixed bins (0–5, 5–10, 10–15, 15–20, 20+ min) and why fixed bins are used: seasons and times stay comparable.
- **Color justification:**
  - The yellow-to-red palette is custom. Its lightness also falls monotonically (L\* 95 → 43), so it is ordered in grayscale. Say whether it was based on ColorBrewer YlOrRd and, ideally, check it with a CVD simulator.
  - **Orange (sun) vs. teal (shade):** a warm vs. cool contrast, and they also differ in lightness (L\* ≈ 71 vs. 48), so they stay distinguishable for red-green CVD.
  - **Gray = unknown, not zero**; its rationale (MacEachren et al. if you cite it).
- **Legibility at the data's scale:** fixed bins across 1,120 tracts, routes drawn only for the selection, the 10/15/20-minute catchment, and layer toggles.
- **Design process [FB −1]:** add a short paragraph on **alternatives considered and iterations**. Use only what actually happened; check your commit history and Figma files. Real examples from the history:
  - two scales for areas and routes → one shared scale;
  - purple → red difficult edge;
  - washed-out raster basemap → full-strength vector basemap;
  - bare tract numbers → neighborhood names;
  - overloaded side panel → one headline answer with folded sections;
  - search moved into the header;
  - map titles and selection chips removed from the map.
  - If you had Figma sketches before the pre-submission, cite or show one.
- **Example alternatives for the sun view** (again, only if the team actually considered them):
  - *Quantile or per-season bins* instead of fixed bins: these would make summer and winter maps look equally dark and hide the seasonal difference (median 3.22 vs. 1.00 min at 08:00).
  - *A continuous color ramp:* harder to read exact minutes from, and harder to match with the legend.
  - *Encoding sun minutes as route width* instead of orange/teal segments: shows the amount but not *where* along the walk the sun is.

### 4.4 Implementation, dashboard integration and data optimization **[FB −1] [IMPL]**
- **Dashboard integration (missing):**
  - Describe the one React + Bootstrap app with two tabs.
  - **What is shared:** the header search (neighborhood, tract or stop) and the **selected community**, which carries over when you switch views.
  - **What is separate:** each view has its own departure-time control (Sun 06:00–18:00 with a season selector; Reachability 00:00–23:30 with the signal-wait toggle). Say why: the sun model only covers daylight.
  - The control row and the folded side panel (one headline number, details collapsed).
  - Refer to Fig. 1.
- **The professor's data-optimization request** (from the Slack announcement):
  - Precomputed static data.
  - On-demand loading of shade tiles, timetables and routes.
  - The size caps: the difficulty catalog export refuses output over 1 MB, the neighborhood lookup is 4.5 KB, and the initial Reachability overview is 420,896 compressed bytes.
  - **The route data is the strongest example of this, so describe it.** The full route set is 55.7 MB, but it is split into **one file per tract** and fetched on demand. Opening Reachability downloads about **0.74 MB** of gzipped files on a first visit: `index.json.gz` 162 KB, `stops.json.gz` 195 KB, `difficulty-routes.json.gz` 387 KB. Clicking a tract adds one route file (~9 KB for Tract 1011.22). Raw and route data stay out of git; the build fetches them from a checksum-pinned release. These are measured in Chrome on the live site; report them as such, or re-measure yourself.
- **GenAI usage:** PAPER_WRITING_GUIDE.md asks for it, and the recent commits are co-authored by an AI coding assistant. Describe briefly and accurately how the team used it (coding, debugging, design checks).

---

## 5. Usage Scenarios and Results

### 5.1 Primary challenge **[FB −1]**
- The tract 1902.01 / Yucca–Cahuenga case still matches the site for the **selected nearest-stop journey** (1.3 min walk, 0.8 min sun; 52.7 min wait in sun; 53.5 min total). The tract's **citywide fill** is about 1.5 min, because it uses the stop with the earliest boarding. Show both in **Fig. 2**; the contrast is the evidence that the linked view is needed.
- Name the community the way the app does (neighborhood + tract) so readers can follow it.
- **Validation (missing):** add a short paragraph that either:
  - compares against a **conventional display** (a fill-only tract choropleth cannot show that the wait dominates; your route + breakdown does), and/or
  - describes a **planned** evaluation as future work, clearly marked *not conducted*. `docs/paper-revised/EVALUATION_PROTOCOL.md` outlines one (4–6 people, fill-only vs. linked display, 4 tasks). Reuse its idea, not its text, and never present it as done.
- The 3.22 vs. 1.00 summer/winter median sentence is fine, but optional if you need space.
- **Example comparisons you can show with what you already have:**
  - *Same tract, two seasons:* use "Compare another season or time" on tract 1902.01 (summer vs. winter at 08:00) and report how the walk and wait sun minutes change. Read the winter numbers off the app; I haven't checked them.
  - *Fill-only vs. linked display:* put a fill-only crop of the tract next to the route + breakdown, and say what each lets the reader conclude.
  - *Ground check:* one timestamped photo at Yucca/Cahuenga near 08:00 in a similar season, compared with the modeled sun/shade state. One stop is enough as an illustration, if you label it as such.

### 5.2 Reachability challenge **[FB −1] [IMPL]**
- **Lead with the new citywide result:**
  - Number of difficult tracts at a stated departure and scenario (e.g., **52 of 1,120 at 08:00 with signal waits**; 52 at 12:00, of which 23 have computed boarding totals and 29 already exceed 30 minutes on the walk alone).
  - Where they cluster. At 08:00 the overview shows many along the hillside and northern Valley edges, plus a few in the harbor area; confirm on the map and name the neighborhoods.
  - Re-check any number you use against the live site or `scripts/reachability` output before writing it.
- **A difficult-community walkthrough** with Fig. 3(b): Tujunga, Tract 1011.22, 39.0 min at 08:00, fastest walk 29:54, only 2 stops within 30 minutes but 55 within 60. This shows why the 60-minute display matters.
- **Example time-of-day comparison** (a strong "comparison" result, from the app's data, 30 s scenario):
  - Difficult tracts: **52 at 08:00**, 50 at 17:00, **131 at 23:00**, and **375 at 03:00**. During the day (06:00–20:00) the count stays between 44 and 78.
  - The 29 tracts whose walk alone exceeds 30 minutes are difficult at every hour. The rest of the night-time increase comes from waits (346 of the 375 at 03:00 have a fastest walk under 30 minutes).
  - This separates a *geographic* access problem (29 tracts, always) from a *service-hours* problem (hundreds of tracts at night). A two-panel figure (08:00 vs. 03:00) would show it well.
- **The signal comparison:** keep 113,395 → 112,201 pairs and the tract 2699.03 case (03:03 → 03:47). That tract is now also flagged difficult with signals (47 min).
  - Add the **before/after figure panel** the grader requested.
- **[IMPL] Remove** "among high-no-vehicle tracts, 29 vs. 30 …". The app no longer uses the vehicle threshold.
- **Legibility evidence:** point to the figure where a route crosses a fill of the same class and stays visible because of the casing. That *shows* the in-class challenge solved.
- **Validation:** compare with what a straight-line buffer or nearest-stop-distance map would show (e.g., tracts that look "close" by distance but are flagged by walk + wait), and/or with existing accessibility tools. Say how you would validate the walking times (e.g., spot-check against a routing service) and the signal delay (field timing). Mark anything not done as future work.
  - **Example spot-check:** take the three cases already in the paper (2699.03 → Venice/Motor, about 2 min; 1902.01 → Yucca/Cahuenga, 1.3 min; Tujunga → Summitrose/Tinker, 29:54). Compare each with a public walking-directions estimate and report the differences in a small table. Three to five pairs are enough to show the model is in the right range.

---

## 6. Conclusion: limitations and future work **[FB −1]**

The grader wants three things here: what each limitation *does to the map*, what was *planned but not finished*, and *how* each future-work item would be done. The 5-page limit means you can't use everything below: pick about 4 limitations and 3–4 future-work items, and give each one or two sentences.

- Open with one or two sentences restating the contributions, tied to the two challenges.

### Limitations: each with its effect on the map and a concrete example
- **One representative origin per tract.**
  - *Effect:* the fill and the `!` describe one point, not the whole tract; a large tract can look better or worse than parts of it.
  - *Example:* hillside tracts like Tujunga (Tract 1011.22) are large, and 57 origins had to be moved to in-tract street proxies because the geometric center was badly connected.
- **Assumed 30 s per signalized crossing.**
  - *Effect:* `!` flags close to 30 minutes are sensitive to the assumption.
  - *Example:* tract 2699.03 at 03:00 boards at 03:03 without the delay and at 03:47 with it; citywide, walk links fall from 113,395 to 112,201.
- **One frozen weekday schedule (Oct 27, 2026), Metro buses only.**
  - *Effect:* no weekends, no real-time delays, no other operators, so waits may be over- or under-stated.
  - *Example:* tracts served mainly by other operators' buses may be flagged even though a bus does come.
- **Clear sky, fixed and opaque vegetation, no shelter roofs.**
  - *Effect:* sun minutes are a geometric upper bound, not heat dose.
  - *Example:* the Yucca/Cahuenga wait counts all 52.7 minutes as sun because the shelter's status is unknown and roof shade isn't modeled.
- **Labels sit under the fills** (a deck.gl/MapLibre interleaving limitation noted in the code history).
  - *Effect:* street and place names are harder to read under dark fills, e.g., in the 30–60-minute classes.

### Planned but not finished (state these honestly)
- The **user study**: the 4-task comparison of a fill-only map vs. the linked display (outlined in `docs/paper-revised/EVALUATION_PROTOCOL.md`, not conducted).
- **Color-vision-deficiency checks** with real users (only computed lightness checks so far).
- **Field validation** of shade states and crossing delays.
- A **load-time benchmark** (you have file sizes but no timed measurements).
- Ask the team whether any other planned feature was dropped, and list it.

### Future work: concrete items, each with *how* and *what it would change on the map*
1. **Finer units when zoomed in (the team's idea).**
   - *How:* keep tracts at city zoom, and switch to **census block groups** (or blocks) when the user zooms into a neighborhood. Route from many more origins, ideally only populated blocks so that empty hillsides and parks drop out. Precompute a second set of per-unit route files, fetched only at that zoom, like the per-tract files today.
   - *Data note:* the ACS publishes many tables down to block groups, but census *blocks* only have decennial counts (population, housing). Check that each table you need exists at the level you choose.
   - *What changes:* within-tract variation becomes visible. For example, the parts of Tujunga near Foothill Blvd vs. the upper hillside.
   - *Why it is a visualization challenge too:* the two levels can tell different stories (MAUP). The color scale must stay identical across zoom levels, the switch must be visible to the user (e.g., keep the tract outline over the block fills), and the number of routes grows several-fold, so the overplotting strategy has to scale with it.
2. **A distribution per tract instead of one origin** (a lighter alternative to item 1). Route from several population-weighted points per tract. Show the range (e.g., best–worst walk) in the side panel, and mark tracts whose range crosses the 30-minute line with a distinct outline.
3. **Calibrated crossing delays.**
   - Replace the flat 30 s with an expected wait per crossing, e.g., about half the red phase if signal timing is available.
   - Or time a sample of crossings in the field, e.g., along the Tujunga and Venice/Motor routes used in the paper.
   - In the app, a 0/15/30/45 s control would turn the current two-scenario toggle into a sensitivity view.
4. **More complete service.**
   - Add the other operators' feeds that serve LA City (e.g., LADOT DASH, Metro Rail), plus weekend calendars.
   - Use archived real-time data (GTFS-Realtime) to show the *spread* of actual waits rather than the scheduled wait, e.g., as a range bar in the walk/wait breakdown.
   - Check which feeds are publicly available before naming them.
5. **A better sun model.**
   - Verify the 1,617 shelter points and add roof shade at sheltered stops.
   - Use leaf-off vegetation for winter, and climatological cloud cover.
   - Optionally move from "sun minutes" to a heat-stress measure such as mean radiant temperature (the SOLWEIG line of work in [4]).
6. **Linking the two challenges in one view.** A combined "priority" view (or filter) for tracts that are *both* difficult to board *and* have long sunny waits. This directly answers "where would a shelter or added service help most". It introduces its own design problem: a bivariate color scheme that stays readable.
7. **A planning what-if tool.** Let a planner place a hypothetical stop or shelter and see which `!` flags disappear. The Sun view already routes arbitrary origin–stop pairs in the browser from the static graph tiles, so recomputing locally is plausible.
8. **Evaluation.**
   - Run the 4-task study with 4–6 participants who didn't build the system.
   - Get a short expert review with a transit planner or shade advocate.
   - Report accuracy and time per task, not just opinions.

---

## References
- Every entry in LNCS style (splncs04). Use consistent `@misc` entries for web sources, with access dates.
- After expanding Related Work, check that every reference is cited in the text, and every citation resolves.
- Add sources for the new data and tools: Mapping L.A. neighborhoods, OpenFreeMap, ColorBrewer (Harrower & Brewer).

---

## Numbers you can rely on (verified Nov 19)

| Fact | Value | Where it comes from |
| --- | --- | --- |
| Difficulty rule | walk + scheduled wait to first boarding > 1,800 s; walk alone > 30 min = lower bound | `docs/REACHABILITY_DIFFICULTY.md`, `src/reachabilityModel.ts` |
| Walking-time classes | 0–5, 5–10, 10–15, 15–20, 20–30, 30–45, 45–60, 60+ min | `WALK_CUTS` |
| Walking-time colors | ColorBrewer YlGnBu classes 2–9, L\* 95 → 13 | `WALK_COLORS` (L\* computed from hex) |
| Route casing / difficult edge | `#26323a` / `#a52d46` (same red as `!`) | `ROUTE_CASING`, `DIFFICULT_ROUTE` |
| Default routes | 3 closest stops per difficult origin; 1,506/1,520 paths; 396 KB | `DEFAULT_DIFFICULT_ROUTES`, difficulty doc |
| Display limit on click | 60 min (difficult tract) / 30 min + nearest stop (ordinary tract) | difficulty doc |
| Difficult origins | 52 / 1,120 (4.6%) at 08:00 with signal waits (live site); 504/509 difficult at some departure (0 s / 30 s) | live site; difficulty doc |
| Fastest-walk distribution (30 s) | median 4.9 min; per class 579 / 366 / 80 / 33 / 33 / 20 / 7 / 2; 84% under 10 min | computed from `index.json.gz` |
| Difficult tracts by time (30 s) | 52 (08:00), 52 (12:00), 50 (17:00), 131 (23:00), 375 (03:00); 44–78 during 06:00–20:00; 29 are difficult at every hour (walk alone > 30 min) | computed from `index.json.gz`; 08:00 and 12:00 match the live site |
| Wait-driven difficulty (30 s) | 23 of 52 at 08:00 and 346 of 375 at 03:00 have a fastest walk under 30 min | computed from `index.json.gz` |
| Example | Tujunga, Tract 1011.22: 39.0 min at 08:00, fastest walk 29:54 (same 20–30 class as its own fill), 2 stops ≤30 min, 55 ≤60 min | live site, "Show an example" |
| Walk-link totals | 113,395 (0 s) vs. 112,201 (30 s) pairs within 30 min | unchanged |
| Sun bins / colors | 0–5 … 20+ min; `#fff1c6`→`#b73b29`, L\* 95 → 43; sun `#ef9b34`, shade `#247d79`, unknown gray | `src/shadeAnalysis.ts` |
| Route data size | 55.7 MB in total, split per tract; opening Reachability loads ~0.74 MB, one tract ~9 KB | difficulty doc; measured on the live site Nov 19 |

---

## Outside the paper (also in FEEDBACK.md)
- **Video (−1):** add the **group name** to the title slide.
- **Presentation (−3):** embed video clips (silent) in the Google Slides deck, and cut it to about 8 slides for 5 minutes, keeping extras as backup after the closing slide.
