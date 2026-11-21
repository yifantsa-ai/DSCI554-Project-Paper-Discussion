# Feedback on `tentative_paper_revision.md`

Checked on Nov 19 against `FEEDBACK.md`, the code (`src/`), the app's own data (`public/data/`), the live site and the main new references.

**Overall:** the structure now answers almost every feedback item. Purpose and users, contributions tied to challenges, the reachability challenge stated as a visualization problem, three-part Related Work, class breaks and color justification, the overplotting strategy, dashboard integration, and limitations with their effects are all there. But there are **three blocking problems** to fix before this goes into Overleaf, followed by a list of factual corrections.

---

## Blocking problem 1: whose words these are (course rule)

- The notes at the top of the file ("Notes before pasting into Overleaf", "Own-words reminder") read like output from an AI tool. ASSIGNMENT.md says the paper must be written by the students in their own words, and AI-generated paper content is not permitted. If this text goes into the paper as is, that is a risk to the whole paper grade, not just a point or two.
- Use the draft as a **checklist of what to say**, and write each paragraph yourselves. The good news is that the hard part (deciding what each section must contain, with which numbers) is done.
- **The GenAI sentence in 4.4 must be accurate.** It currently says "all … paper writing were conducted by the authors". Only keep that if it's true of the final text. Describe AI use for the code (the commits are co-authored by an AI assistant) and for any writing or planning help honestly. PAPER_WRITING_GUIDE.md asks for this disclosure.

## Blocking problem 2: length (the limit is 5 pages)

- The revised body text is about **4,500 words**. The whole pre-submission paper was about **1,700 words** (including references), and that already filled 5 pages with one small figure.
- On top of that you're adding **14 references** (27 in total) and **2–3 full-width figures**. Rough estimate:
  - If references count toward the 5 pages, the body has room for about **1,100–1,400 words**.
  - If they don't, about **1,800–2,000**.
  - Either way, the text has to be cut by **55–75%**.
- **Best test:** paste a plain version into Overleaf now, with placeholder boxes for the figures, and look at the page count before polishing anything.
- **Where to cut** (rough targets for the ~1,400-word case):

  | Section | Draft | Target | What to cut |
  | --- | --- | --- | --- |
  | Abstract | 200 | 160–180 | Drop the hex-level design detail; keep the two challenges, one contribution each, two numbers |
  | 1.1–1.3 + contributions + roadmap | 750 | ~350 | One example per user is too many: keep one user example. 1.2 and 1.3 each need only one example. |
  | Related Work | 440 | ~220 | Three short paragraphs of 2–3 sentences each. Keep about 6–8 new references, not 14. |
  | Data | 500 | ~150 + Table 1 | Move the route-tier numbers to 4.4 (they belong to optimization). Cut the neighborhood and basemap detail to a table row. |
  | 4.1 | 335 | ~150 + equations | The difficulty definition fits in two sentences; drop the repeated "modeled, not observed" caveat. |
  | 4.2 | 635 | ~300 | Keep the classes + one-line distribution, YlGnBu + lightness, casing/edge, default routes. Cut the alternatives to the 2–3 real ones. Drop hex codes. |
  | 4.3 | 330 | ~150 | Bins, palette, orange/teal/gray, one rejected alternative. Shorten the iteration list to 3 items. |
  | 4.4 | 290 | ~130 | Shared vs. separate controls; one sentence of load sizes; the GenAI sentence. |
  | 5.1 | 215 | ~120 | See the correction below. |
  | 5.2 | 380 | ~200 | Keep the 08:00 vs. 03:00 contrast, Tujunga, and 2699.03. Drop one of the two signal results. |
  | 6 | 400 | ~180 | 3 limitations, a one-sentence unfinished list, 3 future-work items. |

## Blocking problem 3: the tract 1902.01 example is wrong (my mistake)

