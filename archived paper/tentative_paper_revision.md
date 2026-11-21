# Sun / LA — Tentative Text Revision (Original vs. Revised by Section)

> **Notes before pasting into Overleaf:**
> - **Reference validation:** All new external papers cited in the revised text (Phan et al. 2005, Holten & van Wijk 2009, Jenny et al. 2018, O'Sullivan et al. 2000, Moreira et al. 2024, El-Geneidy et al. 2014, Fraser & Chester 2017, Lanza & Durand 2021, Harrower & Brewer 2003, MacEachren et al. 2012, Openshaw 1984, and Roberts 2007) have been verified for authors, titles, venues, years, and core claims.
> - **Page budget & formatting:** The revised text is written in prose paragraphs (no bullet lists inside the paper body, per LNCS rubric requirements) and tightened to fit the 5-page limit alongside 2–3 full-width figures.
> - **Own-words reminder:** Per `ASSIGNMENT.md`, treat this as a structural and content draft to review, verify against your live site and commit history, and adapt into your team's own words before final submission.
> - **Items to double-check on the live site before finalizing:** (1) exact neighborhood names of the difficult-tract clusters at 08:00 in Section 5.2, (2) whether you ran a Chrome DevTools CVD check on the sun/reachability palettes in Sections 4.2–4.3, and (3) any dropped features you want to mention in Section 6.

---

## Title, Abstract, and Keywords

### Title

**Original:**
> Sun / LA: Visualizing Sun Exposure and Signal-Aware Reachability Before Boarding a Bus

**Revised:**
> Sun / LA: Visualizing Sun Exposure and Underserved Bus-Boarding Reachability in Los Angeles

---

### Abstract

**Original:**
> Sun / LA examines why a nearby bus stop may not offer quick boarding or a comfortable approach. The system integrates a directed pedestrian network, Metro schedules, estimated signal-crossing delays, and lidar-based shade for 1,120 representative Los Angeles tract origins. Two visualization challenges guide its design. First, we explain where direct-sun time accumulates during a walk and scheduled wait while marking unknown intervals. Second, we distinguish network walking reachability from earliest scheduled boarding when routing assumptions and departure times interact. Following a form-follows-function approach, separate coordinated maps encode walking-time geography, available routes, boarding outcomes, and exposure. With a 30-minute walking budget, adding an assumed 30 seconds per modeled signal crossing reduces reachable origin-stop pairs from 113,395 to 112,201. In one illustrative overnight case, the changed path misses a departure and earliest boarding shifts from 03:03 to 03:47. These results demonstrate why proximity, pedestrian access, and boarding should not be visualized as one measure. The system is exploratory: delays and sunlight are modeled, not observed, and no personal heat-risk or bus-reliability prediction is claimed.

**Revised (~195 words):**
> Sun / LA is an interactive geovisualization dashboard built for transit planners, shade advocates, and Los Angeles riders to examine why a nearby bus stop may still be hard to board or heavily exposed to direct sunlight. Integrating a directed pedestrian network, Metro schedules, estimated signal-crossing delays, and lidar-derived shade across 1,120 Los Angeles census tract origins, the system addresses two visualization challenges. First, for sun exposure, a standard choropleth cannot show where sunlight accumulates along a moving walk versus a stationary wait without confusing missing shade data with shade; we address this with orange/teal/gray route segments and a linked walk-and-wait breakdown over fixed-bin tract fills. Second, for reachability, we must keep street-level routes legible over a fine-unit choropleth while distinguishing walking time from boarding difficulty—defined as walk plus scheduled wait exceeding 30 minutes. Our reachability map shares an eight-class walking-time scale between fills and routes, uses dark casings with red edges for contrast, and shows default routes only for underserved tracts. At 08:00 with an assumed 30-second signal penalty, 52 of 1,120 origins (4.6%) are difficult to board, and reachable 30-minute walk pairs drop from 113,395 to 112,201, illustrating why proximity, walking access, and boarding require distinct visual encodings.

---

### Keywords

**Original:**
> Transit accessibility · Reachability · Sun exposure · Geovisualization · Time-dependent networks

**Revised:**
> Transit accessibility · Choropleth · Flow and route maps · Underserved areas · Sun exposure · Geovisualization

---

## 1. Introduction

### 1.1 Motivation, Purpose, and Scope

**Original:**
> **1.1 Motivation and Why Los Angeles**
> A bus-stop pin cannot show boarding access or sun exposure. Los Angeles provides a compelling case: Metro reported shelters at only 24% of regionally served stops in 2022 [6], while UCLA documented uneven shade within LA City [11]. Available pedestrian, timetable, signal, census, and lidar data support joint analysis. Origins represent LA City; candidate stops may lie just outside it.

**Revised:**
> **1.1 Purpose, Users, and Scope**
> A bus-stop pin on a map cannot tell a rider whether they can actually walk there, board promptly, or avoid prolonged direct sunlight. Sun / LA is designed for three audiences making distinct decisions. Transit planners use the system to locate neighborhoods where combined walking and waiting time to first boarding exceeds 30 minutes, distinguishing stop-coverage gaps from service-frequency gaps; for example, opening the Reachability view at 08:00 flags Tujunga (Tract 1011.22) at 39.0 minutes to board, with its fastest walk alone taking 29:54, pointing to a stop or routing gap rather than just a long wait. Shade advocates and city staff inspect whether a tract's sun exposure occurs while walking or waiting so they can prioritize bus shelters versus street trees—for instance, selecting Hollywood Tract 1902.01 reveals that 52.7 of its 53.5 sunlit minutes happen while waiting at Yucca/Cahuenga, making that stop a shelter priority. Riders and community groups search their own neighborhood in the shared header to compare local access and shade against nearby tracts. Los Angeles makes this problem urgent: LA Metro reported shelters at only 24% of regionally served stops in 2022 [6], and UCLA documented uneven shade across the city [11]. We scope the analysis to the City of Los Angeles because municipal signal, lidar, and shelter inventories are consistent within city boundaries; representative origins lie inside the city, while candidate bus stops may sit just outside it because pedestrians walk across municipal borders.

---

### 1.2 Reachability Visualization Challenge

**Original:**
> **1.2 Reachability: Walking Is Not Boarding**
> A bus-stop point of interest (POI) alone cannot show whether someone can walk there and board. We compute pedestrian routes and connect them to timetables to distinguish network reachability from boarding outcomes as departure times and assumed crossing delays change.

**Revised:**
> **1.2 Reachability Visualization Challenge**
> Visualizing transit reachability across a large city poses two intertwined visualization challenges: keeping a fine-unit choropleth and a street-level route layer legible together, and comparing two reachability conditions (with versus without signal-crossing waits) across departure times. Drawing all roughly 113,000 origin–stop walking pairs within 30 minutes produces severe overplotting as paths converge on shared arterials and stops over 1,120 tract polygons. Moreover, because walking time and walk-plus-wait boarding time are distinct measures, encoding both on one map creates visual conflicts. At 08:00 with signal waits, 23 of the 52 difficult tracts have a fastest walk under 30 minutes: they appear light on a walking-time choropleth yet fail on combined walk and wait. Conversely, when routes share the choropleth's color scale so color consistently means walking minutes, a route crossing a tract of the same class disappears into the fill—for example, Tujunga's 29:54 fastest walk falls in the 20–30-minute class, matching its own tract fill, and would vanish without a contrasting casing. Finally, because underserved tracts represent only about 4–5% of the city during daytime hours, they are easily lost in a citywide view unless visually prioritized.

---

### 1.3 Primary Visualization Challenge: Sun Exposure

**Original:**
> **1.3 Primary Challenge: Visualizing Sun Exposure**
> Sun exposure depends on where and when a person walks and waits, not only on the location of a bus stop. Changing solar position and surrounding buildings can make a short walk different from a long wait. Our primary visualization challenge is to show geographic patterns while revealing how much sun accumulates along paths versus at stops, without mistaking missing shade information for darkness.

**Revised:**
> **1.3 Primary Visualization Challenge: Sun Exposure**
> Sun exposure before boarding depends on where and when a person moves along sidewalks and stands at a stop as solar geometry and building shadows shift across 1,120 origins, four seasons, and 25 daytime departure times (06:00–18:00). A standard choropleth fails here because a single tract fill can show citywide magnitude but cannot indicate where along the trip sunlight occurs or whether walking or waiting dominates. For example, Hollywood Tract 1902.01 falls into the darkest exposure class (53.5 minutes of direct sun), which from the fill alone suggests a long, unshaded walk; in reality, the walk lasts 1.3 minutes and nearly all exposure comes from a 52.7-minute wait. Our primary visualization challenge is therefore to preserve citywide geographic comparability across seasons and hours while decomposing individual trips into walking versus waiting sunlight and keeping missing shade coverage visually distinct from shade.

---

### Contributions and Roadmap (New Contributions Paragraph + Updated Roadmap)

**Original:**
> *(No contributions paragraph in original.)*
> The remainder of this paper is organized as follows. Section 2 reviews related work; Section 3 introduces the datasets; Section 4 explains the models and visual design; Section 5 presents usage scenarios and results; and Section 6 discusses limitations and future work.

**Revised:**
> Our contributions directly address these challenges in three ways. First, for the primary sun-exposure challenge, we combine a fixed-bin tract choropleth with orange/teal/gray walking-path segments and a linked walk-versus-wait breakdown that separates missing shade data from zero sunlight. Second, for the reachability challenge, we introduce an underserved-focused travel-time map that shares one eight-class walking-time scale across fills and routes, preserves route legibility over same-class fills through dark casings and red difficult-tract edges, and defaults to showing routes only for difficult tracts (three closest stops) while revealing 60-minute alternatives on click. Third, we integrate both views into a single coordinated dashboard with shared neighborhood/stop search and persistent community selection across tabs.
>
> The remainder of this paper is organized as follows. Section 2 reviews related work and states the gaps our design addresses; Section 3 describes the datasets and spatial unit; Section 4 details the models, visual encodings, design iterations, and data optimization; Section 5 presents usage scenarios, results, and validation; and Section 6 discusses limitations, unfinished plans, and future work.

---

## 2. Related Work

**Original:**
> Lindberg and Grimmond model urban shadows [4]; Miranda et al. accumulate citywide shade [9]. We sample walking routes and waits instead. Network isochrones [3] and departure-dependent transit accessibility [2] inform our reachability model. Signal delays matter [8], but our penalty is an uncalibrated scenario; we model first boarding, not complete transit trips.

**Revised (Three themed paragraphs with verified references and explicit gap statements):**
> **Accessibility, Isochrone, and Flow-Map Visualization.** Prior transit accessibility research established network-based isochrones and departure-dependent travel-time surfaces [2, 3]. In GIS visualization, O'Sullivan et al. [14] extended desktop network analysis to scheduled public transport by generating isochrones around transit nodes. To display many origin–destination connections without clutter, flow-map and graph-visualization work merges or curves links—for example, hierarchical flow-map layouts [15], force-directed edge bundling [16], and empirically grounded origin–destination flow-map design principles that scale line width by volume and smooth link geometry [17]. However, isochrones typically expand outward from a single destination rather than flagging which residential origins fail on combined walk and wait, and bundled or width-encoded flows abstract away real street geometry to show aggregate volume. Our reachability design fills this gap with an origin-first choropleth that flags walk-plus-wait difficulty separately from walking time while keeping individual, street-faithful routes legible over same-scale fills through selective default display, edge deduplication, and dark casings.
>
> **Urban Sun and Shade Modeling and Visualization.** Microclimate models such as SOLWEIG [4] simulate how buildings and vegetation cast urban shadows and alter mean radiant temperature. On the visualization side, Miranda et al. [9] introduced shadow accrual maps to interactively accumulate city-scale building shadows across time ranges, and Moreira et al. [18] developed the Urban Toolkit to compose multi-layer urban visual analytics—including sunlight-access and shadow what-if scenarios—across buildings and street networks. While these systems summarize shadow accumulation over static areas or whole days, they do not follow a single time-dependent transit access trip that combines a moving sidewalk walk with a stationary wait at a scheduled stop. Sun / LA addresses this gap by sampling lidar-derived horizons along routed walks and scheduled waits at chosen seasonal departure times and encoding missing shade coverage distinctly from shade.
>
> **Walking Access, Signal Delay, and Transit Heat Exposure.** Empirical transit studies show why network paths, crossing delays, and stop shade must be examined together. El-Geneidy et al. [19] demonstrated that real pedestrian walking distances to bus stops frequently exceed the conventional 400 m circular buffer and vary with street layout and wait time, supporting network-based walking analysis over Euclidean circles. At intersections, Marisamynathan and Vedagiri [8] showed that pedestrian signal delays materially affect crossing times. Meanwhile, Fraser and Chester [20] linked transit schedules and waiting times to rider heat vulnerability, Lanza and Durand [21] examined how bus-stop shelters and tree shade moderate heat exposure and ridership, and LA-specific audits documented both sparse shelter coverage (24% regionally [6]) and inequitable urban shade [11]. Existing work studies these factors separately in statistical or policy reports; Sun / LA unifies pedestrian routing, signal-delay scenarios, timetables, and sun exposure in one interactive visual tool.

---

## 3. Data

**Original:**
> One representative origin is used per LA City tract (1,120 total; 57 street proxies). Sun Exposure includes 7,568 stops and 51,359 connections; Reachability considers 10,968 stops, some outside the city. ACS 2020-2024 records identify 278 high no-vehicle-share tracts among 1,112 valid records, not individual riders [12]. Table 1 summarizes other sources [10,5,7,13]. Metro schedules are frozen to October 27, 2026; lidar is aggregated to 4m cells. A 1,617-point shelter inventory is unverified; raw data stays outside Git.
>
> **Table 1. Core inputs and their use in the two maps.**
> - OpenStreetMap | Pedestrian network for walking paths and travel times
> - Metro GTFS | Stops and departures for scheduled boarding
> - LADOT | Signal locations for modeled crossing delays
> - ACS | No-vehicle household shares for tract outlines
> - Lidar | Terrain and surface heights for shade estimates

**Revised:**
> We use the census tract as our spatial unit because American Community Survey (ACS) indicators are published at the tract level—allowing the Sun Exposure view's demographic scatterplot ("Who spends more time in the sun?") and the Reachability map to align on identical boundaries—while 1,120 units are fine enough to reveal intra-city structure in a familiar planning geography. This choice carries a known modifiable areal unit problem (MAUP) cost [22]: representing each tract by one origin hides within-tract variation, especially in large hillside tracts such as Tujunga where a geometric centroid can sit far from homes or well-connected streets. To mitigate connectivity artifacts while acknowledging this unit limit, 57 poorly connected centroids were replaced with audited in-tract street proxies.
>
> Sun Exposure includes 7,568 stops and 51,359 origin–stop connections, whereas Reachability evaluates 10,968 Metro stops, including stops just outside city limits. Metro GTFS schedules are frozen to a single weekday (October 27, 2026), lidar elevation surfaces are aggregated to 4 m cells, and a 1,617-point bus-shelter inventory is retained as an unverified reference. Unlike an earlier prototype, ACS 2020–2024 five-year estimates [12] no longer drive outlines or badges in Reachability—where difficulty depends solely on walk plus wait—and instead power the demographic scatterplot in Sun Exposure. To make tracts recognizable to non-expert users, we join Los Angeles Times *Mapping L.A.* neighborhood boundaries distributed via the City of Los Angeles GeoHub [23]; because 206 tracts share neighborhood names, the tract number always remains visible alongside the community name. Both maps render over the OpenFreeMap Positron vector basemap [24]. Table 1 summarizes all core inputs.
>
> To keep the web application responsive without sacrificing route detail, precomputed routes are split into two tiers. First, a compact default difficult-route catalog stores paths to the three closest stops for every difficult origin (1,506 paths without signal waits and 1,520 with 30-second signal waits, totaling 396 KB compressed). Second, for on-click exploration of difficult tracts, we precompute 60-minute display routes (82,055 and 82,902 routes across the 504 and 509 origins that are difficult at some departure time in the 0 s and 30 s scenarios). All per-tract route files total 55.7 MB, remain outside Git (fetched from a checksum-pinned GitHub Release at build time), and are deployed as one small file per tract so the browser downloads only the clicked tract's routes (e.g., about 9 KB for Tract 1011.22).
>
> **Table 1. Core inputs and their visualization roles across the dashboard.**
> | Source | Data and visualization role |
> | :--- | :--- |
> | OpenStreetMap [10] | Directed pedestrian network for walking paths and travel times |
> | Metro GTFS [5] | Bus stops and frozen Oct 27, 2026 weekday schedules for boarding |
> | LADOT [7] | Mapped traffic signal locations for modeled crossing delays |
> | ACS 2020–2024 [12] | Tract demographic attributes for Sun Exposure scatterplot |
> | Lidar (NASA WERK) [13] | 4 m terrain and surface heights for horizon shade estimates |
> | Mapping L.A. / OpenFreeMap [23, 24] | Neighborhood labels and Positron vector basemap |

---

## 4. Design and Implementation

### 4.1 Computing Reachability and Sun Exposure

**Original:**
> **Walking and boarding.** For origin $o$ and stop $s$, the fastest cost over directed pedestrian paths $\mathcal{P}_{os}$ is
> $$W_{os}^{\delta} = \min_{p \in \mathcal{P}_{os}} \left( \frac{L_p}{v} + \delta n_p + c_o + c_s \right). \quad (1)$$
> Here $L_p$ is path length, $v = 4.8\text{ km/h}$, $n_p$ counts mapped signals, and $c_o, c_s$ estimate endpoint connections. Dijkstra uses $\delta = 0$ or an assumed 30 seconds per crossing, with a 1,800-second walk limit.
> For start time $t$, let $D_s(u)$ be the first permitted GTFS departure at stop $s$ after arrival $u$. The earliest elapsed time to board is
> $$B_o(t, \delta) = \min_{s : W_{os}^{\delta} \le 1800} \left[ D_s(t + W_{os}^{\delta}) - t \right]. \quad (2)$$
> The 48 half-hour start times change boarding, not walking costs; onward travel is excluded.
> **Sun exposure.** The sun model combines pvlib angles [1] with 600 m lidar horizons at 4 m resolution, sampled every six seconds walking and up to once per minute waiting. For known intervals $k$ of length $\Delta t_k$ (minutes), direct-sun time is
> $$E_o(t) = E_{\text{walk}}(t) + E_{\text{wait}}(t) = \sum_{k \in K_{\text{known}}(t)} \Delta t_k \mathbf{1}[\text{sun}_k]. \quad (3)$$
> Sun, shade, and unknown remain separate. Four dates represent seasons, with fixed schedules and vegetation; exposure is not heat or UV dose.

**Revised:**
> **Walking, boarding, and difficulty.** For origin $o$ and stop $s$, the fastest cost over directed pedestrian paths $\mathcal{P}_{os}$ is
> $$W_{os}^{\delta} = \min_{p \in \mathcal{P}_{os}} \left( \frac{L_p}{v} + \delta n_p + c_o + c_s \right), \quad (1)$$
> where $L_p$ is path length, $v = 4.8\text{ km/h}$, $n_p$ counts mapped signals, and $c_o, c_s$ estimate endpoint connections. Dijkstra's algorithm runs with $\delta = 0$ or an assumed $\delta = 30\text{ s}$ per signal crossing. For each of 48 half-hour departure times $t$, let $D_s(u)$ be the first permitted GTFS departure at stop $s$ after arrival $u$. Earliest elapsed time to board within the 1,800-second (30-minute) analytical walk search is
> $$B_o(t, \delta) = \min_{s : W_{os}^{\delta} \le 1800} \left[ D_s(t + W_{os}^{\delta}) - t \right]. \quad (2)$$
> We formally classify an origin $o$ as **difficult to board** at time $t$ if $B_o(t, \delta) > 1,800\text{ s}$. If the shortest network walk alone exceeds 30 minutes ($\min_s W_{os}^{\delta} > 1,800\text{ s}$), the tract is likewise classified as difficult using its shortest walk as a lower bound ("at least"), even without a finite boarding total. Tracts with no scheduled bus or incomplete data remain distinct states and are not counted as difficult unless the walking bound proves it. Importantly, all boarding times, difficulty classifications, counts, and reverse catchments strictly use the 30-minute analytical walk search; the additional 60-minute routes computed for difficult tracts are display-only so users can inspect realistic farther alternatives without altering any metrics.
>
> **Sun exposure.** The sun model combines pvlib solar angles [1] with 600 m lidar horizons at 4 m resolution, sampled every six seconds while walking and up to once per minute while waiting. For known intervals $k$ of length $\Delta t_k$ (minutes), direct-sun time is
> $$E_o(t) = E_{\text{walk}}(t) + E_{\text{wait}}(t) = \sum_{k \in K_{\text{known}}(t)} \Delta t_k \mathbf{1}[\text{sun}_k]. \quad (3)$$
> Sun, shade, and unknown intervals stay separate across four seasonal dates. Throughout the system, crossing delays, timetables, and direct sunlight are modeled exploratory estimates rather than observed field measurements, bus-reliability predictions, or physiological heat/UV doses.

---

### 4.2 Reachability Map Design

**Original:**
> **4.2 Reachability: Why the Map Is Difficult**
> Straight-line circles ignore pedestrian connectivity, while all origin-stop routes together create clutter. Blue bins encode minimum network walking time; stop and tract clicks reveal routes and candidates, respectively. Selected paths and layer toggles reduce overlap, but every tract is represented by one origin.
> Walking and boarding have separate encodings. Departure time affects the timetable, not walking-time colors; signal penalties may change paths. Details show boarding; purple outlines mark high no-vehicle-share tracts, and red flags indicate finite waits over 30 minutes. Missing service stays distinct.

**Revised:**
> **4.2 Reachability Map Design**
> **Mode, thresholds, and choropleth classes.** Walking is the access mode because bus stops are local neighborhood destinations. While empirical studies show that most bus riders walk roughly 400–524 m (about 5–7 minutes) to a stop [19], requiring more than 30 minutes of combined walking and waiting just to board represents severe transit access difficulty; we therefore anchor our difficulty threshold at 30 minutes and provide 60-minute display routes on click so planners can see what farther stops a difficult tract can realistically reach. Tract fills encode minimum network walking time using eight upper-inclusive classes: 0–5, 5–10, 10–15, 15–20, 20–30, 30–45, 45–60, and 60+ minutes. Under the 30-second signal scenario, the citywide median fastest walk is 4.9 minutes, and tract counts across the eight bins are 579, 366, 80, 33, 33, 20, 7, and 2: 84% of tracts fall under 10 minutes (justifying 5-minute steps at the low end), a class break sits exactly at 30 minutes to align with the difficulty rule, and the three upper classes isolate the 29 tracts whose walk alone already makes them difficult. We encode these bins with classes 2–9 of the ColorBrewer sequential YlGnBu palette [25] (dropping the near-white first class for contrast on the light basemap). Computed CIE $L^*$ lightness decreases monotonically from 95 to 13 across the eight classes, preserving perceptual ordering in grayscale and under color-vision deficiency.
>
> **Separating walking from boarding and keeping routes legible.** To make "walking is not boarding" immediately visible, tract fills encode *walking time only*, whereas red `!` markers flag *walk plus wait* exceeding 30 minutes; a pale tract with a `!` immediately reveals a short walk undermined by a long scheduled wait (Fig. 3). Walking routes share the exact same eight-class YlGnBu scale so color always means walking minutes. Because a route crossing a tract of the same class would otherwise vanish, we draw solid routes over translucent fills with a thin dark casing (`#26323a`), and add a red outer edge (`#a52d46`, matching the `!` marker) to routes from difficult tracts so both sides of the path remain distinct. Selected routes widen while retaining their time-class color. Clicking a bus stop displays its reverse catchment—all origins that can reach that stop within 30 minutes—making directional asymmetry visible.
>
> **Overplotting strategy and alternatives considered.** Showing all ~113,000 origin–stop routes at once is unreadable, so the default view renders routes only from difficult tracts to their three closest stops; ordinary tracts show no default routes. Clicking a difficult tract reveals every candidate stop within 60 minutes, whereas clicking an ordinary tract shows stops within 30 minutes plus its nearest stop. Shared street segments across candidate paths are deduplicated and colored by the shortest contributing walk, and layer toggles let users isolate fills, routes, or markers. We evaluated and rejected several alternatives during design. Straight-line buffers around stops were rejected because they ignore freeways, hills, and dead ends [19]. Rendering all routes citywide failed due to severe clutter. Using a separate red route scale over blue fills (an earlier implementation) caused readers to misinterpret routes as a different metric, prompting the switch to one shared scale. Likewise, an earlier purple edge for difficult routes sat too close in hue ($\approx 43^\circ$) to the 45–60-minute blue-indigo class; switching to red pushed hue separation above $60^\circ$ from every bin, enforced by an automated unit test. We also rejected edge bundling and width-encoded flows [15–17] because our task asks which real streets connect a tract to specific stops rather than aggregate passenger volume, and we kept an origin-first choropleth over stop-first isochrones [14] so residents and planners can directly assess whether a given neighborhood is underserved. Finally, we render the full-strength OpenFreeMap Positron basemap under translucent fills so street names remain readable along routes, accepting the trade-off that map labels sit beneath darker fills.

---

### 4.3 Primary Challenge: Sun Exposure Design

**Original:**
> **4.3 Primary Challenge: Visualizing Sun Exposure**
> Exposure accumulates along a moving walk and a stationary wait under changing solar geometry. Tract fills reveal geographic patterns but not where sun occurs or which journey component dominates. Yellow-to-red bins encode total direct-sun minutes; users can inspect walking and waiting separately. Orange/teal path segments reveal sunny/shaded locations, and a duration breakdown explains their contribution (Fig. 1). Gray marks unknown rather than zero, while selected routes and toggled layers reduce clutter without replacing exposure colors.

**Revised:**
> **4.3 Primary Challenge: Sun Exposure Design**
> Tract fills encode total direct-sun minutes using five fixed bins—0–5, 5–10, 10–15, 15–20, and 20+ minutes—so visual comparisons remain consistent across seasons and hours. We rejected quantile or per-season breaks because they would make winter and summer maps look equally dark and obscure seasonal drops (e.g., a 3.22-minute summer median versus 1.00 minute in winter at 08:00), and we rejected a continuous ramp because discrete bins make exact minute thresholds easier to match against the legend. The custom yellow-to-red palette (`#fff1c6` to `#b73b29`, inspired by sequential YlOrRd schemes [25]) exhibits monotonically decreasing CIE $L^*$ lightness (95 to 43), maintaining legibility in grayscale. When a user selects a tract and stop, individual walking segments are colored orange (`#ef9b34`) for sun and teal (`#247d79`) for shade, pairing a warm–cool hue contrast with a lightness separation ($L^* \approx 71$ vs. $48$) so the two states remain distinguishable under red–green color-vision deficiency. We rejected encoding sunlight as route line width because width conveys magnitude but hides *where* along the sidewalk exposure occurs. Missing shade data is encoded in neutral gray rather than zero or teal, following uncertainty-visualization principles that separate missingness from low values [26]. A linked walk-and-wait breakdown in the side panel quantifies each component (Fig. 2). At city scale, drawing routes only for the active selection, offering 10/15/20-minute stop catchments, and providing layer toggles keep the display uncluttered.
>
> **Design process and iterations.** Across both views, our design evolved through concrete iterations driven by visual critiques and usability checks. Beyond unifying the two travel-time scales into one shared ramp and replacing the purple difficult-route edge with red, we replaced a washed-out raster basemap with the full-strength vector Positron map so street names remained legible; added *Mapping L.A.* neighborhood names alongside bare tract IDs; simplified an overloaded side panel into one headline answer with collapsible detail sections; moved search into the shared header; and removed redundant map titles and floating selection chips from the canvas to maximize map area.

---

### 4.4 Implementation, Dashboard Integration, and Data Optimization

**Original:**
> React/Bootstrap provide controls; deck.gl/MapLibre and D3 render layers and scales. Paths, catchments, and shade tiles are precomputed and loaded on demand; initial Reachability data is 420,896 compressed bytes, not a load-time measurement.

**Revised:**
> **4.4 Implementation, Dashboard Integration, and Data Optimization**
> Sun / LA is implemented as a single coordinated React and Bootstrap web application [27] rendered with deck.gl, MapLibre GL, and D3 (Fig. 1). Two top-level tabs switch between *Sun Exposure* and *Reachability* while sharing a unified header search (by neighborhood, tract ID, or bus stop) and persistent community selection, so clicking a tract in one tab carries directly into the other. Temporal controls remain view-specific because the solar horizon model covers only daylight hours (06:00–18:00 with a four-season selector), whereas Reachability spans 00:00–23:30 with a signal-wait toggle. Both views share a consistent control row and a collapsible side panel that foregrounds one headline metric while folding secondary details.
>
> To ensure fast browser startup despite citywide routing and 4 m shade grids, all spatial layers are precomputed offline into static compressed assets and fetched on demand. Automated build checks cap the default difficulty-route catalog under 1 MB, the neighborhood lookup is 4.5 KB, and the initial Reachability overview payload is 420,896 compressed bytes. The route network illustrates our strongest optimization: rather than shipping the full 55.7 MB of 60-minute routes upfront, routes are partitioned into one file per tract and hosted on a checksum-pinned release. Measured in Chrome on the deployed site, opening the Reachability tab downloads roughly 0.74 MB of gzipped data (`index.json.gz` 162 KB, `stops.json.gz` 195 KB, and `difficulty-routes.json.gz` 387 KB), and clicking an individual tract fetches only its single route file (e.g., ~9 KB for Tract 1011.22). Shade tiles and timetables load on demand in the same way. During development, the team used an AI coding assistant for React/TypeScript implementation, debugging, and automated design checks (such as hue-separation tests), while all analytical framing, data verification, and paper writing were conducted by the authors.

---

## 5. Usage Scenarios and Results

### 5.1 Primary Challenge: Explaining Walking and Waiting Exposure

**Original:**
> A user selects season and departure time, then a tract and stop to inspect the path. Route segments locate sunny and shaded stretches; the breakdown separates walking, waiting, and unknown minutes. Switching measures explains a high-exposure tract beyond its color.
> At 08:00, median modeled direct sun is 3.22 minutes across 1,110 valid summer origins and 1.00 minute across 1,112 winter origins; these are unpaired descriptive groups. In summer, tract 1902.01 reaches Yucca/Cahuenga after a 79-second walk with 0.85 sunlit minutes, then has a 52.68-minute scheduled wait, all classified as sun.
> Waiting thus dominates despite the short walk. Route colors explain exposure locations, while the breakdown explains duration. The stop need not offer earliest boarding; sunlight is modeled, not observed heat or UV dose.

**Revised:**
> A shade advocate selects a season and departure time, then clicks a high-exposure community to diagnose why its sun total is high. At 08:00, median modeled direct sun drops from 3.22 minutes in summer (1,110 valid origins) to 1.00 minute in winter (1,112 valid origins). Inspecting **Hollywood (Tract 1902.01)** in summer at 08:00 (Fig. 2), the tract sits in the darkest 20+ minute fill bin (53.5 minutes total). Clicking its connection to Yucca/Cahuenga reveals that the walk takes only 1.3 minutes (79 seconds) with 0.8 minutes of direct sun, whereas the scheduled wait accounts for 52.7 sunlit minutes. Orange and teal segments pinpoint where shade occurs along the short walk, while the breakdown immediately clarifies that stop-level waiting—not sidewalk walking—drives exposure, pointing toward a shelter intervention rather than street trees.
>
> **Validation.** Compared against a conventional fill-only choropleth of total sun minutes, our linked route-and-breakdown display prevents misattributing a dark tract fill to a long, unshaded walk and explicitly separates unknown coverage from shade. A formal user evaluation has not yet been conducted; we designed a four-task formative protocol (with 4–6 external participants comparing a fill-only map against our linked display on walk-versus-wait attribution, unknown-versus-zero coverage, shortest-walk versus earliest-boarding judgment, and signal-delay explanation) and leave its execution and field shade spot-checks as planned future work.

---

### 5.2 Reachability Challenge: Walking Versus Boarding

**Original:**
> With a 30-minute walk budget, an assumed 30 seconds per mapped signal crossing reduces reachable origin-stop pairs from 113,395 to 112,201 (1,194 pairs, not passengers). At 03:00, tract 2699.03 boards at Venice/Motor at 03:03 after a 174-second walk without penalties. The 193-second penalized walk misses that bus, making 03:47 at Venice/Helms the earliest boarding. Both scenarios board within eight minutes at 08:00. Among high-no-vehicle tracts, finite boarding times over 30 minutes occur in 29 versus 30 tracts at 03:00, but none at 08:00. These are modeled outcomes using fixed schedules.

**Revised:**
> Across the city at 08:00 with 30-second signal waits, **52 of 1,120 origins (4.6%)** are difficult to board: 29 already exceed 30 minutes on walking alone, while 23 have walks under 30 minutes but cross the threshold due to scheduled waits (Fig. 3a). Daytime difficult tracts cluster primarily along hillside and northern San Fernando Valley edges, with additional pockets near the harbor area *(verify exact neighborhood names on live site)*. Selecting **Tujunga (Tract 1011.22)** at 08:00 illustrates both the underserved-access finding and our legibility solution (Fig. 3b): boarding takes 39.0 minutes, with a fastest walk of 29:54 and only 2 stops reachable within 30 minutes, compared with 55 stops within 60 minutes. Because the 29:54 route shares the 20–30-minute color of Tujunga's own tract fill, the dark casing and red outer edge are essential to keep the street path readable over the polygon.
>
> Comparing departure times separates structural geography from service hours. Under the 30-second signal scenario, difficult tracts stay between 44 and 78 during daytime hours (52 at 08:00, 52 at 12:00, and 50 at 17:00), rise to 131 at 23:00, and surge to **375 at 03:00**. Because the 29 tracts with walks over 30 minutes are difficult at every hour, the overnight surge is overwhelmingly wait-driven: 346 of the 375 difficult tracts at 03:00 have a fastest walk under 30 minutes. Comparing signal scenarios further shows threshold sensitivity: adding 30 seconds per mapped signal reduces 30-minute walkable origin–stop pairs from 113,395 to 112,201 (1,194 pairs). At 03:00, **Tract 2699.03** boards at Venice/Motor at 03:03 after a 174-second walk without signal penalties, but the 193-second penalized walk misses that departure, pushing earliest boarding to 03:47 at Venice/Helms and flipping the tract into the difficult state (Fig. 3c). Both scenarios board within eight minutes at 08:00.
>
> **Validation.** Unlike straight-line stop buffers or nearest-stop distance maps—which would mark all 23 wait-driven daytime difficult tracts and 346 overnight tracts as adequately served because stops are nearby—our origin-first choropleth with `!` flags exposes schedule-driven access failures directly. While walking paths and boarding times were checked internally against GTFS timetables and street topology, independent spot-checks of modeled walking durations against public pedestrian routing services (e.g., for the Tujunga, Hollywood, and Venice/Motor pairs) and field timing of signal crossings remain future validation steps.

---

## 6. Conclusion

**Original:**
> Sun / LA distinguishes network walking, scheduled boarding, and modeled sunlight from proximity. Short walks can precede sunny waits; small delays can miss buses. Limits include representative origins, incomplete signals, assumed penalties, frozen schedules, fixed vegetation, and clear skies. Future work should validate reliability, shelter protection, heat dose, and usability.

**Revised:**
> Sun / LA demonstrates how coordinated geovisualization can separate proximity from lived transit access before boarding—combining fixed-bin choropleths, orange/teal/gray walk segments, and a walk-versus-wait breakdown for sun exposure with a shared-scale, casing-enhanced, underserved-focused route map for reachability.
>
> **Limitations and unfinished work.** Five modeling and rendering constraints directly affect what the maps show. First, using one representative origin per tract causes each fill and `!` flag to describe a single point rather than within-tract variation, which is most pronounced in large hillside tracts such as Tujunga (requiring 57 audited street proxies). Second, the flat 30-second signal penalty makes `!` flags near the 30-minute boundary sensitive to the assumed delay, as seen in Tract 2699.03 shifting from 03:03 to 03:47. Third, relying on one frozen weekday schedule (October 27, 2026) for LA Metro buses alone omits weekends, real-time delays, and other local operators, potentially flagging tracts served by non-Metro buses. Fourth, assuming clear skies, static opaque vegetation, and no shelter roofs makes sun minutes a geometric upper bound rather than heat dose—counting all 52.7 waiting minutes at Yucca/Cahuenga as sunlit because shelter roof shade is unmodeled. Fifth, deck.gl/MapLibre layer interleaving places basemap labels beneath fills, reducing street-name readability under dark 30–60-minute polygons. Several planned evaluation items were not completed for this submission: the four-task user study comparing fill-only and linked views, color-vision-deficiency testing with human participants beyond computed lightness checks, field validation of shade states and crossing delays, and timed browser load benchmarks.
>
> **Future work.** First, to expose within-tract variation (e.g., lower Foothill Blvd versus upper hillsides in Tujunga), we plan to introduce zoom-dependent census block-group units using precomputed per-unit route files loaded at neighborhood zoom, keeping the eight-class scale identical and retaining tract outlines so users can track MAUP scale transitions. Second, we can replace the flat 30-second penalty with field-timed or signal-phase-calibrated delays and expand the toggle into a multi-step sensitivity slider (0/15/30/45 s). Third, integrating additional publicly available municipal feeds (such as LADOT DASH and Metro Rail), weekend calendars, and archived GTFS-Realtime delays—alongside verified shelter-roof shade, seasonal leaf-off canopies, and mean radiant temperature [4]—would turn scheduled waits and sun minutes into realistic wait ranges and thermal-comfort estimates. Finally, we plan to link both challenges in a bivariate priority view highlighting tracts that are both difficult to board and sun-exposed, and to complete the four-task formative user study and an expert review with transit planners and shade advocates, reporting task accuracy and completion times.

---

## References (LNCS Format — Validated Additions)

*(Keep existing references [1]–[13] from `paper.pdf`, and add the following verified entries to `mybibliography.bib` using `\bibliographystyle{splncs04}`:)*

14. O'Sullivan, D., Morrison, A., Shearer, J.: Using desktop GIS for the investigation of accessibility by public transport: an isochrone approach. International Journal of Geographical Information Science 14(1), 85–104 (2000). https://doi.org/10.1080/136588100240976
15. Phan, D., Xiao, L., Yeh, R., Hanrahan, P., Winograd, T.: Flow map layout. In: Proceedings of the IEEE Symposium on Information Visualization (INFOVIS 2005), pp. 219–224. IEEE (2005)
16. Holten, D., van Wijk, J.J.: Force-directed edge bundling for graph visualization. Computer Graphics Forum 28(3), 983–990 (2009). https://doi.org/10.1111/j.1467-8659.2009.01450.x
17. Jenny, B., Stephen, D.M., Muehlenhaus, I., Marston, B.E., Sharma, R., Zhang, E., Jenny, H.: Design principles for origin–destination flow maps. Cartography and Geographic Information Science 45(1), 62–75 (2018). https://doi.org/10.1080/15230406.2016.1262280
18. Moreira, G., Hosseini, M., Nipu, M.N.A., Lage, M., Ferreira, N., Miranda, F.: The Urban Toolkit: A grammar-based framework for urban visual analytics. IEEE Transactions on Visualization and Computer Graphics 30(1), 1402–1412 (2024). https://doi.org/10.1109/TVCG.2023.3326598
19. El-Geneidy, A., Grimsrud, M., Wasfi, R., Tétreault, P., Surprenant-Legault, J.: New evidence on walking distances to transit stops: identifying redundancies and gaps using variable service areas. Transportation 41(1), 193–210 (2014)
20. Fraser, A.M., Chester, M.V.: Transit system design and vulnerability of riders to heat. Journal of Transport & Health 4, 216–225 (2017). https://doi.org/10.1016/j.jth.2016.07.005
21. Lanza, K., Durand, C.P.: Heat-moderating effects of bus stop shelters and tree shade on public transport ridership. International Journal of Environmental Research and Public Health 18(2), 463 (2021). https://doi.org/10.3390/ijerph18020463
22. Openshaw, S.: The Modifiable Areal Unit Problem. Concepts and Techniques in Modern Geography, vol. 38. Geo Books, Norwich (1984)
23. City of Los Angeles GeoHub / Los Angeles Times: Mapping L.A. Neighborhood Boundaries. https://geohub.lacity.org/ (accessed 2026-11-19) *(verify exact dataset URL from your repo)*
24. OpenFreeMap: Positron Vector Basemap. https://openfreemap.org/ (accessed 2026-11-19)
25. Harrower, M., Brewer, C.A.: ColorBrewer.org: An online tool for selecting colour schemes for maps. The Cartographic Journal 40(1), 27–37 (2003). https://doi.org/10.1179/000870403235002042
26. MacEachren, A.M., Roth, R.E., O'Brien, J., Li, B., Swingley, D., Gahegan, M.: Visual semiotics & uncertainty visualization: An empirical study. IEEE Transactions on Visualization and Computer Graphics 18(12), 2496–2505 (2012)
27. Roberts, J.C.: State of the art: Coordinated & multiple views in exploratory visualization. In: Fifth International Conference on Coordinated and Multiple Views in Exploratory Visualization (CMV '07), pp. 61–71. IEEE (2007)