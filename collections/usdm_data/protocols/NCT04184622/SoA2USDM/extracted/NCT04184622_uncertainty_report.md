# NCT04184622 — SoA extraction uncertainty report

Protocol I8F-MC-GPHK(b). Source: NCT04184622_soa.pdf (11 PDF pages = document pages 18–28 per PAGEMAP.md). Prompt v3.8.1, single pass, no protocol markdown.

## Decisions needed (7)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | Tables 1–2, every '*' (e.g. p.18 "Visit*") | Every '*' is read as pointing to one specific note in the "Additional Information" table (pp. 25–28). Each note got its own id (n1–n27) and is attached to the starred row, visit or cell whose name matches that note's Activity cell. | Treat '*' as one generic pointer ("see table below") and emit a single annotation attached to every starred element. The note-to-element matching is then left to a later step. | 3.1 |
| D2 | p.19, Table 1 row 25 "Dispense study drug", Visit 21 cell "X*" | The note "Dispense study drug Visit 72" (applies only to participants going into the prediabetes 2-year period) is attached only to the one starred cell, Visit 21 / Week 72. | Attach it to the whole Dispense study drug row. The note's own label names "Visit 72", and no visit has that number. | 3.2 |
| D3 | p.20, Table 1 rows 38, 39, 40, 41, 44 (UACR, Cystatin-c, Calcitonin, Hematology, TSH), Visit 1 cells "X*" | The eligibility note ("Screening visit assessment will be used to confirm eligibility …") is attached only to the five starred Visit 1 cells, where the '*' is printed. It is not attached in Table 2, where these rows have no '*'. | Attach it to the five activity rows as a whole (all visits), and/or to the same-named rows in Table 2. | 3.2 |
| D4 | pp.18–24, header rows 4–5 of both tables | "Fasting Visit" and "Telephone Visit" (blue header rows reprinted on every page) are recorded as header properties: Fasting = other, Telephone = modality, hierarchical_level null. | Record them as activity rows with X marks. | 3.3 |
| D5 | p.21, Table 1 row 51 | The label is transcribed literally as printed: "C-SSRS (Baseline/Screening) Version)", stray parenthesis included. | Normalise it to "C-SSRS (Baseline/Screening Version)", the spelling used in the Additional Information table. | 3.4 |
| D6 | p.22, Table 2 | Section 1.3.2 is typed `track` with track_label "Prediabetes". | Type it `main_soa` (an independent schedule with its own columns) rather than a population/phase track. | 4.1 |
| D7 | p.24, Table 2, line "*Please see table below for corresponding additional information" | This pointer line is not emitted as an annotation of its own. Its content is carried by the per-note annotations it points to. | Emit it as an extra `source_note` with marker '*', attached at table level. | 4.3 |

## Recorded, not open (8)

- Additional Information table (pp. 25–28) was converted into annotations rather than a table. Rule: taxonomy "reference only if rows key to nothing in the schedule" / prompt §2.
- Both abbreviation lists (p.24 and p.28) were dropped. No abbreviation is printed as a marker in any cell (prompt §6: an abbreviation block normally yields zero annotations).
- Header-cell stars (Visit 3*, 99*, 199*, ED*, 801*, 802*) are encoded on the Visit-row `schedule_grid` cell of that column, not on the property row (§6 header-cell footnotes).
- n1 (Visit / Fasting Visit note) is bound to the whole "Visit*" and "Fasting Visit*" property rows. n6 is bound to the "Allowable Deviation (days)*" row. These are whole-row notes (§6).
- Table 2, p.24, Self-Harm Follow-up Form, Visit 199: the X is printed near the top of its cell. The raster rule lines place it inside the Follow-up Form row, and it is transcribed there. The Supplement Form row already has its own X at Visit 199.
- Rows before the first section band (Table 1 rows 6–8; Table 2 row 6: Informed consent, Randomization, Register study visit in IWRS) are indentation 0 and carry marks. Section bands are 0 and carry no marks. Children are 1 (§4).
- L = 1 label column in both tables, so the first data column is position 2. Table 1: 24 data columns (2–25). Table 2: 19 data columns (2–20).
- No merged marks, arrows or spans exist in the body. The only merged rows are the full-width section bands, confirmed from raster rule lines (§5).

## 1. Method

