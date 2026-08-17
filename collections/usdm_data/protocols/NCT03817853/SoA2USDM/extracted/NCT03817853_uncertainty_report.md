# NCT03817853 — SoA extraction uncertainty report (prompt v3.7.3)

Source: `/root/ph3/blind/NCT03817853/NCT03817853_soa.pdf` (3 PDF pages = document pages 100–102 per PAGEMAP.md).
Protocol identified in the page footer block as `100/Protocol MO40597, Version 5`.

## 1. Structure discovery — how many tables

The excerpt contains **one** SoA table. Document page 100 carries the entire grid under the title
`Appendix 1` / `Schedule of Activities`; document pages 101 and 102 carry only the general note
paragraph and footnotes a–z, aa, under the running header `Appendix 1: Schedule of Activities`.
No second grid, no abbreviation *table*, no subsidiary timing table.

## 2. Per-table summary

| item | value |
|---|---|
| table_number / type | 1 / `main_soa` |
| page_start – page_end | 100 – 102 |
| leading label columns (L) | **1** — first data column = **2**, last = **12** (11 data columns; 12 columns total) |
| schedule_property rows | 4 (epoch, period, cycle, study_day) |
| schedule_grid cells | 31 |
| activities | 27 (26 physical body rows — see §4) |
| marks (`activity_schedule`) | 89 |
| annotations | 28, all `footnote` |

**Classification.** `main_soa` by the §2 discriminators: the rows are procedures performed on
subjects (so not `reference`), there is no second table to be a continuation/domain/track of, and
this is the primary anchor grid. The call is not in doubt.

**Activity rows per page across the declared range.**
- page 100 — 27 activity rows (all of them).
- page 101 — **0 activity rows.** The page is the first footnote page: the `Notes:` paragraph and
  footnotes a–m. No grid rows print on it.
- page 102 — **0 activity rows.** Second footnote page: footnotes n–z and aa.

This is a declared exception under §4, not a skipped page: the table's body is entirely on page 100
and pages 101–102 are the footnote apparatus of the same table, which is why they are inside
`page_start`..`page_end`.

## 3. Column geometry (L = 1) — how it was established

The page has real vector content but the grid was resolved from the **200 dpi raster rule lines**
(§1d), which is also what fixed the column count. Over the table's full height (y 282–1322 px) there
are exactly **13 full-height vertical rules** at x = 196, 608.5, 711, 817, 884.5, 952, 1022.5,
1140.5, 1337.5, 1436, 1672, 1770.5, 2009 px → one label column (196–608.5) plus 11 data columns.

The label column carries an **internal vertical rule at x = 372 px that exists only across the two
study-drug rows** (y 1090–1171 px); it is absent everywhere else, including the header. It is
therefore a nested split inside one label cell, **not** a second table column, so L = 1 and the first
data column is position 2. Recording this explicitly because if it had been read as a second label
column every `column_position`, `source_range` and `merged_cell_range` in the file would shift by one
with nothing else changing.

Column map (position → header), composed from the four header rows:

| pos | header | pos | header |
|---|---|---|---|
| 2 | Screening, D–28 to D–1 | 8 | Cycles 3–6/8, D1 |
| 3 | Screening, D–7 to D–1 | 9 | EOI |
| 4 | Cycle 1, D1 | 10 | Maintenance (every 8 weeks ± 10 days) |
| 5 | Cycle 1, D8 | 11 | EOM |
| 6 | Cycle 1, D15 | 12 | Follow-up (3 months) |
| 7 | Cycle 2, D1 | | |

## 4. The "Study drug administration" merged label cell (main structural judgement)

The label cell `Study drug administration` is **vertically merged across two body rows** whose right
sub-cells read `obinutuzumab r` and `chemotherapy s` (as recorded in
`activity_name_source.cell_text`). The text layer confirms the vertical merge: binning tokens into
the two row bands splits the parent label between them — the word `Study` and the word `drug` fall in
the upper band, the word `administration` falls in the lower one, alongside the two sub-labels.

I emitted **three** activity rows for these two physical rows:

- row 24 `Study drug administration`, `indentation_level: 0`, **no marks** (§4 organisational header);
- row 25 `obinutuzumab`, level 1, marks in columns 4, 5, 6 (marker `c`) and 7, 8, 10 (marker `d`);
- row 26 `chemotherapy`, level 1, marks in columns 4, 7, 8.

Consequence to be aware of at review: the table has 26 physical body rows but 27 `activity` objects,
and `row_position` from 24 onward is one higher than the physical row index. The alternative — two
level-1 rows with no parent — would have dropped a printed label and left an orphaned hierarchy, so I
chose to keep the label as a mark-free level-0 row and flag the offset here and in
`table_metadata.notes`.

