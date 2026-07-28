# NCT04677179 (J1P-MC-KFAH(b), INSTRUCT-UC) — Extraction Uncertainty Report

Ulcerative colitis. Source: `NCT04677179_soa.pdf` (30 pages = doc pp.17–46). Four tables.
Extractor: Claude Opus 4.8. Status: `ready_for_resolution`. **This protocol needed more reconstruction than the others — review more closely.**

> **Update — 2026-07-21 (post-correction).** The Tables 2/3 tile merge flagged below was reviewed against the PDF and found to have dropped the V10–V19 marks for the recurring/physical rows (root cause: the extractor wrongly assumed those rows print only in the V20–V29 "(continued)" tile — the source shows them in *both* tiles). Fixed via `op=add` cells in the corrections sidecars (`Table_02` t2-c5..c38, +34 cells; `Table_03` t3-c3..c40, +38 cells), source-linked per visit; the raw extraction is unchanged. Resolved output now carries the correct per-visit union. Items corrected here are marked **[RESOLVED]** inline; the consolidation figures below are refreshed to the post-correction run.

> **Update — 2026-07-28 (annotation re-extraction, Option B).** The annotation layer was fully
> **regenerated** to fix fragmentation / mis-bounding / mis-typing (the AS-project review found 41 of 111
> raw annotations truncated mid-sentence, 36 of 76 adjacent pairs sharing duplicated text, and all 111
> typed `source_note`). Method: read each table's Comment column via `pdftotext -bbox`, bound each note by
> its cell (vertical-gap segmentation), strip the one-glyph-per-token spacing, and re-segment words with a
> DP scored on general-English (`wordfreq`) + protocol-vocabulary frequencies — characters are 100% from
> the source text layer, only spaces and acronym casing are restored, nothing is invented. The raw
> extraction proved **lossy** (e.g. the endoscopy footnote tail "colonoscopy should be performed." is
> absent from every raw fragment), so the PDF text layer — not the raw JSON — is the note source. **Marks
> and structure were preserved unchanged** from the 2026-07-21 post-correction state (an independent bbox
> re-read had reproduced those marks 100%). This was published as **Option B**: the fresh raw extraction is
> the new ground truth, so the **corrections sidecars are now EMPTY** — the 2026-07-21 tile-merge marks
> `[RESOLVED]` below are baked into the fresh raw, and their provenance lives in git history. Row binding
> reuses the (correct) prior `marker_locations` where a note contains a verified fragment, else a best
> word-overlap match. Auto-finalized in Cowork; residuals to verify are listed under "Annotation layer" below.

## Tables & classification

- **Table 1 — `main_soa`.** Screening & Induction, V1–V9 (doc pp.17–23). Extracted cleanly (borders + clean text layer). Highest confidence.
- **Table 2 — `track`, "Maintenance (responders)".** V10–V29 (doc pp.24–32).
- **Table 3 — `track`, "Extension (nonresponders)".** V10–V29 (doc pp.33–42).
- **Table 4 — `track`, "Early Termination / Unscheduled / Post-Treatment".** ETV, V997, V801, V802 (doc pp.43–46).

Classification note: Tables 2/3 share the V10–V29 numbering but split the population (responders vs nonresponders at Week 12) into different treatment phases → treated as separate `track` timelines rather than `domain`. Documented judgment call.

## Method — and why this one is lower-confidence

The single-pass path was requested, but this SoA fought it on two fronts:

