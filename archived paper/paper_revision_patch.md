# Sun / LA — Revision Patch (`revision_patch.md`)

## Pre-Overleaf Checklist (Team Action Required)

1. **Rewrite in your own words (course rule):** Use the patched sections below as a content and length blueprint; draft the final Overleaf prose in your team's own words and update the GenAI disclosure in §4.4 to match what was actually done.
2. **Verify locally before finalizing numbers/claims:**
   - Re-measure the initial Reachability transfer size in Chrome DevTools (Network tab, cache disabled) before writing "measured" in §4.4.
   - Run Chrome DevTools → Rendering → *Emulate vision deficiencies* on both maps if you want to claim verified CVD checks in §4.2–4.3 (otherwise keep the hedged "suggests" phrasing below).
   - Check the 08:00 Reachability overview on the live site if you want to name additional difficult-tract neighborhoods beyond Tujunga in §5.2.
   - Export official BibTeX entries from Google Scholar / DOIs for all new references, and confirm public feed availability before naming any non-Metro operator in §6.
3. **Figures & repo hygiene:** Build full-width Figs. 1–3 and paste the text + figure placeholders into Overleaf to verify the 5-page LNCS limit. Add `Claude outputs/` to `.gitignore` (or move it out of the repo) before final submission.

---

## Title, Abstract, and Keywords

### Title
Sun / LA: Visualizing Sun Exposure and Underserved Bus-Boarding Reachability in Los Angeles

### Abstract (~165 words)
Sun / LA is an interactive geovisualization dashboard built for transit planners, shade advocates, and Los Angeles riders to examine why a nearby bus stop can still be hard to board or heavily exposed to direct sunlight. Integrating a directed pedestrian network, Metro schedules, estimated signal-crossing delays, and lidar-derived shade across 1,120 census tract origins, the system addresses two visualization challenges. First, a standard sun-exposure choropleth cannot show whether sunlight accumulates while walking or waiting, nor separate missing shade data from shade; we pair fixed-bin tract fills with orange/teal/gray route segments and a linked walk-and-wait breakdown. Second, keeping street-level routes legible over a fine-unit choropleth while distinguishing walking time from boarding difficulty—defined as walk plus scheduled wait exceeding 30 minutes—requires coordinated encodings; our reachability map shares one eight-class walking-time scale across fills and routes, uses dark casings and red edges for contrast, and shows default routes only for underserved tracts. With an assumed 30-second signal penalty, reachable 30-minute walk pairs fall from 113,395 to 112,201; at 08:00, 52 of 1,120 origins (4.6%) are difficult to board.

### Keywords
Transit accessibility · Choropleth · Flow and route maps · Underserved areas · Sun exposure · Geovisualization

---

## 1. Introduction (~340 words total)

### 1.1 Purpose, Users, and Scope
A bus-stop pin cannot show whether a rider can walk there, board promptly, or avoid direct sunlight. Sun / LA serves three audiences with distinct decisions: transit planners identify neighborhoods where walk-plus-wait access exceeds 30 minutes to separate stop gaps from service gaps; shade advocates and city staff determine whether exposure occurs while walking or waiting to prioritize shelters versus street trees; and riders search their neighborhood to compare local conditions against nearby tracts. For example, opening Reachability at 08:00 flags Tujunga (Tract 1011.22) at 39.0 minutes to board with a 29:54 fastest walk, signaling a stop or routing gap rather than a wait alone. Los Angeles is an urgent testbed: LA Metro reported shelters at only 24% of regionally served stops in 2022 [6], and UCLA documented uneven citywide shade [11]. We scope origins to the City of Los Angeles because municipal signal, lidar, and shelter inventories are consistent within city borders, while candidate stops may lie just outside because pedestrians cross city boundaries.

