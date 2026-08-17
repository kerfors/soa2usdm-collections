# NCT03693430 — SoA extraction uncertainty report

Source: `NCT03693430_soa.pdf`, 4 PDF pages = document pages 9–12 (per PAGEMAP.md).
Novo Nordisk trial NN9536-4378, protocol version 3.0, 29 June 2018 (read from the page header block).
Prompt v3.7.2 / schema soa-table-extraction v1.0.

---

## 1. Structure decision — how many tables

**One table.** Section 2 is headed `Flowchart` and prints one activity × visit grid across
document pages 9–12. The four header rows reprint on every page (Word repeating-header rows), the
column set is byte-identical on all four pages, and there is no second title, no second visit
numbering and no second population. Per §1b, which requires repeated header and schedule-property rows to be de-duplicated and
counted once across continuation pages, the reprinted header rows are
recorded once, from document page 9, and the whole thing is delivered as a single table with
`page_start` 9 / `page_end` 12 rather than a parent plus three `continuation` files.

- **`table_type`: `main_soa`** — the rows are procedures performed on subjects (so not `reference`),
  and it is the protocol's only and primary anchor grid. No subsidiary/track/domain candidate exists
  in the excerpt.
- **Label columns `L` = 1** (the activity-name column). **First data column = `column_position` 2.**
  Data columns run 2–35 for V1–V34.
- **34 data columns**, **63 activity rows**, **470 marks**, **4 schedule properties**, **30 annotations**.

### Activity rows per declared page (§4 coverage check)

| document page | activity rows |
|---|---|
| 9 | 19 |
| 10 | 20 |
| 11 | 19 |
| 12 | 5 |

Every page in the declared range contributes rows. No page is footnote-only for activities: page 12
carries five body rows *and* the footnote block.

### Rows that print above the repeated header (rendering artifact, flagged not repaired)

Two body rows print physically **above** their page's reprinted header block:

- `"Breast neoplasms follow-up"` — first band on document page 11, above the reprinted
  `Visit (V), Phone(P)` / `Timing of Visit (Weeks)` / `Visit Window (Days)` rows;
- `"Training in trial product, pen-handling"` — first band on document page 12, likewise.

Both are ordinary body rows orphaned above the repeating header by the word processor. They are
ordered here as the continuation of the preceding page's list (Breast neoplasms follow-up after
`"Technical complaint"`; Training in trial product after `"Hand out directions for use"`), which is
where they belong by section grouping (SAFETY and REMINDERS respectively). This is recorded in
`table_metadata.notes`: `"Two body rows print ABOVE the reprinted header block on their page …"`.

---

## 2. Image-based extraction method (§1a / §1d)

The PDF has **no text layer at all** — `pdftotext` returns nothing on all four pages — and no vector
rule lines. Everything below the page-header stamp is one raster image per page. The §1a/§1d image
path was used end to end:

1. Rendered at 200 dpi with `pdftoppm -r 200 -png`.
2. **Vertical rules** = image columns whose ink fraction (< 128 grey) exceeds 0.85 over the table
   body height. 36 rules recovered, identically on all four pages → 35 columns = 1 label column +
   34 data columns. Rule x-positions agree page-to-page to within 1 px.
3. **Horizontal rules** = image rows with ink across ≥ 1900 of 2339 px (full table width). Bands
   between consecutive rules give the row cells; slivers < 9 px discarded. Recovered band counts:
   23 / 24 / 23 / 9 on pages 9 / 10 / 11 / 12, matching the visible row counts exactly.
   No redaction bar sits inside the table, so the §1d "read the boundaries from a text column"
   hazard did not arise here (see §7 below for the one redaction in the document).
4. **Marks** = count of near-black pixels (intensity < 90) inside each rule-bounded cell, inset 2–3 px
   from the rules, against an absolute count threshold of 40 (`method: "raster_pixel_detection"` on
   every `activity_schedule` cell).
