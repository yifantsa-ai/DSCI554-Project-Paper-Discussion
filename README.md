# Sun / LA: Paper Revision Workspace (DSCI 554, Group 4)

This repository holds the working documents behind the final paper for **Sun / LA**, the DSCI 554 final project by Group 4: Tianyi Wang, Yi-Fan Tsai, Shuang Wu and Yang Xu.

- Live app: https://dsci-554.github.io/project-20263-team-4/
- The app code lives in the main project repo, not here. This repo only keeps the paper drafts, the reviews of those drafts, and the figure material.

The app has two views:

- **Sun exposure:** modeled direct-sun minutes for the walk to a bus stop plus the scheduled wait.
- **Reachability:** walking time to bus stops, with a `!` flag where walk + wait is over 30 minutes.

---

## TL;DR: which file is the final paper?

**`paper_revision_nov_20_feedback.md`**, in the root of this repo, is the final edition. Everything in `archived paper/` is an older version kept for history. See [Why this is the final edition](#why-this-is-the-final-edition).

---

## Folder structure

```
.
├── README.md                              ← this file
├── paper_revision_nov_20_feedback.md      ← FINAL paper text (Markdown, to be ported to LNCS LaTeX)
├── WHAT_TO_DO_NEXT.md                     ← team checklist of remaining work
├── archived paper/                        ← superseded drafts, oldest → newest
│   ├── tentative_paper_revision.md
│   ├── paper_revision_patch.md
│   └── paper_revision_before_challenges.md
├── feedbacks/                             ← reviews of the drafts (not paper text)
│   ├── Paper_Revision_Guide.md
│   ├── revision_feedback.md
│   └── patch_vs_feedback_Nov20.md
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
| **v4** | **`paper_revision_nov_20_feedback.md`** | **Nov 20 (evening)** | **~4,500** | **FINAL** |

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

### v4: `paper_revision_nov_20_feedback.md` (FINAL)

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

---

## Why this is the final edition

`paper_revision_nov_20_feedback.md` is the final edition because it is the only version that meets all of the following:

1. **It answers every item in the Nov 20 FEEDBACK.** v3 already did this, and v4 keeps every one of those changes.
2. **It covers the challenge framing the professor asked for.** It says what is hard about the reachability view and how related work deals with similar problems. Only v4 does this. Earlier versions described the design without making the challenges clear.
3. **It contains no known factual errors.** All numbers were checked against the app data and the live site:
   - the 1902.01 fill of about 1.5 min;
   - 0.74 MB downloaded when Reachability opens, about 9 KB per tract, and 55.7 MB as the on-disk total;
   - 14,492 crossings and 48 departures;
   - the 30-minute (1,800 s) threshold.
4. **Neither regression is back.** The 55.7 MB sentence and the ref 20 URL are correct in this file.
5. **It is the newest file, and every older draft is in `archived paper/`.** Any later edit should be made to this file. Do not edit the archived copies.

"Final" means final **text content**. A few small items are still open before submission; they are listed next.

---

## Known open items in the final edition

These are tracked in more detail in `WHAT_TO_DO_NEXT.md`.

- [ ] **Andrienko et al.:** finish the paragraph marked `TEAM TODO` in §2. We could not open the full text.
- [ ] Change "69%" to "about 69%".
- [ ] Describe Bootstrap accurately; it is currently called "layout controls".
- [ ] Make the Fig 1 caption match the figure, including whether the scatter plot is mentioned.
- [ ] Make the GenAI-use sentence describe how AI was actually used.
- [ ] **Length:** fit the paper into 5 LNCS pages once it is in LaTeX.
- [ ] Port to LaTeX: change citations to `\cite{}` and add the 9 new BibTeX entries to `mybibliography.bib`.
- [ ] Re-measure the ~0.74 MB download on the live site just before submitting.

---

## Feedback documents (`feedbacks/`)

These are reviews and directions. They are **not** paper text.

| File | Date | Reviews | What it contains |
|------|------|---------|------------------|
| `Paper_Revision_Guide.md` | Nov 19 | The pre-submission paper | Directions for each section, with concrete examples, including block-level units for future work. Corrected on Nov 19 for the 55.7 MB and 1902.01 errors. |
| `revision_feedback.md` | Nov 19 | v1 (tentative draft) | Blocking problems (own words, length, the 1902.01 error) and factual corrections. Led to v2. |
| `patch_vs_feedback_Nov20.md` | Nov 20 | v2 against the Nov 20 FEEDBACK | Item-by-item check marked done, partly done or missing. Led to v3. |

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
