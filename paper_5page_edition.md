# Sun / LA — 5-Page Edition (`paper_5page_edition.md`)

> **What this is.** The Overleaf `main.pdf` (Nov 21) used as the skeleton, with the fixes from `feedbacks/overleaf_pdf_review_Nov21.md` applied and the most valuable content of the long-form draft (`archived paper/paper_revision_nov_20_feedback.md`) ported in. **Verified to compile to exactly 5 LNCS pages** (body ≈ 1,550 words) with the team's `llncs.cls`/`splncs04.bst`, 3 figures, Table 1 and 16 references; see `latex_check/`. Every change from the PDF is listed in *Changes from the Overleaf PDF* at the end.

> **Before pasting into Overleaf:** rewrite in your own words where needed, and confirm the GenAI sentence in §4 matches how you actually used AI.

## Title

Sun / LA: Visualizing Sun Exposure and Underserved Bus-Boarding Reachability in Los Angeles

## Abstract

Sun / LA is a geovisualization dashboard that shows transit planners, shade advocates, and Los Angeles riders why a nearby bus stop can still be hard to board or exposed to direct sun. It combines pedestrian routing, Metro schedules, assumed signal delays, and lidar-derived shade for 1,120 census-tract origins, and addresses two visualization challenges. First, one sun-exposure fill per tract cannot show whether sun falls on the walk or the wait, or separate missing data from shade; we link fixed-bin tract fills to orange/teal/gray route segments and a walk-and-wait breakdown. Second, street-level routes must stay legible over a fine-unit choropleth while walking time is kept apart from boarding difficulty, walk plus scheduled wait above 30 minutes; our reachability map shares one eight-class walking-time scale between fills and routes, adds dark casings, and draws default routes only for underserved tracts. At 08:00, 52 of 1,120 origins (4.6%) are difficult to board; at 03:00, 375 are.

**Keywords:** Transit accessibility · Choropleth · Route maps · Sun exposure

## 1. Introduction

A bus-stop pin cannot show whether a rider can walk there, board soon, or stay out of the sun. Planners need neighborhoods where walking plus waiting exceeds 30 minutes, shade advocates need to know whether sun falls on the walk or at the stop to choose between trees and shelters, and riders compare their own neighborhood. Metro reported shelters at 24% of its stops in 2022 [6], and shade in Los Angeles is uneven [13].

*Reachability visualization challenge.* Walking and boarding times exist in no dataset; we compute them by routing each origin and matching arrivals to timetables. It is hard to show: about 113,000 walks converge on shared streets above 1,120 tract polygons, walking time and walk-plus-wait are different measures, and a route colored on the fills' scale vanishes on an area of its own class.

*Primary visualization challenge: sun exposure.* Sun before boarding is also computed, from lidar heights and solar position at each six-second walking step and waiting minute; no stock map shows it. One fill per tract cannot say whether the walk or the wait is sunny, and missing data must not read as shade.

*Contributions.* For the sun challenge, we contribute a choropleth linked to sun/shade walking segments and a walk-versus-wait breakdown; for the reachability challenge, an underserved-focused travel-time map whose fills and routes share one scale, with casings for legibility; and one dashboard for both. Sections 2–6 cover related work, data, design, results, and limitations.

## 2. Related Work

*Travel-time maps with routes.* Zeng et al. [16] derive transit journey times, including waits and transfers, from smart-card trips, and avoid geographic flow clutter by pairing an isochrone map with an isotime flow map that straightens routes along a time axis. Andrienko et al. [1] summarize how well public transport connects places. These designs start from one source or aggregate routes; we start from every tract and keep real street paths on the fills' scale.

*Sun and shade.* Shadow accrual maps [10] make city-scale shadow accumulation over long periods fast enough to explore, and the Urban Toolkit [11] composes shadow and other urban layers through a grammar. Both summarize shade over areas and periods; we sample one trip, a moving walk and a fixed wait at the chosen departure, and keep unresolved intervals gray.

*Walking access and heat.* Real walks to stops differ from the 400 m buffer [2], signals delay pedestrians [9], and waits drive riders' heat vulnerability [3]; Sun / LA shows these factors together.

## 3. Data