### 1.2 Reachability Visualization Challenge
Visualizing citywide reachability poses two visualization challenges: keeping a fine-unit choropleth and a route layer legible together, and comparing two reachability sets (with versus without signal waits) across departure times. Drawing all ~113,000 walkable origin–stop pairs over 1,120 tract polygons creates severe clutter as paths converge on shared streets. Moreover, encoding walking time and walk-plus-wait boarding on one map creates visual conflicts: at 08:00 with signal waits, 23 of the 52 difficult tracts have walks under 30 minutes, appearing light on the fill yet failing on boarding. When routes share the fill's color scale, paths crossing same-class polygons disappear—Tujunga's 29:54 walk matches its own 20–30-minute fill and would vanish without a contrasting casing. Because difficult tracts represent only about 4–7% of the city during daytime hours (44–78 tracts; 52, or 4.6%, at 08:00), they are easily lost unless visually prioritized.

### 1.3 Primary Visualization Challenge: Sun Exposure
Pre-boarding sun exposure shifts with solar geometry and building shadows across 1,120 origins, four seasons, and 25 daytime departures (06:00–18:00). A standard choropleth fails because one tract fill cannot show where sunlight occurs, whether walking or waiting dominates, or how different candidate stops compare. In summer at 08:00, a Hollywood tract (Tract 1902.01) displays a light citywide fill of roughly 1.5 minutes because the overview uses the stop with earliest boarding; selecting the tract reveals the journey to its nearest stop, Yucca/Cahuenga—a 1.3-minute walk followed by a 52.7-minute sunlit wait (53.5 minutes total). Because the fill and selected journey can refer to different stops, the fill alone hides severe wait exposure. Our challenge is to preserve citywide comparability while decomposing individual trips into walking versus waiting sunlight and keeping missing shade coverage distinct from shade.

### Contributions and Roadmap
Our contributions directly answer these challenges: (1) for sun exposure, a fixed-bin choropleth linked to orange/teal/gray route segments and a walk-versus-wait breakdown that separates unknown coverage from zero sun; (2) for reachability, an underserved-focused travel-time map sharing an eight-class walking scale across fills and routes, using dark casings and red edges for legibility, and defaulting to routes only from difficult tracts (three closest stops) with 60-minute alternatives on click; and (3) one coordinated dashboard unifying both views through shared search and persistent community selection. Section 2 reviews related work; Section 3 describes the data and spatial unit; Section 4 details the models, visual design, and optimization; Section 5 presents scenarios, results, and validation; and Section 6 discusses limitations and future work.

---

## 2. Related Work (~215 words)

Prior accessibility work established network isochrones and departure-dependent transit surfaces [2, 3]. In GIS visualization, O'Sullivan et al. [14] generated desktop isochrones around scheduled public transport nodes, while origin–destination flow-map research addresses link clutter by smoothing geometry and encoding aggregate volume via line width [15]. However, isochrones typically expand outward from a single destination rather than flagging residential origins that fail on walk plus wait, and aggregated flows obscure real street paths. Our origin-first choropleth fills this gap by separating walk-plus-wait difficulty from walking time while keeping individual, street-faithful routes legible over same-scale fills through selective default display, edge deduplication, and dark casings.

For urban shade, microclimate models like SOLWEIG [4] simulate shadows and mean radiant temperature. Visualization systems such as shadow accrual maps [9] and the Urban Toolkit [16] accumulate city-scale building shadows and support sunlight what-if analyses across static areas and time ranges. Unlike area-wide summaries, Sun / LA follows a single time-dependent access trip—combining a moving walk with a stationary scheduled wait—while encoding missing shade data distinctly from shade.

Empirical transit studies motivate integrating these factors. El-Geneidy et al. [17] showed that real walking distances to bus stops differ from conventional 400 m buffers (with an 85th-percentile home-based walk of ~524 m), Marisamynathan and Vedagiri [8] documented substantial pedestrian signal delays, and Fraser and Chester [18] linked transit waits to rider heat vulnerability alongside LA audits of sparse shelters [6] and uneven shade [11]. Sun / LA unifies these factors in one interactive visual tool.

---

## 3. Data (~145 words + Table 1)

