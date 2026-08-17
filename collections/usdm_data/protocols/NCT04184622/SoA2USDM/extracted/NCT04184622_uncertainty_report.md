# NCT04184622 — SoA extraction uncertainty report

Source: `/root/ph3/blind/NCT04184622/NCT04184622_soa.pdf` (11 PDF pages; protocol I8F-MC-GPHK(b),
LY3298176 / tirzepatide). Page numbers below are **document** pages taken from `PAGEMAP.md`
(PDF 1–11 → doc 18–28); declared SoA range doc 18–24, doc 25–28 flagged as back matter.

Prompt v3.7.3, schema soa-table-extraction v1.0. Two extraction files written:

| file | table | type | pages | data cols | activities | annotations |
|---|---|---|---|---|---|---|
| `NCT04184622_Table_01_extraction.json` | 1 | `main_soa` | 18–21 | 24 (positions 2–25) | 54 | 28 |
| `NCT04184622_Table_02_extraction.json` | 2 | `track` ("Prediabetes") | 22–24 | 19 (positions 2–20) | 39 | 19 |

---

## 1. Structure discovery — how many tables

The excerpt contains exactly **two** SoA tables, both inside protocol section 1.3:

- §1.3.1 "Schedule of Activities covering visits to primary study endpoint", doc 18–21.
- §1.3.2 "Schedule of Activities for additional 2-year treatment period for participants with prediabetes at randomization", doc 22–24.

Each spans several pages with its full 5-row header block reprinted on every page. Those reprints
were **de-duplicated** (§1b): each table carries one set of 5 `schedule_property` rows, read from its
first page (doc 18 / doc 22). Each table is therefore modelled as one multi-page table, not as a
parent plus `continuation` children.

A third grid — "Additional Information Regarding Activities Described in Schedule of Activities"
(doc 25–28) — was **not** emitted as a table. It falls beyond the declared SoA range, and its rows key
to activities/visits that already appear in the two SoA tables, which is precisely the case the
taxonomy sends to annotations rather than to a `reference` table. See §5 below for the consequences.

## 2. Table type calls

**Table 1 = `main_soa`.** Rows are procedures performed on subjects; it is the anchor grid for the
study timeline (Visits 1–21 plus 99 / ED / 801, Weeks −2 to 72).

**Table 2 = `track`, `track_label` "Prediabetes".** Not obvious, so recorded in
`table_metadata.notes` as well. Discriminators applied in order: rows are activities (not
`reference`); columns are *not* the same as Table 1 (Visits 101–116, 199, ED, 802; Weeks 78–176), so
neither `continuation` nor `domain`; it is not finer timing for a subset of Table 1's activities, so
not `subsidiary`; it is a separate phase attended only by a subgroup — the title says
"…additional 2-year treatment period for participants with prediabetes at randomization" — hence
`track`. The label is taken from the source's own population wording ("prediabetes"), shortened to
one word per the §2 rule against restating the table number or the phase.

The population split is also asserted inside the source: note n5 says
"Participants with prediabetes continuing to Week 78 should not perform Visit 801." — Table 1's
801 column and Table 2's 802 column are mutually exclusive by construction.

## 3. Columns and label columns

`L = 1` label column in **both** tables (the activity-name column only; the header rows' own labels
"Visit", "Week of Treatment" sit in that same column). **First data column position = 2** in both.
No renumbering was applied.

Column counts were fixed from raster rule-line geometry, not from token positions: 26 vertical rules
on doc 18–21 → 25 columns → 1 label + 24 data; 21 vertical rules on doc 22–24 → 20 columns →
1 label + 19 data. Both match the header labels one-for-one.

Header cells with no printed value are emitted with `cell_value: ""` (the ED column has no
"Week of Treatment" and no "Allowable Deviation" value in either table — the emptiness is data).
Rows 1–3 (Visit / Week / Allowable Deviation) emit an entry for every data column; rows 4–5
(Fasting Visit / Telephone Visit) behave like mark rows and emit entries only where an X is printed.

## 4. Schedule properties

Five per table, `hierarchical_level` 1–5:

1. **Visit** → `visit`
2. **Week of Treatment** → `week`
3. **Allowable Deviation (days)** → `window`
4. **Fasting Visit** → `modality`
5. **Telephone Visit** → `modality`

*Low-confidence call:* rows 4 and 5 are X-mark rows, not label rows, and `modality` vs `condition`
was arguable for **Fasting Visit**. It was typed `modality` because the two rows partition the
columns exhaustively and disjointly — Table 1: fasting at 16 columns, telephone at 8, 16 + 8 = 24;
Table 2: 11 + 8 = 19 — i.e. the pair encodes *visit type*, not a conditional. Both levels are
non-null because the rows carry data that characterises columns; a reviewer who reads them as purely
presentational qualifier bands would set both to `null`.