Table 1 lists the inputs. The unit is the census tract (1,120), the smallest with reliable American Community Survey estimates, labeled with its *Mapping L.A.* neighborhood name because readers recognize "Tujunga," not "Tract 1011.22."

**Table 1. Core inputs and their roles.**

| Source | Data and visualization role |
|---|---|
| OpenStreetMap [12] | Directed pedestrian network for walking paths and times |
| Metro GTFS [5] | 10,968 stops; departures frozen to Oct. 27, 2026 |
| LADOT signals [8] | Mapped signal locations for assumed crossing delays |
| ACS 2020–2024 [14] | Tract demographics for the Sun Exposure scatter plot |
| NASA WERK lidar [15] | Terrain and surface heights (4 m cells) for shade |
| Mapping L.A. [7] | LA Times neighborhood names for tract labels |

## 4. Design and Implementation

*Computing reachability and sun exposure.* Walking time is the fastest Dijkstra path at 4.8 km/h, plus 0 or an assumed 30 s per mapped signal crossing. For each of 48 half-hour departures, time to board is the minimum, over stops within a 30-minute walk, of the walk plus the wait for the first scheduled departure. An origin is *difficult to board* if this exceeds 30 minutes or its shortest walk alone does. Modeled sun exposure sums the sunlit walking and waiting intervals whose shade is known.

*Reachability map design.* Riders reach a stop on foot, so we route on walkable OpenStreetMap streets and footways, excluding freeways and links closed to pedestrians. Each tract starts from one interior point (moved to a nearby street in 57 tracts), a reproducible origin. Walks to a bus are usually short [2], so 30 minutes, six times the median walk, is a conservative threshold. Fills encode walking time in eight classes, with breaks at 5, 10, 15, 20, 30, 45, and 60 minutes. The breaks follow the skewed data: 84% of tracts walk under 10 minutes, one break sits at the threshold, and the classes beyond it hold the 29 tracts whose walk alone is too long. ColorBrewer YlGnBu colors [4] darken monotonically, so the order should survive grayscale and color-vision deficiency. A red "!" marks walk plus wait above 30 minutes.

Each route is colored by its own walking time on the same scale; a dark casing, plus a red edge for difficult tracts, keeps it visible on a fill of its own class (Fig. 3b). Riders start from home, so by default only difficult tracts show routes, to their three closest stops; a click shows a tract's stops within 60 minutes or a stop's catchment, and toggles hide each layer. We rejected straight-line buffers, which ignore street barriers, and drawing all routes; an earlier blue-fill, red-route version read as two measures, and a purple edge resembled the 45–60 class, so the edge became red.

*Sun exposure design.* Fills use five fixed bins (0–5 to 20+ minutes). At summer 08:00 about 69% of tracts fall in 0–5 and at noon 6% exceed 20, so five-minute steps separate the common values, the open bin holds the long-wait tail, and fixed breaks keep seasons comparable. The yellow-to-red palette also darkens monotonically. The selected walk is orange (sun) or teal (shade) on a white halo that separates it from the fills; gray marks unknown, and routes appear only for the selection (Fig. 2).

*Layout and implementation.* Both views are map-first with a side panel giving one headline number, and two tabs keep walking and sun minutes off one color scale. In one React application (Fig. 1), a parent component holds the selected neighborhood, tab, and search and passes them to both maps and a D3 scatter plot, whose dots select neighborhoods on the map. deck.gl draws the layers over a MapLibre basemap; Bootstrap styles menus and buttons. Offline Python scripts build the pedestrian graph, read timetables, run Dijkstra, compute shade, and export JSON; routes (55.7 MB in all) are split into one file per tract, about 9 KB each, loaded on click. An AI coding assistant helped write and debug the React/TypeScript code and automated tests.

## 5. Usage Scenarios and Results

### 5.1 Primary Challenge: Walking Versus Waiting Exposure

In summer at 08:00, Hollywood (Tract 1902.01) has a fill in the lightest class, about 1.5 minutes, because the overview uses the stop with the earliest boarding. Selecting it shows the trip to its *nearest* stop, Yucca/Cahuenga: a 1.3-minute walk, then a 52.7-minute wait in sun (Fig. 2). This contrast is our evidence that a fill-only map hides wait exposure, and it points to a shelter, not trees.

