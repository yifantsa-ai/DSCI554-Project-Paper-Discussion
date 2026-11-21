# Review: Overleaf `main.pdf` (Nov 21) vs. our Markdown drafts

**Reviewed:** `paper_final/main.pdf` and `main.tex` (commit 6f0959c, "Update revised paper.")
**Compared against:**
- `paper_revision_nov_20_feedback.md`, the final Markdown with the challenge rewrite;
- `archived paper/paper_revision_before_challenges.md`;
- the Nov 20 FEEDBACK, RUBRIC, ASSIGNMENT and PAPER_WRITING_GUIDE;
- the app data in `public/data/reachability/index.json.gz`.

**Method:**
- Recompiled `main.tex` locally; it gives the identical 5 pages.
- Measured how full each page is from the PDF's text boxes.
- Built test variants to find out what each addition costs in lines.

---

## 1. Bottom line

1. **The PDF is not simply the "before challenges" version.** The Introduction already states both challenges in a condensed form of the new §1.2:
   - "exist in no dataset … so the map is custom";
   - "about 113,000 walks converge on shared streets";
   - "walking time and walk-plus-wait are different measures";
   - "a route colored on the fills' scale vanishes on an area of its own class".

   §4 already gives the alternatives we rejected: buffers, drawing all routes, the blue-fill/red-route version, and the purple edge. The challenge framing is mostly there. **The real gaps are in Related Work and in the citations.**
2. **The PDF is completely full.** Pages 1–4 end at the bottom of the text block (y = 666 pt), and page 5 has about 1.5 lines free. Anything added must be paid for with a cut of the same size.
3. **Biggest risk: the data sources are no longer cited.** The pre-submission paper cited OSM, Metro GTFS, LADOT signals, ACS, NASA WERK lidar and pvlib as [1, 5, 7, 10, 12, 13]. The final PDF shows them only as hidden hyperlinks in Table 1 (`hidelinks`), so in print they are not identified at all. The entries are in `mybibliography.bib` but never `\cite`d, so BibTeX drops them. ASSIGNMENT asks us to "cite … your data sources, and your main tools", and the rubric deducts 1 pt for "References missing". This is a regression from a paper that already had them.
4. **Related Work (about 100 words) is thin.** It is well short of what the Nov 20 FEEDBACK asked for: "Say in a sentence each what they do and what your project adds" and "how they compute and encode travel time". The works are named in pairs and contrasted once per pair. Zeng et al.'s computation (smart-card trips, waits and transfers) is missing.

---

## 2. What a 5-page LNCS paper actually holds (the baseline)

### Measured from `main.pdf`

| Measure | Value |
|---|---|
| Body words, including abstract, captions and Table 1 | about 1,640 |
| Words per text line | about 10 |
| Lines per full text page | about 45 |
| Floats | 3 figures (Fig. 1 and Fig. 2 side by side; Fig. 3 full width) and 1 table |
| References | 10, about 2.5 lines each; 2 lines for a short `@misc` without a URL |
| Free space | about 1.5 lines, at the bottom of page 5 |

### Section budgets: the PDF vs. our Markdown

| Section | PDF (words) | Final Markdown (words) | Ratio |
|---|---:|---:|---:|
| Abstract + keywords | 159 | 201 | 1.3× |
| 1 Introduction | 256 | 741 | 2.9× |
| 2 Related Work | 100 | 459 | 4.6× |
| 3 Data (incl. table) | 132 | 291 | 2.2× |
| 4 Design & Implementation | 513 | 1,419 | 2.8× |
| 5 Results | 344 | 478 | 1.4× |
| 6 Conclusion | 135 | 304 | 2.3× |
| **Total body** | **~1,640** | **~3,890** | **2.4×** |

**Takeaway for the Markdown:** a 5-page edition has roughly **1,650 body words in total**. Our Markdown is about 2.4 times that. The Markdown is useful as the full, checked source of facts and wording, but it cannot be ported as written.

---

## 3. What each addition costs (measured by recompiling)

Every variant below was compiled with the team's own `llncs.cls` and `splncs04.bst`.

| Variant | Extra lines | Words to cut elsewhere |
|---|---:|---:|
| A. Related Work expanded to about 195 words (three themed paragraphs, below) | about 8 | about 80 |
| B. Cite the 6 data sources in Table 1, short `@misc` entries without URLs | about 8 | about 80 |
| C. Same as B, but keeping the URL and "accessed" notes | about 15 | about 150 |
| A + B | about 23 | about 230 |
| A + C + Jenny et al. + pvlib | about 34 | about 340 |
| A + B, plus the cuts in §5 below (about 70 words) | about 10 | about 100 more |

**Recommendation:** do **A + B** and pay for it with the cuts in §5 below.
- In variant B, the URLs stay as clickable links in Table 1, so cutting them from the reference list loses little.
- Skip Jenny et al. and pvlib if space runs out. The FEEDBACK minimum of two works per theme is already met without them:
  - Zeng and Andrienko for travel-time maps;
  - Miranda and Moreira for shade;
  - El-Geneidy, Marisamynathan and Fraser for access.