We use the census tract as our spatial unit because American Community Survey (ACS) indicators are published by tract—aligning the Reachability map with the demographic scatterplot in Sun Exposure—while 1,120 units capture intra-city structure in a familiar planning geography. Representing each tract by one origin introduces a modifiable areal unit problem (MAUP) trade-off by hiding within-tract variation, especially in large hillside tracts; to mitigate poor centroid connectivity, 57 origins (including Tujunga, Tract 1011.22) were replaced with audited in-tract street proxies.

Sun Exposure evaluates 7,568 stops and 51,359 connections; Reachability evaluates 10,968 Metro stops, including stops just outside LA City. Metro GTFS schedules are frozen to October 27, 2026, lidar is aggregated to 4 m cells, and a 1,617-point shelter list remains unverified. ACS 2020–2024 data [12] now drives only the Sun Exposure demographic scatterplot, not Reachability difficulty. Community labels come from *LA Times Mapping L.A.* boundaries published via LADOT [20], assigned by the neighborhood containing each tract's origin; because 206 tracts share names, tract IDs remain visible. Both maps use the OpenFreeMap Positron vector basemap [21] (Table 1).

**Table 1. Core inputs and their visualization roles across the dashboard.**

| Source | Data and visualization role |
| --- | --- |
| OpenStreetMap [10] | Directed pedestrian network for walking paths and travel times |
| Metro GTFS [5] | Stops and frozen Oct 27, 2026 weekday schedules for boarding |
| LADOT Signals [7] | Mapped signal locations for modeled crossing delays |
| ACS 2020–2024 [12] | Tract demographics for Sun Exposure scatterplot |
| NASA WERK Lidar [13] | 4 m terrain and surface heights for horizon shade estimates |
| Mapping L.A. / OpenFreeMap [20, 21] | Neighborhood labels (via LADOT) and Positron vector basemap |

---

## 4. Design and Implementation

### 4.1 Computing Reachability and Sun Exposure (~145 words + Eqs. 1–3)
**Walking, boarding, and difficulty.** For origin $o$ and stop $s$, the fastest cost over directed pedestrian paths $\mathcal{P}_{os}$ is
$$W_{os}^{\delta} = \min_{p \in \mathcal{P}_{os}} \left( \frac{L_p}{v} + \delta n_p + c_o + c_s \right), \quad (1)$$
where $L_p$ is path length, $v = 4.8\text{ km/h}$, $n_p$ counts mapped signals, and $c_o, c_s$ estimate endpoint connections, solved with Dijkstra for $\delta = 0$ or $\delta = 30\text{ s}$. For 48 half-hour departures $t$, let $D_s(u)$ be the first permitted GTFS departure after arrival $u$; earliest boarding within the 1,800-second (30-minute) analytical walk search is
$$B_o(t, \delta) = \min_{s : W_{os}^{\delta} \le 1800} \left[ D_s(t + W_{os}^{\delta}) - t \right]. \quad (2)$$
An origin is classified as **difficult to board** if $B_o(t, \delta) > 1,800\text{ s}$, or if $\min_s W_{os}^{\delta} > 1,800\text{ s}$ (using the shortest walk as a lower bound); no-service and incomplete-data states remain separate. All boarding metrics, difficulty counts, and reverse catchments strictly use the 30-minute search; 60-minute routes for difficult tracts are display-only.

**Sun exposure.** Combining pvlib solar angles [1] with 600 m lidar horizons (4 m resolution), we sample every 6 s walking and up to once per minute waiting:
$$E_o(t) = E_{\text{walk}}(t) + E_{\text{wait}}(t) = \sum_{k \in K_{\text{known}}(t)} \Delta t_k \mathbf{1}[\text{sun}_k]. \quad (3)$$
Sun, shade, and unknown remain separate across four seasonal dates. Delays, schedules, and sunlight are modeled exploratory estimates, not field observations or heat/UV doses.

