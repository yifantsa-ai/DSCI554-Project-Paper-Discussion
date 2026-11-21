# Sun / LA — Paper Revision (`Paper_Revision.md`)

> **Before pasting into Overleaf:** (1) Rewrite the prose in your team's own words per `ASSIGNMENT.md` and adjust the GenAI disclosure in §4.4 to accurately reflect your workflow. (2) Re-verify the Chrome DevTools initial transfer size (cache disabled) and CVD emulation if claiming direct browser measurements. (3) Export official BibTeX entries from Google Scholar / DOIs for all new references. (4) Keep `Claude outputs/` out of the submitted Git repository.

## Title, Abstract, and Keywords

### Title

Sun / LA: Visualizing Sun Exposure and Underserved Bus-Boarding Reachability in Los Angeles

### Abstract (\~165 words)

Sun / LA is an interactive geovisualization dashboard built for transit planners, shade advocates, and Los Angeles riders to examine why a nearby bus stop can still be hard to board or heavily exposed to direct sunlight. Integrating a directed pedestrian network, Metro schedules, estimated signal-crossing delays, and lidar-derived shade across 1,120 census tract origins, the system addresses two visualization challenges. First, a standard choropleth cannot show whether modeled sunlight accumulates while walking or waiting, nor separate missing shade data from shade; we pair fixed-bin tract fills with orange/teal/gray route segments and a linked walk-and-wait breakdown. Second, keeping street-level routes legible over a fine-unit choropleth while distinguishing walking time from boarding difficulty—defined as walk plus scheduled wait exceeding 30 minutes—requires coordinated encodings; our reachability map shares one eight-class walking-time scale across fills and routes, uses dark casings and red edges for contrast, and displays default routes only for underserved tracts. At 08:00 with an assumed 30-second signal penalty, 52 of 1,120 origins (4.6%) are difficult to board.

### Keywords

Transit accessibility · Choropleth · Flow and route maps · Underserved areas · Sun exposure · Geovisualization

## 1. Introduction

### 1.1 Purpose, Users, and Scope

A bus-stop pin cannot show whether a rider can walk there, board promptly, or avoid prolonged direct sunlight. Sun / LA serves three audiences with distinct decisions: transit planners locate neighborhoods where walk-plus-wait access exceeds 30 minutes to separate stop gaps from service gaps; shade advocates and city staff determine whether exposure occurs while walking or waiting to prioritize bus shelters versus street trees; and riders search their neighborhood to compare local conditions with nearby tracts. For example, opening Reachability at 08:00 flags Tujunga (Tract 1011.22) at 39.0 minutes to board with a 29:54 fastest walk, signaling a stop or routing gap rather than a wait alone. Los Angeles makes this urgent: LA Metro reported shelters at only 24% of regionally served stops in 2022 \[6\], and UCLA documented uneven citywide shade \[11\]. We scope origins to the City of Los Angeles because municipal signal, lidar, and shelter inventories are consistent within city boundaries, while candidate stops may lie just outside because pedestrians walk across municipal borders.

### 1.2 Reachability Visualization Challenge

Walking and boarding times exist in no source dataset, so this is a custom travel-time map rather than a stock choropleth: every value must be computed before it can be drawn. We snap the 1,120 tract origins and 10,968 candidate stops onto a directed walking graph of about 6.7 million nodes, route them with Dijkstra under two crossing scenarios (0 s or 30 s at each of 14,492 confirmed signalized crossings), and link each arrival to the next permitted bus in the GTFS timetable, including after-midnight trips, for 48 departure times; the 0 s scenario alone yields 113,395 walkable origin–stop pairs within 30 minutes. Displaying these results raises two visualization challenges. First, the choropleth and the route layer must stay legible together. Drawing every pair clutters shared streets, so routes must be selected; and if routes get their own color scale, readers take them for a second measure, but if they share the fill's scale, a route vanishes over a tract of the same class—Tujunga's 29:54 route falls in the same 20–30-minute class as its own fill. Second, walking time and walk-plus-wait boarding are different measures that one fill cannot carry: at 08:00 with signal waits, 23 of the 52 difficult tracts have walks under 30 minutes, appearing light on a walking-time fill yet failing to board within 30 minutes. Because difficult tracts are only 4–7% of the city in daytime, they are easily lost unless the map prioritizes them, and comparing scenarios (with versus without signal waits, day versus night) must never change what a fill color means.


