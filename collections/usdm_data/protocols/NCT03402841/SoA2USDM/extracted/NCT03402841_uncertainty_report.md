# NCT03402841 — SoA extraction uncertainty report

Source: `/root/ph3/blind/NCT03402841/NCT03402841_soa.pdf` (5 PDF pages = document pages 40–44 per PAGEMAP.md).
Protocol identifiers on every page header: `Study Code D0816C00020`, `Version 3.0`, `Date 26 October 2018`.
Printed page footers read `40 (111)` … `44 (111)`; these happen to agree with PAGEMAP.md, but PAGEMAP.md was used as the authority throughout. PAGEMAP.md marks no page as back matter, so all five pages are inside the declared SoA range.

The excerpt contains **two** SoA tables. The protocol itself says so:

> "The schedule of assessments for the screening visit is shown in Table 1."

---

## 1. Per-table summary

### Table 1 — `main_soa`
- Title: "Study Schedule – Screening (Visit 1)"
- Pages: document 40–41.
- Label columns **L = 1** (the activity-name column, which also carries the header row's own label "Day"). **First data column position = 2**; data columns are 2 and 3. Column count: 2 data columns (3 physical columns).
- Header rows: 1, labelled `Day`.
- Activities: 18. Marks: 19 (one row carries two).
- Activity rows per page: **page 40 → 14 rows; page 41 → 4 rows**. Every page in the declared range contributed rows.
- Classification reasoning (also in `table_metadata.notes`): the taxonomy states that where a protocol carries independent Screening and Treatment schedules with different column structures, *each* is `main_soa`. Table 1's two-column screening timeline shares no visit with Table 2, and both tables schedule the same participants, so it is neither a `domain` nor a `track` of Table 2. Not `continuation` — it is the first table, and its body simply runs over a page break under a reprinted header.
- Page 41 also carries footnotes a–g plus one unmarked Note; the header row reprints there and was de-duplicated (counted once).

### Table 2 — `main_soa`
- Title: "Study Plan Detailing the Procedures"
- Pages: document 42–44.
- Label columns **L = 1** (activity-name column, which also carries the labels "Visit Number or type", "Day", "Visit Window"). **First data column position = 2**; data columns are 2–8. Column count: 7 data columns (8 physical columns).
- Header rows: 3 (`Visit Number or type` → visit, level 1; `Day` → study_day, level 2; `Visit Window` → window, level 3).
- Activities: 15. Marks: 45.
- Activity rows per page: **page 42 → 11 rows; page 43 → 4 rows; page 44 → 0 rows**.
  **Page 44 contributes no activity rows and this is expected**: it is a footnote-only page carrying the continuation of the footnote block (markers g, h, j, k, l). It is inside the declared range because those footnotes belong to Table 2. No table body appears on it.
- The three header rows reprint on page 43 and were de-duplicated.

---

## 2. Merged cells and merged marks

**None in either table.** No mark or text was distributed across a span; no `source_range` and no `merged_cell_range` is set anywhere in either file.

This is not an eyeball judgement — the vertical rule lines were recovered from the 150 dpi page raster (ink columns whose ink fraction over the table height exceeds 0.6) and every internal vertical boundary is present over the full table height:

- Table 1 (pages 40 and 41, identical): rules at x = 85.0, 442.6, 497.8, 551.0 pt → 3 columns.
- Table 2 (pages 42 and 43, identical): rules at x = 72.0, 211.2, 236.6, 265.0, 389.8, 504.0, 589.4, 688.3, 759.4 pt → 8 columns.

Because no internal vertical rule is missing on any row band, no cell is merged, and every mark belongs to the single column its glyph sits under.

---

## 3. Mechanical mark-check (§1b)

Both tables have a genuine text layer *and* a vector/raster rule grid, so the §1b bbox route was used and cross-checked against the raster rules and a full-page visual read.

Method: `pdftotext -bbox` on all 5 pages; column x-centres fixed from the header rows (Table 2: the `Visit Window` row values `0 / ±3d / ±7d / ±3d / ±7d / +7d` plus the `Long-term follow up` header cell for column 8; Table 1: the `Before screening period` and `-28 to -1` header cells). Mark tokens matched with `^[Xx][*a-zA-Z0-9]?$` so that footnoted marks (`X b`, `X j` — printed as superscripts and tokenised separately here) were not dropped. Each mark token binned to the nearest column centre, then checked against the raster column boundaries.

Recovered column centres (pt):
- Table 1: col 2 ≈ 470.2, col 3 ≈ 524.4. All 19 marks land at x-centres 470.3 or 524.6 — dead on.
- Table 2: col 2 ≈ 223.9, col 3 ≈ 250.8, col 4 ≈ 327.4, col 5 ≈ 446.9, col 6 ≈ 546.7, col 7 ≈ 638.9, col 8 ≈ 723.9. All 45 marks land within ~3 pt of a centre.

**Disagreements between the mechanical matrix and the visual read: none.** Rows that are easy to mis-read and were explicitly re-checked against the rendered page:

- `Pregnancy test` (row 9): X in columns 2, 3, 4, 5 and 7 — **column 6 (Study treatment discontinued) is empty**. Confirmed empty on the page; not filled in.
- `FACT-O and EQ-5D-5L questionnaires` (row 14): X in columns 2, 3, 4, 6, 7 — **column 5 (Visit No. 5 and subsequent safety visits) is empty**. Confirmed empty.
- `ECOG performance status` (row 6): X in columns 2, 4, 6 only.
- `Olaparib dispensed/returned` (row 16): X in columns 2, 3, 4, 6 only; the column-4 mark sits at x 321.6–328.9 (slightly left of centre because the superscript `j` follows it) and still bins to column 4, which the raster boundaries 265.0–389.8 confirm.
- `Subsequent cancer therapy …` (row 17): X in columns 7 and 8; `Time to subsequent therapy and Survival` (row 18): X in column 8 only.

Glyph case: every mark in both tables is an upper-case `X`. Nothing normalised.

---

## 4. Synthesised values

- **Property names:** none synthesised. All four header rows across the two tables carry printed labels in column 1 (`Day`; `Visit Number or type`, `Day`, `Visit Window`), so no `property_name_source.synthesized` is set anywhere.
- **Annotation markers:** exactly one synthesised marker, `note1` in Table 1 — see §6 below.

---

## 5. Annotation text integrity

- The text layer is **not** glyph-spread. Tokens are whole words; no `deglyph_reconstruction` was needed and no `annotation_text_source` is recorded on any annotation. Superscript footnote markers do tokenise separately from the word they follow (e.g. `transfusions` then `a`), which is why the mark regex above allows a trailing character.
- Footnote text was read from the footnote blocks below each table (Table 1: page 41; Table 2: pages 43 and 44). These are ordinary paragraph footnotes, not a right-hand notes column, so each marker's text is bounded by the next marker's superscript — no proximity bounding was needed and no `proximity_bounded` is recorded.
- **Containment pairs within a single table: none.**
- **Containment pair across tables (reported for completeness, source-faithful):** Table 1 footnote `b` is a proper substring of Table 2 footnote `d`.
  - Table 1 `b`: "Coagulation test should be performed if clinically indicated. For a list of all required laboratory tests please refer to Section 5.2.1."
  - Table 2 `d`: "All samples should be taken prior to first dose. Coagulation test should be performed if clinically indicated. For a list of all required laboratory tests please refer to Section 5.2.1."
  Re-verified against the pages: these are **two separate footnotes of two separate tables, printed on two different pages** (page 41 and page 43), and Table 2's version genuinely opens with an extra sentence that Table 1's lacks. This is the source-faithful case, **not** one note cell split across rows. Neither was merged, truncated or dropped.
- Source typos transcribed verbatim, not repaired:
  - "restrospective" in the Table 2 activity label "Blood sample for restrospective gBRCA test".
  - the missing space in "collection,shipping" in Table 2 footnote g.
  - Table 1 footnote g's "before Day1." (no space).
- Footnote lettering in Table 2 runs a, b, c, d, e, f, g, h, **j**, k, l — the source skips `i`. Transcribed as printed; no marker was renumbered.

---

## 6. Annotation binding, orphan risk, method provenance

Both files: every annotation has ≥ 1 `marker_locations` entry, and every marker recorded in a location also appears in that row's / cell's `annotation_markers` (checked mechanically). No `abbreviation` or `legend` annotations were emitted — the tables define no in-grid legend and print no abbreviation block, so there is nothing that would produce an orphan list entry. `by_type` is all `footnote` in both files (8 and 11), which §8 explicitly allows.

**Header-cell footnote (§6).** Table 2 footnote `a` is printed twice inside the header area, not on the property label. It is bound to the two specific grid cells it sits on, and NOT to the `schedule_property` row:
- `schedule_grid` row 1, column 5 — the cell "Visit No. 5 and subsequent safety visits (For the first 12 months only)"; the superscript `a` is printed immediately after the word `visits` in the phrase "subsequent safety visits".
- `schedule_grid` row 2, column 4 — the `Day` cell, where the superscript `a` is printed immediately after the word `thereafter` at the end of "every 12 weeks thereafter".
The marker was cleaned out of both `cell_value`s and recorded in each cell's `annotation_markers`.

**Cell-level footnotes.** Table 2 `b` sits on four marks (`Xᵇ` at rows 4, 5, 7, 8, all column 2) and `j` on two (`Xʲ` at row 16, columns 3 and 4); these are recorded as `schedule_cell` locations with the marker stripped from `cell_value`, not as activity-level markers.

**The one synthesised marker — `note1` (Table 1).** Page 41 prints, between footnote `e` and footnote `f`, an unmarked line:

> "Note: MRI/ CT scan more than 28 days prior to Day 1 may be acceptable, please consult with AstraZeneca."

It carries no printed marker. It was emitted as its own `footnote` with the synthesised marker `note1`, bound by one `activity_name` location on row 16 (`Tumour assessment`) with `method: "synthesized"`, and `note1` added to that activity's `annotation_markers` (which therefore reads `e,note1`). Rationale: the note is printed inside footnote `e`'s block and continues `e`'s subject (pre-treatment RECIST / imaging timing relative to Day 1), and `Tumour assessment` is the only row citing `e`. **This is the weakest binding in the run** — it is an interpretation of placement plus subject matter, not a printed marker, and is the one item to spot-check against page 41. It was not left as an unresolved location because the note's target is strongly indicated by both its position in the footnote block and its content; the `synthesized` method flags it for verification.

**Method provenance recorded (§1e), exhaustively:**
- `activity_name_source.indentation_method: "assumed_flat"` on all 18 Table 1 activities and all 15 Table 2 activities. Both tables are flat — every row is a level-0 activity that itself carries marks, there are no grouping/section-header rows and no visual indentation in either body, so §4's flat-table exception applies and no row was left artificially mark-free.
- `marker_locations[].method: "synthesized"` on the single `note1` location described above.
- Nothing else. No `annotation_text_source` anywhere, no `activity_name_source.method`, no `structure_method`, no cell-level `method` (all marks and header values came from the bbox text layer, with the raster used only to confirm column boundaries).
- **`location_type` = `unresolved`: zero.** No marker in either table had an undeterminable target.

---

## 7. Low-confidence calls / judgement calls

1. **Both tables typed `main_soa` (moderate confidence, documented in `table_metadata.notes` of each file).** The alternative readings were considered and rejected: `continuation` fails because the column structures differ completely (2 screening columns vs 7 visit columns); `domain` fails because `domain` requires the *same* column structure; `track` fails because both tables schedule the same single population — every enrolled patient screens under Table 1 and then follows Table 2 — so there is no mutually exclusive population split and no `track_label` would be meaningful. The taxonomy's own wording covers this shape directly: independent Screening and Treatment schedules are each `main_soa`.
2. **Table 1 `property_type: study_day` (moderate confidence).** The printed row label is `Day`. One value is an explicit study-day range, "-28 to -1"; the other value, "Before screening period", is a phase-style qualifier rather than a day. A downstream reader may prefer `epoch` or `other` for that column. Typed on the printed label, with the mixed content spelled out in `property_comment`.
3. **Table 2 `Day` row (row 2) typed `study_day` (good confidence, prose values).** Columns 2 and 3 carry plain day numbers (1, 29), but columns 4 and 5 carry whole timing rules in prose ("On the first day of next visit period (V4 = Day 57 and thereafter every 8 weeks for the first 12 months; every 12 weeks thereafter)"). Transcribed literally into `schedule_grid`, with the parenthesised sub-lines joined into one cell value in printed reading order. Columns 6, 7 and 8 carry no Day value and no grid entry was emitted for them (empty stays empty).
4. **Empty cells emitted as absent, not as empty strings.** `schedule_grid` and `activity_schedule` contain entries only for non-empty cells; the "Long-term follow up" column has no `Day` and no `Visit Window` value, and those two cells simply do not appear.
5. **Table 1 "Urinalysis" carries no footnote marker; Table 2 "Urinalysis" carries `c`.** Transcribed exactly as printed in each table — no marker was carried across from one table to the other.
6. **Table 1 row 12 "Haematology / clinical chemistry" carries two markers, `b,f`,** printed as a stacked superscript above the line. Both are recorded, and both `b` and `f` have an `activity_name` location on row 12.
7. **Activity name cleaning.** Trailing footnote letters were stripped from `activity_name` and preserved in `activity_name_source.cell_text` (e.g. cell text "Time to subsequent therapy and Survival l" → name "Time to subsequent therapy and Survival"). Multi-line label cells were joined with a single space in printed reading order (e.g. "Subsequent cancer therapy following discontinuation of study treatment", "Blood sample for restrospective gBRCA test"). Italicisation of *BRCA* in the source is not represented; no `text_formatting` was captured.
8. **No inline section/appendix references appear in any activity label**, so no `source_note` annotations were created. Section references occur only inside footnote text (e.g. Table 1 footnote `b` ends "please refer to Section 5.2.1."), where they accompany an explanation and therefore stay part of the `footnote` per §6/§8. No annotation in either file is a bare pointer.

---

## 8. Recommended spot-checks

1. Page 41, the unmarked "Note: MRI/ CT scan…" line — confirm the `note1` → `Tumour assessment` binding.
2. Page 42, `Pregnancy test` column 6 and `FACT-O and EQ-5D-5L questionnaires` column 5 — confirm both are genuinely blank.
3. Page 43, `Olaparib dispensed/returned` — confirm marks in columns 2, 3, 4, 6 and blanks in 5, 7, 8.