- This error came from **my revision guide**, and I'm sorry: I hadn't checked the citywide fill value.
- The draft says, in **1.3** and **5.1**, that Tract 1902.01 "falls into the darkest exposure class (53.5 minutes)". That isn't true:
  - The **citywide fill** uses each tract's stop with the **earliest boarding** (walk + wait minimized over up to 8 nearby stops). For 1902.01 in summer at 08:00, the fill value is about **1.5 minutes** of sun, the **lightest (0–5) class**. Winter: about 0.7 minutes. (From `public/data/shade/summaries/summer.json.gz`, row 458, departure 28800.)
  - **Selecting the tract** shows the journey to its **nearest** stop, Yucca/Cahuenga (bus at 08:54), with **53.5 minutes** of sun, 52.7 of them waiting. I confirmed this on the live site.
- **How to use it correctly:** this is actually a *better* example. The fill says "almost no sun", because a nearby alternative stop has a bus soon. The nearest stop, which a rider would likely pick by default, means almost an hour in the sun. That shows exactly why the linked route + breakdown is needed: the fill alone hides it. It also shows that the fill and the selected journey describe different stops, which the paper must state plainly. Decide as a team how to explain this, and check the wording against the app's own panel text.
- Also fix the **1.1** user example ("52.7 of its 53.5 sunlit minutes … making that stop a shelter priority"). That one is true *for the nearest stop*, so say "at its nearest stop".

---

## Factual corrections (section by section)

| Where | What the draft says | What's actually true / what to do |
| --- | --- | --- |
| 1.2 | "underserved tracts represent only about 4–5% of the city during daytime hours" | During 06:00–20:00 the count ranges **44–78 tracts (3.9–7.0%)**; at 08:00 it is 52 (4.6%). Say "about 4–7% during the day" or give the 08:00 figure. |
| 3 Data | Mapping L.A. boundaries "distributed via the City of Los Angeles GeoHub" [23] | The build script fetches them from an ArcGIS feature service, `VMT_2022_by_LA_Times_Neighborhood` (`scripts/build-neighborhoods.mjs`; the commit says "via LADOT"). Cite that service and its URL, not GeoHub. |
| 3 Data | 57 proxies described generally | Correct. Tujunga (1011.22) **is** one of them (`residential_street_proxy`), so using it as the example is fine. |
| 4.2 | "most bus riders walk roughly 400–524 m (about 5–7 minutes) to a stop [19]" | El-Geneidy et al. report that the **85th-percentile** walk to bus for home-based trips is about **524 m**; 400 m is the conventional buffer they test, not a finding. State it that way. |
| 4.2 | The 30-minute threshold is justified by [19] | [19] supports *walking access is usually short*. It doesn't support 30 minutes of walk + wait as a cut-off. Present 30 minutes as a **deliberately conservative choice** (6× the median walk, and well beyond the 524 m norm), and point to the 0 s / 30 s and time-of-day comparisons as the sensitivity evidence. Or find a source for a 30-minute access standard. |
| 4.4 | "the initial Reachability overview payload is 420,896 compressed bytes" | That is the **pre-submission** figure, and it now contradicts the measured ~0.74 MB in the same paragraph. Remove it. |
| 4.4 | "Measured in Chrome on the deployed site, opening the Reachability tab downloads roughly 0.74 MB…" | Those are **my** measurements. Re-measure yourselves (DevTools → Network, cache disabled) and state the conditions before writing "measured". |
| 4.4 | "Automated build checks cap the default difficulty-route catalog under 1 MB" | It is the **export script** that refuses output over 1 MB (`scripts/export-difficulty-routes.mjs`), not the build. Minor; reword. |
| 5.1 | "the tract sits in the darkest 20+ minute fill bin (53.5 minutes total)" | Wrong; see Blocking problem 3. |
| 5.1 | "median modeled direct sun **drops** from 3.22 … to 1.00" | The two medians come from different valid sets (1,110 vs. 1,112 origins), and the pre-submission paper rightly called them unpaired. "Drops" implies a paired change; say "is 3.22 in summer and 1.00 in winter". |
| 5.2 | "(verify exact neighborhood names on live site)" | A placeholder left in the text. Check the map and name the neighborhoods, or drop the sentence. |
| 5.2 Validation | Straight-line buffers "would mark all 23 wait-driven daytime difficult tracts and 346 overnight tracts as adequately served because stops are nearby" | **Not true for daytime.** Of the 23 wait-driven tracts at 08:00, only **1** has a fastest walk ≤ 5 min and 4 have ≤ 10 min; the median is **23 min**. At 03:00 the claim works better: **100 of 346** are within a 5-minute walk and 248 within 10. Make the claim about 03:00, with these numbers, or weaken it. |
| 5.2 Validation | "checked internally against GTFS timetables and street topology" | Vague. What exists: the validators (`scripts/validate-reachability.mjs`, `validate-shade.mjs`), the unit tests, and an independent offline Dijkstra check of one arbitrary pair (1902.01 → Western/Sunset). Name what was actually run. |
| 6 | "Tujunga (requiring 57 audited street proxies)" | Reads as if Tujunga needed 57 proxies. **57 is citywide**; Tujunga is one of them. |
| 6 | "additional publicly available municipal feeds (such as LADOT DASH and Metro Rail)" | Metro Rail isn't municipal. Also check that each feed is public before naming it. |