### 1.3 Primary Visualization Challenge: Sun Exposure

Pre-boarding sun exposure must likewise be computed from lidar surface heights and solar position at every 6-second walking step and every waiting minute across 1,120 origins, four seasons, and 25 daytime departures (06:00–18:00), which no stock map or chart displays. A standard choropleth fails because one tract fill cannot show where sunlight occurs, whether walking or waiting dominates, or how candidate stops differ. In summer at 08:00, a Hollywood tract (Tract 1902.01) displays a light citywide fill of roughly 1.5 minutes because the overview reflects the stop with earliest boarding; selecting the tract reveals the journey to its nearest stop, Yucca/Cahuenga—a 1.3-minute walk followed by a 52.7-minute sunlit wait (53.5 minutes total). Because the fill and selected journey can refer to different stops, the fill alone hides severe wait exposure. Our challenge is to preserve citywide comparability while decomposing trips into walking versus waiting sunlight and keeping missing shade coverage distinct from shade.

### Contributions and Roadmap

Our contributions directly pair with these challenges. For the sun-exposure challenge, we combine a fixed-bin choropleth with orange/teal/gray walking-path segments and a linked walk-versus-wait breakdown that separates unknown coverage from zero sunlight. For the reachability challenge, we design an underserved-focused travel-time map sharing an eight-class walking scale across fills and routes, using dark casings and red edges for same-color legibility, and defaulting to routes only from difficult tracts (three closest stops) with 60-minute alternatives on click. For dashboard integration, we unify both views in one coordinated web application with shared search, persistent community selection, and a linked demographic scatter plot. Section 2 reviews related work; Section 3 describes the data and spatial unit; Section 4 details the models, visual design, and implementation; Section 5 presents scenarios, results, and validation; and Section 6 discusses limitations and future work.

## 2. Related Work

**Travel-time and accessibility maps.** These maps must first compute times that no dataset records and then encode them without clutter. Network isochrones and departure-dependent transit accessibility established how such times are computed \[2, 3\]. Zeng et al. \[14\] computed public-transit journey times in Singapore from millions of passenger RFID trips, summing riding, waiting, and transfer times in 15-minute bins; to show what a source can reach without the clutter of a geographic flow map, they paired an isochrone map with an isotime flow map that linearizes routes into parallel isolines along a time axis. Andrienko et al. \[15\] *\[TEAM TODO: read the method sections and state in one sentence (a) how they compute place connectedness from GTFS, (b) how they encode it on maps, and (c) which visual problem that solves; the full text was not accessible to us here.\]* Flow-map research addresses link clutter through design principles such as curving and arranging flows and encoding volume as line width \[22\]. Sun / LA faces a different version of the problem: it measures only the walk plus the wait to the first boarding, for every tract rather than from one source, and it must keep real street paths on the map, because the task is which streets connect an underserved tract to its stops. Instead of linearizing or aggregating routes, we show routes only for difficult tracts, merge shared street segments, and keep each route on the fill's own color scale, separated by a dark casing.

**Sun and shade visualization.** Shade likewise has to be computed before it can be shown. SOLWEIG \[4\] models how buildings and vegetation shape shadows and mean radiant temperature. Shadow accrual maps \[9\] address the cost of accumulating city-scale shadows over long time ranges with an efficient accumulation method, so analysts can explore where shade builds up across an area, and the Urban Toolkit \[16\] tackles composing heterogeneous urban layers, including shadow analyses, through a grammar. These systems summarize shade over places and periods. Our problem is a single time-dependent trip: shade must be sampled along a moving walk and a stationary scheduled wait at the departure the user chose, and intervals the model cannot resolve must stay visibly unknown rather than read as shade.

**Walking access and heat at stops.** Empirical transit studies motivate unifying these factors. El-Geneidy et al. \[17\] showed that real walking distances to bus stops differ from conventional 400 m buffers (reporting an 85th-percentile home-based walk of \~524 m), Marisamynathan and Vedagiri \[8\] documented substantial pedestrian signal delays, and Fraser and Chester \[18\] linked transit waits to rider heat vulnerability alongside Los Angeles audits of sparse shelters \[6\] and uneven shade \[11\]. Sun / LA integrates pedestrian routing, signal scenarios, timetables, and sun exposure in one interactive tool.