No `property_name` was synthesised; every label cell is printed. No `structure_method` recorded —
`property_type` and `hierarchical_level` come from the printed header labels.

## 5. Annotations — the single `*` marker, and the out-of-range notes table

**This is the largest interpretation in the run; please review it first.**

Both tables use exactly one printed footnote marker: `*`. It is defined once, by the line printed
directly beneath Table 2 on doc page 24:

> "*Please see table below for corresponding additional information"

and the notes it points to are the 27 rows of the
"Additional Information Regarding Activities Described in Schedule of Activities" table on
doc pages **25–28**, which `PAGEMAP.md` flags as beyond the declared SoA range.

Decision taken: **do not** emit a third table for that grid (it is out of range and, by the taxonomy,
is annotation content rather than a table), but **do** transcribe its 27 note cells as annotations on
the two in-range tables. The alternative — dropping them — would have left `*` printed at 35 places in
Table 1 and 21 in Table 2 with nothing to resolve to, and would have forced the §6
"marker referenced but not defined" wording, which would be false: the definitions plainly are
printed in the excerpt.

Consequence to review: annotation *text* for the 27 notes comes from pages outside
`page_start..page_end`. `table_metadata.page_start/page_end` and every `source_page` remain strictly
inside the declared range.

### Synthesised markers

One `*` stands for 27 different notes. Left as `*`, resolve would link all 27 notes to every
`*`-bearing row. So each note received a **distinct synthesised marker `n1`…`n27`**, numbered in the
printed order of the Additional Information table, and used **consistently across both tables**
(Table 1 cites n1–n27; Table 2 cites the subset n1, n3, n4, n5, n6, n8, n9, n10, n11, n12, n13, n14,
n19, n21, n23, n24, n26, n27). Every citing row's `annotation_markers` carries both the printed `*`
and its synthesised `nK`, e.g. `"*,n8"` on Weight.

The printed `*` itself is emitted once per table as a `source_note` (its whole text is a bare
pointer, per the §8 typing rule) with a `marker_location` at every place `*` is printed — 35
locations in Table 1, 21 in Table 2. Those locations carry **no** `method`: the `*` really is printed
there.

### Marker locations that are not printed marks

Every `n1`…`n27` location carries `method: "text_match"`. The scope is real but is established by the
note's Activity-column key naming the row (e.g. the note keyed "Weight" ↔ the activity `Weight`),
not by the token `nK` being printed at that location. Notes keyed to more than one row got one
location per row, one annotation only (n1 → the Visit and Fasting Visit property rows; n22 → five
lab rows; n26 → four activity rows in Table 1, three in Table 2). **No orphans**: every annotation in
both files has ≥1 location, and every location's marker is present in that row's
`annotation_markers` (checked mechanically).

### Header-cell and grid-cell footnotes (kept per-column, not per-row)

`*` printed on individual header cells was bound to that column's `schedule_grid` cell, never to the
`schedule_property` row:

| table | header cell | column_position | note |
|---|---|---|---|
| 1 | `3*` | 4 | n2 (Visit 3) |
| 1 | `99*` | 23 | n3 (Visit 99 / 199) |
| 1 | `ED*` | 24 | n4 |
| 1 | `801*` | 25 | n5 |
| 2 | `199*` | 18 | n3 |
| 2 | `ED*` | 19 | n4 |
| 2 | `802*` | 20 | n5 |

Six body cells print `X*` rather than a plain X; the marker was cleaned out of `cell_value` and put
on the cell:

- Table 1 row 25 (`Dispense study drug`), column 22 (Visit 21) → n16, whose text is
  "This applies only to those participants going into the additional 2-year treatment period for participants with prediabetes at randomization."
- Table 1 rows 38, 39, 40, 41, 44 (Urinary albumin/creatinine, Cystatin-c, Calcitonin, Hematology,
  Thyroid-stimulating hormone), column 2 (Visit 1) → n22,
  "Screening visit assessment will be used to confirm eligibility. …"

Note that Table 1 row 44 `Thyroid-stimulating hormone` carries no `*` on its **label** — only on its
Visit 1 cell. That is how the source prints it; it was not repaired.

### Abbreviation blocks — deliberately dropped

Two abbreviation blocks are printed in the excerpt, one in range (doc 24, beginning
"Abbreviations: BP = blood pressure; Cr= creatinine; C-SSRS = Columbia-Suicide Severity Rating Scale; ED= early")
and one out of range (doc 28). **Zero `abbreviation` annotations were emitted.** None of the terms is
printed as a marker; they occur only inside activity labels and running text, and §6 forbids binding
an abbreviation by word overlap or by a synthesised location. The nearest call was `ED`, which is the
entire content of a header cell in both tables — but its meaning is already carried by note n4, and
admitting it would equally have admitted TZP, PK, BP, HR, IWRS and the rest. Flagging in case a
reviewer wants `ED` reinstated.

