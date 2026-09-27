# NCT04557384 — SoA extraction uncertainty report

Prompt: PDF_TO_JSON_PROMPT 3.8.1, single pass. Source: NCT04557384_soa.pdf (9 PDF pages = document pages 16-24 per PAGEMAP.md). No protocol markdown was available.

## Decisions needed (11)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | T1, p19, unlabelled top row / Pregnancy test (row 22), marker i13 | The unlabelled row at the top of page 19 (empty cells, shading the same as the Pregnancy test row) is read as the Pregnancy test row carried over the page break. No activity is created, and its Instructions text ("Note: During study treatment, perform monthly ... See Appendix 2.") is joined to the Pregnancy test note. | Keep the page-19 text as a separate note or row (e.g. table scope, or bound to Thyroid panel). | 3.1 |
| D2 | T1, p20, the page's only body row / Injection site assessments (solicited) (row 25), marker i16 | Page 20's unlabelled row (empty cells; shading and merges match the solicited-ISR row) is read as that row carried over the page break. No activity is created, and its text (C1D1-C1D15 / Cycle 2-n timings, ISR questionnaire, Pain VAS, See Section 8.2.5) is joined to the solicited-ISR note. | Keep the page-20 text as a separate note or row. | 3.1 |
| D3 | T1, p22, Administer combination medications (row 32), col 11 (DX) | The white "See instructions" cell ends on a rule inside the DX column, and the rest of DX is grey. Recorded as "See instructions" over cols 4-10 and 12-15, with DX empty. | Extend the span to 4:11, i.e. combination medications also "See instructions" at DX. | 3.2 |
| D4 | T1, p16, note above the table, marker n1 | The "Note:" above the table (procedures in column DX are done at the additional ramucirumab doses) is bound to the DX cell of the Day header row (row 6, col 11). | Bind it to the Cycle-row DX cell (row 4, col 11), or make it table-scope. | 3.3 |
| D5 | T1, p16, Day header row (row 6), marker i1 | "D22 for 28-day cycles only." (Instructions cell of the Day row) is bound to the whole Day header row. | Bind it only to the two D22 cells (row 6, cols 10 and 15). | 3.3 |
| D6 | T1, p21, Sample collection (row 28), marker i19 | "See Section 1.3.1 for PK and IG." is bound only to the Sample collection header row. | Also bind it to the PK and IG rows (29-30). | 3.3 |
| D7 | T1, p16, header row 3 | The row "Cycle = 21 days" / "Cycle = 21 days (or 28 days for Cohorts B & C ...)" is typed `other`, hierarchy level 2. | Type it `period` or `cycle`, or give it level null as a presentational qualifier. | 3.4 |
| D8 | T2, p23 | Table 2 (Continued Access Schedule of Activities) is typed `track`, label "Continued Access": a separate period with its own visits (501-5XX, 901). | Type it `main_soa` as an independent schedule. | 4.1 |
| D9 | T3, p24 | Typed `subsidiary` and kept in its printed layout: one row per sample, Study Cycle / Day within Cycle / Collection Time Point as data cols 2-4, PK and IG X marks in cols 5-6 (L=1). | Type it `reference`; or treat cols 2-4 as label columns (L=4); or transpose it so samples become columns and PK / IG collection become activities. | 5.1 |
| D10 | T3, p24, rows 2-15 | Sample rows are named "Sample 1" ... "Sample 14" (column heading "Sample #" + printed number; cell_text keeps the bare number). | Use the bare numbers "1" ... "14" as names. | 5.2 |
| D11 | T3, p24, header row 1, markers g1-g4 | The four "General Instructions" paragraphs above the table are captured as table-scope footnotes g1-g4. | Leave them out as section prose outside the table. | 5.3 |

## Recorded, not open (7)

