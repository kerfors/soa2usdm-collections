# NCT01797120 — Uncertainty report (PDF_TO_JSON_PROMPT v3.8.1)

## Decisions needed (6)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.69, banner above table, header row 1 (marker tb1) | The bold banner "TREATMENT MUST BEGIN ≤ 7 WORKING DAYS FROM RANDOMIZATION" is kept as a footnote (synthesized marker tb1) attached to the Phase header row. | Treat the banner as part of the table title only, or drop it as a non-table heading. | 3.1 |
| D2 | p.69, header rows 1-2, columns 7-9 | The column 7-9 header cells span both header rows; their text is recorded once in row 1 (Phase) and the row-2 (Visit) cells are left empty. | Repeat the same text in row 2 as well, so every column has a Visit label. | 3.1 |
| D3 | p.69, header row 1, column 7 | Row 1 is typed epoch although column 7's value (End of Induction Phase <OR> End of Treatment) is an end-of-phase visit rather than a phase. | Type row 1 as other, or model column 7's header as a visit. | 3.1 |
| D4 | p.69, row 8 (Vital Signs), column 2 | The Pre-Study cell is kept literally as "X (includes Ht)" (the qualifier stays inside the mark). | Record the mark as "X" and move "(includes Ht)" into a separate note. | 3.2 |
| D5 | p.69, header row 1, column 7 (marker pr2) | "Section 5.6" printed under "End of Treatment" is removed from the header label and recorded as a cross-reference note (pr2) on that header cell. | Keep "Section 5.6" as part of the header label text. | 3.3 |
| D6 | p.71 | The table's page range is 69-71 because footnotes k-o, *, ^ and £ print on page 71, which has no activity rows. | Declare the table as pages 69-70 (grid pages only). | 3.1 |

## Recorded, not open (6)

- §2/type defs: page 70 reprints the header and continues the same untitled table, so it is one table spanning pages, not a `continuation`.
- §1b: the header and property rows reprinted on page 70 were de-duplicated.
- §5: one label column (L = 1), so the first data column is position 2 and the data columns are 2-9.
- §6: the inline "(see Appendix C)" on Concomitant Meds was removed from the activity name and emitted as source_note pr1.
- §6: the header-cell markers *, ^, £ and o sit on the specific schedule_grid cells where they are printed, not on the property rows.
- §4: the table is flat. Every row is a level-0 activity with `indentation_method: assumed_flat`, and none are grouping headers.

## 1. Tables

One table: **Table 01, `main_soa`**, titled "Study Parameters" (section 7). It covers document pages 69-71.
- The schedule has 8 data columns in positions 2-9: Pre-Study; Cycle 1 Day 1; Cycle 1 Day 15; Day 1 Each Subsequent Cycle; Every 12 weeks; End of Induction/End of Treatment; Continuation Phase; Follow-Up Off Therapy.
- There are 2 header property rows and 20 activities, with 71 scheduled cells and 21 annotations.
- Activity rows per page: p.69 has 15 (rows 3-17), p.70 has 5 (rows 18-22) and p.71 has 0. Page 71 holds only footnotes k, l, m, n, o, *, ^ and £, so it has no activity rows. It was not skipped.
- There are no other SoA tables in the excerpt.

## 2. Merged marks

- **Header:** row 1, columns 2-6, "Induction Phase (fulvestrant + everolimus or placebo for a maximum of 12 cycles)" is a horizontally merged cell (`merged_cell_range` "2:6").
- **Header:** columns 7-9 are merged vertically across header rows 1-2 (see D2).
- **Body:** no merged or spanning marks and no arrows.

## 3. Details

### 3.1 Header structure
- Row 1 "Phase": the name is synthesized because the label cell is empty. Type is epoch, level 1.
- Row 2 "Visit": the name is synthesized because the label cell reads "Procedure", which is the heading of the activity column. Type is visit, level 2.
- The banner is captured as footnote tb1 (D1).
- Page range: see D6.

### 3.2 Qualified mark
Vital Signs, Pre-Study: an "X" with footnote a, with "(includes Ht)" printed below it. It is kept literally as "X (includes Ht)" (D4).

### 3.3 Synthesized markers
- **tb1:** the banner, placed on schedule_property row 1 with `method: synthesized`.
- **pr1:** "see Appendix C", on activity row 7.
- **pr2:** "Section 5.6", on the header cell at row 1, column 7.

All three locations carry `method: synthesized`, and each marker is also listed in that element's `annotation_markers`.

## 4. Mechanical mark-check
- **Layer type:** the PDF has a text layer and a vector grid, so it is not image-based.
- **Method:** I ran `pdftotext -bbox` and set column x-centres from the header labels (about 244, 311, 382, 450, 521, 592, 654 and 717 pt). I then matched X tokens with `^[Xx][*a-zA-Z0-9]?$` and assigned each to the nearest column and row y-band.
- **Result:** the bbox matrix matches the visual read on 200-dpi and 100-dpi renders cell for cell, on both pages. There were no disagreements.
- **Footnote superscripts:** these were read from the render and cross-checked against their positions in the `pdftotext -layout` output.

## 5. Annotation text integrity
- **Glyph spacing:** the text layer is not glyph-spread, so no words needed rebuilding.
- **Footnote bounds:** the footnotes a-o, *, ^ and £ are separate paragraphs, each with a printed marker, so their start and end were clear.
- **Overlap:** footnote l repeats the activity label "Study Drug Compliance (Pill Diary)" but no two annotations share text.
- **Source quirks:** "HbsAg" in the row 18 label is transcribed as printed; the footnotes spell it "HBsAg".

## 6. Low-confidence calls
- **Row 2 type:** row 2 mixes visits (Pre-Study, C1D1, C1D15) with recurring occasions ("Day 1 Each Subsequent Cycle", "Every 12 weeks"). It is typed visit as the best overall fit.
- **Column 7 content:** the column-7 header holds the alternative "End of Induction Phase <OR> End of Treatment", which is kept as printed.

## 7. Orphan risk
None.
- Every marker that appears in the table is defined on pages 70-71.
- Every annotation has at least one marker location.
- Footnote o is on both the Follow-Up header cell (row 1, column 9) and the Imaging Scans cell (row 11, column 9).

## 8. Method provenance
- `indentation_method: assumed_flat` is set on all 20 activities because the table is flat.
- `method: synthesized` is set on the locations of tb1, pr1 and pr2.
- There are no `unresolved` locations and no raster, visual-read or proximity methods.