---

## 4. Issues in the PDF, by priority

| # | Priority | Where | Issue | Fix |
|---|---|---|---|---|
| 1 | **High** | Table 1, references | Data sources are not cited; they appear only as invisible links. This is a regression from the pre-submission paper. | Add `\cite{osm}`, `\cite{metrogtfs}`, `\cite{ladotsignals}`, `\cite{census2024}`, `\cite{werk}` and `\cite{mappingla}` in the Source column. The bib entries already exist. |
| 2 | **High** | §2 | Related Work is about 100 words and compares works in pairs. It does not say how Zeng et al. compute travel time, or what each work does and what we add. | Use the paragraph in §6 below (about 195 words). |
| 3 | Medium | §4, last line | "routes (55.7 MB) load one tract file per click" can be read as 55.7 MB per click. This is the same confusion we fixed in the Markdown. | "routes (55.7 MB in all) are split into one file per tract, about 9 KB each, loaded on click." |
| 4 | Medium | §4 | There is no GenAI statement. RUBRIC §3 says "Gen AI, if used, is effectively integrated", and the writing guide asks for a GenAI-use sentence. | Add one honest sentence (about 25 words) on how the AI coding assistant was used. Adapt the last sentence of Markdown §4.4. |
| 5 | Medium | Fig. 1 | Fig. 1 is printed at about 43% of the text width. Its legend and side-panel text cannot be read at 100% zoom. The previous FEEDBACK deducted points for an unreadable Fig. 1. | Its job now is dashboard integration, so the legend matters less. Still, crop tighter to the header, controls and side panel, or let Fig. 1 take more of the row than Fig. 2. |
| 6 | Medium | Fig. 3 | No map legend is visible. The caption names only the two ends of the scale. | Optional: paste the 8-class legend strip into panel (a), or list the classes in the caption (about 1 extra line). |
| 7 | Low | §4, tools | deck.gl, MapLibre, React, D3, Bootstrap, pvlib and the OpenFreeMap basemap are not cited, though ASSIGNMENT asks us to cite "main tools". | Cite only if space allows. pvlib and OpenFreeMap entries already exist in the bib. |
| 8 | Low | Table 1 | After adding cites, "NASA WERK lidar [n]" wraps onto two lines. | Change the column widths to `p{.3\linewidth}p{.65\linewidth}`. |
| 9 | Low | §2 | The Andrienko et al. sentence is generic: "analyze how well transit connects places". | Acceptable while nobody has read the full text. Do not invent a method. |

### Numbers in the PDF, checked against the app data

All of these are correct:
- 52 / 29 / 23 at 08:00;
- 375 / 346 at 03:00;
- 44–78 by day;
- 4.6%;
- Tujunga 39.0 min, 29:54, 2 and 55 stops;
- Palms 3.0 → 47.0 min at 03:03;
- Hollywood 1.5 / 1.3 / 52.7 min;
- 84% under 10 min, and 6× the median walk;
- about 69% and 6% for the sun bins;
- 57 relocated origins;
- about 113,000 walks;
- 24% of stops with shelters.

**One discrepancy, and the PDF is right:** "247 of the 346 are within a 10-minute walk". The data, with signal waits, gives **96 within 5 minutes and 247 within 10**. Without signal waits it gives 97 and 248. The Markdown said "100 … and 248", which mixed the two scenarios. **The Markdown has now been corrected to 96 / 247.**

---

## 5. Where to find the space

These cuts were tested. Together they save about 70 words.

| Cut | Words saved |
|---|---:|
| **Contributions paragraph:** it repeats the abstract almost word for word. Shorten it to "For each challenge we contribute a linked display, described above, and join both in one dashboard", followed by the roadmap sentence. | about 40 |
| **Data paragraph:** move "10,968 Metro stops, frozen to Oct. 27, 2026" and "4 m cells" into the Table 1 rows, and end the paragraph with "Table 1 lists the inputs." | about 15 |
| **Conclusion:** shorten future work to "a four-task study with 4–6 outside participants will compare a fill-only map with the linked view, and street imagery will verify the 1,617-point shelter list." Drop "match the unmatched LADOT signals"; it is already implied by the signal limitation. | about 20 |

After these cuts, A + B was still about 10 lines (about 100 words) over. Candidates for the rest:

| Cut | Words saved |
|---|---:|
| Abstract: drop "It combines pedestrian routing, Metro schedules, assumed signal delays, and lidar-derived shade for" and keep "for 1,120 census-tract origins" in the first sentence | about 15 |
| Fig. 3 caption: drop the "(a)/(b)/(c)" restatements that §5.2 already makes | about 20 |
| §4 Layout: "Python scripts build the pedestrian graph, read timetables, run Dijkstra, compute shade, and export JSON" → "Offline Python scripts precompute all routes and shade as JSON" | about 10 |
| §5.2: "Outputs passed automated validators and an independent Dijkstra check; comparison with public isochrone tools remains." could move to the Conclusion's unfinished-work sentence, merging two statements | about 10 |
| §4 Reachability: "Walks to a bus are usually short [2], so 30 minutes of walk plus wait, six times the median walk, is a deliberately conservative threshold" → drop "deliberately" and "of walk plus wait" (already defined) | about 5 |
| Introduction paragraph 1: drop the three "who …" clauses, since the user groups are already in the abstract | about 25 |

