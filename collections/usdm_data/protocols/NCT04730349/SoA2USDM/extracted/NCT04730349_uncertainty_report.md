# NCT04730349 — SoA extraction uncertainty report

Prompt: PDF_TO_JSON_PROMPT v3.8.1, run in one pass without stopping. Source: NCT04730349_soa.pdf (16 PDF pages = document pages 27–42, per PAGEMAP.md). No protocol markdown was provided, so all text comes from the PDF text layer and was checked against rendered pages.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.31, Table 2-2 row 4 (Targeted Physical Examination…), Day 8 cell (col 5), marker `*` | The Notes-cell line "*At Day 8 visits collect vital signs only." is its own annotation `*`, bound to the Day 8 cell that shows "X*". The rest of that Notes cell is annotation n1, bound to the activity. | Keep the whole Notes cell, including the `*` line, as one activity-level note (n1). The `*` would then stay only as a mark on the cell. | 3.3 |
| D2 | p.37, Table 2-2 row 29 (Oral Hydration Follow-up), col 4 | "X (Day 3-5)" is recorded only in the Cycle 1 Day 5 column (col 4), where it is printed. The rule line between Day 3 and Day 5 is present in that row, so this is not a merged cell. The qualifier stays in the cell text. | Treat "(Day 3-5)" as naming the Cycle 1 Day 3 and Day 5 columns, and spread the X over cols 3:4. | 3.2 |
| D3 | p.31, Table 2-2 header rows 1–2 | The cycle-length lines ("Cycle = 3 wks", "Each cycle = 3 wks") and the visit windows ("± 1 day", "- 1 day") stay inside the cycle and day header cells. This gives two header properties (cycle, study_day). | Split them out into separate header properties (a cycle-length row and a `window` row). | 3.1 |

## Recorded, not open (7)

- All three tables typed `main_soa`. Rule: taxonomy "multiple independent schedules (e.g. Screening table and Treatment table with different column structures) → each main_soa". Section 2 (p.27) presents Tables 2-1, 2-2 and 2-3 as three schedules (Screening, On Treatment, Long-term Follow-up). Each has its own column set, and all three cover the same participants.
- Each table has one label column (Procedure), so L = 1 and the first data column is position 2. The right-hand Notes column is not a schedule column (§5, §6).
- Each non-empty Notes cell is one annotation, bounded by rule lines, with a synthesised marker (n1, n2, …) bound by `method: synthesized` to the activity row beside it (§6).
- Merged text cells are distributed over their span with `source_range` (§5). This covers "Continuously", imaging instruction text, "Performed … as clinically indicated", "See Section 9.5.1 …", "See Notes" and "As clinically indicated".
- Qualified marks are kept as literal cell values because the qualifier is a condition, not a column span (§5): "X (Cycle 5 only)" (T2 row 5, col 7), "If toxicities are present." / "If toxicities are present" (T3 row 10, cols 3–4).
- Footnote c of Table 2-3 is printed on the Notes column header. It is treated as table-scope: one `schedule_property` location with `method: synthesized`, and no marker placed on any element (§6).
- Abbreviation blocks (pp.30, 37, 42) produced no annotations. None of their terms is printed as a marker (§6).

## 1. Per-table summary

| Table | Source | type | pages | data cols | activities (incl. headers) | activity rows per page |
|---|---|---|---|---|---|---|
| 1 | Table 2-1 Screening Procedural Outline | main_soa | 28–30 | 1 (pos 2) | 21 | 28: 12 · 29: 6 · 30: 3 |
| 2 | Table 2-2 On-treatment Procedural Outline | main_soa | 31–38 | 6 (pos 2–7) | 27 | 31: 4 · 32: 4 · 33: 2 · 34: 4 · 35: 5 · 36: 6 · 37: 2 · 38: 0 |
| 3 | Table 2-3 Long-term Follow-up Period | main_soa | 39–42 | 4 (pos 2–5) | 19 | 39: 7 · 40: 3 · 41: 3 · 42: 6 |

- Page 27 holds only the Section 2 introduction, so no table starts there. Table 1 starts on page 28.
- Page 38 contributes no activity rows. It holds only footnotes d and e of Table 2-2, and that is expected.
- Section-header rows (bold, no marks) are at indentation 0 and procedures at 1 (`indentation_method: font_signal`). Table 1 headers: Eligibility Assessments, Safety Assessments, Laboratory Tests, Tumor Assessment. Table 2 headers: Safety, Laboratory Tests, Efficacy, PK/Immunogenicity (marker e), Health Outcomes, Study Drug. Table 3 headers: Safety, Laboratory Tests, Efficacy, PK/Immunogenicity.