### Section prose not captured

The paragraph above Table 1 on doc 18 —
"The Schedule of Activities described below should be followed for all participants enrolled in Study GPHK."
… "refer to Section 10.10 Appendix 10 for additional guidance." — is section body text above the
table title, carries no marker, and was **not** emitted as an annotation. It is arguably a
table-scope note; noting it so the omission is a declared one.

## 6. Mechanical mark-check

Method (both tables identical, so one method for the whole document):

1. `pdftotext -bbox` for every token's box (no pdfplumber).
2. Pages rendered at 200 dpi with `pdftoppm -r 200 -gray`. **Vertical rules** = image columns with
   ink fraction > 0.30 over the page height. **Horizontal rules** = image rows with ink fraction
   > 0.85 *within the activity-label column's* x-range (a text-bearing column, per §1d) — those
   bands were then applied across the whole row.
3. Each text-layer token assigned to the (band × column) rectangle its centre falls in.
4. **Independent cross-check:** a second, purely pixel-based detector counted near-black pixels
   (< 128 grey) inside each inset cell rectangle and flagged > 60 as "marked". Its matrix was diffed
   cell-for-cell against the text-layer read.

**Diff result: 37 disagreements (23 in Table 1, 14 in Table 2), all of them on the full-width section
band rows** — Clinical Assessments, Participant Education and Assessment, Laboratory Tests, Mental
Health Questionnaires, Patient Reported Outcomes / PROs. In those rows the centred band title's
letters fall inside several notional column rectangles, so the pixel detector reports ink where the
text-layer read (correctly, from the merge geometry) reports no cell. **Zero disagreements on any
activity row or header row.** The mark matrices are otherwise identical.

Visual spot-checks against rendered crops confirmed the mechanical read on: the whole Table 1 header
block (doc 18), the dense rows `Adverse events and product complaints` / `Concomitant medications`
(24 of 24 marks each), the sparse rows `Immunogenicity (includes PK sample)` (8 marks) and `TZP PK`
(3 marks), the `X*` cluster on doc 20, `Dispense study drug` (X* at Visit 21), the
`C-SSRS (Baseline/Screening) Version)` label, and both Self-Harm rows on doc 24 (19 of 19 marks
each).

`method` provenance was **not** set on `schedule_grid` / `activity_schedule` cells: the mark values
are text-layer reads (the schema default). Only the *cell boundaries* came from the raster, and the
enum offers no value for that hybrid; `raster_pixel_detection` would misdescribe what was done.

## 7. Merged cells and distributed marks

- **No merged marks anywhere.** Internal vertical rules were tested band-by-band across both tables;
  every activity row and every header row has all internal rules present. Consequently **no**
  `source_range` and **no** `merged_cell_range` values are set in either file, and no mark or text
  was distributed across a span.
- The only merges in the document are the section band rows, each a single cell merged across all
  25 (Table 1) / 20 (Table 2) columns. They are emitted as activities at `indentation_level` 0 with
  **no** `activity_schedule` entries, per §4.
- No arrows, no vertically-merged marks, no qualified marks. Every one of the 414 (Table 1) and 311
  (Table 2) body cell values is the single glyph `X`; no `x`, `✓` or `•` occurs, so no case
  normalisation was needed.

## 8. Page coverage

Every page in each declared range contributed activity rows:

- Table 1: doc 18 → 15 rows, doc 19 → 11, doc 20 → 16, doc 21 → 12. Total 54.
- Table 2: doc 22 → 14 rows, doc 23 → 17, doc 24 → 8. Total 39.

No page in either range contributed zero rows. The tables are not horizontally tiled, so the §4 tiling
exception does not apply.

## 9. Hierarchy / indentation

The source uses **no leading whitespace**; every activity label is flush left. Hierarchy is expressed
only by the full-width, centred, merged section bands. So every activity in both files carries
`activity_name_source.indentation_method: "visual_estimate"` — level derived from the merge geometry
of the band rows, not from text-layer whitespace.

Levels assigned: band rows = 0; rows below a band = 1. Table 1 rows 6–8
(`Informed consent`, `Randomization`, `Register study visit in IWRS`) and Table 2 row 6
(`Register study visit in IWRS`) print **above** the first band and belong to no group, so they are
level 0 while still carrying marks. They are ordinary activities, not grouping headers — the §4 rule
that level-0 rows carry no marks applies to grouping headers, and those four rows group nothing.

## 10. Annotation text integrity

- The text layer is **not** glyph-spread; tokens are whole words. No `deglyph_reconstruction` was
  needed anywhere, and no field was reconstructed.
