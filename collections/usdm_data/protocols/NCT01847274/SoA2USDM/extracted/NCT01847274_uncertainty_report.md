# NCT01847274 — SoA extraction uncertainty report

Prompt 3.8.1 (single pass). Source: NCT01847274_soa.pdf (7 PDF pages = document pages 63, 64, 65, 66, 72, 73, 78 per PAGEMAP.md). No protocol markdown was available, so all text comes from the PDF text layer.

## Decisions needed (7)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p63, Table 1 row 1 "Cycle" (also Table 3 row 1, p78) | Header row labelled "Cycle" typed as a **cycle** row | Type it as an **epoch** row, because it also holds study phases (Screening, Discontinuation, Post Treatment) | 2.3 |
| D2 | p66, Table 1 row 30 "Bone marrow aspirate and biopsy" | Top-level activity (indent 0); the single leading space in the source cell is treated as a typing artefact | Read the leading space as indentation, making it a child (level 1) of "Survival assessment" | 2.3 |
| D3 | p72, Table 2 (printed Table 8) | Typed **track**, label "Food Effect Sub-Study" (separate sub-study population with its own FE Day 1/FE Day 8 visits) | Type as **main_soa** (independent schedule table, no track label) | 3.1 |
| D4 | p73, Table 2 header markers 12, 13 | Reprinted header de-duplicated; markers 12/13 carry the same text as 1/2 and are not emitted separately | Emit footnotes 12 and 13 as their own notes on the header cells | 3.4 |
| D5 | p73, Table 2 full-width "Note: Patients in food effect sub-study…" row | Not an activity; one table-wide footnote, synthesised marker `n1`, anchored on header row 1 | Bind it to specific activity rows, or leave its scope `unresolved` | 3.3 |
| D6 | p78, Table 3 (printed Table 9) | Typed **track**, label "Extended Visit Cycle" (alternative later-phase visit pattern for main-study patients) | Type as **main_soa**, or as **subsidiary** of Table 1's "Subsequent Cycles" column | 4.1 |
| D7 | p78, Table 3 markers 4–15 (rows 5, 7, 10–18) | Footnote texts are not printed in the excerpt; each note states "definition not printed" and adds a clearly-labelled *probable* equivalent from Table 1 where one exists | State only "definition not printed", no cross-reference | 4.3 |

## Recorded, not open (8)

- Table numbering: `table_number` is sequential 1–3 across the protocol's SoA tables (schema definition); printed numbers are Table 7 → 1, Table 8 → 2, Table 9 → 3, recorded in each `table_metadata.notes`.
- Printed Table 8 page 73 "(Continued)" is the same printed table under a reprinted header → extracted as one table spanning pp72–73, not a `continuation` (taxonomy: page break inside one printed table).
- Reprinted header rows on pp64–66 (Table 1) and p73 (Table 2) de-duplicated (§1b).
- Abbreviation blocks (Table 1 p63; Table 2 pp72/73) yield zero annotations: no term is printed as a marker in any grid cell, header or label (§6).
- Leading label columns: L = 1 for all three tables; first data column = 2 (§5).
- Synthesised `property_name`s: Table 2 rows 1 "Period" and 2 "Cycle" (unlabelled header bands; §3), `structure_method: inferred_from_layout`.
- Superscript "20" in the bone-marrow footnote ("WHO criteria²⁰", Table 1 fn 28 / Table 2 fn 15) is a bibliographic reference citation in the note text, not a table marker; kept as text.
- Vertical placement of X marks inside the tall FOSI/EQ-5D-5L row (p65: Screening and Post-Treatment marks top-aligned, others centred) is presentation only — one row, all marks kept.

## 1. Method

- Text layer present on all pages (not glyph-spread; no §1c reconstruction needed). No image-based pages.
- Rule lines recovered from 200-dpi renders (§1d; used uniformly). Vertical rules gave 10 column bands (Table 1), 9 (Table 2), 6 (Table 3); horizontal row bands read from the label column.
- Mechanical mark check (§1b): `pdftotext -bbox` tokens binned to raster column bands per row band; merged spans taken from missing internal vertical rules within each row band. **The bbox matrix agreed with the visual read cell-for-cell on every table; no disagreements.**

## 2. Table 1 — printed Table 7 "Schedule of Events – Main Study" (pp63–66)

### 2.1 Summary
- `main_soa`. 9 data columns (2–10): Screening; Cycle 1 D1, D8, D15, D21; Cycle 2 D1; Subsequent Cycles (Cycle n, Day 1); Study Treatment Discontinuation; Post Treatment Assessments.
- 2 schedule properties (Cycle, Day); 28 activities; 90 schedule cells; 28 annotations (all footnotes).
- Activity rows per page: p63 = 7, p64 = 11, p65 = 6, p66 = 4. Every page contributed.

### 2.2 Merged cells
- Header row 1 "1" (Cycle 1) merged across columns 3:6.
- "Bone marrow aspirate and biopsy" (row 30): one X²⁸ in a cell merged across columns **3:10** (Cycle 1 Day 1 through Post Treatment; Screening excluded). Distributed to 8 cells with `source_range "3:10"`; marker 28 placed on every covered cell.