5. **Header text and footnote text** read by eye from zoomed crops of the render
   (`method: "visual_read"` on `schedule_grid`, `"visual_transcription"` on `activity_name_source`
   and on every `annotation_text_source`).

### Detector validation and separation

The count distribution is cleanly bimodal across all body cells: **every non-empty cell counts
between 47 and 89 near-black pixels, and every empty cell counts exactly 0.** There is no cell
anywhere in the 63 × 34 body between 1 and 46, so the threshold choice cannot flip any cell.

Cell-for-cell validation against direct visual reads was done on zoomed crops covering both dense
and sparse rows, in both the left (V1–P17) and right (V18–V34) column halves:

- dense: `"Diet and physical activity counselling"`, `"Criteria for discontinuation"`,
  `"Waist circumference"`, `"Concomitant medication"`, `"Trial product compliance"`;
- sparse: `"Patient Health Questionnaire– 9 (PHQ-9)"`, `"Columbia Suicide Severity Rating Scale (C-SSRS)"`,
  `"HbA1c"`, `"Fasting serum insulin"`, `"Evaluation of lipid-lowering treatment"`,
  `"First date on trial product"`, `"Dispensing visit"`, `"Drug accountability"`, `"IWRS session"`.

**Mechanical vs visual disagreements: none.** A final automated diff of the delivered JSON against
the raw pixel-count matrix reported 0 mismatches over all 63 rows × 34 columns (470 marks).

Two boundary reads that the visual check specifically confirmed, because they are the kind of thing
a pattern-completer would "fix":

- `"Evaluation of lipid-lowering treatment"`, `"Evaluation of antihypertensive treatment"` and
  `"Evaluation of glycaemic status"` carry their maintenance-period marks on the **phone** contacts
  P13/P21/P29 (column_position 14/22/30), *not* on V12/V20/V28 where the neighbouring laboratory rows
  (`"Lipids"`, `"HbA1c"`) carry theirs. Confirmed on a crop showing the X directly beneath the
  printed `P13` header cell. Transcribed as printed.
- `"Criteria for discontinuation"` ends at V32 (column_position 33) — no mark at V33 or V34 —
  while `"Diet and physical activity counselling"` on the row below ends at V33. Confirmed visually.

---

## 3. Merged cells and distributed values

**Body grid: no merged marks.** Every X sits in its own rule-bounded cell; no `source_range` is set
on any `activity_schedule` entry, and no arrows, vertical merges or qualified marks occur.

**Header row 1 (the period band) is merged**, and the spans were taken from the rule lines that
survive inside that band (rules present only at x-indices 0,1,2,3,11,33,34,35):

| period value | column_position span | `merged_cell_range` |
|---|---|---|
| `Screening` | 2 | not merged |
| `Randomisation` | 3 | not merged |
| `Dose escalation period` | 4–11 | `"4:11"` |
| `Maintenance period` | 12–33 | `"12:33"` |
| `End of treatment` | 34 | not merged |
| `End of trial` | 35 | not merged |

Ranges use numeric `column_position` notation, not A1.

---

## 4. Synthesised values

- **`property_name` "Trial period"** (row 1). The label cell of the period band is blank;
  `property_name_source.synthesized: true`, `structure_method: "inferred_from_layout"`. The name is
  the extractor's, the values are the source's.
- **Annotation markers `pr1`–`pr26`** for the 26 distinct inline section/appendix references
  (§6). The source prints these as hyperlinked cross-references inside the activity labels
  (`Inclusion criteria (6.1)`, `HbA1c (Appendix 2)`, `Trial product compliance (7.1) (7.6)`) with no
  marker symbol of their own. They are numbered in printed order of first appearance and each
  `marker_locations` entry carries `method: "synthesized"`. Every `pr*` marker is also written into
  the citing activity's `annotation_markers`, and a mechanical check asserts the two sides agree
  exactly (0 discrepancies).
