# NCT03421379 — SoA extraction uncertainty report

Prompt v3.8.1, single pass. Source: NCT03421379_soa.pdf (8 PDF pages = document pages 11–18 per PAGEMAP.md). No protocol markdown was available, so all text comes from the PDF text layer.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.11, header row 2 (label cell "Procedure") | This header row is recorded as a study-day row named "Study Day". Its washout entry ("3 to 14 days") is really a duration, and its follow-up entry ("Within 28±2 days after last study treatment") is really a window. | Type the row as "other" because the content is mixed, or keep the printed label "Procedure" as its name. | 3 |
| D2 | p.18, unmarked "Note:" below the table (marker n1) | The general note on procedure order (ECG, vital signs, venipuncture) is kept as a note for the whole table, anchored to the top header row with a synthesised marker n1. | Bind it only to the activities it names (the ECG, vital signs and blood-sampling rows). | 5 |
| D3 | p.16–17, rows 31–34 (PK (Glucagon), Plasma Glucose for PD, Genetic Sample (Stored), Anti-glucagon Antibodies) | These rows are placed at level 1 under the "Laboratory Tests" section header, because no other header comes between them. | Treat them as ungrouped rows, since the source prints no separate header (for example "PK/PD") for them. | 4 |

## Recorded, not open (8)

- §2 / type definitions: one printed table with its header repeated on every page is extracted as ONE `main_soa` table covering pp.11–18. It is not split into continuations, and the repeated header rows are counted once.
- §5: there is one label column (L = 1), so the data columns are positions 2–9. The right-hand "Comments" column (position 10) is a notes column. It is not a schedule column and it is not an activity.
- §6 notes column: each non-empty Comments cell becomes one annotation. The markers c1–c25 are synthesised in printed order, and each is bound to the row it sits beside (`activity_name`, `method: synthesized`). Notes with identical text are merged into one annotation (c15 covers rows 24 and 26; c19 covers rows 29 and 30).
- §6 bare pointers: "Refer to Section 9.5.5.1." (c5) and "See Appendix 2, Clinical Laboratory Tests, for details." (c15) are typed `source_note`. The notes that point and also explain (c16 and c23) stay `footnote`.
- §6 header-cell footnote: the marker "a" printed on "Additional Follow-up for TE ADAᵃ" is attached to that schedule_grid cell (row 1, column 9), not to the whole header row.
- §6 abbreviations: the abbreviation list (CRU, ECG, ED, FSH, HbA1c, IMG, min, PD, PG, PK, TE ADA) has no in-grid markers. Those terms appear only inside running text or labels, so the list yields zero annotations.
- §5 text cells: timing text in grid cells (for example "240 min" and "Pre-hypoglycemia induction, Predose, 15, 30, 60, 120, 240 min") is copied literally as `cell_value`. It is not split up and not turned into X marks.
- §4 section headers: "Clinical Assessments", "Laboratory Tests" and "Health Outcome Instruments" are bold, grey-shaded rows at level 0 with no marks. All other rows are level 1 (`indentation_method: font_signal`).

## 1. Table summary

- **Table 01**: `main_soa`, "Study Schedule Protocol I8R-JE-IGBJ", document pages 11–18. This is the only SoA table in the excerpt.
- **Columns**: 8 data columns (positions 2–9): Screening (Days -28 to -2), Period 1 Day -1, Period 1 Day 1, Wash out (3 to 14 days), Period 2 Day -1, Period 2 Day 1, Follow-up/ED (within 28±2 days after last study treatment), and Additional Follow-up for TE ADA.
- **Header rows**: 2.
- **Activities**: 36 rows (rows 3–38), of which 3 are section headers. There are 80 non-empty schedule cells and 27 annotations (25 footnotes, including a and n1, and 2 source_notes).
- **Rows per page**: p11: 8, p12: 4, p13: 3, p14: 7, p15: 5, p16: 4, p17: 4, p18: 1. Every page in the range contributed rows. Page 18 also carries the abbreviation list, footnote a and the general Note.
- The "Additional Follow-up for TE ADA" column applies only to patients with TE ADA, per footnote a. It is kept as an ordinary column of the same table: it is one column inside the main grid, not a separate schedule.

