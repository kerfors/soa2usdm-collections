# NCT03283098 — SoA extraction uncertainty report

Source: NCT03283098_soa.pdf (5 PDF pages = document pages 32–36 per PAGEMAP.md). Protocol 20140197, AMG 416, dated 25 May 2018. No protocol markdown was available, so all text comes from the PDF text layer.
Prompt 3.8.1, single pass. Three tables were extracted, all printed under the overall caption "Table 1. Schedule of Assessments".

## Decisions needed (6)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.32 (and p.34, p.36), header row 2 | The "(HD)" printed under a day number stays inside that day's header value (e.g. "3 (HD)"). The header is kept as one row. | Split "(HD)" into its own header row (a hemodialysis-day flag), so the day number and the HD flag become separate properties. | 2.1 |
| D2 | p.34, Table 1b rows 4–16, synthesized marker t1 | One cell in the Time column reads "Pre HD" and is merged down all 13 laboratory rows (the rule lines confirm this). It is recorded as one note, "Time: Pre HD", attached to every laboratory row. The activity names stay as printed. | Apply "Pre HD" only to the first row (Hematology, PT, aPTT), where the text is printed, or add "Pre HD" to each laboratory activity name. | 3.3 |
| D3 | p.36, Table 1c | Table 1c is typed `domain`: it has the same day columns and the same participants as Table 1a, with a PK category. | Type it `subsidiary` (finer PK timing for a subset of activities), because its rows are sampling times relative to dosing. | 4.1 |
| D4 | p.36, Table 1c rows 4–9 | The Time label value is added to each activity name: "PK (Pre HD)", "PK (SDA + 10 min)" … "PK (SDA + 18 to 30 hr)". | Keep each row's name as the printed "PK" and carry the time value separately, accepting six rows with the same name. | 4.3 |
| D5 | p.36, Table 1c row 3 | The "Central Laboratory" band printed across the day columns is treated as a level-0 section-header activity with no marks. | Treat it as a header property or condition row (where the samples are analysed), or as a table-level note. | 4.3 |
| D6 | p.36, Table 1c header 29/ET (column 19), marker a | Marker "a" is kept. Its annotation states that the definition is not printed for Table 1c and cites Table 1b footnote a as a probable equivalent, without asserting it. | Assert Table 1b footnote a's wording as this table's footnote a, or drop the marker as undefined. | 4.4 |

## Recorded, not open (6)

- §6 abbreviations: the abbreviation lines (p.33 ET/HD; p.35 HD/ET/cCa/SDA/Kt/V/URR; p.36 HD/ET/SDA) produce no annotations. None of these terms is printed as a marker. They appear only as ordinary text in header cells or activity labels.
- §5 label columns: Table 1a has L=1 (Assessment), so data starts at column 2. Tables 1b and 1c have L=2 (Time, Assessment), so data starts at column 3. The source column numbering is kept.
- §6 header-cell footnotes: "a,h" (1a) and "a" (1b, 1c) sit on the 29/ET grid cell of header row 2, not on the property row.
- §4 section headers: "Laboratory Assessments" (1b row 3) and "Central Laboratory" (1c row 3) carry no marks.
- §3 synthesized property names: "Study Visit (Day)" (row 1 banner) and "Study Day" (row 2) in all three tables.
- Table-type discriminator: Table 1b is `domain`. It has the same columns and the same participants as 1a, with a different category (laboratory). The rows do not continue 1a.

## 1. Method

- Every page has a text layer and vector rule lines. The tables are not image-based, and the glyphs are not spread out.
- Mark check (§1b): `pdftotext -bbox`. Column x-centres come from the header day labels, and X tokens are binned to the nearest centre by row y. The result was compared with a visual read of 110/200-dpi renders of pp.32, 34 and 36. **The mechanical and visual matrices agree in every cell. There are no disagreements.**
- No merged marks, arrows or spanning text cells occur in any body row. Every mark is a single `X` in its own cell, so no `source_range` is set.
- The only merged cells are: the "Study Visit (Day)" header banner in each table (`merged_cell_range` 2:21 in 1a, 3:22 in 1b and 1c); the vertically merged Time cell "Pre HD" in 1b (D2); the "Laboratory Assessments" full-width band; and the "Central Laboratory" band.

