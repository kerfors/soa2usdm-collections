# NCT03421379 — SoA extraction uncertainty report

Prompt v3.7.3 / schema soa-table-extraction 1.0. Source: `NCT03421379_soa.pdf`, 8 PDF pages =
document pages 11–18 per `PAGEMAP.md` (doc = pdf + 10). Printed footers happen to agree with the
page map on every page here, but the page map was used throughout and the footers were not relied on.

---

## 1. Table inventory and classification

**One table.** The excerpt contains a single activity grid, titled "Study Schedule Protocol I8R-JE-IGBJ"
on the first page, running continuously from document page 11 to document page 18.

| Table | Type | Pages | Data columns | Activities | Annotations |
|---|---|---|---|---|---|
| 1 | `main_soa` | 11–18 | 8 (positions 2–9) | 36 | 27 |

**Why `main_soa`:** the rows are procedures performed on subjects (reference test → not `reference`),
and it is the only activity grid in the excerpt — the anchor timeline. No second population, no finer
timing table, no separate domain block.

**Why one table and not main + 7 continuations.** The identical two-row header reprints on all eight
pages and the body rows simply run on; per §1b — header and `schedule_property` rows reprint on
every continuation page, and each activity and each mark is counted once — this is extracted as one table with
`page_start` 11 / `page_end` 18, with each activity carrying its own `source_page`. No
`continuation_of` is emitted.

**Activity rows per page across the declared range** — every page in 11–18 contributes rows, so there
is no skipped page:

| doc page | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 |
|---|---|---|---|---|---|---|---|---|
| activity rows | 8 | 4 | 3 | 7 | 5 | 4 | 4 | 1 |

Cross-check: the recovered horizontal rule bands per page are 10, 6, 5, 9, 7, 6, 6, 3 — in every case
exactly 2 header bands plus the body-row count above, so the band structure accounts for all 36 rows
with none left over. Page 18 carries one activity row plus the abbreviation block and the two
below-table notes.

## 2. Column model (§5)

Eleven vertical rules were recovered from the render, giving 10 source columns:

- **L = 1 leading label column** — the activity-name column, headed "Procedure". **The first data
  column is therefore position 2**, and data columns run 2–9.
- Positions 2–9 are the schedule columns: Screening / Period 1 Day -1 / Period 1 Day 1 / Wash out /
  Period 2 Day -1 / Period 2 Day 1 / Follow-up/ED / Additional Follow-up for TE ADA.
- **Position 10 is the right-hand "Comments" column.** Per §6 it is not a schedule column and not an
  activity: no `schedule_grid` or `activity_schedule` entry is emitted for it, and each non-empty
  Comments cell became one annotation (see §5 below). Source numbering was not compacted — position
  10 simply carries no cells.

## 3. Header rows and merged header cells

Two `schedule_property` rows.

- **Row 1 — `property_name` "Study Period", `epoch`, level 1, SYNTHESISED.** The label cell in the
  Procedure column is empty on this row; the name was synthesised from the row's own values
  ("Screening", "Period 1", "Wash out", "Period 2", "Follow-up/ED", "Additional Follow-up for TE ADA").
  `property_name_source.synthesized: true`, `cell_value: ""`.