1. **~Two-thirds of pages are image-based** (table drawn as an image, text layer stored one glyph at a time — "Vital signs" → "V i t a l  s i g n s"). **X marks are single glyphs and were read reliably by coordinate.** Activity **labels were reconstructed** from the glyph stream with a gap threshold + cleanup dictionary; minor artifacts may remain (a few section-header fragments bled into adjacent activity names — e.g. one row reads "QIDS-SR16 Clinician-Administered").
2. **Tables 2 & 3 are tiled** into two column blocks — V10–V19 and V20–V29 ("(continued)") — that **overlap in their row sets**. Most body rows (including the universal ones: Concomitant medications, Adverse events, Vital signs, and the block-marked Weight, Tobacco use, Symptom-directed PE, ECG — plus, in T3, Review MMS) appear in **both** tiles and must be **union-merged** across them. Only a few rows are genuinely tile-specific — e.g. the source states "Endoscopic Procedure: Not applicable for responders during V10 through V19." **[RESOLVED 2026-07-21]** The original extraction merged by activity name with the V20–V29 tile as canonical order but wrongly treated the recurring rows as V20–V29-only, dropping their V10–V19 marks; this has been corrected in the sidecars (see the update note at top). **[RESOLVED 2026-07-21b]** The appended `CCI`/duplicate rows at the bottom of Tables 2/3 (the unmatched V10–19-tile rows) have now also been merged: their V10–V19 marks were re-read from the source tiles and folded into the canonical CCI/QIDS rows **by row position** (the row order is identical across tiles, so redacted names don't block matching), and the 9 appended artifact rows were removed via `op=remove` (T2 t2-c39..c58, T3 t3-c41..c66). The current data was lossy (only some appended rows existed), so this was a source re-read, not a fold of the appended rows.

Given these two factors, this extraction was produced as a reviewed draft (you chose the Excel-checkpoint flow and approved proceeding). *(The interim draft Excel `NCT04677179_SoA_draft.xlsx` used for side-by-side tile checking was a working checkpoint and is no longer retained in this folder; the corrected resolved/consolidated HTML supersedes it.)*

## Specific things to verify against the PDF

- **Tile merge (T2/T3): [RESOLVED 2026-07-21]** the V10–V19 vs V20–V29 mark ranges per activity were verified against the PDF and the dropped V10–V19 marks restored via corrections (t2-c5..c38, t3-c3..c40). **The trailing appended CCI/QIDS-SR16 duplicate rows are now also merged (2026-07-21b, t2-c39..c58 / t3-c41..c66)** — folded by row position from the source tiles; appended artifact rows removed. Nothing left to merge by hand.
- **Image-page labels:** spot-check activity names on the maintenance/extension/ETV pages (a handful retain section-fragment bleed or a missing space).
- **CCI rows:** every "CCI (redacted)" activity is a black-bar redaction in the source (confidential commercial information); marks preserved, name unknown. Multiple CCI rows per table are not name-mergeable.
- **Property values** (Weeks/Study day/tolerance) were read from the clean header pages and hardcoded per table; low risk but confirm.

### Annotation layer (2026-07-28 re-extraction) — residuals to verify

- **Complete-note recovery is good but not perfect.** 111 raw fragments → 77 fragmented → **39 complete
  footnotes** after re-bounding + de-glyph. Consolidated adjacent-pair overlap dropped from 36 to **2**;
  the two residual overlaps are the same note de-glyphed slightly differently across tables — normalise
  either text to reach 0.
- **Typing:** every note is now `footnote` (`by_type = {footnote: 39}`), correct for this all-explanatory
  Comment column. One line — the abbreviation/legend key ("... acid; ETV = early ...") — is typed
  `footnote` but is really an `abbreviation`/`legend` entry; re-type or drop if it matters.
- **Row bindings** are high-confidence where a note contained the prior verified fragment (endoscopy,
  pharmacogenetics, vital signs, AESIs, etc.); the remainder are a best word-overlap guess and a few may
  sit on a neighbouring row — low harm for a footnote layer, but spot-check against the page.
- **Endoscopy footnote** binds to the *Colon biopsy sample collection* row; the cell geometrically spans
  the *Endoscopy* row above it too (a two-row comment). Confirm the intended row(s).
- **One dropped note:** a "See Section 8.2.2" cross-reference in Table 4 was dropped rather than bound to a
  wrong row (it is present and bound in Tables 1-3). Re-add against the page if wanted.
- **Minor de-glyph artifacts** may remain around numerics (e.g. a stray "Section 8.3.6" spacing); scan the
  note text once.

## Consolidation result

Post-correction run (2026-07-21b, after the CCI/QIDS merge): 4 tables → **60 unified activities, 59% compression; review_queue = 0** (down from 65 as the 9 appended duplicate CCI/QIDS rows were removed and folded into their canonical rows). Four timelines render in the consolidated HTML (Screening/Induction main + Maintenance, Extension, ETV tracks). **Annotations (2026-07-28 re-extraction): 39 unified footnotes** (from 77 fragmented), consolidated adjacent-pair overlap = 2, `by_type = {footnote: 39}`; `schedule_matrix` unchanged at 577 cells and unified activities unchanged at 60 (marks/structure preserved). *(Earlier drafts: pre-correction 67 / 57% / rq=1; post V10–V19 mark-fix 65 / 59% / rq=0.)*