## Claims only your team can confirm (otherwise remove them)

These came from my guide's suggestions, but the draft states them as things you did. Keep each one only if it actually happened:

- **4.2 "We evaluated and rejected several alternatives…":**
  - Supported by the history: the straight-line circles (the pre-submission §4.2), all routes being cluttered (also pre-submission), the red/blue two-scale design (commit `650d8b6`) and the purple edge (commit `6b472e6`).
  - **Not documented anywhere:** "rejected edge bundling and width-encoded flows" and "kept an origin-first choropleth over stop-first isochrones". If you didn't consider them, write them as *justification* ("bundling would hide street geometry") rather than as rejected alternatives.
- **4.3 "We rejected quantile or per-season breaks… a continuous ramp… route width":** the same rule applies. These are my hypothetical examples, not documented decisions.
- **4.3 "inspired by sequential YlOrRd schemes [25]":** the sun palette is custom (`src/shadeAnalysis.ts`). Only cite ColorBrewer for it if that's where it came from.
- **4.3 "iterations driven by visual critiques and usability checks":** no usability checks were run (your own §6 says so). Say "design critiques", or describe what actually drove each change (e.g., the professor's in-class feedback).
- **4.2/4.3 color-vision claims:** "preserving perceptual ordering … under color-vision deficiency" rests on my computed lightness values. Either run the Chrome DevTools vision-deficiency emulation and say so, or phrase it as "monotonic lightness, which suggests the order survives…".

## Rubric coverage after the fixes

| Feedback item | Status in the draft | Still needed |
| --- | --- | --- |
| Purpose / users (−1) | Met (1.1) | Trim to one example |
| Contributions tied to challenges (−2) | Met | — |
| Reachability challenge in intro, why hard (−1) | Met (1.2) | Fix the 4–5% figure |
| Related Work: vis papers on both challenges + topic research (−1) | Met (three paragraphs, gap statements) | Cut to 6–8 new references; fix [23] |
| Limitations with effects, unfinished work, future work with "how" (−1) | Met | Fix the "57 proxies" wording |
| Design process and alternatives (−1) | Partly | Keep only real alternatives (see above) |
| Dashboard integration, described **and shown** (−1) | Described | **Fig. 1 (whole-app overview) still has to be made** |
| Primary design: classes, colors, CVD (−2) | Met | Confirm the CVD claim |
| Primary evidence / validation (−1) | Weak; the "validation" is an assertion | Use the corrected 1902.01 fill-vs-journey contrast **as the evidence**, shown in Fig. 2; keep the planned study as future work |
| Reachability design (−3) | Mostly met | Justify 30 min properly (see the table above) |
| Reachability evidence / validation (−1) | Partly | Fix the buffer claim; add the before/after signal panel and the 08:00 vs. 03:00 figure |
| Fig. 1 readability (−0.5) | Text-only revision | Figures must be full-width and legible at print size |

## References

- Spot-checked:
  - **El-Geneidy et al. 2014**: real; *Transportation* 41(1), 193–210. Fix how the 524 m figure is described (see above).
  - **Moreira et al. (Urban Toolkit)**: real. Its case studies do include what-if shadow comparisons and shadow accumulation, so the claim holds.
  - **Fraser & Chester 2017**: real. Confirm the pages (216–225) when you export the BibTeX from Google Scholar.
- The draft's note says all references "have been verified". Treat that as unconfirmed, and pull every BibTeX entry from Google Scholar or the DOI yourselves.
- **[23]** needs the correct source (see the table above).
- With 27 references, the reference list alone will take roughly 1.5 pages. That is another reason to cut to the 6–8 strongest new ones.

## Use neighborhood names instead of tract numbers

The app now labels communities with LA Times *Mapping L.A.* neighborhood names, and the paper should follow it. Names are easier for readers to place than "Tract 2699.03", and they match what readers see in the figures. The three examples map as follows (from `public/data/shade/neighborhoods.json.gz`):

| Tract | Neighborhood | Tracts sharing that name | How the app labels it |
| --- | --- | --- | --- |
| 1902.01 | **Hollywood** | 26 | "Hollywood, near Yucca / Cahuenga · Tract 1902.01" |
| 2699.03 | **Palms** | 15 | "Palms, near …" (nearest stop: Venice / Motor) |
| 1011.22 | **Tujunga** | 8 | "Tujunga, near Summitrose / Tinker · Tract 1011.22" |

- **A name alone is ambiguous.** Each of these names covers 8–26 tracts, and 206 tracts share a name citywide. "Hollywood" could mean any of 26 tracts with very different results. So:
  - **In running text:** use the name, plus the nearest stop where it helps, the way the app does: "Hollywood (near Yucca/Cahuenga)", "Palms (near Venice/Motor)", "Tujunga".
  - **Give the tract number once**, at first mention or in the figure caption, e.g., "Hollywood (Tract 1902.01)". That way a reader or grader can find the exact case in the app. After that, the name alone is fine.
  - **Don't generalize from one tract to the neighborhood.** For example, write "a Hollywood tract", not "Hollywood residents", because the result describes one representative origin, not the whole neighborhood.
- **Places to change in the draft:**
  - 1.1: "Tujunga (Tract 1011.22)" is already in this form; keep it.
  - 1.1, 1.3, 5.1: "Hollywood Tract 1902.01" → "Hollywood (Tract 1902.01)" at first mention, then "the Hollywood tract".
  - 5.2: "Tract 2699.03" → introduce it as the **Palms** tract (near Venice/Motor). This is new; the draft never names it.
  - 5.2: the 08:00 cluster sentence. Name the neighborhoods there too, once checked on the map.
- **Cite the names' source.** Use the real one from the data file: *LA Times Mapping L.A. neighborhood boundaries, published by LADOT*, ArcGIS feature service `VMT_2022_by_LA_Times_Neighborhood_WFL1`, retrieved 2026-11-16. This replaces the GeoHub reference [23]. Mention in Data (one clause) that names are assigned by the neighborhood that contains each tract's origin.

## Smaller writing notes

- Remove hex codes from the paper text (`#26323a`, `#a52d46`, `#ef9b34`…). Readers see the colors in the figures, and the codes take up space.
- Abstract: "At 08:00 … 52 of 1,120 … and reachable 30-minute walk pairs drop from 113,395 to 112,201" joins two unrelated results with "and". The pair count doesn't depend on 08:00.
- Figures 1–3 are referenced but don't exist yet. Make them before final cutting; they decide how much text fits.
- The `Claude outputs/` folder is inside your repo folder and untracked. Don't commit it (add it to `.gitignore`, or move it out), so planning files don't end up in the graded repository.