## 3. Data

We use census tracts (1,120 units) as our spatial unit because tracts are the smallest geographic units with reliable American Community Survey (ACS) estimates—block-group margins of error are substantially larger—allowing the Reachability map and Sun Exposure's ACS demographic scatter plot to align on identical boundaries while preserving intra-city detail. Because readers recognize neighborhood names such as "Tujunga" rather than "Tract 1011.22," we assign community labels from *LA Times Mapping L.A.* boundaries published via LADOT \[20\] based on the neighborhood containing each tract's origin; because 206 tracts share names, tract IDs remain visible alongside the name. Representing each tract by one origin introduces a modifiable areal unit problem (MAUP) trade-off by hiding within-tract variation, particularly in large hillside tracts; 57 poorly connected centroids (including Tujunga, Tract 1011.22) were replaced with audited in-tract street proxies.

Sun Exposure evaluates 7,568 stops and 51,359 connections, while Reachability evaluates 10,968 Metro stops (including stops just outside LA City) using GTFS schedules frozen to October 27, 2026, 4 m aggregated lidar surfaces, and an unverified 1,617-point shelter list. ACS 2020–2024 data \[12\] drives only the Sun Exposure scatter plot, not Reachability difficulty. Both maps render over the OpenFreeMap Positron vector basemap \[21\] (Table 1).

**Table 1. Core inputs and their visualization roles across the dashboard.**

| Source | Data and visualization role | 
| ----- | ----- | 
| OpenStreetMap \[10\] | Directed pedestrian network for walking paths and travel times | 
| Metro GTFS \[5\] | Stops and frozen Oct 27, 2026 weekday schedules for boarding | 
| LADOT Signals \[7\] | Mapped signal locations for modeled crossing delays | 
| ACS 2020–2024 \[12\] | Tract demographics for Sun Exposure scatter plot | 
| NASA WERK Lidar \[13\] | 4 m terrain and surface heights for horizon shade estimates | 
| Mapping L.A. / OpenFreeMap \[20, 21\] | Neighborhood labels (via LADOT) and Positron vector basemap | 

## 4. Design and Implementation

### 4.1 Computing Reachability and Sun Exposure

**Walking, boarding, and difficulty.** For origin $o$ and stop $s$, the fastest cost over directed pedestrian paths $\mathcal{P}_{os}$ is

$$
W_{os}^{\delta} = \min_{p \in \mathcal{P}_{os}} \left( \frac{L_p}{v} + \delta n_p + c_o + c_s \right), \quad (1)
$$


where $L_p$ is path length, $v = 4.8\text{ km/h}$, $n_p$ counts confidently mapped signals, and $c_o, c_s$ estimate endpoint connections, solved via Dijkstra for $\delta = 0$ or $\delta = 30\text{ s}$. For 48 half-hour departures $t$, let $D_s(u)$ be the first permitted GTFS departure after arrival $u$; earliest boarding within the 1,800-second (30-minute) analytical walk search is

$$
B_o(t, \delta) = \min_{s : W_{os}^{\delta} \le 1800} \left[ D_s(t + W_{os}^{\delta}) - t \right]. \quad (2)
$$


An origin is classified as **difficult to board** if $B_o(t, \delta) > 1,800\text{ s}$, or if $\min_s W_{os}^{\delta} > 1,800\text{ s}$ (using the shortest walk as a lower bound); no-service and incomplete-data states remain separate. All boarding metrics, difficulty counts, and reverse catchments strictly use the 30-minute search; 60-minute routes for difficult tracts are display-only.

**Sun exposure.** Combining pvlib solar angles \[1\] with 600 m lidar horizons (4 m resolution), we sample every 6 s walking and up to once per minute waiting:

$$
E_o(t) = E_{\text{walk}}(t) + E_{\text{wait}}(t) = \sum_{k \in K_{\text{known}}(t)} \Delta t_k \mathbf{1}[\text{sun}_k]. \quad (3)
$$


Sun, shade, and unknown remain separate across four seasonal dates. Delays, schedules, and sunlight are modeled exploratory estimates, not field observations or heat/UV doses.