- §2 / type definitions "page break inside one printed table": Table 1 prints as one table over pp16-22 with the header reprinted on each page. It is extracted as one table, not as continuations, and the header rows are counted once.
- §4: the grey "Procedure" band row (T1 row 7, T2 row 4) is a repeated column-label band. No activity is created for it.
- §6 abbreviations: the abbreviation lines under T1 (p22), T2 (p23) and T3 (p24) produce no annotations, because none of their terms is printed as a marker.
- §6 notes column: each non-empty Instructions cell became one `footnote` bound to the row it sits beside, with a synthesised marker (T1 i1-i21, T2 i1-i3).
- §6 bare pointer: "See Appendix 2." (Hematology, Clinical chemistry) and "See Section 1.3.1 for PK and IG." are typed `source_note`. "See Appendix 2." is one annotation with two locations. Notes that point and also explain stay `footnote`.
- §6 header-cell footnotes: marker "a" on "Short-term follow-up" (T1 row 4, col 16) and on "Follow-Up" (T2 row 2, col 3) is recorded on that schedule_grid cell.
- §5 merged "See Section 1.3.1" text in the T1 PK/IG rows is spread over cols 5-16, following the rule-line geometry. Col 4 (C1 D1) is a separate empty white cell.

## 1. Method (all tables)

- **Image-based source.** Every page is one full-page 144-ppi RGB raster. There is no text layer (`pdftotext` returns nothing, no fonts) and no vector rules. Following §1a/§1d:
  - Horizontal and vertical rule lines were found from ink fractions on the page images (ink < 128). Every body page of T1 gives the same 18 vertical rules on the Day row: x = 156, 284, 339, 388, 438, 486, 539, 585, 630, 683, 738, 863, 908, 953, 1007, 1061, 1151, 1452.
  - Merged spans were read from the vertical rules found inside each row band.
  - Marks were detected by counting near-black pixels (< 90) in each cell. Every X gives 44-57 px; empty and grey cells give 0; "See instructions" / "See Section" text gives about 290-310 px.
- **Validation.** The detector matrix was checked cell by cell against direct reading of all pages, including dense rows (Urinalysis, Vital signs) and sparse ones (Coagulation, ECOG PS). **No disagreements.** A spot-check of the resolved grid against the pages is still recommended.
- **Text.** All text (activity labels, header labels, notes) was transcribed by eye from the page raster: `method: visual_transcription` on activities and annotations. Note-cell extents come from the raster rule lines.
- **Provenance recorded.** Body cells are marked `raster_pixel_detection` and header grid cells `visual_read`. Indentation is `visual_estimate` (T1) and `assumed_flat` (T2, T3).
- **Page footers.** The printed footers (16-24) agree with PAGEMAP. PAGEMAP numbering is used throughout.

## 2. Tables found

| Table | Title | Type | Pages | Data cols (L, first) | Activities |
|---|---|---|---|---|---|
| 1 | Screening, On-Study, and Post-Treatment Schedule of Activities | main_soa | 16-22 | 15 (L=1, first col 2) | 25 |
| 2 | Continued Access Schedule of Activities | track ("Continued Access") | 23 | 2 (L=1, first col 2) | 3 |
| 3 | Pharmacokinetic Sampling Schedule (Section 1.3.1) | subsidiary | 24 | 5 (L=1, first col 2) | 15 |

## 3. Table 1 — main_soa

