# NCT04627038 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT04627038_soa.pdf (4 PDF pages = document pages 10-13 per PAGEMAP.md; printed footers read 9-12, one lower, and were ignored). Document: ISA H0P-MC-OA03 amendment (a), 03-Mar-2021 (CT.gov Prot_000.pdf). No protocol markdown; all text from the PDF text layer, checked against the rendered pages.

## Decisions needed (2)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | pp.11-13, row 5 (Telephone visit), V3 cell, notes a, b, n1 | The shaded "Telephone visit" row, printed with the header on every page, is header row 5 (`visit_attribute`, level null) with "X" and footnotes a, b in the V3 grid cell. The two telephone visits (4 and 8 days after Visit 3) are not columns of the table. | An activity row "Telephone visit" with one mark at V3 (counted once), or two visits of their own between V3 and V4. | Telephone visit row |
| D2 | p.11, row 6 ("Procedures" band) | The shaded "Procedures" band is a label for the activity column, not emitted; row 6 unassigned; rows 7-12 stay at level 0. | A mark-free group row at level 0 with rows 7-12 as its level-1 children. | Procedures band |

## Recorded, not open (10)

- One table (taxonomy): header reprints on pp.11-13, no new number or caption.
- De-duplication (§1b): header rows, including the Telephone visit row and its Notes cell, counted once (text taken from p.11, where the cell is least redacted).
- Leading label columns (§5): L = 1; V3-V7 -> 2-6, ED -> 7.
- §3 Header bands — typing: row 1 typed `epoch` by its spanning value (Double-Blind Treatment). Name synthesised: the label cell prints "H0P-MC-OA03 ISA". "Early / Discontinu / ation" joined without spaces (§4 Whitespace: a word broken by a narrow column).
- §3 window row: "Visit Window (days)" typed `window`, level null; four separate ±3 cells.
- §2 table type: `main_soa` — every participant randomised into this ISA passes through it in sequence. See section 6.
- §4 Hierarchy — printed signals only: "Laboratory Tests and Sample Collection" (row 13) and "Participant Device" (row 26) are full-width shaded bands, mark-free group rows at level 0; their rows at level 1 (`font_signal`); the first group stays open across the page break (HbA1c, PK sample, Somatostatin-related biomarker sample on p.13).
- §4 row_position: the fully redacted row between "Somatostatin-related biomarker sample" and the "Participant Device" band keeps its number (25) unassigned.
- §6 Binding by what a note names: n2 (V3-V7) names its cells in its only sentence; n3/n4 (ECG), n7/n8 (vital signs), n10 (study intervention), n14/n15/n16 (PK) split from their cells and bound to the named cells (`text_match`); the rest of each cell stays on the row (n5, n6, n9, n17). n1 names Visit 3 and Visit 4 as reference points for the telephone visits and stays on the Telephone visit row.
- §6 Redacted content: footnotes a and b end in a CCI box ("[remainder redacted in source]"); the Notes cell of "Participant returns device" is wholly covered (n18).

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (ISA Section 1.3), document pages 10-13.
- 6 data columns (positions 2-7), 5 header rows (levels 1-3; window and Telephone visit null).
- 20 activity rows (row positions 7-24, 26-27): 18 activities and 2 group rows; one activity (Urine drug screen) has no mark, only "Performed at investigator discretion".
- 81 X marks in the body, plus one 'X' in the Telephone visit header cell; 21 annotations (g1, a, b, n1-n18).
- Rows per page: p.10 = 0 (Section 1.3 heading and intro text, g1), p.11 = 4, p.12 = 11, p.13 = 5 (plus one redacted row, section 4).

## 2. Merged marks

- No body mark spans columns. Header span: row 1 "Double-Blind Treatment" 3:6.

## 3. Mechanical mark-check

- bbox binning, column centres from the Visit Number row on each page. Body X count per page equals the extraction (p.11 19, p.12 46, p.13 16; the reprinted Telephone visit 'X' once per page is excluded and counted once as a header cell); per row the columns match the visual read. Cell boundaries from raster rule lines (§1d) agree.

## 4. Redaction

- Telephone visit Notes cell: on pp.11 and 13 a CCI box follows "Visit 3" in both footnote lines a and b; on p.12 the whole cell is covered. Text taken from p.11.
- p.13: a CCI bar covers one full row (label, V3-ED, Notes) between "Somatostatin-related biomarker sample" and the "Participant Device" band. Not transcribed; row position 25 unassigned.
- p.13: the Notes cell of "Participant returns device" is covered (n18).

## 5. Notes that are similar on purpose

- Containment: n12 "For WOCBP only" (Serum pregnancy) is contained in n13 (Urine pregnancy), whose cell prints the same line and a second one, "Collect sample within 24 hours prior to first dose." Re-verified on p.12: two separate Notes cells, source-faithful; not a split cell.
- n3/n7 and n4/n8 share visit lists (V3, V5, V7 / V4, V6, ED) but differ in wording; separate notes.

## 6. Delta schedule: what this table is, and what the schema cannot say

- The intro text (g1) points to the master and the OA DSA CPMP(1) SoAs. The participant's full schedule is the union of three SoAs; the DSA is not public.
- This ISA adds two telephone visits between V3 and V4 (footnotes a, b) and no follow-up visits; it has no cross-document ED override note.
- None of this is captured as data by `table_type` or by any extraction field; it is recorded in `table_metadata.notes` and in g1.

## 7. Method provenance

- Default methods throughout (text layer, bbox). Indentation `font_signal` (section bands). Synthesised markers g1, n1-n18; printed markers a, b. No `unresolved` locations.