- **Row 2 — `property_name` "Study Day", `study_day`, level 2, SYNTHESISED.** The label cell on this
  row prints "Procedure", which is the column heading for the activity-name column, not a name for
  this schedule row. Rather than promote a mis-fitting label, the name was synthesised from the row's
  values ("Days -28 to -2", "Day -1", "Day 1", "3 to 14 days", "Within 28±2 days after last study
  treatment") and the printed text was retained in `property_name_source.cell_value: "Procedure"` with
  `synthesized: true`. This is the one place where a printed label cell was overridden; it is flagged
  here because a reviewer may prefer `property_name: "Procedure"`.
- Both rows carry a `hierarchical_level`: neither alone distinguishes the columns. "Day -1" occurs in
  columns 3 and 6 and "Day 1" in columns 4 and 7, told apart only by the Period row above; and
  "Period 1"/"Period 2" each span two columns, told apart only by the day row below.
- `structure_method` is not set on either row: both `property_type` and the level come from the
  printed header labels, which is the schema default.

**Merged header cells.** The only merged cells in the entire table are in header row 1:
"Period 1" spans columns 3–4 and "Period 2" spans columns 6–7. Each is emitted once per covered
column with `is_merged_cell: true` and `merged_cell_range` `"3:4"` / `"6:7"` (numeric column
positions, not A1 notation). This was confirmed from rule-line geometry, not from where the glyph
sits: in the header band the vertical rules at the 3|4 and 6|7 boundaries have zero ink coverage,
while all eleven rules are present at 100 % coverage in every other band of every page.

## 4. Merged-mark decisions (§5)

**None — no body mark or body text cell is merged, and none was distributed.** Every body row band on
every page shows all eleven vertical rules at full coverage, so each mark and each timing string sits
in exactly one column. `source_range` is therefore empty throughout `activity_schedule`. There are no
spanning arrows, no vertically-merged marks and no qualified marks in this table.

The in-cell timing strings (e.g. "Pre-hypoglycemia induction, 90 min", "240 min",
"Pre-hypoglycemia induction, -5, Predose, 5, 10, 15, 20, 25, 30, 40, 50, 60, 90, 120, 240 min") are
transcribed literally as `cell_value` in the single column they occupy — they are cell content, not
merged spans, and they are not annotations.

## 5. Annotations

27 annotations: 25 footnotes + 2 source_notes. `by_type` is not degenerate and is not all-`source_note`.

**Comments column (25 annotations, markers `c1`–`c25`).** Each non-empty Comments cell is one
annotation. Cell extent was fixed by the horizontal rule lines of that page (see §7), **not** by
vertical-gap proximity, so no note is split across rows and no two notes were merged. The source
prints no markers for these notes, so markers were synthesised in printed order and each annotation's
`marker_locations` entry carries `method: "synthesized"` with `location_type: "activity_name"` on the
row the note sits beside; the same marker was added to that activity's `annotation_markers`.

Two Comments notes are printed verbatim on two different rows each, and are deduplicated to one
annotation with two locations (§6):

- `c15` "See Appendix 2, Clinical Laboratory Tests, for details." — rows 24 (Clinical Serology Tests,
  page 14) and 26 (HbA1c, page 15). Typed `source_note`: the whole note is a bare pointer.
- `c19` "Additional tests can be done at the discretion of the investigator." — rows 29 (Ethanol
  Testing, page 15) and 30 (Urine Drug Screen, page 16). Typed `footnote`: it explains rather than points.

`c5` "Refer to Section 9.5.5.1." is the third bare pointer and is also typed `source_note`. All other
Comments notes explain and stay `footnote`, including `c16`, which points *and* explains.

**Printed footnote `a` (1 annotation).** The superscript "a" is printed on the header cell of column 9
(text layer: "Additional Follow-up for TE ADA" followed by a superscript "a"). Per §6 it is recorded as `annotation_markers: "a"` on **that
column's `schedule_grid` cell** (row 1, column 9), cleaned out of `cell_value`, not on the
`schedule_property` row — so it resolves to the specific column it governs. Its text is printed below
the table on page 18: "Samples for immunogenicity, should be collected every 12 weeks (84±7 days)" …
"agreed upon between both the investigator and sponsor."

**General table note (`gn1`, synthesised marker).** "Note: if multiple procedures take place at the
same time point, the following order of the procedure should be used: ECG, vital signs, and
venipuncture." … "Plasma glucose sample should be collected as close as possible of
protocol-specified time point." It is printed below the table with no marker and applies to the whole
table. Following the schema's own description of `method: "synthesized"` — a general table note
anchored to a property row for traceability — it is anchored to `schedule_property` row 1 with
`method: "synthesized"` and `gn1` was added to that row's `annotation_markers` so the §8 marker-agreement
check passes and `resolve` can link it. **Reviewer note:** this is a table-scope note, not an
epoch-row note; the row-1 anchor is a traceability convention, not a scope claim.

**Abbreviation block — zero annotations, deliberately.** Page 18 carries a 13-term block,
"Abbreviations: CRU = clinical research unit; ECG = electrocardiogram; ED = early discontinuation;
FSH = follicle-stimulating hormone;" … "ADA = treatment-emergent antidrug antibodies." None of these
terms is printed as a *marker* in a grid cell, a header cell or an activity label; they only occur
inside running text and inside activity names (CRU in "Admission to CRU", ECG in "Single 12-lead ECG
(Local)", PK in "PK (Glucagon)", HbA1c, FSH, PG, PD, IMG). §6 forbids binding an `abbreviation` by
word overlap (`text_match`) or by `synthesized`, so the whole block is dropped rather than emitted as
13 orphans. Flagged here because it is a visible drop of source content.

**Source defect observed and NOT repaired:** the abbreviation block prints
"IMG =intramuscular glucagon" with the space on the wrong side of the equals sign. Since the block
yields no annotation the typo does not reach the JSON, but it is recorded here rather than silently
corrected.

**Annotation text integrity.**
- The text layer is **not** glyph-spread; words are whole tokens. No `deglyph_reconstruction` was
  needed and no text field was reconstructed.
- Each annotation's text starts at its cell's first word and ends at its last. Line breaks inside a
  cell are wrapping artifacts and were joined with a single space; where the source sets a double
  space after a sentence period (e.g. inside `c17`), the join normalises it to one space. No other
  normalisation was applied. Unicode was preserved verbatim (±, ≥, ≤, <, and the right single
  quotation mark in "Review patients’ insulin regimens").
- **Containment pairs — both re-verified against the page, both source-faithful, neither merged nor
  truncated:**
  1. `c15` ⊂ `c16`. `c15` is the whole Comments cell of row 24 (page 14) and row 26 (page 15);
     `c16` is the whole Comments cell of row 25 (page 15), which opens with the same sentence and then
     continues "At Periods 1 and 2, samples should be collected from patients who have fasted at least
     8 hours before any study procedures." Three distinct rule-bounded cells on two pages. This is the
     source repeating a pointer, not one cell split across rows.
  2. `c20` ⊂ `c21`. `c20` is the whole Comments cell of row 31, PK (Glucagon), page 16.
     `c21` is the whole Comments cell of row 32, Plasma Glucose for PD, on the same page, which opens
     "-5 mins = stop insulin infusion." and then repeats `c20` verbatim. Two adjacent but distinct
     rule-bounded cells (bands y 600–853 and 853–1142 in the 200 dpi render). Source-faithful.
- No note was left unbounded; there is no `proximity_bounded` annotation in this extraction.

## 6. Activities and hierarchy

36 activities, rows 3–38 (rows 1–2 are the two header rows).

Three rows are organisational section headers carrying no marks, set to `indentation_level: 0`:
"Clinical Assessments" (row 3, page 11), "Laboratory Tests" (row 23, page 14) and
"Health Outcome Instruments" (row 35, page 17). All three are printed **bold on a grey-shaded band
spanning the full table width**, with every schedule cell in that band empty — verified on the
rendered page. All other 33 rows are `indentation_level: 1`.

The text layer carries no leading whitespace for any label, so the hierarchy signal is typographic:
every activity records `activity_name_source.indentation_method: "font_signal"`, and the three section
headers additionally record `text_formatting: ["bold"]`.

No activity label carries an inline section/appendix reference, so no `pr`-series `source_note` was
synthesised; the protocol's cross-references all live in the Comments column and are handled there.
No non-activity rows were created: the repeated "Procedure" header band on each page is modelled as
the header row, not as an activity.

## 7. Mechanical mark-check (§1b/§1d)

Method: **rule lines recovered from a 200 dpi raster render, text read from the PDF text layer, each
token binned into the cell its centre falls in.** `pdftoppm -r 200 -gray`; ink = pixel < 128; vertical
rules = image columns with high ink fraction over the table height (found at x = 193, 490, 627, 697,
919, 1043, 1117, 1339, 1518, 1701, 2005 px, identical on all eight pages); horizontal rules = image
rows with > 0.85 ink fraction **within each column's own x-range**, computed per column so a
full-width band could be distinguished from a partial one. All horizontal rules turned out to be
full-width; there are no vertically merged body cells. Per-band vertical-rule coverage was then
measured to detect merged cells, which is how the two merged header cells were found and how every
body cell was confirmed unmerged. `pdfplumber` was not used.

Marks were matched with `^[Xx][*a-zA-Z0-9]?$` on the raw token stream. **All marks in this table are
plain "X"; no footnoted mark (`X*`, `Xa`) occurs.**

- Token-stream X count per PDF page: 16, 13, 6, 5, 9, 4, 3, 0 = **56**.
- X cells in the delivered `activity_schedule`: **56**. Exact match, so no mark was lost to the
  header/label columns or to a band boundary, and no mark was double-counted from a reprinted header.
- 24 further cells carry timing text rather than an X, for 80 `activity_schedule` entries in total.

**Visual diff:** the mechanical matrix was read cell-for-cell against rendered crops on the header
band and on five representative rows — dense ("Collect Pre-existing Conditions and Adverse Events",
7 marks in columns 2–8), sparse ("Admission to CRU", columns 3 and 6 only), a mixed
mark/text row ("PK (Glucagon)", text in 4 and 7, X in 8 and 9), an irregular pattern
("Pregnancy Test (Female patients of childbearing potential only)", columns 2, 3, 6, 8) and a section
header ("Laboratory Tests", empty). **No disagreement was found between the mechanical matrix and the
visual read on any checked cell.**

A spot-check of the resolved grid against pages 11–18 is nevertheless recommended for the four rows
whose marks fall only in the two right-hand columns (rows 31 and 34 carry X in both column 8,
Follow-up/ED, and column 9, Additional Follow-up for TE ADA), since column 9 is narrow and is the
column governed by footnote `a`.

## 8. Low-confidence calls

1. **`property_name` "Study Day" for header row 2** overrides the printed label cell "Procedure"
   (§3 above). The alternative reading — `property_name: "Procedure"` — is defensible; the printed
   text is preserved in `property_name_source.cell_value` either way.
2. **`property_type` of header row 2.** "Days -28 to -2" and "3 to 14 days" and
   "Within 28±2 days after last study treatment" are day *windows* rather than single protocol days,
   so `window` was considered. `study_day` was chosen because the row's dominant content is protocol
   days relative to dosing ("Day -1", "Day 1", twice each) and there is no separate day row for the
   windows to qualify.
3. **Column 9, "Additional Follow-up for TE ADA", is treated as a schedule column, not a note
   column.** It is bounded by rules like the other schedule columns, it sits left of Comments, and it
   carries two X marks (rows 31 and 34). Only the rightmost column was treated as notes.
4. **The single-page-8 body row.** Page 18 contributes exactly one activity ("Edinburgh Hypoglycemia
   Scale: Experimental Hypoglycemia"); the rest of the page is the abbreviation block and the two
   below-table notes. This is a genuine one-row page, not a truncated read.
5. No full-protocol markdown was supplied, so there is no PDF/markdown text disagreement to report;
   all text came from the PDF text layer.

## 9. Orphan risk

- Every annotation has ≥ 1 `marker_locations` entry (29 locations across 27 annotations), and every
  marker in a `marker_locations` entry also appears in that row's `annotation_markers` — checked
  mechanically against the delivered JSON, zero mismatches.
- No marker is referenced in the table without a printed definition, and no definition is printed
  without a location: footnote `a` is the only printed marker in the table and its text is present.
- The only annotation whose placement is a convention rather than a printed position is `gn1`
  (see §5). Its scope is the whole table; the row-1 anchor should be read as traceability.
- 13 abbreviation terms were deliberately dropped rather than emitted as orphans (§5).

## 10. Method provenance — every non-default method recorded

| Where | Field | Value | Count |
|---|---|---|---|
| `schedule_properties` rows 1, 2 | `property_name_source.synthesized` | `true` | 2 |
| `activities` rows 3–38 | `activity_name_source.indentation_method` | `font_signal` | 36 |
| `annotations` `c1`–`c25` | `annotation_text_source.method` | `raster_band_cells` | 25 |
| `marker_locations` on `c1`–`c25` | `method` | `synthesized` | 27 |
| `marker_locations` on `gn1` | `method` | `synthesized` | 1 |

- `raster_band_cells` is recorded on the Comments-column notes because their cell boundaries were
  recovered from rule lines detected in the rendered page (§1d) rather than from a vector cell box;
  the text itself came from the text layer.
- **No `proximity_bounded`, no `proximity`, no `text_match`, no `visual_transcription`, no
  `glyph_reconstruction`, no `raster_pixel_detection` anywhere** — the text layer is clean and every
  cell boundary was recoverable.
- **No `location_type` of `unresolved` in this extraction**: every marker's target was determinable
  from its printed position or, for the unmarkered Comments notes, from the rule-bounded row it
  occupies.
- `structure_method` is absent on both schedule properties (printed labels), and `method` is absent on
  all `schedule_grid` and `activity_schedule` cells (bbox text-layer read) — both are the schema
  defaults.
