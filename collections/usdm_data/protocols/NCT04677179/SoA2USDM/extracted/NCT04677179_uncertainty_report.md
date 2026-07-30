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

> **Update — 2026-07-30 (annotation re-bounding, rev2).** The 2026-07-28 re-extraction fixed truncation
> but introduced the opposite error: adjacent Comment cells were concatenated into one annotation bound to
> the first row (an AS-project re-verification note flagged 5 cases; geometry found **12**). Root cause was
> the same in both directions — note text was bounded by **vertical-gap proximity**, not by the cell. The
> reason the 2026-07-28 pass could not do better: **20 of this protocol's 30 SoA pages are full-page raster
> images with no vector rule lines at all** (`page.rects`/`lines`/`curves` are empty; only doc p.17 has a
> vector table), so cell geometry is not available from the PDF's vector layer. Rev2 recovers it from the
> **raster**: render each page at 200 dpi, take the two rightmost vertical rules as the Comment column,
> take rows >85% dark across that column as the cell boundaries, and read the text layer band by band. The
> same bands give the row binding (a cell binds to the row band it overlaps), replacing the word-overlap
> guess. This works identically on the vector page, so there is one code path, not two. **Text was not
> regenerated** — the 2026-07-28 de-glyphed text and casing are preserved verbatim and merged notes are
> simply cut at the cell boundary (the only text edit is re-capitalising one split piece that had been
> mid-sentence). Result: raw 61 → 73, unified 39 → **41**; longest duplicated sentence block 145 → **0**
> chars; annotations over 400 chars 2 → **0**; `schedule_matrix` (577), `unified_activities` (60),
> `timeline_segments` and `property_hierarchy` all **byte-identical**; pipeline 0 errors, 0 validation
> warnings. Still Option B — the sidecars remain empty. Split pieces keep the parent marker with a suffix
> (`c18_2`, `c20_2`, …) so they stay traceable to the pre-split annotation.

> **Update — 2026-07-30 (Table 4 missing page + marker sync, rev3).** Two things rev2 exposed are now
> fixed. **(a) Table 4's first body page (doc p.43 / printed 42) had never been extracted.** Its 14
> activity rows — *Concomitant medications, Adverse events, Review MMS, Tobacco use, Physical Evaluation,
> Weight, Vital signs, Symptom-directed physical examination, CCI, 12-lead ECG, Patient Diary (Electronic),
> Diary return, IBDQ, PGI-C* — and their **26 ETV/V997/V801/V802 marks** were read from the page with the
> same raster band method and inserted ahead of the existing rows, which shifted by 14. All 577 previous
> `schedule_matrix` cells are preserved and 26 added (**603**); unified activities stay at **60** because
> every new row unifies with its Tables 1–3 counterpart (the redacted row keys correctly on
> `Physical Evaluation::CCI`). The three notes that had been sitting on surviving rows for want of their
> own now bind correctly — AESIs → *Adverse events*, vital signs → *Vital signs*, "Locally performed." →
> *12-lead ECG* — and the "See Section 8.2.2." cross-reference dropped in 2026-07-28 is restored as `c3_2`
> on *Symptom-directed physical examination*. **(b) `annotation_markers` are now regenerated from
> `marker_locations`.** That per-row field, not `marker_locations`, is what the resolve step uses to attach
> an annotation to an activity, so rev2's corrected bindings had not actually reached the resolved or
> consolidated output; 24 rows across the four tables carried stale markers. Both layers are now in step.
> One benign validation warning remains: `xannot-035` (T4 `c1`, "No fasting in this period") reports as
> orphaned because it is `schedule_property` scope and the consolidator deliberately does not expand
> property references to columns.

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

- **Table 4's missing first page: [RESOLVED 2026-07-30 rev3]** `Table_04`'s
  `activities` list starts at *QIDS-SR16*, which is the first activity on **doc p.44** (printed page 43).
  Everything on the table's first body page, **doc p.43 (printed page 42)**, is absent: *Concomitant
  medications, Adverse events, Review modified Mayo score (MMS), Tobacco use, Physical Evaluation, Weight,
  Vital signs, Symptom-directed physical examination, CCI, 12-lead electrocardiogram (ECG), Patient Diary
  (Electronic), Diary return, Patient-Reported Outcomes (Electronic), IBDQ, Patient Global Impression of
  Change (PGI-C)* — together with those rows' ETV / V997 / V801 / V802 marks. Confirmed in the consolidated
  output: *Concomitant medications*, *Adverse events*, *Vital signs*, *ECG* and *IBDQ* carry marks from
  source tables 1/2/3 only, never 4, and Table 4 contributes just **28** of the 577 `schedule_matrix`
  cells. This was a **Layer-1 completeness gap, not an annotation problem**. It has been closed: the 14
  rows and their 26 marks were read from the page and inserted, Table 4 now contributes **54** cells, and
  those activities carry table-4 marks (Concomitant medications 4, Adverse events 4, Vital signs 4,
  Symptom-directed PE 3, CCI 3, Weight 2, MMS/Tobacco/ECG/Diary return/IBDQ/PGI-C 1 each). Verify the row
  set and the mark pattern against the page once — this is new Layer-1 content, not a re-bounding.
