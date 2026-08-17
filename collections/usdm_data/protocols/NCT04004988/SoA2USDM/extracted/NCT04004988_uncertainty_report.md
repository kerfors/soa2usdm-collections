# NCT04004988 — SoA extraction uncertainty report (prompt v3.7.3)

Source: `/root/ph3/blind/NCT04004988/NCT04004988_soa.pdf` (3 PDF pages = document pages 9, 10, 11 per PAGEMAP.md).
Deliverable: one table file, `NCT04004988_Table_01_extraction.json`.

---

## 1. How many tables the excerpt contains

**One.** Document page 9 carries only the section heading `2. Schedule of Activities` and no grid.
Document page 10 opens the grid under the caption `Study Schedule Protocol I8F-MC-GPGS`; document
page 11 continues the same grid with the two header bands reprinted verbatim (identical column rules
at identical x positions, identical labels `Screening` … `D36` … `ED` … `Comments`) and then the
closing footnote block.

Per §1b ("header and `schedule_property` rows … reprint on every continuation page; count each
activity and each mark once") the reprinted header is de-duplicated and the whole grid is emitted as
**one table, `page_start` 10, `page_end` 11**, not as a parent plus a `continuation` child. This is
recorded in `table_metadata.notes`.

## 2. Per-table summary

| | |
|---|---|
| `table_number` | 1 |
| `table_type` | `main_soa` |
| `table_title` | `Study Schedule Protocol I8F-MC-GPGS` |
| pages | 10–11 (document numbering, from PAGEMAP.md) |
| physical columns | 16 |
| leading label columns `L` | **1** (column 1, headed `Procedure`) |
| **first data column** | **2** |
| data columns | 2–15 (14 columns) |
| trailing notes column | 16 (`Comments`) — excluded from `schedule_grid` / `activity_schedule` per §6 |
| `schedule_properties` | 2 |
| `schedule_grid` cells | 28 |
| activities | 20 |
| `activity_schedule` cells | 90 |
| annotations | 17 |

**`table_type` reasoning.** Rows are procedures performed on subjects (`Informed Consent`,
`Physical Examination`, `PK Sampling`), so not `reference`. There is no second grid to be a
`domain`, `subsidiary`, `track` or `continuation` of. It is the protocol's only and primary
anchor grid → `main_soa`. Not an interpretive call.

**Column map (data columns).** 2 = Screening `D-28 to D-2`; 3 = `D-1`; 4 = `D1`; 5 = `D2`;
6 = `D3`; 7 = `D4`; 8 = `D5`; 9 = `D6`; 10 = `D7`; 11 = `D8`; 12 = `D15`; 13 = `D21`;
14 = `D36 (±1)`; 15 = `ED`.

### Activity rows per page across the declared range

| document page | activity rows | row_positions |
|---|---|---|
| 10 | 17 | 3–19 |
| 11 | 3 | 20–22 |

Every page in the declared range 10–11 contributes activity rows. No page in the range contributed
none. (Document page 9 is outside the declared range — it holds the heading only.)

## 3. Method — how the grid was read

The SoA pages carry a clean, well-behaved text layer (`pdftotext -bbox`); this is **not** an
image-based table and **not** glyph-spread. Cell *geometry* was nevertheless recovered from the
raster per §1d so that one method covers the whole table:

- Rendered at 200 dpi (`pdftoppm -r 200`), ink threshold < 128.
- **Vertical rules:** 17 columns of near-continuous ink at x = 68.2, 154.6, 194.4, 216.7, 254.9,
  277.6, 300.2, 323.1, 346.0, 368.5, 395.8, 423.9, 450.7, 477.9, 510.3, 537.7, 737.3 pt —
  **identical on both pages** — bounding 16 physical columns.
- **Horizontal rules:** taken from inside the `Procedure` column (a text column, never a shaded or
  redacted one, per §1d), ink fraction > 0.85 across the column's x-range. Page 10 → 20 rules → 19
  bands (2 header + 17 activity). Page 11 → 6 rules → 5 bands (2 header + 3 activity).
- Every text-layer token was then assigned to the (band × column) rectangle its centre falls in.

**Mechanical mark-check vs. visual read.** The band×column matrix was diffed cell-for-cell against a
direct visual read of the 150 dpi renders of both pages, plus zoomed crops of the
`Subject Discharge from CRU` / `Outpatient Visit` band and the `Immunogenicity` band (the two rows
where a single isolated `X` sits far from its neighbours). **No disagreement.** All 90 grid values
agree between the two reads.

**Merged-cell check.** For every body row band the presence of each of the 17 vertical rules was
tested individually (max ink fraction over a ±1 px window). All 17 rules are fully present in all 20
body bands → **there are no merged cells anywhere in the table body**, so no mark or text was
distributed across a span and no `source_range` is set on any `activity_schedule` entry.

The only merge in the table is in header row 1: rules at 216.7 … 477.9 pt are absent inside that
band (max ink fraction 0.00–0.40, versus 1.00 at 68.2 / 154.6 / 194.4 / 510.3 / 537.7 / 737.3), so
the cell `Periods 1 and 2 Study Days – at least 35 days washout between Day 1 doses` spans
**columns 3–14**. It is emitted on all 12 covered positions with
`is_merged_cell` true and `merged_cell_range` `3:14`. Note this puts `D-1` (column 3) inside the
"Periods 1 and 2" band, which the rule geometry supports and eyeballing the centred caption would
not have.

`ED` (column 15) and `Comments` (column 16) are **not** vertically merged across the two header
bands: the horizontal rule at y = 99.9 pt runs at ink fraction 1.00 across all 16 columns. Row 2 of
those two columns is genuinely empty; row 2 column 15 is emitted with an empty `cell_value`.

## 4. Merged-mark decisions

None in the body — see §3. No arrows, no vertically-merged marks, no qualified marks.

## 5. Synthesised names and markers

**Synthesised `property_name` (2):**

- Row 1 → `"Study Period"`. The label cell (row 1, column 1) is genuinely empty in the source;
  `property_name_source.synthesized` true and an empty `cell_value`.
- Row 2 → `"Study Day"`. The label cell is **not** empty — it reads `Procedure` — but that word
  heads the activity-name column, it does not name this header row. Rather than promote a column
  heading into a property name, the name was synthesised and the raw cell text preserved:
  `property_name_source.cell_value` = `Procedure`, `synthesized` true. **Flagging this as a
  judgement call**: an alternative reading would set `property_name` to `"Procedure"` literally.

**Synthesised annotation markers (14):**

- `n1` … `n12` — the twelve non-empty cells of the right-hand `Comments` column, in printed order
  (rows 5, 6, 9, 13, 14, 15, 16, 17, 19, 20, 21, 22). Each is one `footnote` bound by
  `marker_locations` to the activity row it sits beside, `method` `synthesized`, and the same
  marker is added to that activity's `annotation_markers` so `resolve` can link it.
- `tn1`, `tn2` — the two general notes printed below the grid on page 11 (`Note:  All sampling
  times are given relative to dosing …` and `If multiple procedures take place at the same time
  point …`). These are **table-scope**: they carry no printed marker and qualify no single row.
  Per §6's table-scope convention each is given one `schedule_property` `marker_location`
  (row 1, `method` `synthesized`) for traceability, and the marker is deliberately **not** written
  into any element's `annotation_markers`. This is the one place where the §8 check "every marker in
  an annotation's `marker_locations` also appears in that row's `annotation_markers`" is knowingly
  not satisfied, per the explicit carve-out in §6.

The printed markers `a`, `b`, `c` are transcribed as-is and are not synthesised.

## 6. Annotation text integrity

The text layer is **not** glyph-spread — words come back whole (`Screening`, `Pharmacogenetic`,
`Immunogenicity`), so no de-glyphing was needed and no field was reconstructed under §1c.

Each `Comments` note was bounded by its own rule-line band, not by proximity. Provenance is recorded
on all twelve as `annotation_text_source.method` = `raster_band_cells` (§1e), because the horizontal
rules came from the 200 dpi render rather than a vector layer read. Note text itself is text-layer
text. No note used `proximity_bounded`.

**Containment / overlap check:** no annotation's text is contained in another's, and no pair shares
a long run at a note boundary. The nearest thing to an overlap is the pair
`"Female subjects only.  Serum pregnancy test will be performed at screening and urine pregnancy
tests at subsequent visits."` (row 16, `Pregnancy Test`) and `"Female subjects considered to be
postmenopausal only.  Women with confirmed nonchildbearing potential status can be exempted from
further pregnancy tests during the study after screening."` (row 17, `FSH test, if applicable`).
Re-verified against the page: these are **two genuinely distinct source cells** in adjacent rule-line
bands (y 397.8–429.1 and 429.1–471.2 pt on page 10); they share only the opening word `Female`. Not
a split note.

Sentence-internal double spacing as printed by the source (`at screening.  Thereafter,`) is
preserved; wrapped lines are joined with a single space.

## 7. Comments column and abbreviation handling

The `Comments` column (column 16) is a right-hand notes column: per §6 it is neither a schedule
column nor an activity. It yields 12 `footnote` annotations and no grid values. `by_type` for this
table is therefore 17 × `footnote` and 0 of everything else — all-`footnote` is the normal shape for
a notes-column table (§8).

One note is a pointer that also explains — `"See Appendix 2 for details.  Day 1 predose sample is
for baseline only."` — so it stays `footnote`, not `source_note` (§8, which uses this exact shape as
its example). No note in this table is a bare pointer, so **no `source_note` is emitted**. No
activity label carries an inline section/appendix reference, so no `pr` markers were needed.

**The abbreviation block yields zero annotations.** Page 11 prints
`ADA = antidrug antibodies; AE = adverse event; CRU = clinical research unit; D or d = day;
ECG = electrocardiogram; ED = early discontinuation; PK = pharmacokinetic(s); TE ADA =
treatment-emergent antidrug antibodies.` None of these eight terms is printed **as a marker** on a
grid cell, header cell or activity label. `AE`, `CRU`, `ECG` and `PK` occur only *inside* activity
names (`AEs/Concomitant Medications`, `Subject Admission to CRU`, `Safety 12-lead ECG`,
`PK Sampling`), and `ED` / `D` occur only as header *cell values* — word overlap, not markers.
§6 forbids binding an `abbreviation` by `text_match` or `synthesized` for exactly this case, so all
eight are dropped rather than emitted as orphans.

## 8. Low-confidence calls

1. **`property_type` of header row 1 — `epoch` vs `period`.** Values are `Screening`, the merged
   `Periods 1 and 2 Study Days – at least 35 days washout between Day 1 doses`, and `ED`. These
   read as major study phases → `epoch`. The source's own word "Periods" would argue for `period`;
   the phases are screening / on-treatment / early-discontinuation, which is the `epoch` pattern,
   and the merged cell covers *both* Period 1 and Period 2 rather than distinguishing them, so it
   cannot be a per-period band. Recorded in `property_comment`.
2. **`property_name` of header row 2** — see §5 above.
3. **`Physical Examination /Medical Assessment` spelling.** The source cell wraps as
   `Physical Examination` / `/Medical Assessment`; the leading slash begins line 2. The wrapped
   lines were joined with a single space, giving `Physical Examination /Medical Assessment`. A
   reader might prefer to close the gap between `Examination` and `/Medical`, but that would be a
   repair of the source rather than a transcription, so the space is kept. Raw
   `activity_name_source.cell_text` preserves the line break.
4. **`0 hour` in the `Tirzepatide Dosing` row** (column 4, `D1`) is kept as a `cell_value`, not an
   annotation — it is an in-grid timing indicator like `Predose` (§5).
5. **`Predose`, `Predose, 12`, `Predose, 8, 12`, `24, 36`, `48`, `72`, `96` … `480`** are
   transcribed literally as `cell_value`s. They are scheduling/timing indicators in the grid, not
   footnote text.
6. **`D-28 to D-2`** (column 2, row 2) is reassembled from a wrapped cell (`D-28 to D-` / `2`).
7. No PDF/markdown disagreement could arise — no protocol markdown was attached; the PDF is the
   sole source and is authoritative for the row set.

## 9. Orphan risk

None. All 17 annotations carry at least one `marker_location`:

- `a` → 5 `activity_name` locations (rows 13, 19, 20, 21, 22 — the five labels printing a
  superscript `a`, confirmed by superscript y-offset in the text layer, e.g. `Safety 12-lead ECG`
  at y 294.0–304.1 with `a` at y 293.2–302.0).
- `b` → 3 `schedule_cell` locations on header row 2, columns 3, 4 and 14 (`D-1`, `D1`,
  `D36 (±1)`). Placed on the specific header cells, not on the `schedule_property` row, per §6 —
  the footnote governs those three visits only.
- `c` → 1 `schedule_cell` location on header row 1, column 15 (`ED`).
- `n1`–`n12` → one `activity_name` location each, `method` `synthesized`.
- `tn1`, `tn2` → one `schedule_property` location each, `method` `synthesized` (table scope).

Every marker whose definition is printed in the source has been transcribed, and every marker
printed in the source has a definition — no "referenced but not defined" defects, no redactions, no
illegible regions.

## 10. Method provenance recorded (§1e)

| where | field | value | why |
|---|---|---|---|
| both `schedule_properties` | `structure_method` | `inferred_from_layout` | `property_type` and `hierarchical_level` come from the two-band stacking geometry and the merge extents, not from printed row labels — the source prints no name for either header row |
| 12 `Comments` annotations (`n1`–`n12`) | `annotation_text_source.method` | `raster_band_cells` | note cell extents recovered from 200 dpi rule lines (§1d) |
| 12 `Comments` annotations | `marker_locations[].method` | `synthesized` | source prints no marker on these notes |
| `tn1`, `tn2` | `marker_locations[].method` | `synthesized` | table-scope general notes with no modelled element |

Not recorded (default method used): `activity_schedule` / `schedule_grid` `method` — every value is
a `pdftotext -bbox` text-layer read; `activity_name_source.method` — direct text-layer read;
`activity_name_source.indentation_method` — every activity label starts at exactly x = 72.0 pt in
the text layer, i.e. the whitespace read itself gives a flat table, so all 20 activities are
at `indentation_level` 0 with no non-default method. There are no organisational/section-header rows;
every row is a level-0 activity carrying at least one mark, which §4 explicitly allows for a flat
table.

**No `unresolved` `location_type` was needed anywhere in this table.**

## 11. Recommended spot-checks for the reviewer

1. Header row 1, columns 3–14 — confirm the "Periods 1 and 2 …" band really does begin at `D-1`
   and not at `D1`.
2. Row 22 (`PK Sampling (hours)`) columns 4–15 — the longest value run in the table
   (`Predose, 8, 12` … `480`, `X`, `X`).
3. Row 18 (`AEs/Concomitant Medications`) — the only row marked in all 14 data columns.