---

## 6. A Related Work section that fits (about 195 words)

**This text was tested in the build.** It keeps every existing citation, adds no new ones, and replaces the current five sentences.

> *Travel-time maps with routes.* Zeng et al. [zeng2014] derive transit journey times, including waits and transfers, from smart-card trips, and avoid geographic flow clutter by pairing an isochrone map with an isotime flow map that straightens routes along a time axis. Andrienko et al. [andrienko2020] summarize how well public transport connects places. These designs start from one source or aggregate routes; we start from every tract, keep real street paths, color them on the fills' scale, and draw them only for difficult tracts.
>
> *Sun and shade.* Shadow accrual maps [miranda2019] make city-scale shadow accumulation over long periods fast enough to explore, and the Urban Toolkit [moreira2024] composes shadow and other urban layers through a grammar. Both summarize shade over areas and periods; we sample it along one trip, a moving walk and a fixed wait at the chosen departure, and keep unresolved intervals gray.
>
> *Walking access and heat.* Real walks to stops differ from the 400 m buffer [elgeneidy2014], signals delay pedestrians [marisamynathan2018], and waits drive riders' heat vulnerability [fraser2017]. Sun / LA shows these factors together.

If one more line is available, add a sentence on flow-map clutter after Andrienko: "and flow-map guidelines [jenny2018] reduce clutter by curving and ordering lines." This needs the Jenny BibTeX entry (in the final Markdown, ref 22) and costs about 3 lines including the reference.

---

## 7. What the Markdown should learn from the PDF

### What the PDF does better, to adopt in the Markdown

1. **One sentence per challenge in the Introduction.** "It is hard to show: about 113,000 walks converge on shared streets above 1,120 tract polygons, walking time and walk-plus-wait are different measures, and a route colored on the fills' scale vanishes on an area of its own class." That one sentence carries what the Markdown's §1.2 says in about 330 words.
2. **The method goes into §4, not the Introduction.** The PDF's Introduction gives what is hard; the computation details (6.7M nodes, 14,492 crossings, the formulas) appear only briefly in §4, or not at all.
3. **No formulas.** Equations (1)–(3) take about 8 lines. The PDF says the same thing in prose: "the minimum, over stops within a 30-minute walk, of the walk plus the wait for the first scheduled departure".
4. **No file names, KB breakdowns, toggle lists or script names.** Examples: `validate-reachability.mjs`, `index.json.gz` at 162 KB, and the five layer-toggle names. None of these are needed to justify a design.
5. **Each design decision is one clause of reasoning.** For example: "the order should survive grayscale and color-vision deficiency". The Markdown's L\* values and hue angles (95→13, 43°, 60°) are good backup for a reviewer's question, but too costly for the 5-page version.
6. **Captions as legends.** Fig. 2's caption carries the whole color key in two lines.

### What only the Markdown has, ranked by value for grading

| Rank | Content (Markdown §) | Why it matters | Approximate cost |
|---|---|---|---|
| 1 | Data and tool citations | Rubric deduction risk; it is a regression | 8 lines |
| 2 | Related Work: per-work "what they do / what we add", Zeng's computation (§2) | Answers a FEEDBACK item directly | 8 lines |
| 3 | GenAI sentence (§4.4) | RUBRIC §3 | 2–3 lines |
| 4 | "Difficult tracts are only 4–7% in daytime, so the map must prioritize them" (§1.2) | Justifies showing routes only for difficult tracts in a single clause | 1 line |
| 5 | Purple edge 43° → red ≥60°, enforced by a unit test (§4.2) | Strong design-process evidence; the PDF has only "resembled the 45–60 class" | 1 line |
| 6 | Hollywood's nearest stop named as Yucca/Cahuenga, as a shelter candidate (§5.1) | Makes the sun result actionable | under 1 line |
| 7 | Origin-first task framing: "how far is a bus from where I am" (§4.2) | Justifies the default view | 1 line |
| — | Formulas, the MAUP block-group plan, sun stop and connection counts, file sizes, script names | Fine for the Markdown and for Q&A, but not worth the space in the 5-page version | — |

### Next steps for the Markdown

1. Keep `paper_revision_nov_20_feedback.md` as the **long-form source of record**, with every number checked.
2. Add a **"5-page edition"** of the Markdown that follows the budgets in §2: Introduction about 250 words, Related Work about 195, Design about 500. Use the PDF as its skeleton, and port in only ranks 1–7 above.
3. Fix the Andrienko TODO in the Markdown the same way as the PDF: one generic, accurate clause. Do not describe a method that nobody has read.