- **Columns:** 2 = ≤28, 3 = ≤7 (Screening); 4-6 = Cycle 1 D1/D8/D15; 7-10 = Cycle 2 D1/D8/D15/D22; 11 = DX (Cycle 2-n, combination held); 12-15 = Cycle 3-n D1/D8/D15/D22; 16 = V801 (Short-term follow-up). The Instructions column (col 17) is a notes column: it is not a schedule column and has no grid entries.
- **Row numbering:** physical rows. Row 1 = title band (goes to `table_title`); rows 2-6 = header rows; row 7 = Procedure band; rows 8-32 = activities.
- **Activity rows per page:**

  | Page | Rows | Count |
  |---|---|---|
  | 16 | 8-13 | 6 |
  | 17 | 14-15 | 2 |
  | 18 | 16-22 | 7 |
  | 19 | 23-25 | 3 |
  | 20 | none | 0 |
  | 21 | 26-30 | 5 |
  | 22 | 31-32 | 2 |

  **Page 20 contributes no activity rows.** Its only body row is the page-broken continuation of Injection site assessments (solicited). It supplies the second half of note i16 and no marks: all its cells are empty (D2). The top row of page 19 is likewise a continuation of Pregnancy test (D1).
- **Hierarchy:** "Sample collection" (row 28) is a level-0 section header with no marks (grey across cols 2-16). PK and IG (rows 29-30) are indented, level 1. "ECOG PS" looks indented but is centred in its cell, so it is level 0. All other rows are level 0.

### 3.1 Page-broken rows

- **p18 → p19:** the Pregnancy test note continues on p19. On the p19 overflow row, col 2 is grey, col 3 is white, cols 4-15 are one merged white cell and col 16 is white, which is exactly the Pregnancy test row pattern. No marks.
- **p19 → p20:** the solicited-ISR note ends p19 with "1.Cycle 1 collection:" and p20 opens with "a.C1D1: ...". On the p20 row, cols 2, 3 and 16 are grey and cols 4-6 are merged, matching the solicited-ISR row.
- Both notes were joined into one annotation each. See D1 and D2.

### 3.2 Merged-mark / merged-text spans (distributed, `source_range` set)

| Row | Activity | Span | Value |
|---|---|---|---|
| 12 | Concomitant medication | 4:15 | X |
| 14 | Vital signs | 4:6 | "See instructions" |
| 15 | AE collection | 4:15 | X |
| 17 | ECG | 4:15 | "See instructions" |
| 22 | Pregnancy test | 4:15 | "See instructions" |
| 23 | Thyroid panel | 4:15 | "See instructions" |
| 24 | Radiologic imaging | 4:15 | "See instructions" |
| 25 | Injection site assessments (solicited) | 4:6 | "See instructions" |
| 26 | Injection site assessments (spontaneous) | 4:15 | "See instructions" |
| 27 | Participant diary | 4:15 | "See instructions" |
| 29 | PK | 5:16 | "See Section 1.3.1" |
| 30 | IG | 5:16 | "See Section 1.3.1" |
| 31 | Administer ramucirumab | 4:15 | "See instructions" |
| 32 | Administer combination medications | 4:10 and 12:15 | "See instructions" |

- **Row 32 geometry anomaly:** the rule ending the first span sits at x≈770, inside DX (738-863). DX is treated as grey/empty (D3).
- **PK/IG geometry:** the rule at x≈433 sits about 5 px left of the col 4/5 boundary at x≈438. It is read as the col 4/5 boundary.
- **Header merges:**
  - Row 2: Screening 2:3, On-Treatment 4:15.
  - Row 3: 4:6 and 7:15.
  - Row 4: 2:3, 4:6, 7:10 and 12:15.
  - Row 5: 4:6, 7:10 and 12:15.
  - Vertical header merges (Screening and Post-Treatment over rows 2-3; "(Day Relative to C1D1)" over rows 4-5) cannot be modelled, so the lower row's covered cells are left empty. This is stated in each `property_comment`.

### 3.3 Annotations (23)

- **Footnote a** (Short-term follow-up, printed below the table on p22).
- **n1**, the note above the table on p16. It is bound by synthesis to the DX column (D4).
- **i1-i21**, the Instructions-column cells, each one bounded by raster rule lines.
- **Types:**
  - 21 footnote.
  - 2 source_note: i10 "See Appendix 2." (rows 18, 20) and i19 "See Section 1.3.1 for PK and IG."
