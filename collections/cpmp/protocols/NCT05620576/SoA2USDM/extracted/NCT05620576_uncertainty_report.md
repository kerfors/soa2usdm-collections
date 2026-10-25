# NCT05620576 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT05620576_soa.pdf (4 PDF pages = document pages 12-15 per PAGEMAP.md; printed footers read 11-14, one lower, and were ignored). Document: ISA H0P-MC-NP05 amendment (a), 13-Oct-2022 (CT.gov Prot_000.pdf). No protocol markdown; all text from the PDF text layer, checked against the rendered pages.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.15 footnote a; rows 10, 11, 20-23, 25-26 | Footnote a ("Collect in addition to schedule described in Master Protocol CPMP.") is bound by text match to the eight rows whose Notes cell reads "See footnote a."; that pointer is not emitted as a note of its own. Each of these rows has its only mark at V801. | Bind footnote a to the V801 cell of each row, or emit "See footnote a." as a `source_note` per row and leave footnote a table-wide. | Footnote a |
| D2 | p.13, row 14 (Study intervention administration), note n9 | "See Section 6.1. Participant should be instructed NOT to take study intervention on the day of V7." stays one note on the row: the row has no V7 mark, so the second sentence has no cell to bind to. | Split the second sentence off and bind it to the V7 header cell (column 8), or record its location as `unresolved`. | Notes naming visits |
| D3 | p.14, row 32 (Urine pregnancy), note n14 | "For all WOCBP at V3: collect sample within 24 hours prior to dosing." is one sentence naming V3, bound to the V3 cell only. | Split it: "For all WOCBP" on the whole row, the V3 instruction on the V3 cell. | Notes naming visits |

## Recorded, not open (10)

- One table (taxonomy): header reprints on pp.12-15, no new number or caption.
- De-duplication (§1b): header rows counted once.
- Leading label columns (§5): L = 1; V1-V2 -> 2-3, V3 -> 4, V4-V7 -> 5-8, ED -> 9, V801 -> 10.
- §3 Header bands — typing: row 1 typed `epoch` by its spanning values (Screening, Double-Blind Treatment); its ED cell is blank (`""`); "Randomiza / tion to ISA" joined as one word (§4 Whitespace). Rows 1 and 2 have blank label cells: names synthesised ("Epoch", "Visit").
- §3 window row: typed `window`, level null; "±3" merged across V4-V7 (5:8), "±3" for V801.
- §2 table type: `main_soa` — every participant in this ISA passes through it in sequence (it prints the screening visits V1-V2 too).
- §4 Hierarchy — printed signals only: rows 5-17 precede the first section band and stay at level 0; three full-width shaded bands (rows 18, 27, 38) are mark-free group rows at level 0, their rows at level 1 (`font_signal`); the first group stays open across the page break (PGI on p.14).
- §4 row_position: rows hidden by redaction bars keep a number each (19, 34, 37, 39), unassigned.
- §6 Binding by what a note names: n2 (V3, V801, ED), n3 (V5, V7), n6 (V3), n16 (V3), n17 (V4-V7, ED) split from their cells and bound to the named cells (`text_match`); n8 names V2 in its only sentence and is bound to the V2 cell; the rest of each cell stays on the row (n1, n7, n18). "See Section 8.2.1." (n4) and "See Section 5.3.1." (n15) are bare pointers -> `source_note`.
- §6 Text below the frame: footnote a is printed below the table with its own marker; the abbreviation line above it is dropped.

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (ISA Section 1.3), document pages 12-15.
- 9 data columns (positions 2-10), 4 header rows (levels 1-3, window null).
- 31 activity rows (row positions 5-18, 20-33, 35-36, 38): 28 activities and 3 group rows.
- 96 X marks, 20 annotations (g1, a, n1-n18).
- Rows per page: p.12 = 8, p.13 = 12, p.14 = 10, p.15 = 1 (the Participant Devices band; its content is under a bar).

## 2. Merged marks

- No body mark spans columns. Header spans: row 1 "Screening" 2:3, "Double-Blind Treatment" 5:8; row 4 "±3" 5:8.

## 3. Mechanical mark-check

- bbox binning, column centres from the visit row on each page. X count per page equals the extraction (p.12 29, p.13 30, p.14 37); per row the columns match the visual read. On p.14 the rule lines around the redaction bar are hidden; rows there were taken from the text lines.

## 4. Redaction

- p.13: a bar covers the label and V1-V801 cells of one row directly under the "Scales, Questionnaires, and Outcome Measures" band; its Notes cell is visible and blank (position 19).
- p.14: a bar between "Urine drug screen" and "HbA1c" (position 34) and a bar under "LY3857210 PK sample" (position 37).
- p.15: a large bar under the "Participant Devices" band (position 39): the whole content of that group is hidden.
- Each hidden region may hold more than one row; the count cannot be read. The abbreviation line has one CCI box.

## 5. Notes that are similar on purpose

- Containment: none of the annotation texts is contained in another.
- n1 ("Physical examination (see Section 8.2.1).") and n4 ("See Section 8.2.1.") point to the same section from different rows; different texts, separate notes.

## 6. Delta schedule: what this table is, and what the schema cannot say

- g1 points to the master and the DPNP DSA CPMP(3) SoAs. Footnote a says eight assessments at V801 are collected *in addition to* the schedule of the Master Protocol CPMP (as in BP05; OA05 names the master and the DSA).
- None of this is captured as data by `table_type` or by any extraction field; it is recorded in `table_metadata.notes`, g1 and footnote a.

## 7. Method provenance

- Default methods throughout (text layer, bbox). Indentation `font_signal`. Footnote a bound by `text_match` (Notes-cell pointer); synthesised markers g1, n1-n18. No `unresolved` locations.
