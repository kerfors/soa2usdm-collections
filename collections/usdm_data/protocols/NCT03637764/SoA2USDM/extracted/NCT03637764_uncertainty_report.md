# NCT03637764 — SoA extraction uncertainty report

Source: `/root/ph3/blind/NCT03637764/NCT03637764_soa.pdf` (7 PDF pages = document pages 18–24 per
PAGEMAP.md). Prompt v3.7.3, schema soa-table-extraction v1.0.
Output: `/root/ph3/staging/NCT03637764/NCT03637764_Table_01_extraction.json`.

All page numbers below are DOCUMENT pages from PAGEMAP.md. The printed footers of this excerpt run
one page LOWER than the document page (PDF page 1 prints "Page 18" in the footer and is document
page 18 — here footer and document page happen to agree, but PAGEMAP was used throughout and no
footer was read as a page number).

---

## 1. How many tables the excerpt contains, and the scope decision for pages 22–24

**One table extracted.** Document pages 18–21 carry Section 1.3, titled
"SCHEDULE OF ACTIVITIES (SOA)" — a single activity × timepoint grid whose three-row header block is
reprinted verbatim at the top of each of the four pages while the body rows simply continue.

Document pages 22–24 lie beyond the declared SoA range (18–21) and are flagged as such in
PAGEMAP.md. They carry two further grids:

* pages 22–23 — "PHARMACOKINETICS AND IMMUNOGENICITY FLOW CHART" (grid on p22, its footnotes a–j and
  abbreviation block on p23);
* page 24 — "EXPLORATORY BIOMARKER FLOW CHART".

**Decision: out of scope — not extracted as tables, and not folded into the main table as
annotations.** Reasoning, stated so a reviewer can overturn it cheaply:

* PAGEMAP.md declares the SoA range as 18–21 and marks 22–24 as back matter carried for context.
  That declaration is authoritative for this run.