## 2. Table 1 — Table 1a. Schedule of Non-laboratory Assessments (main_soa), pp.32–33

- 20 data columns (2–21): Screening, −2, 1, 2, 3, 6, 8, 10, 13, 15, 17, 20, 22, 24, 27, 28, 29/ET, 34, 41, 55.
- 14 activities and 94 marks.
- Rows per page: p.32 = 14; p.33 = 0. Page 33 holds only the abbreviation line and footnotes a–i, so no rows are expected there.
- Indentation is flat (level 0, `assumed_flat`).
- 2.1 (D1): header row 2 carries values such as "1 (HD)", "29/ET (HD)". Row 1 is the banner "Study Visit (Day)" (hierarchical_level null).
- Annotations: 9 footnotes (a–i), all with printed markers. a and h are on the 29/ET header cell. b is shared by Body Height, Body Weight and Physical exam.
- Consistency note, not an error: footnote b gives abbreviated physical exams on days 1, 15, 29 and 55, which matches the grid.

## 3. Table 2 — Table 1b. Schedule of Laboratory Assessments (domain), pp.34–35

- 20 data columns (3–22).
- 14 activities (1 section header plus 13 tests) and 65 marks.
- Rows per page: p.34 = 14; p.35 = 0. Page 35 holds only the abbreviation lines and footnotes a–g.
- Indentation: "Laboratory Assessments" is level 0 and the tests are level 1 (`visual_estimate`). In the text layer, "Kt/V or URR" and "Anti-drug antibodies" look slightly less indented. The render shows them in the same Assessment cell column as the other tests, so they are kept at level 1.
- Footnote b is printed on the Day −2 cell (column 4) of Albumin, Phosphorus, Calcium (cCa), Serum or Urine Pregnancy and iPTH, and is bound to each of those cells. The Day −2 X of Breath Alcohol Screen has no b.
- 3.3 (D2): the synthesized marker **t1** ("Time: Pre HD", `method: synthesized`) is placed on all 13 test rows.
- Footnote g ends "…exclusion criteria section 4.2.4". It both explains and points, so it is typed `footnote`, not `source_note`. Footnote b has no terminal period in the source; this is transcribed as printed.

## 4. Table 3 — Table 1c. Schedule of Pharmacokinetic Assessments (domain), p.36

- 4.1 (D3): typed `domain`, with the reasoning also recorded in `table_metadata.notes`.
- 20 data columns (3–22).
- 7 activities (1 section header plus 6 PK rows) and 23 marks.
- Rows per page: p.36 = 7.
- 4.3 (D4, D5): activity `cell_text` keeps the raw "Time | Assessment" text, e.g. "SDA +\n10 min | PK". The PK row for SDA + 18 to 30 hr is marked at Day 2 and Day 28 (the day after each dosing day). This is confirmed by both bbox and render.
- 4.4 (D6): marker "a" appears on the 29/ET header, but no footnote a is printed for 1c. Only an abbreviation line follows the table. This is an orphan-definition source defect: the annotation text says so, and Table 1b's footnote a is cited as a *probable* equivalent only.

## 5. Synthesized items and method provenance

- Synthesized property names: "Study Visit (Day)" and "Study Day" (all tables). Row 1 has `structure_method: inferred_from_layout`.
- Synthesized annotation marker: t1 (Table 2), with 13 `activity_name` locations, each `method: synthesized`.
- `indentation_method`: `assumed_flat` for Table 1, `visual_estimate` for Tables 2 and 3.
- No `unresolved` locations, no `proximity` or `text_match` bindings, and no non-default annotation-text methods.

## 6. Annotation text integrity

- The text layer is clean, with no glyph spreading. Footnote text was joined across line wraps from the plain `pdftotext` output. A stray PDF-parser warning that `-layout` mode printed inside footnote f (p.33) is not part of the source and was excluded.
- There are no containment or overlap pairs among the annotations. Footnotes 1a-a and 1b-a are similar but differ ("perform Day 29 assessments" versus "obtain day 29 samples"), and they belong to different tables.
- Orphan risk: only Table 3 marker a (D6).