### 4.2 Reachability Map Design (~285 words)
**Mode, threshold, and classes.** Walking fits local stop access. Because empirical evidence shows typical bus walks are short (85th-percentile home-based walk $\approx 524\text{ m}$ versus the conventional 400 m buffer [17]), we set walk-plus-wait difficulty at a deliberately conservative 30 minutes (6$\times$ the 4.9-minute citywide median walk), using signal-delay and time-of-day comparisons to test sensitivity and 60-minute click routes to show farther options. Eight upper-inclusive walking-time classes (0–5, 5–10, 10–15, 15–20, 20–30, 30–45, 45–60, 60+ min) align with the data distribution (30 s scenario counts: 579, 366, 80, 33, 33, 20, 7, 2—84% under 10 minutes), place a break exactly at 30 minutes, and isolate the 29 walk-only difficult tracts. Fills use ColorBrewer sequential YlGnBu classes 2–9 [19] (dropping the near-white first class); computed CIE $L^*$ lightness decreases monotonically from 95 to 13, which suggests perceptual ordering survives grayscale and color-vision deficiency.

**Encoding split and route legibility.** Tract fills encode *walking time only*, whereas red `!` markers flag *walk plus wait* over 30 minutes, making light-fill, long-wait tracts immediately visible (Fig. 3). Routes share the same eight-class scale so color consistently means walking minutes. To keep routes readable over same-class fills, solid paths render over translucent fills with a thin dark casing, plus a red outer edge (matching `!`) on difficult-tract routes; selected routes widen while keeping their time color. Clicking a stop displays its 30-minute reverse catchment to show directional asymmetry.

**Overplotting and design alternatives.** Default display shows routes only from difficult tracts to their three closest stops; clicking reveals all stops within 60 minutes (difficult) or 30 minutes plus the nearest stop (ordinary). Shared edges are deduplicated by shortest walk, and layer toggles isolate layers. Unlike aggregated flow maps [15] or stop-first isochrones [14], keeping street-faithful paths over an origin-first choropleth directly answers which real streets connect an underserved neighborhood to stops. Documented rejected alternatives include straight-line buffers (ignoring street barriers), drawing all ~113k routes citywide (unreadable clutter), an earlier two-scale blue-fill/red-route design (misread as two metrics), and an earlier purple difficult-route edge whose hue sat too close ($\approx 43^\circ$) to the 45–60-minute class (replaced by red with $>60^\circ$ separation, enforced by a unit test). The full-strength Positron basemap preserves street names along routes, accepting the trade-off that labels sit under dark fills.

### 4.3 Primary Challenge: Sun Exposure Design (~145 words)
Tract fills use five fixed bins (0–5, 5–10, 10–15, 15–20, 20+ min) so seasons and hours stay directly comparable without masking seasonal shifts. The custom yellow-to-red sequential palette decreases monotonically in CIE $L^*$ (95 to 43), suggesting grayscale legibility. On selection, walking segments use orange for sun and teal for shade—combining warm–cool hue contrast with lightness separation ($L^* \approx 71$ vs. $48$)—while neutral gray marks unknown intervals so missing data is never read as shade. A linked walk-and-wait breakdown quantifies each component (Fig. 2), while selection-only routes, 10/15/20-minute catchments, and layer toggles manage citywide scale. Driven by design critiques and in-class feedback, key cross-app iterations included unifying the dual travel-time scales and shifting the difficult edge from purple to red, replacing a washed-out raster basemap and bare tract IDs with full-strength Positron and neighborhood names, and moving search into the shared header while collapsing the side panel to one headline metric.

### 4.4 Implementation, Dashboard Integration, and Data Optimization (~130 words)
Sun / LA is a coordinated React and Bootstrap dashboard rendered with deck.gl, MapLibre GL, and D3 (Fig. 1). Two top-level tabs switch between *Sun Exposure* and *Reachability* while sharing a header search (neighborhood, tract, or stop) and persistent community selection. Time controls stay view-specific because the shade model covers only daylight (06:00–18:00 across four seasons), whereas Reachability spans 00:00–23:30 with a signal-wait toggle.

