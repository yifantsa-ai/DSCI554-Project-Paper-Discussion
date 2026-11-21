# 5-page edition vs. FEEDBACK.md: item-by-item check (Nov 21)

**What was checked:**
- `paper_5page_edition.md` (v5), as compiled in `latex_check/main_5page.tex` / `main_5page_check.pdf`: exactly 5 LNCS pages with the team's `llncs.cls` and `splncs04.bst`.
- Against the paper deductions in `FEEDBACK.md`, the graded pre-submission paper at commit c3467149, read together with the "Grade details" and "Suggestions" sections.

**Bottom line:**
- 10 of the 13 paper items are fixed.
- 3 are mostly fixed, with small remaining risk: References, Implementation and Dashboard integration.
- Figure format is still at risk because Fig. 1 is too small.

---

## 1. Fixes made during this check

Earlier trims in the 5-page edition had reopened four deductions. All four are now restored, and the paper still fits in 5 pages.

| Reopened item | What had happened | Now |
|---|---|---|
| Abstract length (150–250 words; the old paper had full credit) | Trimmed to 138 words | About 154 words. The input list is back, and the abstract adds "at 03:00, 375 are". |
| Contributions (−2) | Shortened to "a linked display for each challenge" | One contribution per challenge, plus the dashboard (the Overleaf wording) |
| Purpose (−1) | Riders dropped from the Introduction | Planners, shade advocates **and riders**, each with what they decide |
| Limitations & future work (−1) | Plan for missing signals and the planner's review removed | "Matching LADOT signals to OpenStreetMap crossings", plus "a transit planner's review" |

Also restored:
- "No stock map shows it" in the primary challenge statement (a Suggestion item).
- The list of preprocessing steps: build the pedestrian graph, read timetables, run Dijkstra, compute shade, export JSON (an Implementation item).

How the space was found:
- The Fig. 1/2 float was moved into §4, next to its first reference, so text can fill page 3.
- Two small extras were dropped: the "4–7% of the city by day" clause and the 43° hue detail.

**There are no spare lines left.** Adding a single word to the abstract makes the paper 6 pages.

---

## 2. Paper items

### Organization & Presentation

| FEEDBACK item | Lost | Status | Where in v5 | What remains |
|---|---:|---|---|---|
| Figure format: "Fig. 1 is a very small screenshot … legend, labels and panel text cannot be read at print size, and the colors of the route segments cannot be told apart" | 0.5 | **Still at risk** | Fig. 2 now shows the legend, the "no data" gray and the orange/teal/gray route. Fig. 3 adds the Reachability map. | Fig. 1 (the dashboard) is printed at about 43% of the text width, and its legend and panel text cannot be read at 100% zoom. This is the same complaint as before. Crop it to the header, controls and side panel, or give it more of the row than Fig. 2. |

All the other Organization checks had full credit and still pass:
- section order, Data as its own section, and subsections at most one level deep;
- the abstract is 150 words or more (about 154 now);
- the Keywords line;
- the roadmap sentence (Sections 2–6);
- prose only, no bullet lists;
- LNCS / splncs04 formatting;
- Table 1's caption above the table and no vertical rules;
- each figure placed near its first reference.

### Purpose & Contribution

| FEEDBACK item | Lost | Status | Where in v5 | What remains |
|---|---:|---|---|---|
| Purpose: never says who the system is for | 1 | **Fixed** | §1 ¶1: "Planners need neighborhoods where walking plus waiting exceeds 30 minutes, shade advocates need to know whether sun falls on the walk or at the stop to choose between trees and shelters, and riders compare their own neighborhood." The abstract names all three groups too. | — |
| Contributions: none stated, and none tied to a challenge | 2 | **Fixed** | §1 *Contributions*: for the sun challenge, a choropleth linked to sun/shade walking segments and a walk-versus-wait breakdown; for the reachability challenge, an underserved-focused travel-time map with fills and routes on one scale and casings for legibility; and one dashboard for both. | — |
| Reachability challenge: doesn't say why the map is hard to build | 1 | **Fixed** | §1 *Reachability visualization challenge*: the times exist in no dataset and must be computed by routing and timetable matching; about 113,000 walks converge on shared streets above 1,120 tracts; walking time and walk-plus-wait are different measures; a route colored on the fills' scale vanishes on an area of its own class. | — |
| (Suggestion) Primary challenge statement thin on difficulty | 0 | **Fixed** | §1: sun is computed "from lidar heights and solar position at each six-second walking step and waiting minute; no stock map shows it." | — |
| References: Related Work is five sentences; works named but not compared; need 2+ works per theme | 1 | **Mostly fixed** | §2 has three themed paragraphs, each with a "we instead …" contrast. **Travel-time maps:** Zeng et al., including how they compute time (smart-card trips, waits, transfers) and how they draw it (an isochrone map paired with an isotime flow map), and Andrienko et al. **Sun/shade:** Miranda et al. and Moreira et al. **Access/heat:** El-Geneidy et al., Marisamynathan and Vedagiri, and Fraser and Chester. All six data sources are now cited in Table 1. | Andrienko et al. gets only a generic sentence, because nobody has read the full text, so Zeng is the only strong example of a map with routes. Adding Jenny et al. on flow-map clutter would close this. It costs about 3 lines; the BibTeX entry is in the v4 Markdown as ref 22. |
| Limitations and future work: a single list, with no effect on the displays and no plan | 1 | **Fixed** | §6 says what each limitation changes on the map: one origin per tract can misstate hillside tracts; the flat 30 s delay makes flags near 30 minutes sensitive; one weekday schedule ignores other operators; clear skies and unmodeled roofs make sun minutes an upper bound. It says what was verified (validators, an independent Dijkstra check) and the plan: a four-task study with 4–6 outside participants (fill-only vs. linked view), a transit planner's review, matching LADOT signals to OSM crossings, and checking the shelter list with street imagery to model roof shade. | — |

