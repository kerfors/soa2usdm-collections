# NCT03817853 — SoA extraction uncertainty report

Prompt 3.8.1, schema soa-table-extraction 1.0. Source: NCT03817853_soa.pdf (3 PDF pages = document pages 100–102 per PAGEMAP.md). No protocol markdown available; all text comes from the PDF text layer.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.100, rows 24–26 (Study drug administration / obinutuzumab / chemotherapy) | "Study drug administration" (a label cell spanning two sub-rows) is a mark-free parent row (level 0); "obinutuzumab" and "chemotherapy" are child activities (level 1) that carry the marks. | Two flat level-0 activities, "Study drug administration – obinutuzumab" and "Study drug administration – chemotherapy", with no separate parent row. | 3.2 |
| D2 | p.100, header row 2 (Induction / EOI / Maintenance / EOM) | The row is typed `period` (sub-phases of Treatment). EOI and EOM are single-column entries in that row. | Type the row `visit` or `other`, because EOI and EOM are end-of-phase visits rather than periods. | 3.3 |
| D3 | p.100, header rows 1–4 | A header cell that spans several header rows (Screening rows 1–3; EOI, Maintenance and EOM rows 2–4; Follow-up rows 1–4) is recorded only on the top row it occupies. The lower cells it covers are left empty. | Repeat the label in every header row the cell covers (for example, "EOI" also in the Cycle and Day rows). | 3.3 |

## Recorded, not open (6)

- §2: The single table is `main_soa`, because it is the only SoA grid and it is the anchor.
- §4: Pages 101–102 contribute no activity rows. They are footnote pages that continue the table's own notes (the general "Notes:" paragraph and footnotes a–aa).
- §5: There is L = 1 label column, so the first data column is position 2. The split label area of the Study drug administration row counts as the same label column (see D1).
- §6: The abbreviation paragraph under the grid ("(a)PTT=…; D=day; … PT=prothrombin time.") is not emitted. None of its terms appears as a marker in the grid, so it yielded zero annotations.
- §6: The unmarked general "Notes:" paragraph (top of p.101) is a table-scope `footnote` with the synthesized marker `gn1`. Its one `schedule_property` location (row 1) has `method: "synthesized"`. Following the Notes-header table-scope convention, the marker is deliberately NOT added to any element's `annotation_markers`.
- §6: Footnotes b, c, d, s and w contain "see Section/Table …" references but also explain something, so they are typed `footnote`, not `source_note`.

## 1. Tables

| Table | type | pages | data columns | activities | activity_schedule entries | annotations |
|---|---|---|---|---|---|---|
| 1 "Appendix 1 Schedule of Activities" | main_soa | 100–102 | 11 (positions 2–12) | 27 (incl. 1 mark-free parent) | 89 | 28 (a–z, aa, gn1) |

Activity rows per page: p.100 = 27, p.101 = 0, p.102 = 0. Pages 101–102 hold footnotes only, which is expected and not a skipped page.

Columns: 2 = Screening D–28 to D–1; 3 = Screening D–7 to D–1; 4/5/6 = Cycle 1 D1/D8/D15; 7 = Cycle 2 D1; 8 = Cycles 3–6/8 D1; 9 = EOI; 10 = Maintenance (every 8 weeks ± 10 days); 11 = EOM; 12 = Follow-up (3 months).

## 2. Merged-mark decisions

- There are no merged body cells. Every body row has all internal vertical rules (checked on the 200 dpi raster), so every mark is a single-column entry and no `source_range` is set.
- The header cells span columns: Screening 2:3, Treatment 4:11, Induction (6–8 cycles) 4:8, Cycle 1 4:6. The printed markers a (Screening) and b (Induction) are placed on every column their merged cell covers.

## 3. Synthesised / interpretation

3.1 The synthesized `property_name`s are "Study period" (row 1), "Treatment phase" (row 2) and "Cycle" (row 3), because their label cells are empty. Row 4 uses its printed label "Day". The synthesized annotation marker is `gn1`.

3.2 (D1) In the label area, "Study drug administration" is printed in its own sub-column, vertically spanning two sub-rows, with "obinutuzumab r" and "chemotherapy s" beside it. All other rows are flat level-0 activities that carry marks, so their `indentation_method` is left at the default, since the table has no indentation. Rows 24–26 have `indentation_method: visual_estimate`.

3.3 (D2, D3) The header hierarchy is epoch (1) → period (2) → cycle (3) → study_day (4). Row 2 has `structure_method: inferred_from_layout`.

## 4. Mechanical mark-check

This is a text-layer page with vector rules. I ran `pdftotext -bbox` and binned each `x` token into the column bands bounded by vertical rules recovered from the 200 dpi raster. The bbox matrix agreed with the visual read cell for cell in all 27 rows, and I found no disagreements. Notable cells:

- Chemistry has no mark in C1D1 (col 4), while Hematology does.
- Patient-reported measures has no marks in C1D8, C1D15 or Screening.
- Provider-reported measures has one mark, in col 8 (Cycles 3–6/8), carrying the superscript footnote marker "x". This lowercase marker is the same letter as the grid mark "x". The bbox layer separates the two (a small superscript token above a regular token), so the cell is `cell_value: "x"`, `annotation_markers: "x"`.
- Marks are lowercase "x" throughout and were transcribed literally.

## 5. Annotation text integrity

- The text layer is not glyph-spread, so no reconstruction was needed. Footnote text was taken from `pdftotext -layout` of pp.101–102, with hanging-indent lines joined.
- No annotation's text is contained in another's.
- Wording was transcribed literally even where it looks like a source typo: "G-chemo indication therapy" (b; probably "induction"), "administered according a 90-minute" (d), "on Days, 1, 8 and 15" (r).
- The general "Notes:" text keeps its leading "Notes:" label.

## 6. Low-confidence calls

- Footnote v ("Concomitant medications and adverse events will be collected throughout the study") is printed as a marker only on the Concomitant medications label, not on Adverse events. It is bound as printed and was not extended to row 29.
- Footnote x refers to "Cycle 4 Day 1", but its mark sits in the combined "Cycles 3–6/8 D1" column (col 8). It is transcribed as printed.
- Footnote u markers appear on the label and on the Screening/C1D1 cells (cols 2–4) of Concomitant medications. Footnote w markers appear on the label and on cols 2–4 of Adverse events. All are bound as printed.

## 7. Orphan risk

None. Every one of the 28 annotations has at least one marker_location, and every non-synthesized location's marker is also in that element's `annotation_markers`. Every marker used in the grid has a printed definition.

## 8. Method provenance

- `activity_name_source.indentation_method: visual_estimate` is set on rows 24, 25 and 26.
- `structure_method: inferred_from_layout` is set on schedule_property row 2.
- `marker_locations[].method: synthesized` is set once, on gn1 → schedule_property row 1.
- No `unresolved` locations, no proximity-bounded notes, and no raster or visual-read cell methods (marks read from the bbox text layer).