- The text layer is clean on all pages: no glyph spreading and no scanned grid. The pages have raster-detectable ruling.
- Row bands: horizontal rules were detected in the label column from a 200-dpi render (§1d). Every text-layer word was assigned to the band holding its centre.
- Columns: per page, each mark token was binned to the nearest Visit-row header centre (§1b). The regex allowed `X*`.
- Merge check: every band was tested for missing internal vertical rules. Only the section-header bands (full-width) lack them.
- Cross-check: the header-centre binning was diffed against the rule-line cell assignment. The two agree on every cell except p.20's last two columns (ED, 801). There the raster detector missed the thin ED/801 vertical rule and pooled both columns. The header-centre read was used, and it was checked visually against the render: the OGTT row has X at ED only, and all other rows have X at both ED and 801.
- Spot checks against the render: several dense and sparse rows per page (Weight, ECG, Dispense study drug, Immunogenicity, TZP PK, C-SSRS, PGIS; Table 2 Dispense, OGTT, Self-Harm Follow-up). All matched.
- Method provenance recorded:
  - `indentation_method: visual_estimate` on all activities (hierarchy taken from shaded full-width section bands).
  - `structure_method: inferred_from_layout` on Fasting Visit and Telephone Visit.
  - `method: text_match` on every marker location (see 3.1).
  - No `unresolved` locations and no proximity-bounded notes.

## 2. Tables

| Table | Type | Pages | Data columns | Activity rows (incl. section bands) | Marks | Annotations |
|---|---|---|---|---|---|---|
| 1 — 1.3.1 SoA covering visits to primary study endpoint | main_soa | 18–21 | 24 (Visits 1–21, 99, ED, 801) | 54 | 414 | 27 |
| 2 — 1.3.2 SoA for additional 2-year treatment period, prediabetes | track ("Prediabetes") | 22–24 | 19 (Visits 101–116, 199, ED, 802) | 39 | 311 | 18 |

- Rows per page, Table 1: p.18 = 15, p.19 = 11, p.20 = 16, p.21 = 12.
- Rows per page, Table 2: p.22 = 14, p.23 = 17, p.24 = 8.
- Every page in both ranges contributed rows.
- Pages 25–28 lie outside both declared ranges. They hold only the Additional Information notes table and abbreviations, which supplied annotation text.
- The five header rows (Visit, Week of Treatment, Allowable Deviation, Fasting Visit, Telephone Visit) are reprinted on every page. They were de-duplicated after a check that each reprint is identical to the first.
- Header levels: Visit = 1 (visit), Week = 2 (week), Allowable Deviation = 3 (window), Fasting and Telephone = null.
- No synthesized property names.

## 3. Table 1 details

### 3.1 Marker disambiguation (D1)
- The source uses a single '*' for all footnotes. Its meaning is given on p.24 as "*Please see table below for corresponding additional information".
- Each row of the Additional Information table became one `footnote` annotation, with synthesized marker ids n1–n27 in printed order. The ids are the same in both tables.
- Each '*' was bound by matching the starred label to the note's Activity cell, so every location carries `method: text_match`. The '*' itself is printed at every location.
- In cell_text and property_name_source, the raw '*' is preserved.
- All 27 notes bind at least once in Table 1. No starred element lacks a note.

### 3.2 Cell-scoped notes (D2, D3)
- n16 (Dispense study drug Visit 72) → the X* at row 25, col 22 (Visit 21, Week 72). The source label "Visit 72" is evidently a week reference.
- n22 → the Visit 1 X* cells of rows 38–41 and 44.

### 3.3 Fasting / Telephone rows (D4)
- Grey-shaded body columns coincide with the Telephone Visit X's. The shading was treated as formatting only.

### 3.4 Text
- Hyphenated line breaks were joined in activity_name: "study-drug", "c-peptide".
- cell_text keeps the printed line joins.
- The C-SSRS row keeps the source typo (D5).

## 4. Table 2 details

### 4.1 Classification (D6)
- It has its own section number (1.3.2), title, visits (101–116, 199, ED, 802) and weeks (78–176).
- It schedules only participants with prediabetes at randomization. This is a separate phase for a sub-population, hence `track`.
- It is not `continuation` or `domain`: the columns differ.

### 4.2 Bindings
- 18 of the 27 notes apply (the others concern rows that are absent from, or unstarred in, Table 2).
- The UACR/Cystatin-c/Calcitonin/Hematology rows carry no '*' here, so n22 is not bound (see D3).

### 4.3 Pointer note (D7)
- The "*Please see table below…" line prints only on p.24 (below Table 2). It is not emitted as a separate annotation.

## 5. Annotation text integrity
- The notes were read from the vector-ruled Additional Information table (cells bounded by rule lines, so the default method applies).
- The text layer on p.28 reads "assignedby" in the TZP PK note. The render shows "assigned by", and "assigned by" was used.
- Printed spellings were kept: "study drug (s)" and "Counselling".
- The underlined italic "after" in n26 is rendered as plain text.
- Containment pairs: n11 ("All training should be repeated as needed to ensure participant compliance.") is contained in n14 and in n15 (n15 lacks the final period). Re-verified against pp.26–27: these are three separate rule-bounded cells for three different activities. They are source-faithful, not a split note.
- Orphan risk: none. Every annotation has at least one location, and every marker in a location appears on that element's `annotation_markers`.