### Methodology & Implementation

| FEEDBACK item | Lost | Status | Where in v5 | What remains |
|---|---:|---|---|---|
| Design process: no alternatives, iterations or reasoning | 1 | **Fixed** | §4 gives the rejected alternatives: straight-line buffers (they ignore street barriers), drawing all routes, an earlier blue-fill/red-route version that read as two measures, and a purple edge that resembled the 45–60 class. It also gives the reasoning: two tabs keep walking and sun minutes off one color scale, a map-first layout with one headline number, and routes start from where riders are. | Figma iterations are not mentioned. Optional. |
| Implementation: how React/Bootstrap support the display; preprocessing | 1 | **Mostly fixed** | §4 *Layout and implementation*: one React app; a parent component holds the selected neighborhood, tab and search and passes them to both maps and the D3 scatter plot, whose dots select neighborhoods; deck.gl over a MapLibre basemap; the Python steps (pedestrian graph, timetables, Dijkstra, shade, JSON); per-tract route files loaded on click. | Bootstrap is described accurately as styling menus and buttons. The grader asked how Bootstrap's *layout* helps, and the paper doesn't claim that because the code doesn't use it that way. Small risk. |
| Dashboard integration: both views in one coordinated app, described and shown | 1 | **Mostly fixed** | The §4 paragraph above explains the shared state, the two tabs and the scatter-to-map link. Fig. 1 shows the whole app. | Fig. 1 is too small to read; see Figure format. |
| Primary design: bins, breaks, colorblind safety, legibility of legend / unknown / route colors | 2 | **Fixed** | §4 *Sun exposure design* gives five fixed bins justified by the data (about 69% in 0–5 min at summer 08:00, 6% over 20 at noon; fixed breaks keep seasons comparable) and a yellow-to-red palette that darkens steadily, so the order survives grayscale. Orange/teal routes sit on a white halo that separates them from the fills; gray means unknown; routes appear only for the selected neighborhood, which avoids overplotting. Fig. 2 shows the legend, the "no data" gray and the route colors. | — |
| Primary evidence: validation against existing views or with users | 1 | **Fixed** | §5.1: Hollywood's light fill (about 1.5 min) vs. its nearest-stop trip (a 52.7-min sunny wait at Yucca/Cahuenga), showing what a fill-only map hides. §6: a user study against a fill-only map and a transit planner's review. | — |
| Reachability design: clinic-map elements | 4 | **Fixed** | All elements are covered: routes to the 3 closest stops; tracts as the smallest units with reliable ACS estimates, named with Mapping L.A.; walking as the mode, on walkable OSM ways with freeways excluded; the 30-min threshold at six times the median walk; one origin per tract (57 moved to a nearby street); eight classes with breaks that follow the skewed data and a break at the threshold; YlGnBu colors; a red "!" for walk plus wait; routes on the same scale with a dark casing and a red edge; clicks, a stop's catchment and layer toggles; Fig. 3. | — |
| Reachability evidence: no figure, no validation | 1 | **Fixed** | Fig. 3 shows (a) the 08:00 overview, (b) Tujunga's route over a fill of its own class, and (c) Palms with and without signal delays. §5.2 compares with a 400 m buffer (247 of the 346 overnight tracts are within a 10-minute walk) and gives the counts at 08:00 vs. 03:00. §6 notes validators, the independent Dijkstra check, and the planned comparison with isochrone tools. | — |

---

## 3. Non-paper items (tracked in `WHAT_TO_DO_NEXT.md`)

| FEEDBACK item | Lost | Status |
|---|---:|---|
| Video: title slide has no group name | 1 | Open. The title slide must show "Group 4" (items 12–15). |
| Presentation: no video clips | 3 | Open. Embed the three muted YouTube clips in slides 3, 4 and 6 (items 16–19). |
| Presentation length suggestion (14 slides) | 0 | Addressed in `Sun_LA_English_Presentation_Mod_v2.pptx`, which has 8 slides. The team is adding backup slides after them. |

---

## 4. Cheapest remaining gains

1. **Fig. 1 legibility.** This is the only paper deduction that is clearly still open (0.5), and it also strengthens Dashboard integration. Crop it tighter or give it more width, then recompile and confirm the paper is still 5 pages.
2. **Jenny et al.** (about 3 lines) gives a second solid reference for travel-time maps with routes. Pay for it by shortening the captions.
3. **Keep the fixes when rewriting in your own words.** Keep the abstract at 150 words or more, the per-challenge Contributions sentence, riders, "no stock map shows it", and the future-work plan. Each of these answers a FEEDBACK item.