- Reference **text normalisation**: an inline reference is stored as its bare token —
  `"6.1"`, `"9.4"`, `"9"`, `"Appendix 2"` — with the surrounding parentheses/commas dropped, so that
  a reference printed as `(9.4.5, Appendix 5)` on one row and as `Appendix 5` on another
  deduplicates correctly. The literal printed form is preserved in each activity's
  `activity_name_source.cell_text` (e.g. `"Pregnancy test ᵇ (9.4.5, Appendix 5)"`). Inline references
  are stripped out of `activity_name` per §6.

---

## 5. Source contradictions and defects — transcribed, not resolved

1. **V33 visit window disagrees between pages.** The `Visit Window (Days)` cell for V33 reads `±3`
   on document pages 9 and 10 and `±5` on document pages 11 and 12. Both were re-read on 4× crops;
   this is a genuine source inconsistency in the repeated header, not a scan artifact. The page-9
   value `±3` is recorded (the header block is deduplicated from page 9) and the contradiction is
   stated verbatim in that property's comment: `"the V33 cell reads ±3 … left unresolved"`.
2. **Header label case disagrees between pages.** `"Timing of Visit (Weeks)"` on pages 9–11 vs
   `Timing of visit (Weeks)` on page 12. Page-9 form recorded, disagreement noted in the property
   comment.
3. **Two distinct "Vital signs" parent rows.** `"Vital signs"` (page 10, parent of Systolic/Diastolic
   Blood Pressure) and `"Vital Signs"` (page 11, parent of Pulse) are two separate rows in the source,
   with different capitalisation and both citing `9.4.3`. Both kept; not merged.
4. **Line-break hyphens.** The section band prints `SUBJECT RELATED INFOR-MATION AND ASSESSMENTS`
   (a mid-word hyphenation) and the rotated period cell prints `Randomisa-` / `tion`. The clean
   fields carry the dehyphenated forms `"SUBJECT RELATED INFORMATION AND ASSESSMENTS"` and
   `"Randomisation"`; the literal printed form is preserved in `activity_name_source.cell_text` for
   the section band (`"SUBJECT RELATED INFOR-MATION AND ASSESSMENTS"`). `schedule_grid` has no raw
   field, so the `Randomisa-tion` line break is recorded only here.
5. **Redaction.** A black bar redacts text in the page-header stamp of all four pages, immediately after the printed protocol version. The bar sits above the flowchart, outside the table frame, and conceals no activity rows and no marks. Nothing was reconstructed.
6. **Glyph case.** `"Control of Eating Questionnaire (CoEQ)"` is the one row whose marks are printed
   as a lower-case **`x`** (four cells: V2, V12, V20, V33); pixel counts for that row (47–53) are
   correspondingly lower than the X rows (55–89). Transcribed literally as `"x"`, not normalised.
   Its reference is also printed without parentheses — `(CoEQ)ᵈ 9.1.3` — unlike every other row.

---

## 6. Annotation integrity

- **No glyph-spread text layer to reconstruct** — there is no text layer at all, so every annotation
  text is a visual transcription (`annotation_text_source.method: "visual_transcription"`,
  note: `"read by eye from a 200 dpi pdftoppm render; the PDF carries no text layer"`).
- **Four printed footnotes** (`a`–`d`), all typed `footnote`, all from the block below the grid on
  document page 12. Each explains rather than merely points, so none is a `source_note`:
  `"Demography consists of date of birth, sex, ethnicity, and race (according to local regulation)"`,
  `"For all female subjects"`,
  `"Smoking is defined as smoking at least one cigarette or equivalent daily"`,
  `"Applicable for US and Canada only"`.
- **Twenty-six `source_note`s**, one per distinct inline cross-reference — pure pointers, correctly
  typed `source_note` per §8.
- **`by_type` is not degenerate**: 4 `footnote` + 26 `source_note`. There is no notes/comments column
  in this table (the grid runs to the page edge), so no note-cell bounding by proximity was needed
  anywhere — `proximity_bounded` is used zero times.
- **No `abbreviation` and no `legend` annotations.** The table prints no legend defining `X`, and the
  only term expansions (`(PHQ-9)`, `(C-SSRS)`, `(CoEQ)`) appear inside activity labels with no marker
  and no definition block, so per §6 they are not emitted as annotations.
