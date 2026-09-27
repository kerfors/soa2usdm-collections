# CDISC_Pilot — SoA extraction uncertainty report

Source: `CDISC_Pilot_soa.pdf` (2 PDF pages = document pages 53–54 per PAGEMAP.md). Protocol H2Q-MC-LZZT(c), Protocol Attachment LZZT.1 "Schedule of Events". No protocol markdown was available, so the PDF text layer supplied all text.
Output: 1 table, `CDISC_Pilot_Table_01_extraction.json`.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p53–54, rows 24–26 (Study drug record / Medications dispensed / Medications returned) | The three labels are printed as three lines inside one rule-bounded row cell, with one X per visit column printed on the first line. Extracted as three separate activities, each given the same X at visits 3, 4, 5, 7, 8, 9, 10, 11, 12, 13 and ET. | (a) The X marks belong only to "Study drug record", and "Medications dispensed" and "Medications returned" get no marks; or (b) the cell is one activity, "Study drug record", and the other two lines describe it rather than being separate activities. | 3.2 |
| D2 | p53, column 8 (between Visit 5 and Visit 7) | Kept a real ruled column that has empty VISIT and WEEK header cells and no marks in any row. It is column 8 and all its values are empty. | Drop the column as a layout leftover (e.g. a deleted Visit 6), or label it as an implied Visit 6 even though no 6 is printed. | 3.3 |
| D3 | p54, VISIT header columns 16 (ET) and 17 (RT) | Kept ET = Early Termination and RT = Retrieval as abbreviation annotations (markers ab1 and ab2), bound to the header cells that consist of exactly those terms. Dropped CT and ECG from the same abbreviation line because they only appear inside activity labels. | Drop ET and RT as well, treating the abbreviation line as a standalone list; or keep CT and ECG bound to the "CT Scan", "ECG" and "Ambulatory ECG …" labels. | 3.4 |

## Recorded, not open (6)

- §2 / type definitions (horizontally tiled table): p54, titled "(concluded)", reprints the same 30 body rows under a new column block (visits 9–RT). It is extracted as one table with marks merged across both pages. It is not a `continuation` and not a second table.
- §5 leading label columns: L = 2. Column 1 holds activity names and column 2 holds the header-row labels VISIT/WEEK, which is empty in the body. The first data column is 3, and the data columns are 3–17 (15 columns).
- §5 legend marks in the grid: `P` (practice only) and `X` stay as `cell_value`. Each has a `legend` annotation, and its marker is added to the `annotation_markers` of every cell that uses it, so the marker locations and the per-cell markers agree.
- §6 abbreviations: CT and ECG were dropped because they are only word matches inside activity labels. See D3 for ET/RT.
- §6 deduplication: the legend lines "X = Performed at this visit." and "Xb = …", printed on both pages, are each emitted once.
- §4 flat table: there are no grouping headers, so every activity has indentation 0 with `indentation_method: assumed_flat`. The thicker horizontal rules (after "Patient randomized" and after the Study drug cell on p53, after "MMSE 10-23" on p54) differ between pages and have no labels, so they are treated as presentation only.

## 1. Per table

**Table 1** is `main_soa`, titled "Schedule of Events for Protocol H2Q-MC-LZZT(c)", on document pages 53–54.
- There are 15 data columns: Visits 1, 2, 3, 4, 5, (blank), 7, 8, 9, 10, 11, 12, 13, ET, RT.
- There are 2 schedule properties: VISIT (visit, level 1) and WEEK (week, level 2). The WEEK cell is empty for ET, RT and the blank column.
- There are 30 activities and 161 activity_schedule entries: 157 X, of which 1 is Xa and 4 are Xb, plus 4 P.
- Activity rows per page:
  - p53 contributes all 30 rows.
  - p54 contributes 0 rows. This is expected for a tiled table (§4): p54 reprints the same 30 rows and supplies every mark in columns 11–17 (visits 9, 10, 11, 12, 13, ET, RT). Its marks are on Physical examination (13, ET), Vital signs, ECG, Concomitant Medications, Laboratory (Chem/Hemat), Laboratory (Urinalysis) (9, 12, ET), Plasma Specimen (9, 11, ET), the Study drug cell, TTS Acceptability Survey (13, ET), ADAS-Cog, CIBIC+ and DAD (10, 12, ET, RT), NPI-X (Xb at 9, 10, 11) and Adverse events.