- All 27 note texts were bounded by rule-line geometry (a 2-column grid: Activity | Notes, with
  horizontal rules recovered from the raster), never by vertical-gap proximity. Each carries
  `annotation_text_source: {"method": "raster_band_cells"}`. **No note anywhere in this run is
  `proximity_bounded`.**
- **Containment pairs — re-verified against the page, and they are source-faithful, not a split
  note.** Three notes share an opening run:
  - n15 (Injection training with Autoinjector demonstration device):
    "All training should be repeated as needed to ensure participant compliance" (no full stop)
  - n11 (Hand out diary, instruct in use):
    "All training should be repeated as needed to ensure participant compliance."
  - n14 (Review diet and exercise goals):
    "All training should be repeated as needed to ensure participant compliance. Study personnel to provide reinforcement and encouragement for lifestyle modifications."

  n15 ⊂ n11 ⊂ n14. A rendered crop of doc page 27 shows `Lifestyle Program instructions`,
  `Review diet and exercise goals` and `Injection training with Autoinjector demonstration device`
  as three separate rule-bounded rows, each with its own Notes cell; n11 sits on doc page 26. The
  sponsor simply repeats the sentence. **Nothing was merged, truncated or dropped.**
- Each note text starts at its cell's first word and ends at its last; no letter-spacing artefacts.

## 11. Source defects transcribed, never repaired

| where | as printed | left as-is |
|---|---|---|
| Table 1 row 51 activity label | `C-SSRS (Baseline/Screening) Version)` | stray closing parenthesis after "Screening"; the Additional Information table spells it "C-SSRS (Baseline/Screening Version)" |
| note n25 (TZP PK) | "…as assignedby IWRS at randomization." | missing space in "assignedby" |
| Table 1 row 38 vs Table 2 row 27 | `Urinary albumin/creatinine` vs `Urinary albumin/creatinine ratio` | same assay, two spellings |
| Table 1 row 21 vs Table 2 row 16 | `Review study participant diary, including study-drug compliance` vs `Review study participant diary, including study drug compliance` | hyphen present in one table, absent in the other |
| Table 1 row 32 vs Table 2 row 23 | `2-hour oral glucose tolerance test (includes glucose, insulin, c-peptide at each timepoint)` vs `2 hour oral glucose tolerance test (includes glucose, insulin, c-peptide)` | hyphen and scope wording differ |
| Table 1 row 33 vs Table 2 row 24 | `Chemistry panel (include Cr for eGFR calculation and glucose)` vs `Chemistry panel (includes Cr for eGFR calculation and glucose)` | include / includes |
| Table 1 row 17 vs Table 2 row 12 | `Adverse events and product complaints` vs `Adverse events/Product Complaints` | |
| Table 1 rows 57–58 vs Table 2 rows 42–43 | `SF-36, version 2, acute form` / `IWQOL-Lite CT` vs `SF-36, version 2 acute` / `IWQOL-Lite -CT` | |
| Table 1 row 55 vs Table 2 row 40 | band `Patient Reported Outcomes` vs band `PROs` | |

Two line-broken compounds were rejoined in `activity_name` while the printed break is preserved in
`activity_name_source.cell_text`: `study- / drug` → `study-drug` (Table 1 row 21) and
`c- / peptide` → `c-peptide` (Table 1 row 32). Flagged because rejoining is a judgement; the raw
cell text lets a reviewer undo it.

No redactions, no illegible regions, no missing pages.

## 12. Method provenance — full list of non-default values recorded

- `activity_name_source.indentation_method: "visual_estimate"` — every activity in both files
  (54 + 39). Reason in §9.
- `annotation_text_source.method: "raster_band_cells"` — all 27 notes in Table 1 and all 18 in
  Table 2. Reason in §10. The `*` `source_note` in each file has no
  `annotation_text_source`: its one-line text was read directly from the text layer on doc 24.
- `marker_locations[].method: "text_match"` — every location of every `nK` annotation. Reason in §5.
  The `*` annotation's 35 (Table 1) / 21 (Table 2) locations carry no method: printed.
- **No `location_type: "unresolved"` anywhere.** Every marker's target was determinable.
- No `structure_method`, no `property_name_source.synthesized`, no
  `activity_name_source.method`, no cell-level `method`.

## 13. Recommended reviewer spot-checks

1. The §5 decision to source annotation text from doc 25–28 (out of declared range) — accept or drop.
2. The `n1`…`n27` synthesised marker scheme, and whether `*` should also remain as its own
   `source_note` (it currently resolves to 35 / 21 elements at once).
3. `Fasting Visit` typed `modality` rather than `condition` (§4).
4. Whether `ED` should be reinstated as an `abbreviation` annotation (§5).
5. Table 2's `track_label` "Prediabetes" — short by design; the source's own phrase is
   "participants with prediabetes at randomization".
