# NCT04707157 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT04707157_soa.pdf (5 PDF pages = document pages 9-13 per PAGEMAP.md; printed footers read 8-12, one lower, and were ignored). Document: ISA H0P-MC-NP03 amendment (b), 01-Oct-2021 (CT.gov Prot_000.pdf). No protocol markdown. Text from the PDF text layer: normal on p.9, **glyph-spread on pp.10-13** (rebuilt, §1c), checked against the rendered pages.

## Decisions needed (1)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | pp.10-13, row 5 (Telephone Visit), V3 and V801 cells, notes a, b, c, n1 | The shaded "Telephone Visit" row, printed with the header on every page, is header row 5 (`visit_attribute`, level null) with "X" + a, b at V3 and "X" + c at V801. Footnote c suggests V801 itself is a telephone visit; a and b are redacted. | An activity row "Telephone Visit" with marks at V3 and V801 (counted once), or the telephone visits behind a and b as visits of their own between V3 and V4. | Telephone visit row |

## Recorded, not open (11)

- One table (taxonomy): header reprints on pp.10-13, no new number or caption.
- De-duplication (§1b): header rows, including the Telephone Visit row and its Notes cell, counted once.
- §4 Do NOT create activity rows for non-activities: the shaded "Procedures" band is reprinted under the header on every page — a repeated column-label band, not emitted; its row position 6 is left out and rows 7-13 stay at level 0.
- Leading label columns (§5): L = 1; V3-V7 -> 2-6, ED -> 7, V801 -> 8.
- §3 Header bands — typing: row 1 typed `epoch` by its spanning value (Double-Blind Treatment); "Follow- / up" joined as "Follow-up" (§4 Whitespace). Name synthesised: the label cell prints "H0P-MC-NP03 ISA".
- §3 window row: "Visit Window (days)" typed `window`, level null; four separate ±3 cells.
- §2 table type: `main_soa` — every participant randomised into this ISA passes through it in sequence. See section 6.
- §4 Hierarchy — printed signals only: "Laboratory Tests and Sample Collection" (row 14; its text prints right of centre in the full-width band) and "Participant Device" (row 27) are mark-free group rows at level 0; their rows at level 1 (`font_signal`); the first group stays open across the page break (rows 21-25 on p.12).
- §4 row_position: the row on p.12 whose label and V3-ED cells are redacted keeps its number (26) unassigned.
- §6 Binding by what a note names: n2 (V3-V7) and n11 (V3; one text in the Hematology and Chemistry cells, deduplicated) name their cells in their only sentence; n3/n4 (ECG), n7/n8 (vital signs), n10 (study intervention), n15/n16/n17 (PK) split from their cells and bound to the named cells (`text_match`); the rest of each cell stays on the row (n5, n6, n9, n18).
- §6 Markers referenced but not defined / Redacted content: footnotes a and b are wholly redacted — their annotations say so and carry a clearly-labelled probable equivalent from OA03/BP03; the Notes cell of "Participant returns device" is covered (n19).

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (ISA Section 1.3), document pages 9-13.
- 7 data columns (positions 2-8), 5 header rows (levels 1-3; window and Telephone Visit null).
- 21 activity rows (row positions 7-25, 27-28): 19 activities and 2 group rows; one activity (Urine drug screen) has no mark.
- 82 X marks in the body, plus two 'X' in the Telephone Visit header row; 23 annotations (g1, a, b, c, n1-n19).
- Rows per page: p.9 = 0 (Section 1.3 heading and intro text, g1), p.10 = 4, p.11 = 10, p.12 = 5, p.13 = 2 (plus one redacted row on p.12).

## 2. Merged marks

- No body mark spans columns. Header span: row 1 "Double-Blind Treatment" 3:6.

## 3. Mechanical mark-check

- bbox binning, column centres from the Visit Number row on each page (glyph-spread 'V 801', 'E D' merged before binning). Body X count per page equals the extraction (p.10 19, p.11 35, p.12 26, p.13 2; the reprinted Telephone Visit marks excluded and counted once as header cells); per row the columns match the visual read. Cell boundaries from raster rule lines (§1d) agree.

## 4. Redaction

- Telephone Visit Notes cell: on every page the lines between "Visit window +2 days" and footnote c are covered by CCI boxes (footnotes a and b).
- p.12: a CCI bar covers the label and the V3-ED cells of one row below "Somatostatin-regulated hormone sample"; its V801 and Notes cells are visible and blank. Position 26 unassigned.
- p.13: the Notes cell of "Participant returns device" is covered (n19).

## 5. Text-layer reconstruction (§1c) and similar notes

- pp.10-13 are glyph-spread. Rebuilt fields: every activity name (`glyph_reconstruction`), every Notes-cell annotation (`deglyph_reconstruction`), and the header cell values. Re-read as prose against the page.
- Containment: n13 "For WOCBP only" (Serum pregnancy) is contained in n14 (Urine pregnancy). Re-verified on p.11: two separate Notes cells, source-faithful.
- Compared with BP03 and OA03 (same compound, LY3556050): same body rows and marks, plus an "AEs" row with one mark at V801 and the V801 column itself; ECG note says "Single safety ECG".

## 6. Delta schedule: what this table is, and what the schema cannot say

- The intro text (g1) points to the master and the DPNP DSA SoAs, without document numbers. The participant's full schedule is the union of three SoAs; the DSA is not public.
- This ISA (amendment b) adds V801 (footnote c: 4-10 days after last dose) to the OA03/BP03 pattern; AEs at V801 is the only body mark in that column.
- None of this is captured as data by `table_type` or by any extraction field; it is recorded in `table_metadata.notes` and in g1.

## 7. Method provenance

- `glyph_reconstruction` on every activity name; `deglyph_reconstruction` on n1-n18 and c. Indentation `font_signal`. Synthesised markers g1, n1-n19; printed markers a, b, c. No `unresolved` locations.