### 2.3 Low-confidence calls
- D1: row 1 values mix numbered cycles with phases; kept as `cycle` after the printed label "Cycle". Same call for Table 3 row 1.
- D2: Only "Bone marrow aspirate and biopsy" has a leading space in the cell (p66); the table has no grouping headers and the row carries its own mark, so all rows are level 0 (`indentation_method: assumed_flat`). Raw leading space kept in `cell_text`.

### 2.4 Annotations
- Markers on the header: 1 on the "Cycle" row label (schedule_property), 2 on the Subsequent Cycles header cell (col 8), 3 on the C1 "Day 1" header cell (col 3).
- Footnote 14 ends "(Section 6.1)" but also explains — kept as `footnote`.
- Footnote 19 has an unbalanced parenthesis "(within 30± 15 minutes Note:" — transcribed as printed.
- No containment pairs: fn 10 (Table 1) and fn 14 (Table 2) share the same text ("SAEs recorded…"), and fn 28 / Table 2 fn 15 share the bone-marrow text, but these are in different tables and are source-faithful repeats.

## 3. Table 2 — printed Table 8 "Schedule of Events – Open-Label Food Effect Sub-Study (Study Completed)" (pp72–73)

### 3.1 Classification
- `track`, `track_label` "Food Effect Sub-Study" (D3). Different population (sub-study patients) and different columns (FE Day 1, FE Day 8 ahead of Cycle 1). 8 data columns (2–9): Screening; FE 1; FE 8; C1 D1; C1 D15; C2 D1; Subsequent Cycles; Treatment Discontinuation.
- 3 schedule properties (Period [synth], Cycle [synth], Day); 17 activities; 79 schedule cells; 14 annotations.
- Activity rows per page: p72 = 14, p73 = 3.

### 3.2 Merged cells
- Header: "14-Day Food Effect" 3:4, "Cycle1" 5:7 (row 1); "C1" 5:6 (row 2). Screening, 14-Day Food Effect, Subsequent Cycles and Treatment Discontinuation cells span header rows 1–2 vertically; their row-2 grid cells are recorded as empty.
- "Bone marrow aspirate and biopsy" (row 19): X¹⁵ merged across **3:9** (FE 1 through Treatment Discontinuation; Screening excluded), distributed to 7 cells.

### 3.3 Note row (D5)
- The full-width note row at the foot of the p73 grid (rule-bounded cell spanning all columns) is one note, taken whole from its cell. Synthesised marker `n1`, location schedule_property row 1, `method: synthesized`; `n1` added to row 1's `annotation_markers`.

### 3.4 Header reprint markers (D4)
- The p73 reprinted header renumbers the Cycle1 / Subsequent Cycles footnotes as 12 / 13 (same text as 1 / 2). Header de-duplicated, so markers 1 and 2 stand; 12 and 13 are not emitted. Marker 1 is placed on each covered cell of the merged "Cycle" header (cols 5–7).

## 4. Table 3 — printed Table 9 "Schedule of Events – Main Study –Extended Visit Cycle" (p78)

### 4.1 Classification
- `track`, `track_label` "Extended Visit Cycle" (D6). Main-study patients in a later phase following a different visit pattern (in-clinic extended cycle / telephone contact / local-clinic or in-home nursing), with its own column structure; it does not give finer timing for Table 1, so it is not subsidiary.
- 5 data columns (2–6). 3 schedule properties (Cycle, Day, Location = `modality`); 15 activities (all p78); 40 schedule cells; 15 annotations.

### 4.2 Merged cells
- "Bone marrow aspirate and biopsy" (row 18): X¹⁵ merged across **2:6** (all data columns), distributed to 5 cells.

### 4.3 Orphan risk — markers without printed definitions (D7)
- Markers 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15 are printed in the grid/labels but their footnote text is not on p78 (it ends after footnote 3; the following page is not in the excerpt). Each gets an annotation stating the definition is not printed, with a *probable* Table 1 equivalent where obvious (5→fn10, 6→fn12, 7/8→fn13/14 (order undeterminable), 9→fn16, 10→fn22, 11→fn24, 12→fn25, 13→fn26, 14→fn27, 15→fn28). Marker 4 (Vital signs, weight at the local-clinic column) has no obvious equivalent. All are placed at their printed locations, so none is an orphan.
- Header markers 1, 2, 3 sit on the three "Subsequent Cycles" header cells (cols 2, 3, 4) and are defined on p78.

## 5. Synthesised items
- Property names: Table 2 "Period" (row 1), "Cycle" (row 2).
- Annotation markers: Table 2 `n1` (note row).

## 6. Method provenance (non-default)
- All activities: `indentation_method: assumed_flat` (flat tables, no grouping headers).
- Table 2 schedule properties rows 1–2: `structure_method: inferred_from_layout`.
- Table 2 `n1` location: `method: synthesized`.
- No `unresolved` locations; no proximity-bound note text; no visual-only cells (all marks confirmed from the text layer + raster rules).