### 4.2 Reachability Map Design

**Computing and mapping travel time.** Walking is the access mode because bus stops are local destinations. We route on walkable OpenStreetMap ways—streets and footways, respecting foot-access and pedestrian one-way tags—and exclude freeways and links closed to pedestrians. Each tract starts from **one geometric interior point** (not population-weighted; 57 moved to audited in-tract street proxies when poorly connected), giving one reproducible, routable origin per tract at the cost of hiding within-tract variation. Because typical bus walks are short (85th-percentile home-based walk $\approx 524\text{ m}$ versus the conventional 400 m buffer \[17\]), we set walk-plus-wait difficulty at a deliberately conservative 30 minutes (6$\times$ the 4.9-minute citywide median walk) and use signal-delay and time-of-day comparisons to test its sensitivity. Eight upper-inclusive walking-time classes (0–5, 5–10, 10–15, 15–20, 20–30, 30–45, 45–60, 60+ min) match the skewed distribution (30 s counts: 579, 366, 80, 33, 33, 20, 7, 2—84% under 10 minutes), place a break at 30 minutes, and display areas beyond the threshold in three upper classes isolating the 29 walk-only difficult tracts. Fills use ColorBrewer sequential YlGnBu classes 2–9 \[19\] (dropping the near-white first class); computed CIE $L^*$ lightness falls monotonically from 95 to 13, suggesting perceptual ordering survives grayscale and color-vision deficiency.

**Same-color routes: problem, options, decision.** Routes must show walking time, but every way of coloring them has a cost. Our first version gave routes their own red scale over blue fills, which read as a second measure. Sharing the fill's eight-class scale fixes the meaning, but a route then disappears where it crosses a tract of its own class, as Tujunga's 29:54 route does over its 20–30-minute fill. We kept the shared scale—each route colored by its own total walking time to its stop, with shared street segments taking the shortest contributing walk—and drew solid routes over translucent fills on a thin dark casing, adding a red outer edge (the color of the `!` flag) to routes from difficult tracts. The edge color was itself an iteration: an initial purple edge sat only about 43° in hue from the 45–60-minute class and blended into long routes, so we replaced it with red, more than 60° from every class, and a unit test enforces that separation. Selected routes widen while keeping their time color.

**Walking versus boarding.** One fill cannot encode two measures, so fills encode *walking time only*, while red `!` markers flag *walk plus wait* over 30 minutes (Fig. 3). The 23 light tracts carrying a `!` at 08:00 are exactly the cases a walking-time map alone would misread.

**Clutter and interaction.** Drawing all \~113,000 routes citywide was unreadable, and straight-line buffers ignore street barriers, so we rejected both. Because the task is origin-first ("how far is a bus from where I am", not "how far does this stop reach"), the default view draws routes only from difficult tracts to their three closest stops; clicking reveals all stops within 60 minutes (difficult) or 30 minutes plus the nearest stop (ordinary), and clicking a stop shows its 30-minute reverse catchment. Layer toggles (*Walk route*, *Bus stops*, *Boarding flags*, *Mapped signal crossings*, *City boundary*) isolate layers. The full-strength Positron basemap keeps street names readable along routes, accepting that labels sit beneath dark fills.


### 4.3 Primary Challenge: Sun Exposure Design

Tract fills use five fixed bins (0–5, 5–10, 10–15, 15–20, 20+ min) grounded in the modeled distribution: at summer 08:00, 69% of tracts fall in 0–5 minutes (median 3.2 min), and at summer noon, 48% lie in 0–5 minutes while 6% exceed 20 minutes. Five-minute steps resolve common low values, the 20+ bin captures the long-wait tail without letting extremes stretch the scale, and fixed breaks keep seasons comparable (88% fall in 0–5 minutes at winter 08:00). The custom yellow-to-red sequential palette decreases monotonically in CIE $L^*$ (95 to 43), suggesting grayscale legibility. On selection, walking segments encode sun in orange and shade in teal—combining warm–cool hue contrast with lightness separation ($L^* \approx 71$ vs. $48$). To keep orange route segments distinct from yellow-to-red tract fills, routes render on a **white halo** over the local shade layer when a neighborhood is selected. Neutral gray marks unknown coverage so missing data is never read as shade, and the legend explicitly distinguishes gray "not enough data" tract fills from gray "unknown" route segments. A linked walk-and-wait breakdown quantifies each component (Fig. 2), while selection-only routes, 10/15/20-minute catchments, and layer toggles manage citywide scale.

