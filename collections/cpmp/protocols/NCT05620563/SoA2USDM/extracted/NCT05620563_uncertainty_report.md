# NCT05620563 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT05620563_soa.pdf (4 PDF pages = document pages 12-15 per PAGEMAP.md; printed footers read 11-14, one lower, and were ignored). Document: ISA H0P-MC-OA05 amendment (a), 14-Oct-2022 (CT.gov Prot_000.pdf). No protocol markdown. **The SoA pages have no text layer** (pdftotext returns nothing; inventory lists pp.12-15 among the image-only pages): every text field is a visual transcription, every mark a raster pixel detection (§1a).

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.15 footnote a; rows 10, 11, 20-23, 25-27 | Footnote a ("Collect in addition to schedule described in Master Protocol CPMP and DSA CPMP(1).") is bound by text match to the nine rows whose Notes cell reads "See footnote a."; that pointer is not emitted as a note of its own. Each of these rows has its only mark at V801. | Bind footnote a to the V801 cell of each row, or emit "See footnote a." as a `source_note` per row and leave footnote a table-wide. | Footnote a |
| D2 | p.13, row 14 (Study intervention administration), note n9 | "See Section 6.1. Participant should be instructed NOT to take study intervention on the day of V7." stays one note on the row: the row has no V7 mark, so the second sentence has no cell to bind to. | Split the second sentence off and bind it to the V7 header cell (column 8), or record its location as `unresolved`. | Notes naming visits |
| D3 | p.14, row 33 (Urine pregnancy), note n14 | "For all WOCBP at V3: collect sample within 24 hours prior to dosing." is one sentence naming V3, bound to the V3 cell only. | Split it: "For all WOCBP" on the whole row, the V3 instruction on the V3 cell. | Notes naming visits |

## Recorded, not open (11)

- One table (taxonomy): header reprints on pp.12-15, no new number or caption.
- §1a image-based table: text by visual transcription (`visual_transcription` on activity names and annotation texts; `visual_read` on header grid cells); marks by near-black pixel count, method `raster_pixel_detection` on every mark.
- De-duplication (§1b): header rows counted once.
- Leading label columns (§5): L = 1; V1-V2 -> 2-3, V3 -> 4, V4-V7 -> 5-8, ED -> 9, V801 -> 10.
- §3 Header bands — typing: row 1 typed `epoch` by its spanning values (Screening, Double-Blind Treatment); its ED cell is blank (`""`); "Randomiza / tion to ISA" joined as one word (§4 Whitespace). Rows 1 and 2 have blank label cells: names synthesised ("Epoch", "Visit").
- §3 window row: typed `window`, level null; "±3" merged across V4-V7 (5:8), "±3" for V801.
- §2 table type: `main_soa` — every participant in this ISA passes through it in sequence (it prints the screening visits V1-V2 too).
- §4 Hierarchy — printed signals only: rows 5-17 precede the first section band and stay at level 0; three full-width shaded bands (rows 18, 28, 39) are mark-free group rows at level 0, their rows at level 1 (`font_signal`); the first group stays open across the page break (rows 24-27 on p.14).
- §4 row_position: rows hidden by redaction bars keep a number each (19, 35, 38, 40), unassigned. The PK sample row continues on p.15 (an empty-celled band carrying the end of its Notes text) and gets no new number.
- §6 Binding by what a note names: n2 (V3, V801, ED), n3 (V5, V7), n6 (V3), n16 (V3), n17 (V4-V7, ED) split from their cells and bound to the named cells (`text_match`); n8 names V2 in its only sentence and is bound to the V2 cell; the rest of each cell stays on the row (n1, n7, n18). "See Section 8.2.1." (n4) and "See Section 5.3.1." (n15) are bare pointers -> `source_note`.
- §6 Text below the frame: footnote a is printed below the table with its own marker; the abbreviation line above it is dropped.

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (ISA Section 1.3), document pages 12-15.
- 9 data columns (positions 2-10), 4 header rows (levels 1-3, window null).
- 32 activity rows (row positions 5-18, 20-34, 36-37, 39): 29 activities and 3 group rows.
- 97 X marks, 20 annotations (g1, a, n1-n18).
- Rows per page: p.12 = 7, p.13 = 11, p.14 = 13, p.15 = 1 (the Participant Devices band; plus the end of the PK Notes cell and two redacted regions).

## 2. Merged marks

- No body mark spans columns. Header spans: row 1 "Screening" 2:3, "Double-Blind Treatment" 5:8; row 4 "±3" 5:8.

## 3. Mechanical mark-check (raster, §1a)

- Rule-line cells from the 200 dpi raster; near-black pixel count per cell (X = 90-110 px, empty = 0; threshold 50). Validated cell by cell against direct visual reads on dense rows (Neurological assessment, Vital signs, LY3857210 PK sample) and sparse rows (Prespecified OA medical history, C-SSRS). Mark count per page: p.12 25, p.13 27, p.14 45, p.15 0; no disagreement with the visual read. Recommend a spot-check of the resolved grid, as for any image-based table.
- Urine drug screen and HbA1c (p.14) and C-SSRS Since Last Visit (p.13) border a redaction bar that hides their rule line: their row band was taken from the label column's text profile and the bar edges, then counted the same way.

## 4. Redaction

- p.13: a bar covers one band directly under the "Scales, Questionnaires, and Outcome Measures" band (label, all cells, Notes): position 19.
- p.14: a bar (about 3.5 single-line row heights) between "Urine drug screen" and "HbA1c": position 35.
- p.15: a bar between the end of the PK Notes cell and the "Participant Devices" band (position 38), and a large bar under that band (position 40) — the whole content of the Participant Devices group is hidden.
- Each hidden region may hold more than one row; the count cannot be read. The abbreviation line has two CCI boxes.

## 5. Notes that are similar on purpose

- Containment: none of the annotation texts is contained in another.
- n1 ("Physical examination (see Section 8.2.1).") and n4 ("See Section 8.2.1.") point to the same section from different rows; different texts, separate notes.

## 6. Delta schedule: what this table is, and what the schema cannot say

- g1 points to the master and the OA DSA CPMP(1) SoAs. Footnote a says nine assessments at V801 are collected *in addition to* the schedule of the master and the DSA: the only explicit statement of the allocation kind (addition) in this table. This ISA prints V1-V2 (screening) as columns, unlike OA01-OA03.
- None of this is captured as data by `table_type` or by any extraction field; it is recorded in `table_metadata.notes`, g1 and footnote a.

## 7. Method provenance

- `visual_transcription` on every activity name and annotation text; `visual_read` on every header grid cell; `raster_pixel_detection` on every mark. Indentation `font_signal`. Footnote a bound by `text_match` (Notes-cell pointer); synthesised markers g1, n1-n18. No `unresolved` locations.
