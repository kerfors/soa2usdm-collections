# NCT01847274 — SoA extraction uncertainty report (prompt v3.7.3)

Source: `NCT01847274_soa.pdf`, 7 PDF pages. Page numbers below are **document** pages, taken from
`PAGEMAP.md` (PDF 1–4 → doc 63–66, PDF 5 → 72, PDF 6 → 73, PDF 7 → 78). Printed footers were not used;
they happen to agree with the map here (`63 of 103` … `78 of 103`), but the map is what was followed.
The excerpt is a non-contiguous slice: doc 67–71 and 74–77 are absent, and PAGEMAP marks no page as
falling beyond the declared SoA range.

Three tables were found and extracted, one file each.

---

## 1. Per table

### Table 01 — source "Table 7 Schedule of Events – Main Study", doc pp 63–66 — `main_soa`

* **Why `main_soa`:** rows are procedures performed on subjects; it is the protocol's primary anchor
  grid (screening → Cycle 1 → Cycle 2 → subsequent cycles → discontinuation → post-treatment).
* **Columns:** leading label columns **L = 1** (the single column that holds `Cycle` / `Day` and the
  activity names). **First data column = position 2**; 9 data columns, positions **2–10**:
  2 Screening, 3–6 Cycle 1 Days 1/8/15/21, 7 Cycle 2 Day 1, 8 Subsequent Cycles, 9 Study Treatment
  Discontinuation, 10 Post Treatment Assessments.
* **Header rows:** 2 (`Cycle`, level 1; `Day`, level 2).
* **Activities:** 28. **Marks:** 90. **Annotations:** 28.
* **Activity rows per page across the declared range 63–66:** 63 → 7, 64 → 11, 65 → 6, 66 → 4.
  Every page in the range contributed rows. This is one logical table whose two header rows reprint on
  all four pages; the reprints were de-duplicated to a single header (§1b).

### Table 02 — source "Table 8 Schedule of Events – Open-Label Food Effect Sub-Study (Study Completed)", doc pp 72–73 — `track`, `track_label` = "Food Effect Sub-Study"

* **Why `track` and not `domain` / `subsidiary` / `continuation`:** the population question decides it.
  The table schedules a mutually exclusive sub-study population — the source's own note says
  "Note: Patients in food effect sub-study will receive a single oral dose of niraparib 300 mg on FE Days 1
  and 8." — and it has its own timeline, including a 14-day food-effect period ("FE 1", "FE 8") that
  does not exist anywhere in Table 7. It is not finer timing for an existing activity (it repeats the
  whole assessment set), so not `subsidiary`; its columns differ from Table 7's, so not `domain`.
* **`track_label`:** "Food Effect Sub-Study" — taken from the table title, shortened past the
  "Open-Label …" qualifier and past "Schedule of Events".
* **Columns:** **L = 1**, **first data column = position 2**; 8 data columns, positions **2–9**:
  2 Screening, 3 FE 1, 4 FE 8, 5 C1 Day 1, 6 C1 Day 15, 7 C2 Day 1, 8 Subsequent Cycles / Cycle n Day 1,
  9 Treatment Discontinuation.
* **Header rows:** 3 (synthesised "Study Period" level 1; synthesised "Cycle" level 2; printed `Day`
  level 3).
* **Activities:** 17. **Marks:** 79. **Annotations:** 16.
* **Activity rows per page across the declared range 72–73:** 72 → 14, 73 → 3. Both pages contributed.
  Doc page 73 reprints the title with "(Continued)" and reprints all three header rows; header rows were
  de-duplicated into one set.

### Table 03 — source "Table 9: Schedule of Events – Main Study –Extended Visit Cycle", doc p 78 — `track`, `track_label` = "Extended Visit Cycle"