### 5.2 Reachability Challenge: Walking Versus Boarding

At 08:00 with signal delays, 52 of 1,120 origins (4.6%) are difficult, 29 on the walk alone and 23 because of the wait, mostly in the hills, the north-west Valley, and near the harbor (Fig. 3a). Tujunga (Tract 1011.22) needs 39.0 minutes; its 29:54 walk has the color of its own tract, and the casing keeps it readable (Fig. 3b). The count stays at 44–78 by day but reaches 375 at 03:00, 346 of them from waiting. The signal delay makes Palms (Tract 2699.03) miss a 03:03 bus, moving boarding from 3.0 to 47.0 minutes (Fig. 3c). A 400 m buffer would call many of them served: 247 of the 346 are within a 10-minute walk.

## 6. Conclusion

Sun / LA separates proximity from access before boarding. Four limits shape what the maps show: one origin per tract can misstate large hillside tracts; the flat 30-second delay on mapped signals makes flags near 30 minutes sensitive; one weekday Metro schedule ignores other operators; and clear skies and unmodeled shelter roofs make sun minutes an upper bound. Outputs passed automated validators and an independent Dijkstra check. Next, a four-task study (4–6 outside participants, fill-only versus linked view) and a transit planner's review will test the display; matching LADOT signals to OpenStreetMap crossings and checking the 1,617-point shelter list with street imagery will add missing signals and roof shade.

## Figure Captions

- **Fig. 1.** The dashboard: tabs, shared search, view controls, map, legend, and side panel.
- **Fig. 2.** Hollywood, summer 08:00. Fills: sun minutes, yellow to red; gray = no data. Route: orange = sun, teal = shade, gray = unknown.
- **Fig. 3.** Reachability: fills and routes share one walking-time scale, light yellow (0–5 min) to dark blue (60+); red "!" = walk plus wait over 30 min. (a) 08:00. (b) Tujunga. (c) Palms at 03:00, without and with signal delays.

## References (16, in `splncs04` order)

1. Andrienko, N., Andrienko, G., Patterson, F., Stange, H.: Visual analysis of place connectedness by public transport. IEEE Trans. Intell. Transp. Syst. 21(8), 3196–3208 (2020)
2. El-Geneidy, A., Grimsrud, M., Wasfi, R., Tétreault, P., Surprenant-Legault, J.: New evidence on walking distances to transit stops: identifying redundancies and gaps using variable service areas. Transportation 41(1), 193–210 (2014)
3. Fraser, A.M., Chester, M.V.: Transit system design and vulnerability of riders to heat. Journal of Transport & Health 4, 216–225 (2017)
4. Harrower, M., Brewer, C.A.: ColorBrewer.org: An online tool for selecting colour schemes for maps. The Cartographic Journal 40(1), 27–37 (2003)
5. Los Angeles County Metropolitan Transportation Authority: Metro Bus GTFS *(new citation; short entry)*
6. Los Angeles County Metropolitan Transportation Authority: 2022 customer experience plan. Tech. rep., LA Metro (2022), p. 27
7. Los Angeles Department of Transportation: Mapping L.A. Neighborhood Boundaries *(new citation; short entry)*
8. Los Angeles Department of Transportation: Signal locations *(new citation; short entry)*
9. Marisamynathan, S., Vedagiri, P.: A new approach to estimate pedestrian delay at signalized intersections. Transport 33(1), 249–259 (2018)
10. Miranda, F., Doraiswamy, H., Lage, M., Wilson, L., Hsieh, M., Silva, C.T.: Shadow accrual maps: Efficient accumulation of city-scale shadows over time. IEEE Trans. Vis. Comput. Graph. 25(3), 1559–1574 (2019)
11. Moreira, G., Hosseini, M., Nipu, M.N.A., Lage, M., Ferreira, N., Miranda, F.: The Urban Toolkit: A grammar-based framework for urban visual analytics. IEEE Trans. Vis. Comput. Graph. 30(1), 1402–1412 (2024)
12. OpenStreetMap contributors: Southern California extract *(new citation; short entry)*
13. Reginald, M.: Progress, priorities, and obstacles to providing adequate shade and lighting at bus stops in Los Angeles. Tech. rep., UCLA Institute of Transportation Studies (2024)
14. U.S. Census Bureau: American Community Survey 2020–2024 5-Year Estimates *(new citation; short entry)*
15. WIFIRE Commons: NASA WERK Lidar—Los Angeles (1m, 2023) *(new citation; short entry)*
16. Zeng, W., Fu, C.W., Müller Arisona, S., Erath, A., Qu, H.: Visualizing mobility of public transportation system. IEEE Trans. Vis. Comput. Graph. 20(12), 1833–1842 (2014)