- **Tile merge (T2/T3): [RESOLVED 2026-07-21]** the V10–V19 vs V20–V29 mark ranges per activity were verified against the PDF and the dropped V10–V19 marks restored via corrections (t2-c5..c38, t3-c3..c40). **The trailing appended CCI/QIDS-SR16 duplicate rows are now also merged (2026-07-21b, t2-c39..c58 / t3-c41..c66)** — folded by row position from the source tiles; appended artifact rows removed. Nothing left to merge by hand.
- **Image-page labels:** spot-check activity names on the maintenance/extension/ETV pages (a handful retain section-fragment bleed or a missing space).
- **CCI rows:** every "CCI (redacted)" activity is a black-bar redaction in the source (confidential commercial information); marks preserved, name unknown. Multiple CCI rows per table are not name-mergeable.
- **Property values** (Weeks/Study day/tolerance) were read from the clean header pages and hardcoded per table; low risk but confirm.

### Annotation layer (2026-07-30 rev2 re-bounding) — residuals to verify

- **Cell boundaries and row bindings are now geometric**, so the 2026-07-28 residuals below about
  best-guess word-overlap bindings and the two-row endoscopy comment are superseded. 12 over-merged notes
  were split; 6 bindings were corrected (T1 c2 → *Preexisting conditions*, c5 → *Height*, c8 → *12-lead
  ECG*, c16 → *TB test*, c22 → *Dosing*; T4 c5 → *Lipid panel*). T4 c1 ("No fasting in this period") is now
  `schedule_property` scope on the *Fasting visit* row, which is what it annotates.
- **The four Table 4 notes that had nowhere to bind are now correct** (see the rev3 note at the top):
  `c2` → *Adverse events*, `c3` → *Vital signs*, `c4` → *12-lead ECG*, and the restored `c3_2`
  ("See Section 8.2.2.") → *Symptom-directed physical examination*.
- **`T4 c11` ("No dosing at ETV, V997, or post-treatment follow-up visits.") sits on *Urine pregnancy
  (local)*** and almost certainly belongs on *Randomization and Dosing* (row 38), the way `T1 c22`
  ("No dosing at V3.") binds. It is a footnote-block note rather than a Comment-column cell, so the
  geometry pass does not cover it and it was left as found rather than moved on a guess.
- **Four corrupt abbreviation-key fragments remain**: `T1 c23`, `T2 c13`, `T3 c13`, `T4 c12` — e.g.
  "Srs=columbia–suicide severity rating scale; Bs urface antigen; QIDS-SR16=16-item activities". These are
  **not Comment-column cells**: they come from the multi-column abbreviation block below the table on doc
  pp.23/32/42/46, where the column clip interleaved two columns of running text. Rev2 did not touch them.
  Re-read from the page or drop; they carry no schedule logic.
- **One de-glyph artefact, now fully visible**: `T1 c21_2` reads "Addition alc. Difficile testing is allowed
  throughout the study at the investigator's discretion." The source (doc p.23) reads "Additional
  *C. difficile* testing is allowed throughout the study at the investigator's discretion." — the italic run
  broke word segmentation. It was buried mid-annotation before the split and is now the whole text of the
  note bound to *C. difficile testing*. Preserved verbatim because rev2 does not regenerate text.
- **One Comment cell is still not extracted at all**: "The intervals between ETV and the post-treatment
  follow-up visit(s) should be adequate to ensure that V801 occurs at least 8 weeks since last dose and V802
  (if applicable) occurs at least 16 weeks since last dose." It sits on Table 4's *Weeks from randomization*
  row (a `schedule_property` scope comment) and is present on doc pp.43–45. Adding it would be new content
  rather than a re-bounding, so rev2 left it out — add it deliberately if the property-level note matters.

### Annotation layer (2026-07-28 re-extraction) — superseded by rev2 above

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

Post-correction run (2026-07-21b, after the CCI/QIDS merge): 4 tables → **60 unified activities, 59% compression; review_queue = 0** (down from 65 as the 9 appended duplicate CCI/QIDS rows were removed and folded into their canonical rows). Four timelines render in the consolidated HTML (Screening/Induction main + Maintenance, Extension, ETV tracks). **Post-rev3 run (2026-07-30): 4 tables → 60 unified activities, 63% compression, review_queue = 0, `schedule_matrix` = 603 cells** (577 preserved + 26 restored from Table 4's missing page; none removed). **Annotations: 41 unified footnotes** from 74 raw (T1 31 / T2 14 / T3 14 / T4 15), `by_type = {footnote: 41}`, longest duplicated sentence block 0 chars, no annotation over 400 chars. One benign validation warning (`xannot-035`, property-scope annotation with no column references). *(2026-07-28 re-extraction: 39 unified footnotes from 61 raw, adjacent-pair overlap 2 — superseded, that pass over-merged 12 notes.)* *(Earlier drafts: pre-correction 67 / 57% / rq=1; post V10–V19 mark-fix 65 / 59% / rq=0.)*