* **Why `track`:** it is a separate timeline for the subset of main-study patients moved onto the
  extended visit cycle, with its own column structure — three "Subsequent Cycles" columns told apart only
  by a `Location` row ("In Clinic" / "Site Staff Telephone Contact to Patient" / "Patient Visit to Local
  Clinic or In-Home Nursing"). Table 7 has no such columns, so not `domain` and not `continuation`; it
  carries the full assessment set rather than finer timing for a subset, so not `subsidiary`.
* **`track_label`:** "Extended Visit Cycle", from the table title.
* **Columns:** **L = 1**, **first data column = position 2**; 5 data columns, positions **2–6**.
* **Header rows:** 3 (`Cycle` level 1, `Day` level 2, `Location` level 3).
* **Activities:** 15. **Marks:** 40. **Annotations:** 15.
* **Activity rows per page:** 78 → 15. Single-page table; the page contributed rows.

No horizontally tiled tables in this excerpt, so the §5 tiling exception does not arise.

---

## 2. Merged-mark decisions

Three rows carry one printed mark inside a cell that is merged across many columns. In each case the
absence of internal vertical rules across that band was confirmed from the raster rule-line geometry
(§1d) before distributing, and the mark was distributed to every covered column with `source_range` set
rather than collapsed onto the column the glyph sits under.

| Table | Row | Activity | Span (`source_range`) | Marker carried |
|---|---|---|---|---|
| 01 | 30 | "Bone marrow aspirate and biopsy" (doc p 66) | `3:10` — 8 cells | `28` on each |
| 02 | 20 | "Bone marrow aspirate and biopsy" (doc p 73) | `3:9` — 7 cells | `15` on each |
| 03 | 18 | "Bone marrow aspirate and biopsy" (doc p 78) | `2:6` — 5 cells | `15` on each |

In all three the Screening / first data column is separately ruled and stays **empty**; the merge starts
at the column after it. The footnote marker was distributed with the mark, so each of these annotations
has one `marker_locations` entry per covered column and each covered cell carries the marker.

Merged **header** cells recorded with `is_merged_cell` / `merged_cell_range`:

* Table 01, header row 1: `"1"` spans columns **3:6** (the four Cycle 1 day columns). Confirmed from the
  band's rule pattern — the vertical rules after columns 3, 4 and 5 are absent in that band only.
* Table 02, header row 1: `"14-Day Food Effect"` spans **3:4**; `"Cycle"` spans **5:7**.
* Table 02, header row 2: `"C1"` spans **5:6**; `"C2"` is column 7 alone.
* Table 03: no merged header cells.

Vertically merged header cells (Table 01 columns 9–10 in the `Day` row; Table 02 columns 2, 3–4, 8, 9 in
the `C1`/`C2` row) have no representation in the schema. They are recorded as **empty** cells on the
lower row, and the fact is stated in each file's `table_metadata.notes`.

---

## 3. Synthesised values

**Synthesised `property_name` (2, both Table 02):**

* Table 02 header row 1 → `"Study Period"`. The label cell is empty in the source
  (`property_name_source.cell_value` is `""`, `synthesized: true`, `structure_method:
  "inferred_from_layout"`). The row spans the study's phases (Screening, 14-Day Food Effect, Cycle,
  Subsequent Cycles, Treatment Discontinuation).
* Table 02 header row 2 → `"Cycle"`. Label cell empty; the row holds only `C1` (cols 5–6) and `C2`
  (col 7). Same provenance flags.

**Synthesised annotation markers (1):**

* Table 02, marker `n1` — the last row of the table on doc page 73 is a full-width, rule-bounded cell
  containing only note text and no activity: "Note: Patients in food effect sub-study will receive a
  single oral dose of niraparib 300 mg on FE Days 1 and 8. … followed using the main study visit schedule
  for safety." Per §4 it is not an activity row. It is emitted as one table-scope `footnote` with the
  synthesised marker `n1`, anchored to `schedule_property` row 1 with `method: "synthesized"`.
  **Deliberate deviation from the marker-pairing rule:** `n1` is *not* written onto any element's
  `annotation_markers`, because the note has no modelled element to attach to and scoping it to the
  bone-marrow row it happens to sit beneath would be a fabricated scope. This follows the §6 table-scope
  convention. It is the only annotation in the three files whose marker is not mirrored on its target.

