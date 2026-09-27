# NCT05176314 - SoA extraction uncertainty report

Prompt 3.8.1, single pass. Source: NCT05176314_soa.pdf (3 PDF pages = document pages 10, 11, 12 per PAGEMAP.md). No protocol markdown was available, so all text comes from the PDF text layer.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p. 11, between rows 21/22 and rows 22/23 | The two full-width black redaction bars labelled "CCI" on page 11 are treated as hidden content. No activity rows are created for them. | Each bar hides one or more activity rows, with their own marks. Add placeholder rows (e.g. "[redacted row]") so the hidden procedures are represented. | 4 |
| D2 | p. 11, row 21 "Genetic blood sample for screening" | The label is transcribed from its three visible lines as "Genetic blood sample for screening". The last line is partly clipped by the redaction bar below it. | Part of the label is under the bar (the text layer places the bar's "CCI" label between "for" and "screening"). Record the name as incomplete, e.g. "Genetic blood sample for [redacted] screening". | 4 |
| D3 | p. 10, header rows 1-2, column 2 | The rotated Screening header cell covers both header rows. It is split by its two printed lines: "Screening" goes on the epoch row and "(D-42 to -2)" on the study-day row. | Keep the whole text "Screening (D-42 to -2)" as one value on the epoch row, and leave the Screening column empty on the study-day row. | 3 |

## Recorded, not open (7)

- §2 / type definitions: one table was printed across pages 10-11, with the header reprinted on page 11. It was extracted as a single `main_soa`, not as a `continuation`, because the source gives the overflow no number or caption of its own. The reprinted header was de-duplicated (§1b).
- §6 source defect: markers d (Supine vital signs), f (on "P" in the Day 6 PK cell) and g (Rosuvastatin PK samples) are printed in the table, but their footnote text is not. On page 12 their positions are covered by "CCI" redaction boxes. Each marker got an annotation whose text says the definition is not printed, ending in "[remainder redacted in source]". No text was made up.
- §6 abbreviations: the abbreviation block (BP, CRU, D, ECG, ED, FU, h, P, PK, PR) produced only two annotations, P = predose and h = hours postdose. Both are typed `legend` because they are in-grid scheduling marks, and each is bound to every cell that uses it (P: 12 cells, h: 23 cells). The other terms appear only in labels or header text, so they were dropped. No text_match or synthesized bindings were used.
- §6 / §5: P and h stay in `cell_value` as scheduling marks. To keep binding consistent, they are also listed in those cells' `annotation_markers`.
- §6 header-cell footnotes: marker a sits on the Day 18 header cell (row 2, col 21) and marker b on the FU/ED header cell (row 1, col 22). Each was put on that column's `schedule_grid` cell, not on the property row.
- §4: page 12 is outside the declared SoA range (PAGEMAP). It was used only for footnote text, and `page_end` = 11.
- §5: there is L = 1 label column ("Study Procedure"), so the first data column is position 2. Columns: 2 = Screening, 3 = Day -1, 4-21 = Days 1-18, 22 = FU/ED Day 24 (± 2 days).

## 1. Per table

**Table 01**: `main_soa`, "Schedule of Activities (SoA)" (Section 1.3), document pages 10-11.

- 21 data columns (positions 2-22), 2 header rows, 22 activities, 122 non-empty schedule cells, 9 annotations.
- Activity rows per page: p. 10 contributed 15 (rows 3-17) and p. 11 contributed 7 (rows 18-24). Page 12 is outside the range and holds footnotes only.

## 2. Merged-mark decisions

- No body cell is merged. Raster rule lines show every internal vertical boundary present in every body band on both pages.
- Header: "Treatment Period (Study Days)" is merged across columns 3-21 (`merged_cell_range` "3:21"). The span was confirmed by the missing vertical rules in the header band.
- The Screening cell (col 2) is merged vertically across both header rows (see D3).
- Pirtobrutinib administration (Days 6-17) is 12 separate X cells, not a span.

## 3. Synthesised

- Property names "Epoch" (row 1) and "Study Day" (row 2) were synthesised because the label column holds "Study Procedure" merged across both header rows. Row 1 has `structure_method: inferred_from_layout`.
- No annotation markers were synthesised.

## 4. Mechanical mark-check and redactions

- **Method:** the text layer is present and not glyph-spread. Cell boundaries were recovered from 200-dpi raster rule lines (§1d): 23 vertical rules on each page, and horizontal rules taken from the label column. Each pdftotext -bbox token was assigned to its cell.
- **Result:** the mechanical matrix matched the visual read cell for cell on both pages. There were no disagreements.
- **Text values transcribed literally:** "P, 2 h" and "24 h" (vital signs), "P" (ECG and clinical labs on Days 1, 6 and 13), and "P, 0.5, 1, 1.5, 2, 2.5, 3, 4, 5, 6, 8, 12 h" and "24 h"-"120 h" (PK). The wrapped lines "24 / h" were joined into "24 h". PK on Day 12 (col 15) is empty in the source.
- **Redactions (D1):** on page 11, the two full-width bars cover the row bands they sit in, so the label-column horizontal rules merge there. Row boundaries were therefore read from the unredacted bands. No text exists under either bar in the text layer (only the "CCI" labels). Rows may be hidden there.
- **D2:** the bottom of the Genetic blood sample label cell is clipped by the first bar.

## 5. Annotation text integrity

- Footnotes a, b, c and e were read directly from the page 12 text layer. There is no glyph spreading, and none of the texts contains another.
- d, f and g are placeholders stating that the source does not print their text. The page 12 text layer has no hidden text under the redaction boxes.

## 6. Low-confidence calls

- The FU/ED column's "24 (± 2 days)" is kept whole on the study_day row. No separate window row was created, because the source prints none.
- Row 1 is typed `epoch`, since it holds Screening, Treatment Period and FU/ED.

## 7. Orphan risk

- Every annotation has at least one marker location, and each location's marker is also in that element's `annotation_markers`.
- d, f and g are referenced but have no printed definition (see above).

## 8. Method provenance

- `structure_method: inferred_from_layout` on schedule_property row 1 ("Epoch"), whose type and level come from the band layout, not a printed label.
- Every other value was read by the default method (bbox text layer).
- No `unresolved` locations and no proximity or text_match bindings.