*Short entries:* the six data-source `@misc` entries in `latex_check/mybibliography_5page.bib` drop `howpublished`/URL and the access note to save about 7 lines; the URLs remain as clickable links in Table 1. If space opens up later, restore the URLs from the team's `mybibliography.bib`.

---

## Changes from the Overleaf PDF

Measured with the team's class files: the Overleaf PDF ends 1.5 lines from the bottom of page 5; this edition ends at about the same place (page 5, y = 658 pt of 666). **There are no spare lines**: a single extra word in the abstract pushes the paper to 6 pages.

**Kept from the Overleaf PDF on purpose** (an earlier draft of this edition had cut them, which would have reopened FEEDBACK items): the abstract's input list (abstract length), the per-challenge Contributions sentence, riders as a user group, "no stock map shows it", and the list of preprocessing steps.

**Added (from the review and the long-form draft)**

| Where | Change | Why |
|---|---|---|
| Table 1 | `\cite` the six data sources (OSM, Metro GTFS, LADOT signals, ACS, NASA WERK lidar, Mapping L.A.) in the Source column | Data sources were uncited in the PDF (regression from the pre-submission paper; ASSIGNMENT requires them) |
| §2 | Related Work rewritten as three themed paragraphs (≈100 → ≈160 words): Zeng's computation (smart-card trips, waits, transfers) and encoding (isochrone + isotime flow map), and one "we instead…" contrast per theme | Nov 20 FEEDBACK: "what they do and what your project adds", "how they compute and encode travel time" |
| §4 | "Riders start from home, so by default only difficult tracts show routes" | Origin-first task framing |
| §4 | GenAI sentence | RUBRIC §3; **team must confirm wording** |
| §4 | "routes (55.7 MB in all) are split into one file per tract, about 9 KB each, loaded on click" | PDF wording could be read as 55.7 MB per click |
| §5.1 | Names the nearest stop, Yucca/Cahuenga | Makes the shelter finding actionable |
| §6 | Future work names the transit planner's review and how missing signals (match LADOT signals to OSM crossings) and shelter roofs (street-imagery check) would be added | FEEDBACK "Limitations and future work" and "Primary challenge evidence" |
| LaTeX | Fig. 1/2 float moved into §4 (`[!tb]`, next to its first reference), which lets text fill page 3; `\raggedbottom`; Table 1 columns `p{.3}`/`p{.65}`; `\usepackage{textcomp}` for ° | Stops stretched paragraphs on page 3; stops "NASA WERK lidar [15]" wrapping |

**Cut or shortened to pay for it (≈110 words)**

| Where | Change |
|---|---|
| Abstract | Tightened the first challenge sentence; added "at 03:00, 375 are" (kept at ≈154 words, inside the 150–250 range) |
| §1 | User groups folded into one sentence; computation sentence shortened |
| §3 | Stop count, schedule date and 4 m cells moved into Table 1 rows; "Table 1 lists the inputs." moved to the start of the paragraph |
| §4 | Class list given as breaks; origin sentence shortened (limitation stays in §6); "deliberately" and "differing in hue and lightness" dropped |
| §5.2 | Dropped "2 stops within a 30-minute walk and 55 within 60"; buffer sentence shortened; validators sentence moved to §6 |
| §6 | Limitations in one sentence |
| Captions | Fig. 1–3 captions shortened (Fig. 3's panel labels are explained in §5.2) |

**Not changed:** title, keywords, all numbers, the three figures and their order, every original citation.

**Still open (see `WHAT_TO_DO_NEXT.md`):** Fig. 1 legibility at print size (FEEDBACK deducted 0.5 for this); optional legend strip in Fig. 3; Andrienko et al. stays a generic clause until someone reads the full text; tool citations (deck.gl, MapLibre, React, D3, pvlib) only if space appears.
