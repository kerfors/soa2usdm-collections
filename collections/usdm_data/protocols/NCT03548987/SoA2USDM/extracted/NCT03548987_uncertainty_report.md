# NCT03548987 - SoA extraction uncertainty report

Prompt v3.8.1, single pass. Source: NCT03548987_soa.pdf (4 PDF pages = document pages 8-11 per PAGEMAP.md; protocol NN9536-4376, v2.0, 21 December 2017). No protocol markdown was available.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.8, rows 5-6 (and the whole table) | Three indentation levels read from shading and indent: dark-grey section bands (SUBJECT RELATED INFORMATION AND ASSESSMENTS, EFFICACY, SAFETY, TRIAL MATERIAL, REMINDERS) = 0; medium-grey unindented rows = 1; light-grey indented rows = 2 (children of Body measurements, Vital Signs, Clinical Outcome Assessments, Administration of trial product). | Flat two-level reading: treat the medium-grey rows as level 0 peers of the section bands, with only the indented rows as children. | 3.1 |
| D2 | p.9 rows 22-24; p.10 rows 41-42 | Rows at the top of a page continue the group open at the end of the previous page: the three "Evaluation of ..." rows sit under SUBJECT RELATED INFORMATION AND ASSESSMENTS; PGI-C and SPS-6 are level-2 children of Clinical Outcome Assessments (9.1.2). | Treat those rows as having no parent group (hierarchy restarts at each page). | 3.1 |
| D3 | p.8, row 16 (History of Colon Neoplasm), column 2 (V1) | The mark prints as an underlined X; transcribed as a plain "X", reading the underline as a formatting artefact. | Record the underline as meaningful (e.g. flag the cell as a distinct glyph). | 3.3 |

## Recorded, not open (6)

- Synthesised property name "Period" for the unlabelled epoch band (row 1) (prompt section 3).
- One printed table on pages 8-11 with reprinted headers extracted as ONE `main_soa` table, not a continuation: the overflow pages have no number or caption of their own (taxonomy "continuation").
- Header rows reprinted on pages 9, 10 and 11 were de-duplicated (section 1b).
- Inline section/appendix references stripped out of activity names into `source_note` annotations pr1-pr29, deduplicated by text, split where one label cites several, synthesised markers in order of first appearance (section 6). Annotation text is written "Section x.y" or "Appendix n"; the source prints the bare number in parentheses, e.g. "(6.1)".
- "Rando- misation" (hyphen at a line break in the header cell) rendered as "Randomisation"; "SPS- 6" (line break) rendered as "SPS-6" (text-layer line-break joins).
- Label columns L = 1; first data column position = 2; data columns 2-26 = V1, V2, P3, V4, P5, V6, P7, V8, P9, V10, P11, V12, V13, V14, P15, V16, P17, V18, P19, V20, P21, V22, P23, V24, V25 (section 5).

## 1. Table inventory

| Table | type | title | pages | columns | activities |
|---|---|---|---|---|---|
| 1 | main_soa | Flowchart | 8-11 | 25 data columns (+1 label) | 69 rows (5 section bands, 4 group headers, 60 marked activities) |

Activity rows per page: p.8 = 17, p.9 = 19, p.10 = 19, p.11 = 14. Every page in the declared range contributed rows. Row positions: header rows 1-4 and activities 5-73, numbered continuously across pages after header de-duplication. Mark cells: 378.

## 2. Schedule properties

- Row 1 "Period" (synthesised): epoch, level 1. Merged spans (from missing internal vertical rules in the raster): Screening 2:2, Run-in 3:12, Randomisation 13:13, Maintenance period 14:24, End of treatment 25:25, End of trial 26:26.
- Row 2 "Visit(V), Phone (P)": visit, level 2. The V/P prefix encodes site visit vs phone contact. The text layer runs "V20P21V22P23" together as one token; it was split per the raster cell boundaries and the visual read (V20, P21, V22, P23 in columns 21-24).
- Row 3 "Timing of Visit (Weeks)": week, level 3.
- Row 4 "Visit Window (Days)": window, level 4 (a per-column qualifier; kept in the hierarchy rather than null because it carries per-visit schedule data).

## 3. Activities and grid

### 3.1 Hierarchy
The whole body is shaded. Section bands are darker grey and carry no marks. Group headers are Body measurements (9.1.1), Vital Signs (6.4.2, 9.4.3) (printed twice: under EFFICACY on p.9 and under SAFETY on p.10, kept as two rows), Clinical Outcome Assessments (9.1.2) / (9.4.1), and Administration of trial product (7.1, 7.5). The group headers are level 1, carry no marks, and have light, indented children at level 2. All indentation_method = visual_estimate (from shading plus the x-offset of the text, not from whitespace).

### 3.2 Merged marks
None. Every internal vertical boundary is present in every body band, so no mark or text is distributed across a span. The only merged cells are in the epoch header row (section 2).

### 3.3 Mechanical mark-check
The pages are portrait with the table rotated 90 degrees. Method: pdftotext -bbox word boxes, rotated into table orientation. Cell rectangles came from the raster rule lines (200 dpi render, section 1d): 27 vertical rules (26 columns), and horizontal rules taken from the label column. Each token was binned into its (band, column) cell. The resulting matrix matched the visual read of all four rotated page renders cell for cell, with 0 disagreements. All in-grid marks are plain "X". There are no footnoted marks, no other glyphs and no cells with text. The only oddity is the underlined X at History of Colon Neoplasm / V1 (D3).

## 4. Annotations

- Footnotes a-d (p.11, below the table) are read from the text layer, and each is bound to the activity label that carries its printed marker. a: Informed consent and Demography. b: Childbearing potential, History of Breast Neoplasm, Breast neoplasms follow-up (3 printed locations). c: Randomisation criteria and randomisation. d: Tobacco Use.
- Footnote c ("If subjects not fulfil randomisation criteria see Section 6.3.2") both explains and points, so it is kept as a `footnote`.
- Source notes pr1-pr29 are listed under "Recorded, not open". Their marker_locations carry method "synthesized", since no marker is printed.
- The text layer is not glyph-spread, so no text was reconstructed. No containment pairs among the footnotes. The source-note pairs "Section 9" / "Section 9.4" etc. are distinct references, not split cells.
- No abbreviation or legend list is printed with the table, so no abbreviation or legend annotations were emitted.

## 5. Orphan risk
None. Every annotation has at least 1 marker_location, and each location's marker appears in that row's annotation_markers. All footnote markers used in the grid are defined.

## 6. Method provenance (non-default)
- activity_name_source.indentation_method = visual_estimate on all 69 activity rows.
- marker_locations.method = synthesized on all source_note (pr1-pr29) locations.
- The schedule_grid and activity_schedule use the default bbox method throughout. Cell geometry came from raster rules (section 1d), but the cell contents came from the text layer. The row-2 "V20P21V22P23" split is noted in section 2.
- No `unresolved` locations.