## 5. Merged cells and merged marks

**No mark or text was distributed across a span in the table body** — every one of the 89 marks sits
in its own rule-bounded cell. All merges in this table are in the header:

| header row | value | span (`merged_cell_range`) |
|---|---|---|
| 1 | `Screening` | 2:3 |
| 1 | `Treatment` | 4:11 |
| 2 | `Induction (6–8 cycles)` | 4:8 |
| 3 | `Cycle 1` | 4:6 |

`Treatment` spans columns 4–11 only: the raster band y 282–312 px has interior vertical rules at
x = 817 and x = 1770 px, so it starts at the Cycle 1 D1 column and ends at EOM, excluding both
Screening columns and Follow-up.

Two header cells are **vertically** merged and the schema has no vertical merge, so each is recorded
once, on the row where it starts: `Screening` (spans header rows 1–3) and `Follow-up (3 months)`
(spans header rows 1–4). This is why header row 4 has no cell for columns 9–12.

## 6. Mechanical mark-check (§1b)

Method: `pdftotext -bbox` on page 100; each token assigned to the (row band × column band) rectangle
its centre falls in, using the raster-recovered rule lines of §3 as the cell boundaries; mark tokens
matched with `^[Xx][*a-zA-Z0-9]?$`.

Result: the mechanical matrix and the visual read of the rendered page agree **cell for cell — 89
marks, zero disagreements**; the set difference in both directions is empty. Mark glyph is lowercase
`x` throughout, and it is transcribed that way — not normalised to upper case.

Rows worth a spot-check at review because they are sparse in a way that looks like an omission but
is what the page prints:

- `Chemistry` has marks in 3, 5, 6, 7, 8, 9, 10, 11 — **no mark in column 4 (Cycle 1 D1)**, while
  `Hematology` immediately above it does have column 4. Confirmed on the render and in the bbox
  matrix; the missing mark is data.
- `Patient-reported measures` has marks in 4, 7, 8, 9, 10, 11, 12 — no marks in columns 5 and 6
  (Cycle 1 D8, D15), consistent with footnote `y`: "Before administration of treatment at Day 1 of
  Cycles 1–6, at the end of induction, during maintenance, at end of maintenance, and at end of
  study, patients will complete the MDASI."
- `Provider-reported measures` carries exactly one mark, in column 8, and it is footnoted: the cell
  prints `x` plus a superscript `x`. The footnote marker and the mark glyph are the same letter —
  encoded as `cell_value: "x"` with `annotation_markers: "x"`. Worth a look because a naive read
  could turn it into two marks or into no footnote.

## 7. Annotations

- 28 annotations, **all `footnote`**. `by_type` is not degenerate in the forbidden sense (§8 says
  all-`footnote` is normal); there is no notes/comments column in this table.
- None of the 27 lettered footnotes is a bare pointer, so none was typed `source_note`. The two that
  come closest still explain as well as point — e.g. `b` ends "…depends on the chemotherapy chosen
  for each patient (see Section 4.3.2.2 Combination chemotherapy)." and `s` ends "…or CVP (Days 1–5,
  Cycles 1–8; 21-day cycles). See Section 4.3.2.2 Combination chemotherapy."
- No activity label carries an inline section/appendix reference, so no `pr…` source notes were
  synthesised.
- **Containment check: no annotation's text is contained in another's.** Verified mechanically over
  all 28 texts.
- The text layer is **not** glyph-spread; no `deglyph_reconstruction` was needed and no
  `annotation_text_source` method was recorded on any annotation. Footnote text was read from the
  page 101–102 text layer, whose footnote block is a hanging-indent list (marker in column 0,
  continuation lines indented), so each note is bounded by its own marker rather than by proximity.
- **Abbreviation block deliberately yields zero annotations.** Page 100 ends with a 13-term block
  beginning "(a)PTT=(activated) partial thromboplastin time; D=day; ECG=electrocardiogram;". Terms
  such as `D`, `ECG`, `EOI`, `EOM`, `LVEF` do appear in header cells and activity labels, but only as
  running text / cell content — none is printed as a *marker*. Binding them would require
  `method: "text_match"`, which §6 and the §8 checklist forbid, and §6's rule that an unreferenced
  abbreviation block should normally yield zero annotations applies directly, so the block was not
  emitted. Flagging it because it is a
  visible, deliberate omission: the block's content is on page 100 and is not represented anywhere in
  the JSON.

### Header-cell footnotes (§6)

Four markers print on header cells and were placed on the `schedule_grid` cell of the exact column(s)
they sit on, not on the `schedule_property` row:

| marker | placed on |
|---|---|
| `a` | header row 1, columns 2 and 3 (the merged `Screening` cell covers both) |
| `b` | header row 2, columns 4–8 (the merged `Induction (6–8 cycles)` cell) |
| `e` | header row 2, column 10 (`Maintenance`) |
| `aa` | header row 1, column 12 (`Follow-up (3 months)`) |

For `a` and `b` the marker is printed once, on a horizontally merged cell; it is recorded on every
column the merge covers, mirroring the §5 merged-mark rule. If a reviewer prefers one location per
printed glyph, these are the four annotations to look at.

### Cell-level footnotes

`f` (row 5 col 2), `c` (row 25 cols 4,5,6), `d` (row 25 cols 7,8,10), `u` (row 28 cols 2,3,4 — also
on the label), `w` (row 29 cols 2,3,4 — also on the label), `x` (row 30 col 8). All others are
activity-label markers.

## 8. Synthesised values

**Synthesised `property_name` (3).** The header's label column is empty for the top three bands; only
the bottom band is labelled, and it prints `Day`.

| row | synthesised name | evidence |
|---|---|---|
| 1 | `Study phase` | cells are `Screening` / `Treatment` / `Follow-up (3 months)` |
| 2 | `Treatment period` | cells are `Induction (6–8 cycles)` / `EOI` / `Maintenance…` / `EOM` |
| 3 | `Cycle` | cells are `Cycle 1` / `Cycle 2` / `Cycles 3–6/8` |

Row 4 `Day` is **not** synthesised — it is printed in the label column
(`property_name_source.cell_value: "Day"`).

**Synthesised annotation marker (1).** The `Notes:` paragraph at the top of page 101 is a general,
unmarked table note: "Notes: Dosing (i.e., Day 1 of each cycle), in induction phase should be done
within ± 2 days of the scheduled visit date…". It has no printed marker and no single element to
attach to. I synthesised marker **`n1`**, gave it one `marker_location` on `schedule_property` row 1
with `method: "synthesized"`, and — so that `annotation_markers` and `marker_locations` agree as §6
requires — also put `n1` in that property row's `annotation_markers`. See §10 for why this is the one
binding in the file I am least sure about.

## 9. Low-confidence calls

1. **`property_type` of header row 2 = `period`.** The row mixes genuine sub-phases (`Induction
   (6–8 cycles)`, `Maintenance (every 8 weeks ± 10 days)`) with two end-of-phase encounters (`EOI`,
   `EOM`) that read more like visits. I typed the row by what dominates it and by its position under
   the `Treatment` epoch. A reviewer could defend `epoch` or `visit` for this row; the reasoning is in
   `property_comment` and `structure_method: "inferred_from_layout"` is set.
2. **Hierarchical levels 1–4** come from the rule-line stacking, not from printed labels, for rows 2
   and 3 (`structure_method: "inferred_from_layout"`). Row 1 and row 4 are unambiguous.
3. **L = 1 vs L = 2** — see §3. Decided on rule-line geometry; the only thing that would argue for
   L = 2 is the two-row-only internal divider.
4. **27 activities vs 26 physical rows** — see §4.
5. No PDF/markdown comparison was possible: no protocol markdown accompanies this excerpt, so all
   text is from the PDF text layer.

## 10. Orphan risk

- Every annotation has ≥ 1 `marker_location`, and every marker in a `marker_location` also appears in
  that row's `annotation_markers` (checked mechanically; zero mismatches). No `location_type:
  "unresolved"` entries.
- Every marker used anywhere in the JSON has a definition printed in the source; there is no
  referenced-but-undefined marker in this table.
- **The one soft binding is `n1`.** Its location on `schedule_property` row 1 is a convention, not a
  printed position: the note is table-scope. §6's Notes-column-header carve-out would have it carry
  the synthesised location *without* the marker on the row; the general rule for unmarked notes, plus
  the §8 checklist item that markers and locations must agree, would have it on the row. I chose the
  latter so the note resolves rather than dangling, and I am flagging the disagreement here so the
  reviewer can flip it if the corpus convention is the other one.

## 11. Method provenance (non-default methods recorded)

| where | field | value | why |
|---|---|---|---|
| `schedule_properties` row 2 | `structure_method` | `inferred_from_layout` | type/level from banding geometry, label cell empty |
| `schedule_properties` row 3 | `structure_method` | `inferred_from_layout` | same |
| annotation `n1` | `marker_locations[0].method` | `synthesized` | unmarked table-scope note |

Nothing else carries a non-default method: activity names and footnote text were read directly from
the text layer, cell values and merge extents from `pdftotext -bbox` binned into rule-bounded cells,
and indentation from the printed layout. No `proximity`, no `proximity_bounded`, no
`visual_transcription`, no `unresolved`.