## 2. Merged marks / spans

- **Body**: there are no merged body cells. Every internal vertical rule is present in every body row, and each mark or text cell sits inside a single column.
- **Header row 1**: "Period 1" spans columns 3:4 and "Period 2" spans columns 6:7. This is recorded as `is_merged_cell` / `merged_cell_range` on each column the header covers.

## 3. Schedule properties

- **Row 1, "Study Period"** (synthesised name, because the label cell is empty): type `epoch`, level 1.
- **Row 2, "Study Day"** (synthesised name; the label cell prints "Procedure", which is the heading of the activity column): type `study_day`, level 2. See D1. Column 9 is empty in this row.

## 4. Hierarchy

- The three shaded, bold rows are the section headers.
- The rows that follow "Laboratory Tests" run from Clinical Serology Tests to Anti-glucagon Antibodies, including the PK, PD glucose and genetic rows (see D3).

## 5. Synthesised items

- **Property names**: "Study Period" (row 1) and "Study Day" (row 2).
- **Annotation markers**: c1–c25 for the Comments column, and n1 for the unmarked general "Note:" (see D2).

## 6. Mechanical mark-check

- **Page type**: the pages have a text layer and vector rule rectangles, so the table is not image-based.
- **How the check was done**:
  - Column boundaries came from the vertical rules. They are identical on all 8 pages, at x = 70, 176, 226, 251, 331, 375, 402, 482, 546, 613 and 722 pt.
  - Row bands came from the horizontal rules in the label column. Those bands match the rules in the Comments column exactly on every page.
  - X tokens were matched with `^[Xx][*a-zA-Z0-9]?$` and binned into the column they fall in.
  - Cell text was collected by band and column.
- **Result**: the resulting matrix agrees cell for cell with a visual read of every rendered page. There were no disagreements.
- **Text-layer check**: the text layer reads "≥90 mg/dL" in c9, where the page renders an underlined ">". The text-layer value was kept.

## 7. Annotation text integrity

- **Letter-spacing**: the source is not glyph-spread, so no text was reconstructed.
- **Note boundaries**: every Comments note was bounded by its cell's horizontal rules. No note spans more than one row.
- **Containment pairs**: two were found, both re-checked against the page. Both are faithful to the source, meaning they are separate cells and not one cell split across rows.
  - c15 "See Appendix 2, Clinical Laboratory Tests, for details." (Clinical Serology Tests on p.14 and HbA1c on p.15) is contained in c16 (Clinical Lab Tests, p.15). c16 opens with the same sentence and then adds instructions about fasting.
  - c20 (PK (Glucagon), p.16) is contained in c21 (Plasma Glucose for PD, p.16). c21 prefixes it with "-5 mins = stop insulin infusion."
- **Shared opening, not containment**: c24 and c25 begin with the same sentence as c20 ("Sampling times are relative to the time of study treatment administration (0 min).") and then add different text. Each is a separate cell.
- **Footnote a**: its source text contains the grammatical oddity "immunogenicity, should", which is kept verbatim.

## 8. Low-confidence calls

- **Header row 2 typing**: see D1.
- **Genetic Sample (Stored)**: the X is printed only in the Period 1 Day -1 column, although its note says "taken prior to/on Period 1 Day -1". It is copied as printed (column 3 only), and nothing was inferred for Screening.

## 9. Orphan risk

- None. Every annotation has at least one marker_location, and every marker in a marker_location also appears in that row's `annotation_markers`. The one exception is `a`, which is carried on its schedule_grid cell.
- Every marker printed in the table has its definition printed in the source.

## 10. Method provenance

- `activity_name_source.indentation_method: font_signal` on all 36 activities. The hierarchy comes from bold and shading, because the text layer has no leading whitespace.
- `marker_locations[].method: synthesized` on every location of c1–c25 (the Comments column notes) and on n1.
- There are no `unresolved` locations, no `proximity_bounded` text and no `raster_pixel_detection` / `visual_read` cells.
