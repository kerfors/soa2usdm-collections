# NCT04874636 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT04874636_soa.pdf (4 PDF pages = document pages 8-11 per PAGEMAP.md; printed footers read 7-10, one lower, and were ignored). Document: ISA H0P-MC-BP03, original ISA, 17-Mar-2021 (CT.gov Prot_000.pdf). No protocol markdown. Text from the PDF text layer: normal on p.8, **glyph-spread on pp.9-11** (rebuilt, §1c), checked against the rendered pages.

## Decisions needed (1)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | pp.9-11, row 5 (Telephone Visit), V3 cell, notes a, b, n1 | The shaded "Telephone Visit" row, printed with the header on every page, is header row 5 (`visit_attribute`, level null) with "X" and footnotes a, b in the V3 grid cell. The two telephone visits (4 and 8 days after Visit 3) are not columns of the table. | An activity row "Telephone Visit" with one mark at V3 (counted once), or two visits of their own between V3 and V4. | Telephone visit row |

## Recorded, not open (11)

- One table (taxonomy): header reprints on pp.9-11, no new number or caption.
- De-duplication (§1b): header rows, including the Telephone Visit row and its Notes cell, counted once (text from p.9; the p.10 reprint ends footnote b with a '.' after the CCI box, p.9 and p.11 do not).
- §4 Do NOT create activity rows for non-activities: the shaded "Procedures" band is reprinted under the header on every page — a repeated column-label band, not emitted; its row position 6 is left out and rows 7-12 stay at level 0.
- Leading label columns (§5): L = 1; V3-V7 -> 2-6, ED -> 7.
- §3 Header bands — typing: row 1 typed `epoch` by its spanning value (Double-Blind Treatment). Name synthesised: the label cell prints "H0P-MC-BP03 ISA".
- §3 window row: "Visit Window (days)" typed `window`, level null; four separate ±3 cells.
- §2 table type: `main_soa` — every participant randomised into this ISA passes through it in sequence. See section 6.
- §4 Hierarchy — printed signals only: "Laboratory Tests and Sample Collection" (row 13) and "Participant Device" (row 26) are full-width shaded bands, mark-free group rows at level 0; their rows at level 1 (`font_signal`); the first group stays open across the page break (PK sample, Somatostatin-regulated hormone sample on p.11).
- §4 row_position: the fully redacted row between "Somatostatin-regulated hormone sample" and the "Participant Device" band keeps its number (25) unassigned.
- §6 Binding by what a note names: n2 (V3-V7) and n11 (V3; one text in the Hematology and Chemistry cells, deduplicated) name their cells in their only sentence; n3/n4 (ECG), n7/n8 (vital signs), n10 (study intervention), n15/n16/n17 (PK) split from their cells and bound to the named cells (`text_match`); the rest of each cell stays on the row (n5, n6, n9, n18). n1 names Visit 3 and Visit 4 as reference points and stays on the Telephone Visit row.
- §6 Redacted content: footnotes a and b end in a CCI box ("[remainder redacted in source]"); the Notes cell of "Participant returns device" is covered (n19).

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (ISA Section 1.3), document pages 8-11.
- 6 data columns (positions 2-7), 5 header rows (levels 1-3; window and Telephone Visit null).
- 20 activity rows (row positions 7-24, 26-27): 18 activities and 2 group rows; one activity (Urine drug screen) has no mark, only "Performed at investigator discretion.".
- 81 X marks in the body, plus one 'X' in the Telephone Visit header cell; 22 annotations (g1, a, b, n1-n19).
- Rows per page: p.8 = 0 (Section 1.3 heading and intro text, g1), p.9 = 5, p.10 = 11, p.11 = 4 (plus one redacted row, section 4).

## 2. Merged marks

- No body mark spans columns. Header span: row 1 "Double-Blind Treatment" 3:6.

## 3. Mechanical mark-check

- bbox binning, column centres from the Visit Number row on each page (glyph-spread 'E D' merged before binning). Body X count per page equals the extraction (p.9 24, p.10 43, p.11 14; the reprinted Telephone Visit 'X' once per page excluded and counted once as a header cell); per row the columns match the visual read. Cell boundaries from raster rule lines (§1d) agree.

## 4. Redaction

- Telephone Visit Notes cell: on every page a CCI box follows "Visit 3" in footnote lines a and b. Text taken from p.9.
- p.11: a CCI bar covers one full row (label, V3-ED, Notes) between "Somatostatin-regulated hormone sample" and the "Participant Device" band. Not transcribed; row position 25 unassigned.
- p.11: the Notes cell of "Participant returns device" is covered (n19).

## 5. Text-layer reconstruction (§1c) and similar notes

- pp.9-11 are glyph-spread ('Physicalexamination', 'Te mperatureand Respiratory Rate', 'For W O C BP only'). Rebuilt fields: every activity name on pp.9-11 (`glyph_reconstruction`) and every Notes-cell annotation (`deglyph_reconstruction`); header cell values rebuilt the same way. Re-read as prose against the page.
- Containment: n13 "For WOCBP only" (Serum pregnancy) is contained in n14 (Urine pregnancy). Re-verified on p.10: two separate Notes cells, source-faithful.
- Compared with OA03 (same compound, LY3556050): same rows and marks, except "Somatostatin-regulated hormone sample" (OA03: "Somatostatin-related biomarker sample"); BP03 adds "V3: Collect laboratory tests before dosing." to Hematology and Chemistry, and prints the Procedures band on every page.

## 6. Delta schedule: what this table is, and what the schema cannot say

- The intro text (g1) points to the master and the CLBP DSA SoAs, without document numbers. The participant's full schedule is the union of three SoAs; the DSA is not public.
- This ISA adds two telephone visits between V3 and V4 (footnotes a, b) and no follow-up visits; no cross-document ED override note.
- None of this is captured as data by `table_type` or by any extraction field; it is recorded in `table_metadata.notes` and in g1.

## 7. Method provenance

- `glyph_reconstruction` on every activity name; `deglyph_reconstruction` on every annotation except g1 and n19. Indentation `font_signal` (section bands). Synthesised markers g1, n1-n19; printed markers a, b. No `unresolved` locations.
