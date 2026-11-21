# Sun / LA — what to do next (team checklist, updated Nov 20, 22:40)

**Where things stand**
- **Paper:** `paper_revision_nov_20_feedback.md` covers every item in the Nov 20 FEEDBACK.md. Tonight's revision also explains **why each challenge is hard**: §1.2 (what had to be computed, and the route-color dilemma), §2 (each cited work as *their challenge → their solution → how ours differs*) and §4.2 (*problem → options → decision → check*).
- **Slides:** `Sun_LA_English_Presentation_Mod_v2.pptx` (in `project_bkp`) has the clip placeholders and speaker notes (5:00 total). Backup slides are being added by the team.
- **Video:** `VIDEO_SHOT_LIST_v2.md` and `Video_Script_v2.md` (190 words) are ready to record.

Put a name next to each item.

---

## 1. Paper (highest priority)

| # | Task | Why | Owner |
| --- | --- | --- | --- |
| 1 | **Fill in the Andrienko et al. [15] TEAM TODO in §2.** Read its method sections and write one sentence: how they compute place connectedness from GTFS, how they show it on maps, and which visual problem that solves. Then one clause on how we differ. | Placeholder text is still in the paper; the full text wasn't accessible to Claude. | |
| 2 | **Paste into Overleaf with the 3 figures and compile.** Check the page count *first*. | The text is now about 4,500 words in the file (body roughly 3,500) + 22 references + 3 figures; the limit is **5 pages**. Tonight's changes added ~190 words. | |
| 3 | **Cut to fit 5 pages.** Cut repeated caveats ("modeled, not observed" appears several times), Data paragraph 2, abstract detail, the §5.2 signal-pair details. **Don't cut** the parts the feedback asked for: users, contributions, the challenge explanations in §1.2 and §4.2, the related-work comparisons, captions with color keys. | Over-length costs points. | |
| 4 | **Write the final text in our own words** (ASSIGNMENT.md rule). | Course rule. | |
| 5 | **Three small wording fixes still open:** | Not accurate as written. | |
| | • §4.3: "69% / 48% / 6% / 88%" → "about 69%…" (computed over all 1,120 tracts, not only the ~1,110 with enough data). | | |
| | • §4.4: Bootstrap provides "select menus and buttons", not "layout controls" (we only found menus and buttons in the code). | | |
| | • Fig. 1 caption mentions the scatter plot, but the Fig. 1 screenshots don't show it. Add a scatter crop as panel (c), or remove it from the caption. | | |
| 6 | **GenAI sentence (§4.4):** it currently says all analytical framing and final paper prose were done by the authors. Make it match what actually happened, for both the code and the writing. | It must be true. | |
| 7 | **Use `\cite{key}`, never typed numbers.** LNCS sorts references alphabetically, so all numbers will change (Jenny et al. was just added as [22]). | Wrong citations otherwise. | |
| 8 | **Export BibTeX** from Google Scholar or the DOI for every new reference ([14]–[22]). | LNCS format. | |
| 9 | **Re-measure** the ~0.74 MB Reachability load in Chrome DevTools (Network tab, cache disabled), or drop "measured". | The current figure came from Claude's check. | |
| 10 | **Recheck numbers** against the live site before submitting: 52 at 08:00, 375 at 03:00, Tujunga 39.0 min / 29:54, Hollywood 1.5 vs 53.5, Palms 3.0 → 47.0, 113,395 → 112,201. | Must match. | |

## 2. Figures

| # | Task | Owner |
| --- | --- | --- |
| 11 | Use the cropped PNGs in `figure_cropped/` at **full text width**; stack the Palms pair (Fig. 3c) vertically. | |
| 12 | Check readability in the compiled PDF at 100% zoom; if small text is unreadable, recapture at 150% browser zoom in a ~1,300 px window and crop. | |
| 13 | Optional: recapture Fig. 3(c) with the panel scrolled so the **Board 03:03 / 03:47** row shows. | |
| 14 | Keep the **color key in every caption** when shortening. | |

## 3. Video (−1 left: title slide)

| # | Task | Owner |
| --- | --- | --- |
| 15 | Record from `VIDEO_SHOT_LIST_v2.md` + `Video_Script_v2.md`. **Do all 08:00 steps before moving the slider to 03:00.** | |
| 16 | Title slide shows **"Group 4"** (exactly as the course lists it). | |
| 17 | Rehearse twice: the 03:00 overview count should read **375**, and clicking a bus stop should show who can walk to it. Final file **≤ 2:00**. | |
| 18 | Keep the three clip segments as continuous takes: **0:22–0:45**, **1:00–1:25**, **1:28–1:46**. If editing shifts them, note the new times for the slides. | |

## 4. Presentation (−3 left: no video clips)

| # | Task | Owner |
| --- | --- | --- |
| 19 | Import `Sun_LA_English_Presentation_Mod_v2.pptx` into Google Slides (File → Import slides). | |
| 20 | **Insert the three silent clips** (slides 3, 4, 6): Insert → Video → YouTube, set Start/End to the clip times, tick **Mute audio**. Delete each "SILENT CLIP" label and its screenshot. This is the largest remaining deduction. | |
| 21 | Add the backup slides after slide 8 (team is doing this). | |
| 22 | After import, check slides 3 and 4 for text overflow (font substitution). Rehearse to 5:00 using the speaker notes. | |

## 5. Repository and submission

| # | Task | Owner |
| --- | --- | --- |
| 23 | Replace `paper.pdf` in the repo with the final Overleaf PDF (same name); update links in `SUBMISSION.md` if they changed (Overleaf, YouTube, slides). | |
| 24 | **Don't commit** `Claude outputs/` or raw data; check `git status` before every push. Pull before you push; batch edits into one push. | |
| 25 | Everyone writes their own `SUBMISSION.md` contribution entry (≤ 250 characters per artifact, own words). | |

---

## Keep an eye on
- **The page limit:** the biggest risk.
- **Don't let earlier fixes slip back in:** the 55.7 MB sentence (it describes *all* route files) and the neighborhood-data URL in reference 20 both regressed once.
- **The Hollywood example:** the map color (about 1.5 min) and the selected journey (53.5 min) refer to **different stops**; keep that explanation when shortening.
- **Tujunga:** 39.0 min is walk **+ wait**; the route itself is the 29:54 walk. Don't call it "the 39-minute route" (in the paper, slides or video).
- **Challenges are framed as design trade-offs** the team resolved (options → decision → check). Keep that framing in your own words; it's what the professor asked for.