- **Synthesised markers:** n1 and i1-i21. Every location for these is `method: synthesized`.
- **Placement of i1 and i19:** i1 is on the Day header row (D5) and i19 is on the Sample collection row only (D6).
- **Containment check (§6, §8):** i10 "See Appendix 2." is contained in i11, i12, i13 and i14. Re-checked against the pages: **source-faithful**. Each is a separate rule-bounded Instructions cell on a different row (Hematology/Chemistry; Coagulation, Urinalysis, Pregnancy test [p19 part], Thyroid panel) that opens or ends with the same pointer. It is not one cell split across rows. No other overlaps.

### 3.4 Low-confidence calls

- **Row 3 type:** typed `other`, with `structure_method: inferred_from_layout` (D7).
- **Row 6 (Day) type:** typed `study_day` even though col 16 holds the visit code "V801" and col 11 holds "DX".
- **Row 5 (±3 days):** typed `window` with level null, because it does not distinguish any columns.
- **Screening days:** "≤28" and "≤7" are days relative to C1D1, as the "(Day Relative to C1D1)" label over them states.

## 4. Table 2 — track "Continued Access"

- **Structure:** 2 data columns: 2 = Study Treatment / 501-5XX, 3 = Follow-Up^a / 901. Instructions (col 4) is a notes column.
- **Rows:** row 1 = title band; row 2 = Epoch (name synthesised); row 3 = Visit (the printed label "Visit" sits on row 3 of the merged label cell); row 4 = Procedure band.
- **Activities:** 3 activities on p23 (rows 5-7).
- **Marks:** AE Collection X at 2 and 3; PK, IG, and exploratory hypersensitivity has no marks (both cells grey; event-driven, see i2); Administer ramucirumab X at 2.
- **Annotations:** a (printed on the Follow-Up header cell, row 2 col 3) and i1-i3 (Instructions cells, synthesised markers). All are footnote.

### 4.1 Classification

- A separate period for participants who continue treatment after the main study, with its own visit numbering. This fits the type definitions' "post-study access schedules" example of `track` (D8).

## 5. Table 3 — subsidiary

- **Structure:** 15 rows on p24: Samples 1-14 and "End of treatment".
- **Marks:** PK X on all 15 rows. IG X on Samples 1, 5, 8, 13 and End of treatment (confirmed by pixel count; the IG cells without a mark are grey).
- **End of treatment:** Study Cycle and Day are grey and empty.

### 5.1 Classification

- The Table 1 PK and IG rows point to Section 1.3.1, and this table gives the finer per-sample timing for those two activities. It is therefore typed `subsidiary` (the §2 PK-sampling note), even though its rows read as samples.
- It is kept in its printed orientation (D9). This is also recorded in `table_metadata.notes`.

### 5.2 Names

- Activity names "Sample n" are composed from the column heading plus the printed number (D10).

### 5.3 General Instructions

- g1-g4 are anchored to the single header row (row 1) with `method: synthesized` (D11).

## 6. Orphan risk / undefined markers

- None. Every annotation has at least one marker_location, and every location's marker is present on the matching row or cell `annotation_markers` (checked programmatically). All 3 files validate against the schema.

## 7. Method provenance (non-default)

- **Activities:** `activity_name_source.method = visual_transcription` on every activity. `indentation_method` is `visual_estimate` (T1) and `assumed_flat` (T2, T3).
- **Annotations:** `annotation_text_source.method = visual_transcription` on every annotation. The note says whether the cell was bounded by raster rules or printed outside the grid.
- **Cells:** `activity_schedule[].method = raster_pixel_detection` on every body cell; `schedule_grid[].method = visual_read` on every header cell.
- **Header rows:** `structure_method = inferred_from_layout` on T1 row 3.
- **Marker locations:** `method = synthesized` for T1 n1 and i1-i21, T2 i1-i3, and T3 g1-g4.
- **Unresolved:** there are no `unresolved` marker locations.