All spatial data is precomputed into static compressed assets loaded on demand. The default difficult-route catalog (three closest stops; 1,506/1,520 paths; 396 KB compressed) is capped under 1 MB by the export script, and the neighborhood lookup is 4.5 KB. All precomputed walking routes (55.7 MB in total, including 82,055/82,902 additional 60-minute display routes for the 504/509 tracts that are difficult at some departure) remain outside Git on a checksum-pinned release and deploy as one file per tract: opening Reachability loads ~0.74 MB gzipped (`index.json.gz` 162 KB, `stops.json.gz` 195 KB, `difficulty-routes.json.gz` 387 KB), and clicking a tract fetches only its route file (~9 KB for Tujunga). *[GenAI disclosure — edit to match actual team process: An AI coding assistant supported React/TypeScript implementation, debugging, and automated design tests; all analysis, data verification, and final paper writing were completed by the authors.]*

---

## 5. Usage Scenarios and Results

### 5.1 Primary Challenge: Explaining Walking and Waiting Exposure (~120 words)
At 08:00, median modeled direct sun is 3.22 minutes in summer (1,110 valid origins) and 1.00 minute in winter (1,112 valid origins), representing unpaired groups. Inspecting **Hollywood (Tract 1902.01)** in summer at 08:00 illustrates why the linked display is necessary (Fig. 2). The Hollywood tract's citywide fill is only ~1.5 minutes (lightest 0–5-minute bin) because the overview reflects the stop with earliest boarding. Selecting the tract, however, reveals the journey to its *nearest* stop, Yucca/Cahuenga: a 1.3-minute (79 s) walk with 0.85 sunlit minutes followed by a 52.7-minute sunlit wait (53.5 minutes total). This fill-versus-journey contrast provides direct evidence that a fill-only choropleth hides severe wait exposure at a rider's nearest stop, whereas the linked route and breakdown pinpoint Yucca/Cahuenga as a shelter candidate. A four-task formative user study (4–6 external participants comparing fill-only against linked views) and field shade checks are planned future work, not yet conducted.

### 5.2 Reachability Challenge: Walking Versus Boarding (~190 words)
At 08:00 with 30-second signal waits, **52 of 1,120 origins (4.6%)** are difficult to board: 29 exceed 30 minutes on walking alone, and 23 cross the threshold via scheduled waits, clustering largely along hillside and northern Valley edges (Fig. 3a). Selecting **Tujunga (Tract 1011.22)** at 08:00 shows a 39.0-minute boarding time, a 29:54 fastest walk, and only 2 stops within 30 minutes versus 55 within 60 minutes (Fig. 3b). Because the 29:54 route shares Tujunga's 20–30-minute fill color, the dark casing and red outer edge keep the path legible over the polygon.

Comparing hours separates geography from service span: under 30-second signal waits, difficult tracts remain between 44 and 78 during daytime (52 at 08:00, 50 at 17:00), climb to 131 at 23:00, and reach **375 at 03:00**. Since the 29 walk-only tracts are difficult at every hour, 346 of the 375 overnight tracts are wait-driven. Signal sensitivity is illustrated by **Palms (Tract 2699.03, near Venice/Motor)** at 03:00 (Fig. 3c): a 174 s unpenalized walk boards at 03:03, whereas a 193 s penalized walk misses that bus, shifting boarding to 03:47 at Venice/Helms and flagging the Palms tract difficult (both board within 8 minutes at 08:00).

**Validation.** Simple proximity or nearest-stop distance maps would misclassify many wait-driven overnight tracts—at 03:00, 100 of the 346 wait-driven difficult tracts lie within a 5-minute walk of a stop and 248 within 10 minutes—whereas our `!` markers expose these schedule failures. Model outputs were checked via automated validators (`validate-reachability.mjs`, `validate-shade.mjs`), unit tests, and an independent offline Dijkstra check (Hollywood Tract 1902.01 to Western/Sunset); public-router spot-checks and field signal timing remain future work.

---

## 6. Conclusion (~175 words)

