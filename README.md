# Sun / LA: Paper Revision Workspace (DSCI 554, Group 4)

This repository holds the working documents behind the final paper for **Sun / LA**, the DSCI 554 final project by Group 4: Tianyi Wang, Yi-Fan Tsai, Shuang Wu and Yang Xu.

- Live app: https://dsci-554.github.io/project-20263-team-4/
- The app code lives in the main project repo, not here. This repo only keeps the paper drafts, the reviews of those drafts, and the figure material.

The app has two views:

- **Sun exposure:** modeled direct-sun minutes for the walk to a bus stop plus the scheduled wait.
- **Reachability:** walking time to bus stops, with a `!` flag where walk + wait is over 30 minutes.

---

## TL;DR: which file is the final paper?

**`paper_5page_edition.md`**, in the root of this repo, is the final edition. It is the text that goes into Overleaf, and it has been compiled and verified to fit the 5-page LNCS limit.

Everything in `archived paper/` is an older version kept for history. That includes `paper_revision_nov_20_feedback.md`, the long-form v4, which is still the full, number-checked source of record. See [Why this is the final edition](#why-this-is-the-final-edition).

The team's compiled Overleaf PDF will be added to this repo later.

---

## Folder structure

```
.
├── README.md                              ← this file
├── paper_5page_edition.md                 ← FINAL paper text (v5, fits 5 LNCS pages)
├── WHAT_TO_DO_NEXT.md                     ← team checklist of remaining work
├── latex_check/                           ← the build that proves v5 fits in 5 pages
│   ├── main_5page.tex                     (v5 + v6 changes as LaTeX; uses figures/fig1–3.png)
│   ├── figures/                           (fig1–3.png; fig1 and fig3 rebuilt in v6)
│   ├── mybibliography_5page.bib           (team bib + short data-source entries)
│   └── main_5page_check.pdf               (compiled check, 5 pages; not the submission PDF)
├── archived paper/                        ← superseded drafts, oldest → newest
│   ├── tentative_paper_revision.md        (v1)
│   ├── paper_revision_patch.md            (v2)
│   ├── paper_revision_before_challenges.md (v3)
│   └── paper_revision_nov_20_feedback.md  (v4, long form; source of record)
├── feedbacks/                             ← reviews of the drafts (not paper text)
│   ├── Paper_Revision_Guide.md
│   ├── revision_feedback.md
│   ├── patch_vs_feedback_Nov20.md
│   ├── overleaf_pdf_review_Nov21.md
│   └── 5page_vs_FEEDBACK_Nov21.md
├── figure_references/                     ← original screenshots + how to take them
│   ├── FIGURE_SHOT_LIST.md
│   └── Fig 1(a).jpeg … Fig 3(c2).jpeg     (8 files)
└── figure_cropped/                        ← paper-ready crops of the screenshots
    └── Fig_1a.png … Fig_3c2.png           (8 files)
```

---

## Paper version history

All dates are Pacific time, November 2026. Word counts are for the whole file, including captions, references and editing notes, not just the body text.

| # | File | Date | Words | Status |
|---|------|------|------:|--------|
| v1 | `archived paper/tentative_paper_revision.md` | Nov 19 | ~6,700 | Archived: the team's first full draft |
| v2 | `archived paper/paper_revision_patch.md` | Nov 19 | ~3,500 | Archived: trimmed and corrected |
| v3 | `archived paper/paper_revision_before_challenges.md` | Nov 20 | ~4,100 | Archived: answers the Nov 20 feedback |
| v4 | `archived paper/paper_revision_nov_20_feedback.md` | Nov 20 (evening) | ~4,500 | Archived: long form with the challenge rewrite; source of record |
| **v5** | **`paper_5page_edition.md`** | **Nov 21** | **~1,550 body** | **FINAL: fits 5 pages** |

### v1: `tentative_paper_revision.md` (first full draft)

This was the team's first rewrite of the pre-submission paper, following `feedbacks/Paper_Revision_Guide.md`. It covered every section, but it had three blocking problems, which `feedbacks/revision_feedback.md` reviews:

- It was far over the 5-page LNCS limit.
- Some passages needed rewording into the team's own words.
- It made factual mistakes:
  - It described tract 1902.01's fill as the darkest class. The fill is actually about 1.5 min; 53.5 min is the nearest-stop journey.
  - It said the browser downloads 55.7 MB. That figure is the total size of all route files on disk. The browser downloads about 0.74 MB when Reachability opens, and about 9 KB per tract clicked.

### v2: `paper_revision_patch.md` (trimmed and fixed)

- Cut to roughly 2,900 body words.
- Fixed the 1902.01 and 55.7 MB errors.
- Used neighborhood names instead of bare tract numbers.
- Reworded §4.4 (implementation and data optimization).
- Checked all numbers against the app's data files (`public/data/reachability/index.json.gz` and the shade summaries).

### v3: `paper_revision_before_challenges.md` (answers the Nov 20 FEEDBACK)

This version answers the professor's Nov 20 FEEDBACK. `feedbacks/patch_vs_feedback_Nov20.md` checks it item by item.

- Added Zeng et al. and Andrienko et al. to Related Work.
- Explained why we use the walking network and why routes start at tract centroids.
- Added implementation details: Dijkstra on the directed OSM walking graph (about 6.7M nodes) at 4.8 km/h. Mapped crossings (14,492) add 0 or 30 s of signal wait. Boarding uses the next GTFS bus on a frozen weekday, Oct 27, 2026, including after-midnight trips.
- Expanded the figure captions with color keys and what to look at.
- Fixed two passages that had **slipped back** to wrong text during editing:
  - The 55.7 MB sentence now says it is the total size of the precomputed routes, not what the browser downloads.
  - Ref 20 had a placeholder; it now has the real LA Times / ArcGIS VMT URL.

### v4: `paper_revision_nov_20_feedback.md` (long form, now archived)

Built from v3. The change is the **challenge rewrite**: v3 still did not clearly say *what is hard* or *how each related work deals with its own version of the problem*.

- **§1.2: Reachability challenge.** The section now names the two hard parts:
  - computing walk + wait times for every tract at city scale;
  - the route-color dilemma: routes and area fills share one color scale, so routes can disappear into their background.
- **§2: Related Work.** Rewritten as three labeled paragraphs. Each cited work follows the same pattern: *their challenge → their solution → how ours differs.*
  - Zeng et al. is now described from the paper itself.
- **§4.2: Reachability map design.** Reordered into four steps:
  1. Computing and mapping travel time
  2. Same-color routes: the problem, the options, and the decision (dark casing plus a red edge for difficult tracts)
  3. Walking versus boarding
  4. Clutter and interaction
  - The "arterial roadways" wording was also corrected.
- **References.** Added Jenny et al. as ref 22. The list now has 9 new entries, 22 in total.
- Kept the v3 regression fixes (the 55.7 MB sentence and the ref 20 URL).
- **Problem:** at about 3,900 body words it is roughly 2.4 times what 5 LNCS pages hold. It was kept as the complete, number-checked source, and archived once v5 replaced it.
- **Correction on Nov 21:** "100 … and 248 within 10 minutes" was changed to "96 … and 247". The data with signal waits gives 96 and 247; 248 is the no-signal-wait figure.

### v5: `paper_5page_edition.md` (FINAL)

The team's own Overleaf paper (`main.pdf`, 5 pages, about 1,640 body words) showed what actually fits. `feedbacks/overleaf_pdf_review_Nov21.md` reviewed that PDF against v4 and measured, by recompiling, how many lines each addition costs.

v5 uses the Overleaf text as the skeleton and ports in only the most valuable v4 content:

- **Cites the six data sources** in Table 1 and §3. The Overleaf PDF had dropped them; they appeared only as hidden links.
- **Related Work** grows to three themed paragraphs, about 160 words. They cover how Zeng et al. compute travel time (smart-card trips, waits and transfers) and how they show it (an isochrone map paired with an isotime flow map), with one "we instead…" contrast per theme.
- **Added from v4:**
  - the origin-first task framing;
  - the Yucca/Cahuenga stop in the Hollywood example;
  - a future-work plan for the missing signals (match LADOT signals to OSM crossings), shelter roofs (street-imagery check) and a transit planner's review.
- **New sentences:**
  - a GenAI statement, which the team needs to confirm;
  - a clearer 55.7 MB sentence: "55.7 MB in all … about 9 KB each, loaded on click".
- **Cut** to pay for the additions:
  - table details moved into Table 1;
  - shorter captions and design sentences;
  - the limitations condensed into one sentence.
- **Kept on purpose**, because each one answers a FEEDBACK item: the abstract's input list (the abstract must stay at 150 words or more; it is about 154), the per-challenge Contributions sentence, riders as a user group, "no stock map shows it", and the preprocessing steps.
- **Small LaTeX changes:**
  - the Fig. 1/2 float sits in §4, next to its first reference, so text fills page 3;
  - `\raggedbottom`;
  - wider Table 1 columns;
  - short `@misc` entries for the data sources (their URLs stay as links in Table 1).

The full list of changes is at the end of `paper_5page_edition.md`.

**v6 update (Nov 22, same file):** Fig. 1 rebuilt as larger excerpts, the legend strip added to Fig. 3, Jenny et al. added to Related Work, and the "4–7% by day" clause restored, paid for with shorter wording. Still 5 pages. See *Changes in v6* at the end of `paper_5page_edition.md`.

---

## Why this is the final edition

`paper_5page_edition.md` is the final edition because it is the only version that meets all of the following:

1. **It fits the 5-page LNCS limit.**
   - `latex_check/main_5page.tex` compiles with the team's `llncs.cls` and `splncs04.bst` to exactly 5 pages.
   - That includes 3 figures, Table 1 and 17 references (v6 adds Jenny et al.).
   - Every version from v1 to v4 is 2 to 4 times too long.
2. **It answers the Nov 20 FEEDBACK within that space.**
   - Users and contributions are stated.
   - Both challenges are named, with what makes them hard.
   - Related Work says what each work does and what we add.
   - Units, network, threshold, bins and colors are justified.
   - The same-color route conflict and its fix are explained.
   - Alternatives that were rejected are listed.
   - Dashboard integration is described.
   - Validation and unfinished work are stated.
3. **It fixes what the Overleaf PDF got wrong.**
   - The data sources are cited again.
   - The ambiguous 55.7 MB sentence is clarified.
   - A GenAI statement is added.
4. **It contains no known factual errors.** Every number was checked against the app data:
   - 52 = 29 + 23 difficult tracts at 08:00;
   - 375 / 346 at 03:00, and 247 of 346 within a 10-minute walk;
   - Tujunga 39.0 min with a 29:54 walk;
   - Hollywood 1.5 min fill vs. a 52.7-min wait;
   - Palms 3.0 → 47.0 min;
   - about 69% / 6% for the sun bins.
5. **Nothing is lost.** v4 stays in `archived paper/` as the long-form source for Q&A and future work. Any later edit goes into `paper_5page_edition.md`, and the team should recompile after each change.

"Final" means final **content within the page budget**. The team still needs to put it in their own words, confirm the GenAI sentence, and fix the figure items listed next.

---

## Known open items in the final edition

These are tracked in more detail in `WHAT_TO_DO_NEXT.md`.

- [ ] Port v5 into Overleaf, either from `latex_check/main_5page.tex` or by editing `main.tex`. Recompile and confirm it is **5 pages**.
- [ ] Confirm or extend the GenAI sentence so it matches how AI was actually used.
- [ ] Put the text in the team's own words **without adding length**. There are no spare lines: one extra word in the abstract makes it 6 pages.
- [ ] **Fig. 1:** v6 crops it to excerpts and prints its text about 1.7 times larger, but the small print is still only about 2 pt. Recapture at 150% browser zoom to fix it fully.
- [x] Legend strip added to Fig. 3 (under panel b) in v6.
- [ ] Confirm the Jenny et al. clause in §2 against the paper (added in v6, wording not checked).
- [ ] Optional: restore the data-source URLs or cite the tools if space opens up.
- [ ] Andrienko et al. stays one generic clause until someone reads the full text.
- [ ] Add the team's final Overleaf PDF to this repo.

---

## Feedback documents (`feedbacks/`)

These are reviews and directions. They are **not** paper text.

| File | Date | Reviews | What it contains |
|------|------|---------|------------------|
| `Paper_Revision_Guide.md` | Nov 19 | The pre-submission paper | Directions for each section, with concrete examples, including block-level units for future work. Corrected on Nov 19 for the 55.7 MB and 1902.01 errors. |
| `revision_feedback.md` | Nov 19 | v1 (tentative draft) | Blocking problems (own words, length, the 1902.01 error) and factual corrections. Led to v2. |
| `patch_vs_feedback_Nov20.md` | Nov 20 | v2 against the Nov 20 FEEDBACK | Item-by-item check marked done, partly done or missing. Led to v3. |
| `overleaf_pdf_review_Nov21.md` | Nov 21 | The team's Overleaf `main.pdf` against v4 | Measured 5-page budget (about 1,650 body words), what each addition costs in lines, priority issues (missing data citations, thin Related Work, 55.7 MB wording, no GenAI sentence, Fig. 1 size), and a tested Related Work. Led to v5. |
| `5page_vs_FEEDBACK_Nov21.md` | Nov 21 | v5 against every item in FEEDBACK.md | Item-by-item status (10 of 13 paper items fixed, 3 mostly fixed; Fig. 1 still at risk), the four deductions an earlier trim had reopened and how they were restored, and the cheapest remaining gains. |

---

## Figures

### `figure_references/`: original screenshots

- `Fig 1(a).jpeg` … `Fig 3(c2).jpeg` are the team's screenshots of the live app.
- `FIGURE_SHOT_LIST.md` explains how each one was taken: view, tract, time and settings.
- These are the source files. Keep them unchanged.

### `figure_cropped/`: paper-ready crops

- `Fig_1a.png` … `Fig_3c2.png` are cropped from the matching screenshots. Their names drop the parentheses, so `Fig 1(a).jpeg` becomes `Fig_1a.png`.
- In Fig 1a, the tooltips were cropped out.
- **Still to do:**
  - Fig 3c does not show the bus board times.
  - The Fig 2b tooltip is still visible.

  Retake or re-crop these from new screenshots following `FIGURE_SHOT_LIST.md`.

| Figure | Shows |
|--------|-------|
| 1a, 1b | Whole-app overview: the Sun exposure tab (Summer, 08:00) and the Reachability tab (city view, 52 flagged communities) |
| 2a, 2b | Sun exposure in Hollywood (Tract 1902.01): the light fill (about 1.5 min) next to the selected 53.5-min journey |
| 3a | Reachability city overview at 08:00: `!` markers, default routes and the 8-class legend |
| 3b | Tujunga (Tract 1011.22): a route drawn with a dark casing and red edge over the 20–30-min fill |
| 3c1, 3c2 | Palms (Tract 2699.03) at 03:00, without and with signal waits (boarding at 03:03 vs. 03:47) |

**Resolution:** the screenshots are low-resolution previews, about 740 px wide. Recapture the figures at print quality (3200 × 2000) using the method in `FIGURE_SHOT_LIST.md` before the final PDF.

The figure captions themselves are at the end of the final paper, under "Figure Captions".

---

## Related files kept elsewhere

These files are not in this repo but belong to the same work. They are in the team's shared backup folder:

- `VIDEO_SHOT_LIST_v2.md` and `Video_Script_v2.md`: the 2-minute video. The clips are timed to match the slides.
- `Sun_LA_Final_Presentation.pptx`, `Sun_LA_English_Presentation_Mod.pptx` and `Sun_LA_English_Presentation_Mod_v2.pptx`: the 5-minute presentation drafts. Mod_v2 is the edited version, with speaker notes timed to 5:00.

---

## Notes for this repo

- Do **not** copy these files into the main project repo. There, `Claude outputs/` is in `.gitignore` on purpose.
- If a local scratch folder such as `_to_delete/` comes back, keep it out of git. Add it to `.gitignore`:
  ```
  _to_delete/
  .DS_Store
  ```
- The figures add about 13 MB in total, which is fine for a normal git repo.
