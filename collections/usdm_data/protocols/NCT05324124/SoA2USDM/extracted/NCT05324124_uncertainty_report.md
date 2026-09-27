# NCT05324124 — SoA extraction uncertainty report

Prompt 3.8.1, single pass. Source: NCT05324124_soa.pdf (4 PDF pages = document pages 9-12 per PAGEMAP.md). No protocol markdown available; all text from the PDF.

## Decisions needed (4)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.12, between rows 19 (Genetic sample) and 20 (Selpercatinib administration) | The black "CCI" redaction box across the full table width is treated as hiding unknown content; no activity rows were created for it. | Add one or more placeholder rows (e.g. "Redacted (CCI)") with no marks so the hidden rows are represented. | 3.1 |
| D2 | p.12, rows 21-22 (AE/SAE review, Concomitant medication review) | The double-headed arrow is treated as one merged cell covering Day 1 to Day 11 (cols 4-14), and "↔" is recorded in each of those 11 columns. | Limit the arrow to the columns the drawn arrow physically crosses (about Day 1 to Day 10). | 3.2 |
| D3 | p.9 (header on every page), rows 2-3, col 16 | The Follow-Up text "within 7 to 10 days after last dose" (one cell merged down across header rows 2-3) is stored on the study-day row 3; row 2 is empty for that column. | Store it as a separate visit-window property, or on row 2. | 3.3 |
| D4 | p.12, abbreviation line below the table | The abbreviation line (CRU, ECG, ED, h; the rest is redacted as CCI) is not emitted as annotations, because no term is printed as a marker. They only appear as ordinary words in labels and cells. | Emit ab1-ab4 (CRU, ECG, ED, h) bound to the labels/cells where those words appear. | 3.4 |

## Recorded, not open (6)

- §2 / taxonomy "page break inside ONE printed table": the header reprints on pp.10-12 with no new table number, so this is one table spanning pp. 9-12, not a continuation.
- §3: header row 2 ("Days" band) has `hierarchical_level: null`. It does not tell any two columns apart.
- §3: synthesized property names "Study period" (row 1), "Days" (row 2) and "Study day" (row 3; the printed cell reads "Procedure", which heads the activity column).
- §5: L = 1 label column (Procedure). Data columns are 2-16. Column 17 (Comments) is a notes column and is left out of the grid.
- §5: "P" is kept as an in-grid scheduling value, and composite cells ("P, 2h", "P, 1, 2h", "P, 1,2h", "24h") are copied as printed.
- §6: the Comments-column notes become footnote annotations with synthesized markers cm1-cm8, each bound to the row beside it (`method: synthesized`). cm7 ("See Appendix 10.2, Clinical Laboratory Tests, for details.") is a bare pointer and is typed `source_note`.

## 1. Table summary

**Table 01**: `main_soa`, "Schedule of Activities (SoA)" (section 1.3), document pages 9-12.
- Columns: 17 physical columns. Col 1 is Procedure (label). Data columns are 2-16 (Screening; Day -1; Days 1-11; ED; Follow-Up phone call). Col 17 is Comments (notes).
- Header rows: 3 (epoch, a presentational "Days" band, study day).
- Activities: 19 (rows 4-22). All are level 0 (a flat table with no group headers; `indentation_method: assumed_flat`).
- Activity rows per page: p.9: 6 (rows 4-9); p.10: 3 (rows 10-12); p.11: 6 (rows 13-18); p.12: 4 (rows 19-22). Every page contributes rows.
- Schedule cells: 77, including 22 distributed arrow cells.
- Annotations: 8 (7 footnote, 1 source_note).

## 2. Mechanical mark-check

- Pages 9-11 have a vector text layer with rule lines. Column x-centres were fixed from the header day labels, and every X/P/24h/2h token was assigned to the nearest centre with `pdftotext -bbox`. The result matches the visual read cell for cell.
- Page 12 is one full-page raster image (1650x1275) with an invisible, glyph-spread text layer. The X tokens on that page (Genetic sample Day 1; Selpercatinib Day 1 and Day 8; AE/SAE and ConMed at Screening, Day -1 and ED) were also column-binned and agree with the rendered page.
- The arrows on rows 21-22 are graphics that have no text token. They were read visually (`method: visual_read`).
- No mismatches were found. A quick check of page 12 against the resolved grid is still recommended.

## 3. Details of decisions

**3.1 Redaction (D1).** A black box labelled "CCI" (Company Confidential Information) covers the whole table width on p.12 below "Genetic sample". Its height, about 3-4 normal rows, suggests it hides one or more activity rows and their marks. Nothing was invented for it. This is the main place where the extraction could be incomplete.

**3.2 Arrow spans (D2).** On rows 21-22 the cells for Day 1 through Day 11 have no internal vertical rules in the raster, so they form one merged cell (cols 4-14). The arrow is drawn inside that cell and was distributed across the whole cell (`source_range "4:14"`), following §5. ED (col 15) has its own X. The Follow-Up column (col 16) is empty for both rows, as printed; it was not filled in.

**3.3 Follow-Up window (D3).** Col 16 has "Follow-Up phone call" in header row 1 and a cell merged down across rows 2-3 with the window text. The schema has no vertical merge for headers, so the text is on row 3.

**3.4 Abbreviations (D4).** The line reads "Abbreviations: CRU = clinical research unit; ECG = electrocardiogram; ED = early discontinuation; h = hour;" and the rest is redacted as CCI. None of these terms is a marker, so nothing was emitted, per §6.

## 4. Merged cells distributed

- Header row 1: "Treatment Period" across cols 3-15 (`merged_cell_range "3:15"`).
- Header row 2: "Days" across cols 3-15 (`"3:15"`).
- Activity rows 21 and 22: "↔" across cols 4-14 (`source_range "4:14"`).
- No other merged marks.

## 5. Annotation text integrity

- Pages 9-11: the Comments-cell text comes from the text layer, with each cell bounded by its vector rule lines.
- Page 12: the text layer is glyph-spread (for example "P arti ci p a nt c o ns u m es hi g h -f at"). The text of cm8 was rebuilt into words and checked against the rendered page (`annotation_text_source.method: deglyph_reconstruction`). The cell edges come from the rule lines visible in the raster.
- No activity labels come from page 12's glyph layer that needed more than simple rebuilding: "Genetic sample", "Selpercatinib administration", "Adverse event /Serious adverse event review" and "Concomitant medication review" were checked visually. The slash spacing in "event /Serious" is kept as printed.
- No annotation text overlaps or contains another.

## 6. Orphan risk / undefined markers

- The in-grid mark "P" (Day 1 and Day 8 on medical assessment, ECG, vital signs and clinical laboratory tests) is not defined anywhere in the excerpt. It probably means pre-dose, but that is not printed, so no legend annotation was emitted. "h" is defined as hour in the abbreviation line.
- All 8 annotations have one `marker_location`, and each marker also appears in that row's `annotation_markers`.

## 7. Method provenance (non-default)

- `activity_name_source.indentation_method: assumed_flat`: all 19 activities.
- `activity_schedule.method: visual_read`: the 22 arrow cells (rows 21-22, cols 4-14).
- `schedule_property.structure_method: inferred_from_layout`: row 2 ("Days" band).
- `marker_locations.method: synthesized`: cm1-cm8 (Comments-column notes).
- `annotation_text_source.method: deglyph_reconstruction`: cm8.
- No `unresolved` locations.