No synthesised `pr` (source-note) markers were needed: no activity label in any of the three tables
carries an inline section/appendix reference. (Section references appear only *inside* footnote text —
e.g. Table 01 footnote 14 ends "…are required for confirmation (Section 6.1)." — where they are
transcribed verbatim as part of the footnote.)

---

## 4. Mechanical mark-check

* **Method:** §1b bbox column-binning, with the cell geometry recovered from the raster per §1d.
  `pdftotext -bbox` gave every token's box; the **column boundaries and row bands** were taken from
  200-dpi `pdftoppm -gray` renders by detecting vertical rules (image columns with high ink fraction
  over the table height) and horizontal rules (rows above ~85% ink within the label column's x-range).
  pdfplumber was not used, per §1. Vector rules were therefore never read directly; the raster method was
  used uniformly for all 7 pages rather than switching per page.
* **Merged spans** were resolved per band: for each row band, the presence/absence of ink at each column
  boundary was tested inside that band only. This is what identified the three bone-marrow merges, the
  Table 01 `"1"` header merge and the Table 02 header merges.
* **Diff result:** the bbox-derived mark matrix was rebuilt independently from the delivered JSON and
  compared cell-for-cell across all 7 PDF pages, matching marks with `^[Xx]$` and expanding the three
  merged bone-marrow rows to their raster-derived spans. **Zero disagreements.** Row counts per page also
  matched (63:7, 64:11, 65:6, 66:4, 72:14, 73:3, 78:15).
* **Glyph case:** every in-grid mark in the excerpt is an uppercase `X`. No lowercase `x`, `✓`, `•` or
  arrow glyphs occur; nothing was normalised.
* **Row-band caveat handled:** no redaction bars are present, so the §1d warning about taking row
  boundaries from a redacted column did not bite. Bands were read from the label column, which holds text
  on every row.

Recommended spot-checks for the reviewer (places where a reader could reasonably expect a mark and the
source has none): Table 01 row 20 "12-lead ECG" has **no** mark in column 8 (Subsequent Cycles) although
it has one in column 9; Table 01 rows 13 "ECOG performance status", 18 "Serum CA-125" and 20 "12-lead
ECG" have **no** mark in column 5 (Cycle 1 Day 15) while their neighbours do; Table 02 row 11 "Physical
examination" has no mark in column 4 (FE 8) while row 12 does; Table 03 row 12 "12-lead ECG" carries a
single mark, in column 5 only. All were verified against the rendered page and are transcribed as
printed — a missing mark is data.

---

## 5. Annotation text integrity

* **Not glyph-spread.** Single-character tokens are 426 of 2591 (16%), and those are superscript markers
  and short words, not letter-spacing. No §1c reconstruction was applied to activity labels or header
  labels.
* **Superscript token rejoining (the one text reconstruction done).** The text layer emits superscripts
  and registered marks as separate tokens. Where the inter-token x-gap is under ~1.4 pt the tokens are
  adjacent on the page and were rejoined without a space, so `annotation_text` reads:
  `BRACAnalysis®` (tokens `BRACAnalysis` at x 405.55–467.83 and `®` at 468.31–473.23, gap 0.48 pt),
  `gBRCAmut` (tokens `gBRCA` 486.86–519.12 and `mut` 519.19–529.27), and `WHO criteria20)` in Table 01
  footnote 28 / Table 02 footnote 15. Naive whitespace joining would have produced "BRACAnalysis ®" and
  "gBRCA mut". Affected annotations: Table 01 markers 4, 5, 6, 28; Table 02 marker 15.
* **A genuine containment pair, source-faithful — do not merge.** In Table 02, annotation `12` has text
  identical to annotation `1` ("Treatment cycles are 28 days long, visits on Day 1 of each cycle unless
  otherwise specified."), and `13` identical to `2` ("Visits continue every 4 weeks until study treatment
  discontinuation."). **Re-verified against the pages: this is source-faithful, not a split note cell.**
  Doc page 72 prints these two notes as footnotes 1 and 2; doc page 73 reprints both, verbatim, as
  footnotes 12 and 13, and the reprinted header on page 73 carries the markers "Cycle12" and "Cycles13"
  where page 72's header carries "Cycle1" and "Cycles2". Because the two header rows were de-duplicated
  into a single header, the de-duplicated header cells carry both marker sets: columns 5–7 have
  `annotation_markers: "1,12"` and column 8 has `"2,13"`. Both annotations are kept so that either
  printed marker resolves.
* **Cross-table duplicate (not a within-file containment):** Table 01 footnote 28 and Table 02 footnote
  15 have identical MDS/AML text; they are in different files and both are printed in the source.
* **Note bounding.** The only note read from a rule-bounded table cell is the Table 02 `n1` note; its
  extent was fixed from the raster horizontal rules (the cell runs from the rule below "Bone marrow
  aspirate and biopsy" to the table's bottom rule) and its text starts at "Note:" and ends at
  "…visit schedule for safety." All other annotations are numbered footnote blocks below the table, each
  delimited by its own printed marker, so no proximity bounding was needed anywhere. No
  `annotation_text_source.method` values were recorded.
* **Abbreviation blocks deliberately dropped.** Doc pages 63, 72 and 73 each print an abbreviation list
  ("Abbreviations: C = Cycle; CA = cancer antigen; CBC = complete blood cell count; …"). None of those
  terms is printed *as a marker* in a grid cell, a header cell or an activity label — they occur only
  inside running activity names — so per §6 zero `abbreviation` annotations were emitted rather than
  binding them by `text_match`. This is recorded in each file's `extraction_notes`.
* `by_type` is all-`footnote` in every file (28 / 16 / 15). That is the normal outcome per §8; no
  annotation in this excerpt is a bare "See Section x.y" pointer.

---

## 6. Low-confidence calls

1. **Mixed `property_type` on the top header row of every table.** In Table 01 the row is *printed*
   "Cycle" and holds cycle numbers (`1` spanning the Cycle 1 days, `2`) alongside phase labels
   ("Screening", "Subsequent Cycles", "Study Treatment Discontinuation (visit within 7 days of last
   dose)", "Post Treatment Assessments"). Typed **`cycle`** on the strength of the printed row label and
   footnote 1 ("Treatment cycles are 28 days long…"); `epoch` is defensible. Table 03's top row is also
   printed "Cycle" and typed `cycle` on the same grounds. Table 02's top row has **no** printed label and
   is mostly phase names, so it was typed **`epoch`** with `structure_method: "inferred_from_layout"`.
   The two tables therefore differ in typing on rows that look similar; each call follows its own printed
   evidence and both are recorded in `property_comment`.
2. **Table 03 `Location` row typed `modality`.** Its values are how the encounter happens (in clinic vs
   telephone contact vs local-clinic / in-home nursing visit), which matches the schema's `modality` definition of visit type, in-person versus phone. It is not a `condition` band because it applies to every activity
   row and is what tells columns 3 and 4 apart.
3. **Table 02 header row 2 given `hierarchical_level` 2.** It only exists under the "Cycle" span, but
   without it columns 5 and 7 are indistinguishable — both read "Day 1" — so it participates in telling
   columns apart and gets a level rather than `null`.
4. **Table 01 modelled as one table, not four.** Doc pages 64–66 print no table caption at all; they
   carry only the reprinted header grid. The source treats this as one Table 7, so it is one file with
   `page_start` 63 / `page_end` 66 rather than a `main_soa` plus three `continuation` tables. Same
   reasoning for Table 02, where the source itself labels doc page 73 "(Continued)" under the same
   table number.
5. **`table_number` renumbered 1/2/3.** The source numbers these Table 7, Table 8 and Table 9. The schema
   asks for a sequential number within the protocol starting at 1, and the deliverable filenames are
   zero-padded from 01 in document order, so `table_number` is 1/2/3 and the source's own numbering is
   preserved verbatim in `table_title`.
6. **Slight left offset on one row.** "Bone marrow aspirate and biopsy" on doc page 66 starts at
   x = 80.18 pt where every other activity label in the excerpt starts at x = 77.64 pt. A 2.5 pt offset
   inside a row whose data cells are merged edge-to-edge is a centring artefact, not a hierarchy signal;
   it is kept at `indentation_level` 0 like every other row.
7. **No markdown was available** for this study, so no PDF/markdown text disagreements could arise or be
   checked. All text comes from the PDF text layer.

---

## 7. Orphan risk and undefined markers

* **No orphans.** Every annotation in all three files has at least one `marker_locations` entry, and
  every marker recorded in a location also appears in that row's / cell's `annotation_markers` — with the
  single, deliberate table-scope exception `n1` in Table 02 described in §3 above. This was checked
  mechanically against the delivered JSON.
* **Markers referenced but not defined — Table 03, markers 4 through 15 (12 annotations).** Doc page 78
  prints footnote definitions **1, 2 and 3 only**; the page ends after footnote 3, and doc page 79 (which
  would carry the rest) is not in the excerpt. Markers 4–15 are nonetheless printed in the grid and on
  activity labels of Table 9 and are transcribed where they appear:
  `4` on the row-5 column-4 cell, `5` on row 7 column 5, `6` on the "CBC" label, `7,8` on "Serum CA-125",
  `9` on "12-lead ECG", `10` on "Tumor Assessment (RECIST)", `11` on "FOSI, EQ-5D-5L, neuropathy
  questionnaire", `12` on row 15 column 5, `13` on row 16 column 6, `14` on "Survival assessment", `15`
  on the merged row-18 cells.
  Per §6 no text was fabricated. Each of the twelve annotations states plainly, and individually by
  marker number, that its definition is not printed in the extracted source — e.g. "Footnote marker 6 is
  printed in this table but its definition is not printed in the extracted source: document page 78
  carries footnote definitions 1 to 3 only. No text has been fabricated or imported from another table."
  **Several of these assessments have a same-named footnote in Table 01** (CBC ↔ marker 12, Serum CA-125
  ↔ 13/14, 12-lead ECG ↔ 16, Tumor Assessment (RECIST) ↔ 22, FOSI/EQ-5D-5L ↔ 24, Anticancer therapies ↔
  26, Survival assessment ↔ 27, Bone marrow ↔ 28). §6 permits offering those as clearly-labelled
  *probable* cross-references; **that option was deliberately not taken**, because the Table 9 numbering
  is independent and the correspondence is an inference. If a reviewer wants those links they should be
  added downstream against doc page 79, not read into this extraction.
* **No `unresolved` marker locations** were needed in any of the three files.

---

## 8. Method provenance (non-default methods recorded in the data)

| Where | Field | Value | Why |
|---|---|---|---|
| Every activity, all 3 tables (60 rows) | `activity_name_source.indentation_method` | `assumed_flat` | All three tables are flat: every activity label starts at the same x, there are no grouping headers, and every row carries marks. No whitespace or font hierarchy evidence exists, so level 0 is a documented working assumption, not a read. |
| Table 02, schedule_properties rows 1 and 2 | `property_name_source.synthesized` = `true`, `structure_method` = `inferred_from_layout` | — | Label cells empty; names, types and levels come from the spanning geometry (§3 above). |
| Table 02, annotation `n1` | `marker_locations[0].method` = `synthesized` | — | Table-scope note with no modelled element (§3 above). |

Nothing else deviates from the defaults. In particular **no** `method` was set on any `schedule_grid` or
`activity_schedule` cell and **no** `annotation_text_source` was set: every cell value and every
annotation text came from the `pdftotext -bbox` text layer, which is the schema's default. What did *not*
come from the default is the **cell geometry** — row bands, column boundaries and merged spans were
recovered from 200-dpi raster rule-line detection (§1d) rather than from a vector-rule read, because
pdfplumber is forbidden by §1. The schema has no provenance field for geometry-only recovery, so it is
recorded here and in each file's `table_metadata.notes` instead of being forced into a `method` enum that
would misdescribe how the values themselves were read.
