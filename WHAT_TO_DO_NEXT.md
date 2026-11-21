# Sun / LA — what to do next (team checklist, updated Nov 21, 02:30)

**Where things stand**
- **Paper:** the team's Overleaf `main.pdf` (5 pages) was reviewed against our drafts (`feedbacks/overleaf_pdf_review_Nov21.md`). **`paper_5page_edition.md` is now the edition to work from**: the Overleaf text as the skeleton, plus the data-source citations, a fuller Related Work, the GenAI sentence and the clarified 55.7 MB sentence. It was **compiled with our class files and fits exactly 5 pages** (`latex_check/main_5page.tex`, `main_5page_check.pdf`). The long-form draft (`archived paper/paper_revision_nov_20_feedback.md`) is now a source of record, not the text to paste.
- **Slides:** `Sun_LA_English_Presentation_Mod_v2.pptx` (in `project_bkp`) has the clip placeholders and speaker notes (5:00 total). Backup slides are being added by the team.
- **Video:** `VIDEO_SHOT_LIST_v2.md` and `Video_Script_v2.md` (190 words) are ready to record.

Put a name next to each item.

---

## 1. Paper (highest priority)

| # | Task | Why | Owner |
| --- | --- | --- | --- |
| 1 | **Bring the 5-page edition into Overleaf.** Easiest path: in Overleaf, replace the body of `main.tex` with `latex_check/main_5page.tex` (it uses the same `figures/fig1–3.png`), and either upload `mybibliography_5page.bib` or apply its 6 short data entries + 2 shortened titles to `mybibliography.bib` and change `\bibliography{…}` back. Recompile and confirm **5 pages**. | Tested build; the changes table at the end of `paper_5page_edition.md` lists every edit. | |
| 2 | **Data sources must be cited** (all six in the Table 1 Source column: `\cite{osm}`, `metrogtfs`, `ladotsignals`, `census2024`, `werk`, `mappingla`). If you keep the Overleaf version instead, add at least these. | Regression from the pre-submission paper; ASSIGNMENT requires citing data sources (−1 risk). | |
| 3 | **Confirm the GenAI sentence** in §4 ("An AI coding assistant helped write and debug the React/TypeScript code and automated tests.") and extend it if AI was used for anything else (e.g., writing). | Must be true; RUBRIC §3. | |
| 4 | **Write the final text in our own words** (ASSIGNMENT.md rule). Any rewrite that adds words must remove the same amount: the paper has **no free lines** (one extra word in the abstract makes it 6 pages). Keep the abstract at **150+ words** (now about 154) and keep the per-challenge Contributions sentence; both were FEEDBACK deductions. | Course rule; page limit. | |
| 5 | **Andrienko et al.:** the 5-page edition keeps one generic, accurate clause ("summarize how well public transport connects places"). If someone reads the paper, replace it with how they compute and show connectedness, **in the same length**. | Don't describe a method nobody has read. | |
| 6 | **Optional additions, only if space opens up:** restore the 6 data-source URLs in the bib (~7 lines), cite tools (deck.gl, MapLibre, React, D3, pvlib; ~10 lines), add Jenny et al. on flow-map clutter (~3 lines). | Nice to have; not required by the FEEDBACK. | |
| 7 | **Recheck numbers** against the live site before submitting: 52 at 08:00 (29 walk + 23 wait), 375 / 346 at 03:00, 247 of 346 within 10 min, Tujunga 39.0 min / 29:54, Hollywood 1.5 vs 52.7-min wait, Palms 3.0 → 47.0, about 69% / 6%. | Must match. | |
| 8 | **Export `main.pdf` and replace `paper.pdf`** in the project repo; add the final PDF to the Claude-outputs repo too. | Submission. | |

## 2. Figures

| # | Task | Owner |
| --- | --- | --- |
| 9 | **Fig. 1 is hard to read at print size** (43% of text width; the legend and panel text are illegible at 100% zoom). The previous FEEDBACK deducted for an unreadable Fig. 1. Crop it tighter (header + controls + side panel), or give it more of the row than Fig. 2. Recompile and check the page count. | |
| 10 | Optional: add the 8-class legend strip to Fig. 3(a); the caption names only the two ends of the scale. | |
| 11 | Keep the **color key in every caption** if you shorten them further. | |

## 3. Video (−1 left: title slide)

| # | Task | Owner |
| --- | --- | --- |
| 12 | Record from `VIDEO_SHOT_LIST_v2.md` + `Video_Script_v2.md`. **Do all 08:00 steps before moving the slider to 03:00.** | |
| 13 | Title slide shows **"Group 4"** (exactly as the course lists it). | |
| 14 | Rehearse twice: the 03:00 overview count should read **375**, and clicking a bus stop should show who can walk to it. Final file **≤ 2:00**. | |
| 15 | Keep the three clip segments as continuous takes: **0:22–0:45**, **1:00–1:25**, **1:28–1:46**. If editing shifts them, note the new times for the slides. | |

## 4. Presentation (−3 left: no video clips)

| # | Task | Owner |
| --- | --- | --- |
| 16 | Import `Sun_LA_English_Presentation_Mod_v2.pptx` into Google Slides (File → Import slides). | |
| 17 | **Insert the three silent clips** (slides 3, 4, 6): Insert → Video → YouTube, set Start/End to the clip times, tick **Mute audio**. Delete each "SILENT CLIP" label and its screenshot. This is the largest remaining deduction. | |
| 18 | Add the backup slides after slide 8 (team is doing this). | |
| 19 | After import, check slides 3 and 4 for text overflow (font substitution). Rehearse to 5:00 using the speaker notes. | |

## 5. Repository and submission

| # | Task | Owner |
| --- | --- | --- |
| 20 | Update links in `SUBMISSION.md` if they changed (Overleaf, YouTube, slides). | |
| 21 | **Don't commit** `Claude outputs/` or raw data; check `git status` before every push. Pull before you push; batch edits into one push. | |
| 22 | Everyone writes their own `SUBMISSION.md` contribution entry (≤ 250 characters per artifact, own words). | |

---

## Keep an eye on
- **The page limit:** every addition needs a cut of the same length. Recompile after each change.
- **Don't let earlier fixes slip back in:** the 55.7 MB sentence must say it is the total of all route files (about 9 KB per tract is what a click loads); the data-source citations must stay in Table 1.
- **The Hollywood example:** the map color (about 1.5 min) and the selected journey (53.5 min) refer to **different stops**; keep that explanation when shortening.
- **Tujunga:** 39.0 min is walk **+ wait**; the route itself is the 29:54 walk. Don't call it "the 39-minute route" (in the paper, slides or video).
- **247 vs 248:** with signal waits it is **247** of 346 within a 10-minute walk (96 within 5). 248 is the no-signal-wait figure.