- I checked that every row appears in both tiles. No row is missing from either page.

## 2. Merged marks

- No horizontal merged cells or arrows were found. Every vertical rule is present across the full table height on both pages.
- Vertical: the Study drug cell covers three label lines and is one rule-bounded cell. Its marks are copied to rows 24–26 (see D1).

## 3. Detail

### 3.1 Classification
The table is `main_soa` because it is the only schedule. The p54 half is a horizontal tile (§5), as recorded above.

### 3.2 Study drug record cell (D1)
Raster rule detection (200 dpi) finds no horizontal rule between y≈500 and y≈540 pt on p53. The same is true on p54. So the three label lines share one cell. The X glyphs sit on the first line, which is where every multi-line cell in this table places its content (compare the CT Scan cell). Under §5 (geometry over glyph position, and marks spanning covered rows), the mark was applied to all three labels. Clinically, "Medications returned" at Visit 3 (randomization) looks unlikely. That was not used to change the extraction, but it is why this is flagged high.

### 3.3 Blank column (D2)
There are vertical rules at x≈432 and x≈463 pt on p53, which bound a real column between Visit 5 and Visit 7. The header cells are empty (no visit 6, no week), and none of the 30 rows has a mark there. It was transcribed literally as column 8 with empty `schedule_grid` values and no `activity_schedule` entries. The thicker rule on the left of this column suggests a deliberately removed visit, but that is an inference.

### 3.4 Abbreviations (D3)
The abbreviation line is on p53 (CT, ECG) and on p54 (CT, ECG, ET, RT). ET and RT are the VISIT header values of columns 16 and 17. They are bound there as `schedule_property` locations with `column_position`, and the synthesized marker names ab1/ab2 are placed on those `schedule_grid` cells. The location is where the term is printed, so no `method` is recorded.

## 4. Synthesised
- There are no synthesised property names. VISIT and WEEK are printed in label column 2.
- The synthesised annotation markers are `ab1` (ET) and `ab2` (RT). `X` and `P` are used as markers for the two legend annotations.

## 5. Mechanical mark-check
- The PDF has a text layer and is not glyph-spread.
- I ran `pdftotext -bbox` on both pages and placed each X/P token in the nearest header column (column centres taken from the VISIT/WEEK tokens). I then compared the result cell by cell with my visual read of 110/200 dpi renders.
- Result: no disagreement. The footnoted marks come through as separate tokens ("X" + "a", "X" + "b") and were paired back together.
- The raster rule-line check on p53 confirmed that there are no merged spans and that the blank column is a real ruled column.

## 6. Annotation text integrity
- There is no glyph spreading, and no annotation text contains another.
- The P legend spans three printed lines. It was joined into one annotation: "Practice only - It is recommended … would not be collected."
- The footnotes are:
  - a: HbA1c at Visit 1.
  - b: NPI-X at Visits 8, 9, 10, 11.

## 7. Low-confidence calls
- Activity label spelling: p53 prints "Hemoglobin A1C" and p54 prints "Hemoglobin A1c". The p53 spelling is used and the difference is noted in `table_metadata.notes`.
- "Laboratory (Chem/Hemat):" keeps its printed trailing colon.
- The Visit 2 week value "-.3" is transcribed exactly as printed.

## 8. Orphan risk
None. Every annotation has at least one location, and every location's marker appears in that element's `annotation_markers`. No marker lacks a printed definition.

## 9. Method provenance
- `indentation_method: assumed_flat` on all 30 activities (flat table).
- No other non-default methods were used, and there are no `unresolved` locations.