**Layout rationale and design iterations.** Both views adopt a map-first layout with a side panel to support overview-first exploration followed by one neighborhood's headline answer, folding secondary details so panels stay uncluttered; separating Sun Exposure and Reachability into two tabs ensures walking minutes and sun minutes never share one color scale. Driven by design critiques and in-class feedback, key iterations included unifying dual travel-time scales, shifting the difficult-route edge from purple to red, upgrading from a washed-out raster basemap and bare tract IDs to Positron and neighborhood names, and moving search into the shared header.

### 4.4 Implementation, Dashboard Integration, and Data Optimization

Sun / LA is a coordinated React web application rendered with deck.gl, MapLibre GL, and D3 (Fig. 1). One parent React component (`ShadeApp`) holds the shared state—selected neighborhood, active tab, and header search—and passes it to the Sun Exposure map, the Reachability panel, and the demographic scatter plot. In the scatter plot, clicking any dot selects that neighborhood on the map, and the active neighborhood highlights as a larger dark dot. Temporal controls remain view-specific because the shade model covers only daylight (06:00–18:00 across four seasons), whereas Reachability spans 00:00–23:30 with a signal-wait toggle. Across the interface, deck.gl draws tract fills, routes, stops, and shade tiles over a MapLibre basemap; D3 supplies scales for the scatter plot while legends are built in React/CSS; and Bootstrap provides light styling for select menus, buttons, and layout controls.

Offline Python preprocessing builds the directed pedestrian graph from OSM (`build_network.py`), parses GTFS timetables (`transit.py`), runs Dijkstra and the boarding search (`reachability/build.py`), computes horizon shade (`build_rasters.py`, `build_exposure.py`), and exports compressed static JSON loaded on demand. The default difficult-route catalog (three closest stops; 1,506/1,520 paths; 396 KB compressed) is capped under 1 MB by the export script, and the neighborhood lookup is 4.5 KB. All precomputed walking routes (55.7 MB in total, including 82,055/82,902 additional 60-minute display routes for the 504/509 tracts that are difficult at some departure) remain outside Git on a checksum-pinned release and deploy as one file per tract: opening Reachability fetches roughly 0.74 MB gzipped (`index.json.gz` 162 KB, `stops.json.gz` 195 KB, `difficulty-routes.json.gz` 387 KB), and clicking a tract downloads only its route file (\~9 KB for Tujunga). During development, an AI coding assistant supported React/TypeScript implementation, debugging, and automated design tests; all analytical framing, data verification, and final paper prose were completed by the authors.

## 5. Usage Scenarios and Results

### 5.1 Primary Challenge: Explaining Walking and Waiting Exposure

At 08:00, median modeled direct sun is 3.22 minutes in summer (1,110 valid origins) and 1.00 minute in winter (1,112 valid origins), representing unpaired descriptive groups. Inspecting **Hollywood (Tract 1902.01)** in summer at 08:00 demonstrates why the linked display is necessary (Fig. 2). The Hollywood tract's citywide fill is only \~1.5 minutes (lightest 0–5-minute bin) because the overview reflects the stop with earliest boarding. Selecting the tract, however, reveals the journey to its *nearest* stop, Yucca/Cahuenga: a 1.3-minute (79 s) walk with 0.85 sunlit minutes followed by a 52.7-minute sunlit wait (53.5 minutes total). This fill-versus-journey contrast provides direct evidence that a fill-only choropleth hides severe wait exposure at a rider's nearest stop, whereas the linked white-halo route and breakdown pinpoint Yucca/Cahuenga as a shelter candidate. A four-task formative user study (4–6 external participants comparing fill-only against linked views), expert feedback from a transit planner, and field shade checks are planned future work, not yet conducted.

### 5.2 Reachability Challenge: Walking Versus Boarding