* Nothing on pages 22–24 is a note *about* the main SoA. The main SoA's own marker set is a, b, c
  and **all three definitions are printed inside pages 18–21** ("A cycle is 21 days",
  "Women of child bearing potential must have a negative serum pregnancy test result within 7 days
  prior to first IMP administration.", "evaluation not applicable for Cohort E"). The lettered
  footnotes on pages 22–23 (a–j) and the note on page 24 belong to those charts' own grids and would
  be orphans if attached to Table 01.
* The main SoA's *own* cells already carry the cross-reference to them:
  "See Pharmacokinetics and immunogenicity Flow Chart" (rows PK and ADA) and
  "See Biomarker Flow Chart" (row Tumor Biopsy, Archival Tumor Tissue Collection, Biomarker Blood
  Draw). Those are transcribed as distributed merged cell values in `activity_schedule`, so the
  pointer survives in the data without importing the charts.

**If a reviewer wants them in**, they are two `subsidiary` tables, not tracks: each gives finer
timing (per-sample, per-cycle) for activities that already exist as single rows in Table 01 (PK, ADA,
and the biomarker/biopsy row). They would be Table 02 (pages 22–23, sample-ID × cycle/day grid with
"Sample RNT (h)" and "Sample time window" rows) and Table 03 (page 24, biomarker sample × phase grid
with its own right-hand Notes column). No part of Table 01 depends on that choice.

**One table, not four.** The four pages could equally have been emitted as a parent plus three
`continuation` tables. I emitted one table with `page_start` 18 / `page_end` 21 because the footnote
markers and their definitions sit on *different* pages (marker `a` on every page's header, defined on
every page's Notes column; marker `c` used on pages 18, 19 and 20 but defined only on page 20), and
splitting would have left markers on pages whose definitions live elsewhere. Recorded in
`table_metadata.notes`.

---

## 2. Table 01 — shape and page coverage

| item | value |
|---|---|
| `table_type` | `main_soa` — primary anchor grid of the protocol (Section 1.3); every other schedule content in the excerpt refers back to it |
| pages | 18–21 |
| label columns (L) | **1** (the "Evaluation" column) → **first data column = position 2** |
| schedule columns | **10** (positions 2–11) |
| Notes column | source column **12** — a right-hand notes column, excluded from `schedule_grid` / `activity_schedule` per §6; its contents are emitted as annotations |
| header rows | 3 (`schedule_properties` rows 1–3) |
| activities | **29** (`row_position` 4–32; header rows occupy 1–3) |
| `activity_schedule` cells | 159 |
| annotations | 11 (5 `footnote`, 6 `source_note`) |

**Activity rows per page across the declared range** — every page contributes:

| document page | activity rows | row_positions |
|---|---|---|
| 18 | 6 | 4–9 |
| 19 | 7 | 10–16 |
| 20 | 9 | 17–25 |
| 21 | 7 | 26–32 |

No page in 18–21 contributed zero rows. The table is not horizontally tiled; the header block
reprints on each page and was de-duplicated (counted once, as rows 1–3).

Two of the 29 rows are organizational, full-width bold banner rows carrying no marks:
"Laboratory Assessments" (row 11) and "Disease Assessment" (row 20).

---

## 3. Merged marks and merged text — every distributed cell and its span

Spans were taken from per-row rule-line geometry, never from where the glyph sits. `source_range`
values are numeric column positions in the table's own numbering.

**Screening columns 2:3.** The two screening columns ("D-28 to D-15", "D-14 to D-1") are merged in
some body rows and separate in others — this varies row by row and is the single most error-prone
feature of this table. Rows where the internal rule at x≈182.3 pt is **absent** (merged, mark
distributed to both columns 2 and 3, `source_range` `"2:3"`):

* row 4 Informed consent/ Inclusion and exclusion criteria — `X`
* row 5 Demography, Medical/Surgical and Disease History — `X`
* row 7 Height (at baseline only) /Weight/ ECOG (HCC,SCCHN,EOC) or Karnofsky PS (GBM) — `X`
* row 8 Vital Signs — `X`
* row 9 Resting O2 saturation for SCCHN — `X`
* row 10 12-Lead ECG — `X`
* row 18 Serology HBV and HCV (for HCC only) — `X`
* row 21 CT/MRI (for HCC, SCCHN, and EOC) — `X`
* row 26 AE/SAE Assessment — `X`
* row 27 Prior/Concomitant Medication — "X (within 14 days prior to first dose)"

Rows where the rule is **present** and the mark therefore sits in column 3 only (column 2 is a
separate, empty and usually grey-shaded cell): row 6 Physical examination
("X (<7days prior to first dose)"), row 12 Pregnancy test ("X (within 7 days prior to first dose)"),
rows 13 Blood Chemistry, 14 Hematology, 15 Coagulation(GBM), 16 Coagulation (for HCC, SCCHN, and
EOC), 17 Blood Typing Interference Test, 19 Urinalysis, 22 Brain MRI ("X (within 14 days prior to
first dose)"). Row 23 AFP (for HCC) / CA125 (for EOC) has both screening cells separate and empty.

**Other distributed cells:**

| row | activity | value | span |
|---|---|---|---|
| 10 | 12-Lead ECG | "As clinically indicated" | 7:8 |
| 16 | Coagulation (for HCC, SCCHN, and EOC) | "As clinically indicated" | 7:8 |
| 19 | Urinalysis (at baseline and if required) /urine dipstick | "As clinically indicated" | 5:7 |
| 21 | CT/MRI (for HCC, SCCHN, and EOC) | "X (until PD is confirmed if no PD documented & confirmed)" | 10:11 |
| 22 | Brain MRI (for GBM only) | same | 10:11 |
| 23 | AFP (for HCC) / CA125 (for EOC) | same | 10:11 |
| 26 | AE/SAE Assessment | "Continuously throughout period" | 4:8 |
| 26 | AE/SAE Assessment | "X (ongoing related AEs, ongoing SAEs at EOT and new related AE/SAEs)" | 9:11 |
| 27 | Prior/Concomitant Medication | "Continuously throughout period" | 4:8 |
| 27 | Prior/Concomitant Medication | "X (related to AE/SAEs listed above)" | 9:11 |
| 28 | PK | "See Pharmacokinetics and immunogenicity Flow Chart" | 2:11 |
| 29 | ADA | "See Pharmacokinetics and immunogenicity Flow Chart" | 2:11 |
| 30 | Tumor Biopsy, Archival Tumor Tissue Collection, Biomarker Blood Draw | "See Biomarker Flow Chart" | 2:11 |

**Merged header cells** (`merged_cell_range` on each covered position, row 1 = phase band, row 2 =
cycle/period band): "Screening (up to 28 days before Day 1)" 2:3 · "Treatment Phase" 4:7 ·
"Post Treatment Follow-up Phase" 9:11 · "Cycle 1" 4:6 · "Safety follow-up Period" 9:10.
"End of Treatment (EOT)" (col 8), "Cycle 2 and Beyond" (col 7) and "Survival follow-up" (col 11) are
single-column cells.

**Two spans a reviewer should look at:**

1. **Rows 28–30 (PK, ADA, Tumor Biopsy…) physically span source columns 2 through 12** — the printed
   cell swallows the Notes column as well. Since column 12 is not a schedule column, I set
   `source_range` to `"2:11"` and emitted the ten schedule cells. The alternative (a range ending at 12) would
   point at a column that does not exist in this table's grid. Flagged rather than silently chosen.
2. **Rows 10 and 16 merge column 7 (D1 ± 2, "Cycle 2 and Beyond") with column 8 (EOT)** for
   "As clinically indicated". That merge is unusual semantically but the rule line between them is
   genuinely absent in those two rows and present in every neighbouring row; confirmed against the
   rendered page.

**Vertical merge in the header.** The row-1 "Screening (up to 28 days before Day 1)" cell extends
down over the row-2 band (no rule at y≈194.8 pt in columns 2–3), and column 1's "Evaluation" cell
spans all three header rows. The schema has no vertical merge for header cells, so each value is
recorded once, on the row where it is printed; row 2 columns 2–3 and row 2 column 8 are therefore
absent from `schedule_grid` rather than carrying a repeated value. Noted in `table_metadata.notes`.

---

## 4. Synthesised names and markers

**Synthesised `property_name` (3).** The header block's label column carries only "Evaluation",
merged vertically across all three header rows, so no header row has its own printed label:

* row 1 → "Study Phase" (`epoch`)
* row 2 → "Cycle / Follow-up Period" (`period`)
* row 3 → "Study Day" (`study_day`)

Each carries `property_name_source.synthesized: true` with `cell_value: ""`.

**Synthesised annotation markers (8), `n1`–`n8` in printed order.** The right-hand Notes column
prints its notes with no marker of any kind, so each distinct note got a synthesised marker, added to
the citing activity's `annotation_markers` and linked by a `marker_locations` entry with
`method: "synthesized"`:

| marker | text | type | bound to (row_position) |
|---|---|---|---|
| n1 | "Informed Consent: Informed consent may be signed prior to D -28." | footnote | 4 |
| n2 | "Section 8.2.1" | source_note | 6, 7 |
| n3 | "See Section 8.2.2" | source_note | 8, 9 |
| n4 | "See Section 8.2.3" | source_note | 10 |
| n5 | "See Section 10.3 Table 12" | source_note | 13 |
| n6 | "Section 10.3" | source_note | 14, 15, 16, 18, 19 |
| n7 | "Section 10.3 Before each transfusion." | footnote | 17 |
| n8 | "Section 8.1" | source_note | 21, 22, 23 |

Deduplicated by text: "Section 10.3" is one annotation with five locations, not five annotations.

The three **printed** markers keep their own letters and are bound where the marker is actually
printed: `a` on the four `schedule_grid` cells of the merged "Treatment Phase" header cell (row 1,
columns 4–7); `b` on the activity label "Pregnancy test (WOCBP only)b" (row 12); `c` on 16
`schedule_cell` positions — columns 5 and 6 (D8, D15) of rows 6, 7, 8, 9, 13, 14, 15 and 24.

---

## 5. Mechanical mark-check

Method: text-layer bbox read (§1b) with cell geometry recovered from the raster (§1d), applied
uniformly to all four pages.

1. `pdftotext -bbox` for every token's box; `pdftoppm -r 200 -gray` renders for the rules.
2. Column boundaries fixed once from the header day-row rules: 13 vertical rules at x ≈ 73.8, 152.5,
   182.3, 218.7, 252.9, 282.4, 311.9, 470.7, 535.9, 603.5, 661.1, 731.7, 790.6 pt → 12 source
   columns (1 label + 10 schedule + 1 Notes). Identical on all four pages.
3. Row bands from the horizontal rules read in the **label column** (a text column, never a shaded
   one), then applied across the row.
4. Per-row merge resolution: for each row band, the vertical rules actually present were re-detected
   inside that band; a missing internal boundary = merged cell, and the value was distributed across
   every covered column.
5. Each cell's text was then taken from the tokens whose centres fall inside the cell rectangle,
   ordered by line then x.

**Tolerance defect found and fixed during the run.** The vertical rules are not pixel-identical
between rows: the same boundary is drawn at 218.7 pt in some rows and 220.1 pt in others (likewise
252.9 / 252.2). A ±0.7 pt match window reported those rows as *merged* — e.g. it claimed columns 3
and 4 were merged on the Blood Chemistry row, which would have collapsed two separate `X` marks into
one span. Widening the window to ±3 pt removed the artefact; the corrected geometry was then checked
against the rendered page. Reviewers repeating this check should use the wider tolerance.

**Diff against the visual read: no disagreements.** After the tolerance fix, the mechanical matrix
was re-derived from scratch by a second independent pass and compared cell-for-cell with the
delivered JSON: 159 cells on both sides, zero differences in value, span or cell-level marker.
Rendered crops were read by eye for: page 18 screening block (rows 4–9), page 18 rows 7–9 full width,
page 19 screening block (rows 10–16), page 19 right-hand columns of row 12, page 20 label column and
screening block (rows 17–25), page 20 columns 10–11 of rows 21–23, page 21 full width (rows 26–30).
All agreed with the mechanical read, including the row-by-row 2:3 merge pattern in section 3 above.

**Grey shading.** Many cells are grey-filled to mean not-applicable-at-this-timepoint (e.g. column
4/D1 on the Physical examination row, columns 9–11 on the Resting O2 saturation for SCCHN row). Per
§1 this is formatting, not content: those cells stay empty and no mark was invented for them.
Conversely no mark was dropped because a cell was white.

**A missing mark that is data, not an omission.** Row 6 Physical examination has marks in columns 3,
5, 6, 7, 8, 9, 10 but **none in column 4 (D1 ±1)** — that cell is grey-shaded in the source, while
the neighbouring rows 7, 8 and 9 all carry an `X` there. Transcribed as printed.

---

## 6. Annotation text integrity

* The text layer is **not** glyph-spread; words come back whole, so no §1c reconstruction was needed
  for activity names, header labels or annotation text.
* One word-wrap rejoin was applied: footnote `b` prints "administra" / "tion." on two lines with no
  hyphen; the delivered text reads "Women of child bearing potential must have a negative serum pregnancy test result within 7 days prior to first IMP administration." This is the source's own
  word, re-joined, not an edit.
* **Containment pairs — checked against the page, both source-faithful:**
  * "Section 10.3" (n6) is contained in "Section 10.3 Before each transfusion." (n7) and in
    "See Section 10.3 Table 12" (n5). These are **three distinct printed cells**, not one note split
    across rows: n6 is the whole content of the Notes cell on the Hematology, Coagulation(GBM),
    Coagulation (for HCC, SCCHN, and EOC), Serology and Urinalysis rows; n7 is the whole content of
    the Blood Typing Interference Test Notes cell, which prints "Section 10.3" on its first line and
    "Before each transfusion." underneath, inside one rule-bounded cell; n5 is the whole content of
    the Blood Chemistry Notes cell. Each was bounded by its own horizontal rules, not by proximity.
    Do not merge them.
  * "See Section 8.2.2" (n3) and "See Section 8.2.3" (n4) are not a containment pair, only similar.
* Every note's extent was fixed from the Notes column's rule-line geometry; **no note used
  `proximity_bounded`**, so no `annotation_text_source` block appears anywhere in the file.
* Source oddity, transcribed verbatim and NOT repaired: rows 21, 22 and 23 carry
  "X (until PD is confirmed if no PD documented & confirmed)" in the merged columns 10–11 cell. The
  wording is internally odd (it reads as though "documented" and "confirmed" have been transposed),
  but that is exactly what is printed on page 20 in all three rows; verified on the rendered page.

---

## 7. Low-confidence calls

1. **`property_type` of header row 2 → `period`.** The row mixes treatment cycles ("Cycle 1",
   "Cycle 2 and Beyond") with named follow-up periods ("Safety follow-up Period",
   "Survival follow-up") in one band. `cycle` would fit the left half only, so the row is typed
   `period` (sub-phases) and the mix is spelled out in `property_comment`. Reasonable alternative:
   `cycle`.
2. **Header row 3 → `study_day`, windows not separated.** Each cell carries its own window inline —
   "D1 (±1)", "D8 (± 1)", "30 (±7) days after last IMPs admin" — and the source has no separate
   window row, so no `window` property was synthesised.
3. **Indentation (`indentation_method: "font_signal"` on all 29 activities).** The label column has
   no whitespace indentation at all; the only hierarchy signals are the two full-width **bold**
   banner rows. Assignment: banner rows "Laboratory Assessments" (11) and "Disease Assessment" (20)
   → level 0; the rows beneath each banner → level 1 (12–19 and 21–23); everything else → level 0.
   The judgement call is where the "Disease Assessment" section **ends**: rows 24
   "Isatuximab Administration" and 25 "Atezolizumab Administration" are printed **bold** like the
   banners (unlike the plain-text rows 21–23) and are IMP administration rather than disease
   assessment, so they and everything after them are treated as level 0. There is no printed
   end-of-section rule; a reviewer could equally read rows 24–32 as flat siblings of everything else,
   which is what they are in the delivered file.
4. **`source_note` vs `footnote` in the Notes column.** Bare pointers ("Section 8.2.1",
   "See Section 8.2.2", "See Section 8.2.3", "See Section 10.3 Table 12", "Section 10.3",
   "Section 8.1") are typed `source_note` per §8. Two notes point *and* instruct and are typed
   `footnote`: "Section 10.3 Before each transfusion." (n7) and
   "Informed Consent: Informed consent may be signed prior to D -28." (n1). n7 is the borderline one.
5. **In-grid cross-references left as cell values.** "See Pharmacokinetics and immunogenicity Flow
   Chart" and "See Biomarker Flow Chart" are scheduling content occupying the grid across a merged
   span (§5), so they are `activity_schedule` values, not `source_note` annotations. They are not
   double-encoded as annotations; if the resolved layer wants them as pointers, they are recoverable
   from the cell text.
6. **Marker `a` distributed across the merged header cell.** The glyph is printed once, after
   "Treatment Phase", but the cell spans columns 4–7, so the marker was placed on all four covered
   `schedule_grid` cells rather than on the column the glyph happens to sit over.
7. **No markdown was available** for this protocol, so there is no PDF/markdown text comparison to
   report; all text comes from the PDF text layer.
8. Minor transcription kept as printed: "Coagulation(GBM)" (no space), "D8 (± 1)" versus "D15 (±1)"
   (inconsistent spacing inside the window), "X (<7days prior to first dose)" (no space after "<").

---

## 8. Orphan risk and method provenance

* **Orphans: none.** All 11 annotations have ≥1 `marker_locations` entry (37 locations in total), and
  every marker in every location also appears in that row's / cell's `annotation_markers` — verified
  mechanically against the delivered JSON.
* **No `unresolved` locations.** Nothing in this table required a guessed target.
* **No `abbreviation` annotations.** Pages 18–21 carry no abbreviation block; the one in the excerpt
  (page 23) belongs to the PK flow chart, which is out of scope. Nothing was bound by `text_match`.
* **Non-default methods recorded in the data:**
  * `activity_name_source.indentation_method: "font_signal"` — all 29 activities (hierarchy from
    bold/full-width banner rows, not from whitespace).
  * `marker_locations[].method: "synthesized"` — the 12 locations of the eight Notes-column notes
    n1–n8 (the source prints no marker for them).
  * No `annotation_text_source.method` anywhere (all note text read from rule-line-bounded
    text-layer cells).
  * No `method` on `schedule_grid` / `activity_schedule` cells: all cell *values* came from the
    text-layer bbox read (the default). Only the cell *boundaries and merge spans* came from
    raster-recovered rule lines, which is the §1b/§1d procedure for a text-layer table rather than a
    pixel detection of content, so no `raster_pixel_detection` flag was set. Stated here so the two
    sources of truth are on the record.
  * `property_name_source.synthesized: true` — the three header rows (section 4 above). No
    `structure_method` was set: `property_type` comes from the printed header values and
    `hierarchical_level` from the printed stacking order.

## 9. Recommended spot-checks for the reviewer

1. The row-by-row 2:3 screening merge pattern in section 3 — it is the only place in this table where
   an identical-looking `X` means one column in some rows and two in others.
2. Rows 10 and 16, span 7:8 for "As clinically indicated" (merge across the Cycle-2 and EOT columns).
3. Rows 28–30, `source_range` `"2:11"` where the printed cell actually runs to source column 12.
4. The scope call on document pages 22–24 (section 1), if the corpus wants the PK and biomarker flow
   charts extracted as subsidiary tables.