### 3.1 Header structure (Table 2)
- Row 1 is `cycle`: "Cycle 1 Only" is merged over 2:5 with markers a,b, and "Cycle 2 and Beyond" is merged over 6:7 with markers a,b,c.
- Row 2 is `study_day`: Day 1, Day 3 (± 1 day) with marker d, Day 5 (± 1 day), Day 8 (- 1 day), Day 1, Day 3-5.
- Both property names are synthesised ("Cycle", "Day") because the label cell is the merged "Procedure" heading.
- Tables 1 and 3 each have one `visit` header row with the name "Visit" synthesised.
- The Survival Follow-up window "(± 14 Days)" is kept in its cell value.
- Header footnote markers are on the specific `schedule_grid` cells. Their locations are `schedule_property` entries with a `column_position`.

### 3.2 Merged-mark decisions
The spans were confirmed from 200-dpi raster rule lines: the internal vertical rules are missing across each span.
- Table 2, span 3:7 (the Cycle 1 Day 1 column stays empty, and its rule line is present): row 6 AE Assessment ("Continuously"), row 7 Concomitant Medication Use ("Continuously"), row 12 Body Imaging (bullet text), row 13 Brain Imaging (bullet text), row 14 CSF - Solid tumors, row 15 Bone Marrow- Solid tumors, row 16 CSF - Leukemia, row 17 Bone Marrow - Leukemia, rows 19–21 PK/Immunogenicity samples ("See Section 9.5.1 for further details."), row 23 PRO ("See Notes").
- Table 3, span 2:5: row 13 Body Imaging, row 14 Brain Imaging, row 15 CSF ("As clinically indicated."), row 16 Bone marrow aspirate ("As clinically indicated"), rows 19–20 PK and Immunogenicity samples.
- Oral Hydration Follow-up (T2 row 29): every rule line is present, so the mark is a single cell in col 4 (see D2).

### 3.3 Synthesised items
- Property names "Visit" (T1, T3), "Cycle" and "Day" (T2).
- Notes-column markers: T1 n1–n17, T2 n1–n17, T3 n1–n12.
- For D1, the `*` note text was taken out of the Notes cell of T2 row 4. The `*` marker itself is printed in the grid ("X*"), so its location is not synthesised.

## 4. Mechanical mark-check
- Every SoA page has a text layer and vector rule lines. The text layer is not glyph-spread, so no word reconstruction was needed.
- Marks: `pdftotext -bbox` tokens matching `^[Xx][*a-zA-Z0-9]?$` were binned to the nearest header column centre on every page. The resulting matrix matched the visual read cell for cell, including "X*" (T2 row 4, Day 8) and the "X" parts of "X (Cycle 5 only)" and "X (Day 3-5)". There were no disagreements.
- Row and cell bands were recovered from raster rule lines, as in §1d, to confirm spans and Notes-cell extents.

## 5. Annotation text integrity
- Containment pairs were checked against the page, and both are source-faithful. They are separate Notes cells in the source, not one cell split across rows:
  - T2 n4 "Record at each visit." (Concomitant Medication Use, p.32) is contained in T2 n3 (AE Assessment, p.31).
  - T3 n8 "See Section 9.1.1 for further details." (Body Imaging) is contained in T3 n9 (Brain Imaging, p.41).
- Redacted content was transcribed up to the black box, with "[remainder redacted in source]" appended:
  - Table 1 notes n14–n17.
  - Table 2 notes n9–n12.
  - T2 footnote e has an inline redaction, written as "[redacted in source]".
- The superscript in "[18F]fluorodeoxyglucose" was flattened to "[18F]". Bullets are rendered as "•" on separate lines. "±" was restored from the rendered page because the text layer maps it to a private-use glyph.
- The label of T2 row 23 is printed in grey font ("Patient-reported Outcomes (PRO) Version of the CTCAE"). It is kept as a normal activity, and the grey is recorded in `font_info`.

## 6. Source defects / orphan risk
- Redaction blocks that may conceal activity rows:
  - T1: a full-width block below the last row on p.30.
  - T2: a block covering the table bottom on p.35 (roughly 5 rows' height), and a full-width band just under the header on p.36.
  - T3: a full-width block below Pregnancy Test on p.40.
- No rows were emitted for any of these regions. The reviewer should confirm against an unredacted source if one is available.
- Every annotation has at least one marker location, and no marker is used without a definition. T3 marker c is intentionally not placed on any element (table-scope).

## 7. Method provenance
- `activity_name_source.indentation_method: font_signal` on all activities (bold headers = level 0).
- `marker_locations[].method: synthesized` on all Notes-column annotations (n*) and on T3 footnote c.
- No `unresolved` locations. No `proximity_bounded`, `raster_band_cells` or `visual_transcription` text.