At 08:00 with 30-second signal waits, **52 of 1,120 origins (4.6%)** are difficult to board: 29 exceed 30 minutes on walking alone, and 23 cross the threshold via scheduled waits, clustering along hillside neighborhoods, the north-west San Fernando Valley, Sunland–Tujunga, and pockets near the harbor (Fig. 3a). Selecting **Tujunga (Tract 1011.22)** at 08:00 shows a 39.0-minute boarding time, a 29:54 fastest walk, and only 2 stops within 30 minutes versus 55 within 60 minutes (Fig. 3b). Because the 29:54 route shares Tujunga's 20–30-minute fill color, the dark casing and red outer edge keep the path legible over the polygon.

Comparing departure hours separates geography from service span: under 30-second signal waits, difficult tracts stay between 44 and 78 during daytime (52 at 08:00, 50 at 17:00), rise to 131 at 23:00, and reach **375 at 03:00**. Since the 29 walk-only tracts are difficult at every hour, 346 of the 375 overnight tracts are wait-driven. Comparing signal scenarios shows threshold sensitivity: adding 30 seconds per mapped signal reduces 30-minute walkable pairs from 113,395 to 112,201. At 03:00, **Palms (Tract 2699.03, near Venice/Motor)** boards in 3.0 minutes (03:03) without penalties, whereas the penalized walk misses that bus, pushing boarding to 47.0 minutes (03:47 at Venice/Helms) and flagging the tract difficult (Fig. 3c); both scenarios board within 8 minutes at 08:00.

**Validation.** A straight-line 400 m buffer map or nearest-stop distance map would misclassify many wait-driven overnight tracts—at 03:00 with signal waits, 96 of the 346 wait-driven difficult tracts lie within a 5-minute walk of a stop and 247 within 10 minutes—whereas our `!` markers expose these schedule failures directly. Model outputs were verified with automated validators (`validate-reachability.mjs`, `validate-shade.mjs`), unit tests, and an independent offline Dijkstra check (Hollywood Tract 1902.01 to Western/Sunset); comparisons against public isochrone/routing tools and field signal timing remain future work.

## 6. Conclusion

Sun / LA separates proximity from pre-boarding access by pairing a fixed-bin sun-exposure choropleth, white-halo orange/teal/gray walk segments, and a walk-versus-wait breakdown with a shared-scale, casing-enhanced, underserved-focused reachability map in one coordinated dashboard.

**Limitations and unfinished work.** Four constraints directly affect what the maps show: (1) one interior origin per tract hides within-tract variation, especially in large hillside tracts such as Tujunga (one of 57 citywide audited street proxies); (2) the flat 30-second penalty on confidently mapped signals makes `!` flags near 30 minutes sensitive to crossing assumptions, as in the Palms tract; (3) one frozen weekday LA Metro schedule omits weekends, real-time delays, and other operators; and (4) clear skies, static vegetation, and unmodeled shelter roofs make sun minutes an upper bound, while deck.gl/MapLibre layer interleaving places labels beneath dark fills. Planned items not completed for this submission include the four-task user study, human color-vision-deficiency checks, field shade and signal validation, and timed browser load benchmarks.

**Future work.** First, to expose within-tract MAUP variation (e.g., Foothill Blvd versus upper hillsides in Tujunga), we plan to load precomputed census block-group route files at neighborhood zoom while keeping the eight-class scale identical and retaining tract outlines. Second, we will expand signal coverage by matching unmatched LADOT signal locations \[7\] to OSM crossings, calibrate crossing delays from field timing or signal phases with a 0/15/30/45 s sensitivity control, and verify the 1,617-point shelter inventory via field checks or street imagery to model roof shade at verified shelters alongside mean radiant temperature \[4\] and additional public transit feeds. Third, we plan to execute the four-task formative study (4–6 non-author participants comparing fill-only against linked views, reporting per-task accuracy and completion time) together with an expert review by a transit planner or shade advocate, and explore a bivariate priority view linking boarding difficulty with high sunny waits.

## Figure Captions (Include Color Keys & What to Look At)

* **Fig. 1.** Whole-app overview of the coordinated Sun / LA dashboard showing (a) *Sun Exposure* and (b) *Reachability* tabs, the shared neighborhood/stop search header, view-specific time controls, and the collapsible side panel with linked demographic scatter plot (clicking a dot selects that tract on the map).