- **Containment pairs — checked, and all are source-faithful, not split cells.** Several
  `source_note` texts are substrings of others: `"9"` ⊂ `"9.4"` ⊂ `"9.4.1"`, `"9.2"` ⊂ `"9.2.9"`,
  `"7.1"` ⊂ `"7.1.1"` and `"7.1.2"`, `"Appendix 2"`/`"Appendix 3"`/`"Appendix 4"`/`"Appendix 5"` share
  a stem. These are **distinct cross-references printed as distinct hyperlinks in distinct activity
  labels**, re-verified against the page — not one note cell split across rows. Nothing was merged,
  truncated or dropped. The four footnote texts share no run at all.
- **Orphans: none.** All 30 annotations have ≥ 1 `marker_location`; a mechanical set-comparison
  confirms `marker_locations` and per-row `annotation_markers` are in exact agreement in both
  directions.
- **Markers referenced but not defined: none.** All four printed footnote letters `a`–`d` have text
  in the source, and all four are cited on at least one row (`a`×1, `b`×4, `c`×1, `d`×1).

---

## 7. Low-confidence calls

1. **Marker-location `method` for the `pr*` references.** Recorded as `"synthesized"` because the
   marker symbol itself is invented. Arguably the *scope* is printed (the reference text sits in that
   activity's own label), so a reviewer may prefer no `method` at all. The choice is uniform across
   all 26 references and all 42 locations.
2. **`hierarchical_level` 4 for `"Visit Window (Days)"`.** The window row does little to tell columns
   apart (30 of 34 columns read `±3`), so a `null` level would also be defensible. It is a genuine
   stacked header row, so it was given a level.
3. **Indentation levels are visual estimates** (`indentation_method: "visual_estimate"` on every
   activity) — there is no text layer to read leading whitespace from. Five grey section bands are
   level 0 (`SUBJECT RELATED INFORMATION AND ASSESSMENTS`, `EFFICACY`, `SAFETY`, `TRIAL MATERIAL`,
   `REMINDERS`), and none carries a mark, as §4 requires. Level 2 was given only to the eight rows
   that are visibly indented under a parent: `Height`, `Body weight`, `Waist circumference`,
   `Systolic Blood Pressure`, `Diastolic Blood Pressure`, `Pulse`, `Dispensing visit`,
   `Drug accountability`. The four mark-free level-1 parents (`Body measurements`, `Vital signs`,
   `Vital Signs`, `Administration of trial product`) behave as sub-headers.
4. **One-table vs four-table delivery.** Recorded as a single `main_soa` spanning pages 9–12 rather
   than a parent plus three `continuation` tables; the repeated header rows drove the call (§1b).
   If the corpus convention is one file per printed page, this table would split at pages 10, 11
   and 12 with `continuation_of: 1` and the same rows.
5. **No PDF/markdown cross-check was possible** — no protocol markdown was provided, and the PDF has
   no text layer, so every string in the delivered JSON rests on a single visual read of the render.
   A spot-check of the resolved grid against the rendered pages is recommended, per §1a.

---

## 8. Method provenance recorded (§1e)

| field | value | scope |
|---|---|---|
| `activity_schedule[].method` | `raster_pixel_detection` | all 470 marks |
| `schedule_grid[].method` | `visual_read` | all 136 header cells |
| `activity_name_source.method` | `visual_transcription` | all 63 activities |
| `activity_name_source.indentation_method` | `visual_estimate` | all 63 activities |
| `annotation_text_source.method` | `visual_transcription` | all 30 annotations |
| `schedule_properties[1].structure_method` | `inferred_from_layout` | period band (row 1) |
| `property_name_source.synthesized` | `true` | period band (row 1) |
| `marker_locations[].method` | `synthesized` | 42 locations, all on `pr*` source_notes |

**`unresolved` marker locations: none.** Every marker's target is a row whose label prints that
marker or that reference. No location was guessed.