Sun / LA separates proximity from pre-boarding access by pairing a fixed-bin sun-exposure choropleth, orange/teal/gray walk segments, and a walk-versus-wait breakdown with a shared-scale, casing-enhanced, underserved-focused reachability map in one coordinated dashboard.

**Limitations and unfinished work.** Four constraints directly shape what the maps display: (1) one origin per tract hides within-tract variation, especially in large hillside tracts such as Tujunga (one of 57 citywide audited street proxies); (2) the flat 30-second signal penalty makes `!` flags near 30 minutes sensitive to crossing assumptions, as in the Palms tract; (3) a single frozen weekday LA Metro schedule omits weekends, real-time delays, and other operators; and (4) clear skies, static vegetation, and unmodeled shelter roofs make sun minutes an upper bound, while deck.gl/MapLibre layer interleaving places labels beneath dark fills. Planned items not completed for this submission include the four-task user study, human color-vision-deficiency testing, field shade and signal validation, and timed browser load benchmarks.

**Future work.** First, we plan to load precomputed census block-group route files at neighborhood zoom—keeping the eight-class scale identical and retaining tract outlines—to expose within-tract MAUP variation (e.g., Foothill Blvd versus upper hillsides in Tujunga). Second, we can calibrate crossing delays from field timing or signal phases with a multi-step sensitivity control (0/15/30/45 s), and incorporate additional public transit feeds, weekend/GTFS-Realtime wait ranges, verified shelter roofs, and mean radiant temperature [4]. Third, we plan a bivariate priority view highlighting tracts that are both difficult to board and sun-exposed, followed by the four-task user study and an expert review reporting task accuracy and completion times.

---

## Trimmed New References (8 Entries to Add to `mybibliography.bib`)

*(Keep existing [1]–[13] from `paper.pdf` and add these 8 entries — total 21 references — pulling official BibTeX from Google Scholar / DOIs.)*

14. O'Sullivan, D., Morrison, A., Shearer, J.: Using desktop GIS for the investigation of accessibility by public transport: an isochrone approach. International Journal of Geographical Information Science 14(1), 85–104 (2000). https://doi.org/10.1080/136588100240976
15. Jenny, B., Stephen, D.M., Muehlenhaus, I., Marston, B.E., Sharma, R., Zhang, E., Jenny, H.: Design principles for origin–destination flow maps. Cartography and Geographic Information Science 45(1), 62–75 (2018). https://doi.org/10.1080/15230406.2016.1262280
16. Moreira, G., Hosseini, M., Nipu, M.N.A., Lage, M., Ferreira, N., Miranda, F.: The Urban Toolkit: A grammar-based framework for urban visual analytics. IEEE Transactions on Visualization and Computer Graphics 30(1), 1402–1412 (2024). https://doi.org/10.1109/TVCG.2023.3326598
17. El-Geneidy, A., Grimsrud, M., Wasfi, R., Tétreault, P., Surprenant-Legault, J.: New evidence on walking distances to transit stops: identifying redundancies and gaps using variable service areas. Transportation 41(1), 193–210 (2014)
18. Fraser, A.M., Chester, M.V.: Transit system design and vulnerability of riders to heat. Journal of Transport & Health 4, 216–225 (2017). https://doi.org/10.1016/j.jth.2016.07.005
19. Harrower, M., Brewer, C.A.: ColorBrewer.org: An online tool for selecting colour schemes for maps. The Cartographic Journal 40(1), 27–37 (2003). https://doi.org/10.1179/000870403235002042
20. Los Angeles Department of Transportation (LADOT): LA Times Mapping L.A. Neighborhood Boundaries (`VMT_2022_by_LA_Times_Neighborhood_WFL1` ArcGIS Feature Service). Retrieved 2026-11-16 https://services1.arcgis.com/tp9wqSVX1AitKgjd/arcgis/rest/services/VMT_2022_by_LA_Times_Neighborhood_WFL1/FeatureServer/0
21. OpenFreeMap: Positron Vector Basemap. https://openfreemap.org/ (accessed 2026-11-19)