* **Fig. 2.** Primary challenge (*Sun Exposure*): Hollywood (Tract 1902.01) in summer at 08:00. Yellow-to-red tract fills encode total direct-sun minutes to the earliest-boarding stop (five fixed bins: 0–5, 5–10, 10–15, 15–20, 20+ min; gray fill = not enough data). Although the tract's fill sits in the lightest 0–5-minute class (\~1.5 min), selecting it reveals the journey to its nearest stop, Yucca/Cahuenga: white-halo route segments colored orange (sun), teal (shade), and gray (unknown) show a 1.3-minute walk, while the opened walk-and-wait breakdown exposes a 52.7-minute sunlit wait (53.5 min total).

* **Fig. 3.** Reachability challenge: eight-class ColorBrewer YlGnBu fills and routes share one walking-time scale (0–5, 5–10, 10–15, 15–20, 20–30, 30–45, 45–60, 60+ min), while red `!` markers flag walk-plus-wait boarding exceeding 30 minutes. (a) Citywide 08:00 overview (30 s signal scenario) displaying default three-closest routes only for the 52 difficult tracts; light-fill tracts carrying `!` indicate short walks undermined by long waits. (b) Zoomed view of Tujunga (Tract 1011.22, 39.0 min to board, 29:54 fastest walk, 55 stops within 60 min): the dark casing and red outer edge keep the 20–30-minute route legible over its same-class tract fill. (c) Signal-delay comparison for Palms (Tract 2699.03, near Venice/Motor) at 03:00: earliest boarding shifts from 3.0 min (03:03) without signal penalties to 47.0 min (03:47) with 30-second penalties, flipping the tract into the difficult state.

## New References (9 Entries to Add to `mybibliography.bib`)

*(Keep existing \[1\]–\[13\] from `paper.pdf` and add these 9 entries — total 22 references — exporting official BibTeX from Google Scholar / DOIs.)*

14. Zeng, W., Fu, C.-W., Müller Arisona, S., Erath, A., Qu, H.: Visualizing mobility of public transportation system. IEEE Transactions on Visualization and Computer Graphics 20(12), 1833–1842 (2014). https://doi.org/10.1109/TVCG.2014.2346893

15. Andrienko, N., Andrienko, G., Patterson, F., Stange, H.: Visual analysis of place connectedness by public transport. IEEE Transactions on Intelligent Transportation Systems 21(8), 3196–3208 (2020). https://doi.org/10.1109/TITS.2019.2924796

16. Moreira, G., Hosseini, M., Nipu, M.N.A., Lage, M., Ferreira, N., Miranda, F.: The Urban Toolkit: A grammar-based framework for urban visual analytics. IEEE Transactions on Visualization and Computer Graphics 30(1), 1402–1412 (2024). https://doi.org/10.1109/TVCG.2023.3326598

17. El-Geneidy, A., Grimsrud, M., Wasfi, R., Tétreault, P., Surprenant-Legault, J.: New evidence on walking distances to transit stops: identifying redundancies and gaps using variable service areas. Transportation 41(1), 193–210 (2014)

18. Fraser, A.M., Chester, M.V.: Transit system design and vulnerability of riders to heat. Journal of Transport & Health 4, 216–225 (2017). https://doi.org/10.1016/j.jth.2016.07.005

19. Harrower, M., Brewer, C.A.: ColorBrewer.org: An online tool for selecting colour schemes for maps. The Cartographic Journal 40(1), 27–37 (2003). https://doi.org/10.1179/000870403235002042

20. Los Angeles Department of Transportation (LADOT): LA Times Mapping L.A. Neighborhood Boundaries (`VMT_2022_by_LA_Times_Neighborhood_WFL1` ArcGIS Feature Service). Retrieved 2026-11-16. https://services1.arcgis.com/tp9wqSVX1AitKgjd/arcgis/rest/services/VMT_2022_by_LA_Times_Neighborhood_WFL1/FeatureServer/0

21. OpenFreeMap: Positron Vector Basemap. https://openfreemap.org/ (accessed 2026-11-19)

22. Jenny, B., Stephen, D.M., Muehlenhaus, I., Marston, B.E., Sharma, R., Zhang, E., Jenny, H.: Design principles for origin–destination flow maps. Cartography and Geographic Information Science 45(1), 62–75 (2018). https://doi.org/10.1080/15230406.2016.1262280
