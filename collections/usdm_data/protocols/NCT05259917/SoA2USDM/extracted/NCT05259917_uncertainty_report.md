# NCT05259917 — SoA extraction uncertainty report (prompt v3.7.3, §7)

Source: `/root/ph3/blind/NCT05259917/NCT05259917_soa.pdf` (2 PDF pages).
Page map: PDF page 1 → document page 17, PDF page 2 → document page 18. Document page numbers are used
throughout, in `table_metadata` and in every activity's `source_page`. The printed footers happen to agree
with the page map here (`Page 17 of 67`, `Page 18 of 67`); the page map was still treated as authoritative.

## 1. Table inventory and classification

**One table.** Table 1 in the source, titled `Schedule of Events`, document pages 17–18,
`table_type: main_soa`.

- Rows are procedures performed on subjects (`In-clinic`, `Vital Signs`, `Dose`, …) → not `reference`.
- There is no other table in the excerpt for it to be a `continuation` / `domain` / `subsidiary` of, and
  there is no second population or study phase → not `track`. It is the protocol's primary anchor grid →
  `main_soa`.
- **Page split, not a second table.** The two header rows reprint on document page 18 above the single
  remaining body row, `Adverse Event Review`. Per §1b (repeated header and `schedule_property` bands on a
  continuation page are de-duplicated and each activity and mark counted once) this is one logical table
  spanning pages 17–18, extracted as one file with the reprinted header counted once. No separate
  `continuation` table was emitted.
- Footnote r refers to a table that is **not in this excerpt**:
  "Patient assessments will be performed as outlined in Table S1." Its content is unavailable and nothing
  was inferred from it.

## 2. Structure

- Leading label columns **L = 1** (the activity-name column, which also carries the header rows' own label
  `Visit`). First data column position = **2**. Six data columns, positions 2–7:
  2 `Screening`, 3 `Randomization`, 4 `1st eligible HAE attack`, 5 `2nd eligible HAE attack`,
  6 `3rd eligible HAE attack`, 7 `Final Visit/ET`.
- Column boundaries in PDF points, recovered from 200 dpi rule-line detection:
  71.3 | 309.6 | 368.6 | 454.9 | 530.3 | 600.1 | 669.2 | 720.4.
- **Activity count 24** (row_position 3–26). **`activity_schedule` entries: 61** (46 X cells + 15 arrow
  cells). **Annotations: 19.** **`schedule_grid` entries: 9.**
- Activity rows per page across the declared range: **page 17 → 23 rows; page 18 → 1 row**
  (`Adverse Event Review`). Every page in the declared range contributes rows.
- Two header rows → two `schedule_property` rows (positions 1 and 2); activities start at row_position 3.
- The table is flat: every activity-label token starts at the same x (77.1 pt) in the text layer, so
  `indentation_level: 0` on all rows is read from the text layer rather than assumed, and no
  `indentation_method` was recorded.

## 3. Merged cells and distributed marks

Every span below was confirmed from **rule-line geometry** (presence/absence of the internal rule inside the
row band at 200 dpi), not from where the glyph sits.

| Row | Activity | Decision | Span |
|---|---|---|---|
| 3 / 4 | `In-clinic` and `TeleVisit` | **Vertical** merge of the Randomization cell: the horizontal rule between the two rows is present in every column except column 3 (measured ink fraction 0.02 in column 3 versus 1.00 in columns 1, 2 and 4 at y ≈ 192.6–193.0 pt). The single X is centred between the two baselines (x-centre 412, y 186.2, between In-clinic at y 178.2 and TeleVisit at y 194.2). Per §5 it is emitted on **both** rows. | column 3, both rows |
| 18 | `Conventional on-demand treatment washout` | Double-headed arrow, no X. Internal rules at 530.3 and 600.1 pt are **absent** in this row band; arrow ink runs 457.9 → 661.7 pt. Distributed one entry per covered column. | `source_range` "4:6" |
| 25 | `Concomitant Medication Review` | Double-headed arrow. The only rules in the band are at 71.3 / 309.6 / 720.4 pt; arrow ink runs 313.2 → 715.3 pt. | `source_range` "2:7" |
| 26 | `Adverse Event Review` | Double-headed arrow, document page 18. The only rules in the band are at 71.3 / 309.6 / 720.4 pt; arrow ink runs 318.2 → 715.3 pt. | `source_range` "2:7" |
| header row 1 | `Treatment Period` | Merged header cell across the three attack columns (no internal rules at 530.3 / 600.1 in that header band). `is_merged_cell` true and `merged_cell_range` "4:6" on each covered position. | 4:6 |

`cell_value` for the three arrow rows is the glyph `↔` (both ends carry arrowheads in the render).

The schema has **no vertical-merge field**, so the two `In-clinic` / `TeleVisit` Randomization cells carry
no `source_range`; the vertical merge is documented in `table_metadata.notes` and here instead. This is the
one place where the delivered JSON cannot express what the page shows.

The `Screening`, `Randomization` and `Final Visit/ET` header cells also span vertically down into header
row 2. No `schedule_grid` entries were emitted for (row 2, columns 2 / 3 / 7): those positions are covered
by the row-1 cells, and emitting empty cells there would read as "blank", which is not what the page says.
Recorded in the row-2 `property_comment`.

## 4. Synthesised values

- **`schedule_property` row 2 `property_name`: `Eligible HAE attack`** — synthesised; the label column is
  blank for that row. `property_name_source.synthesized: true`, `structure_method: "inferred_from_layout"`
  (its `property_type` and `hierarchical_level` come from the spanning geometry of the merged
  `Treatment Period` cell above, not from a printed label).
- **No synthesised annotation markers.** All 19 annotations carry the source's own printed letters a–s.

## 5. Mechanical mark-check

Method: **§1b bbox column-binning** for the X marks, plus **§1d raster rule-line recovery** for cell and
merge geometry and for the arrows (arrows are vector graphics and invisible to `pdftotext`).

- Column x-centres fixed from the header labels: 341.9, 414.4, 495.3, 567.9, 637.4, 697.5. All `X` tokens
  bin to 339.3 / 411.9 / 492.6 / 565.4 / 634.2 / 695.0 — unambiguous, no token more than ~3 pt from a
  centre.
- The mark regex `^[Xx][*a-zA-Z0-9]?$` was applied. **No footnoted mark** (`X*`, `Xa`) exists in this
  table: every marker sits on a header cell or an activity label, never on a grid cell.
- The text layer contains **45 `X` tokens**; the extraction emits **46 X cells**, the one extra being the
  vertically merged Randomization mark distributed onto its second row (§3).
- **Disagreements between the mechanical matrix and the visual read: none** — all 46 X cells and all three
  arrow rows agree cell-for-cell.
- Rows whose only content is an arrow: 18, 25, 26. No activity row is empty across all six data columns.

## 6. Annotation text integrity

- The text layer is **not** glyph-spread; words come back whole and no `deglyph_reconstruction` was needed.
  All footnote text was read from the page-18 text layer verbatim, so no `annotation_text_source.method` is
  recorded on any annotation (all default).
- **No containment pairs** — no annotation's text is contained in another's (checked mechanically across all
  19 texts). No note-splitting or note-merging risk arises here: the annotations are printed footnotes with
  their own letters, not a right-hand notes column, so no proximity bounding was used anywhere.
- **Superscript marker fused to a label — flagged.** On page 18 the text layer yields
  `Adverse Event Reviews`: the trailing "s" is the superscript footnote marker (a separate token at x 179
  with a raised baseline and ~9 pt height against ~13.7 pt for the label), not a plural. Confirmed against
  the render. Extracted as `activity_name: "Adverse Event Review"` with `annotation_markers: "s"`, and the
  raw fused form kept in `activity_name_source.cell_text`. Footnote s begins
  "Adverse events recorded from the first dose of KVD900 or placebo…", consistent with a marker rather than
  a plural — but the reading is called out because one character changes the activity name.
- Two footnotes contain what look like source wording slips. Both are transcribed **as printed**, not
  repaired: footnote n says "at the Final/ET visit" where the column header reads `Final Visit/ET`, and
  footnote s says "up to and including to Final Visit/ET".

## 7. Abbreviations block — deliberately zero annotations

The page-18 block beginning "Abbreviations: C1-INH=C1-esterase inhibitor;" defines 11 terms.
**None is printed as a marker** on a grid cell, a header cell or an activity label — the only markers in
this table are the footnote letters a–s. Several terms *occur inside* activity labels (`12-lead ECG`,
`PGI-S`, `VAS`, `GA-NRS`, `Randomize patient in RTSM`), but binding those would be `method: "text_match"`,
which §6/§8 forbid as a sole binding, and a `method: "synthesized"` location on a `schedule_property` is the
same standalone list wearing a location. Per §6, a term whose only appearance is the abbreviation block is
dropped, so **no `abbreviation` annotations were emitted**. If a reviewer wants that block preserved it must
be preserved as something other than annotations on this table.

## 8. Low-confidence calls

1. **`property_type` of header row 1 = `visit`.** The row's printed label is `Visit`, but its cells mix
   visit names (`Screening`, `Randomization`, `Final Visit/ET`) with a phase name (`Treatment Period`)
   merged across columns 4–6. A defensible alternative is `epoch` for that row with the visit names one
   level down. I followed the source's own printed row label and recorded the mixture in
   `property_comment`. Moderate confidence.
2. **`property_type` of header row 2 = `visit`.** `1st eligible HAE attack`, `2nd eligible HAE attack` and
   `3rd eligible HAE attack` are event-triggered televisit encounters rather than calendar visits, so
   `condition` or `other` are arguable. They are the only thing distinguishing columns 4, 5 and 6, so the
   row gets `hierarchical_level: 2` regardless of the type call.
3. **Arrow semantics.** The three double-headed arrows are transcribed as `↔` distributed over their spans.
   Whether the intent is "continuous throughout the span" or "at each of those encounters" is study logic,
   not table content, and was left to downstream resolution.
4. **`In-clinic` and `TeleVisit` are visit-modality rows, not procedures.** They were kept as activities
   because they are body rows of the grid carrying marks; a resolver may prefer to treat them as a modality
   property of the columns. Flagged rather than reinterpreted.
5. No protocol markdown was available, so no PDF/markdown text comparison was possible; all text comes from
   the PDF text layer.

## 9. Orphan risk and method provenance

- **Orphans: none.** All 19 annotations have ≥ 1 `marker_locations` entry, and every marker in a
  `marker_locations` entry also appears in that row's or cell's `annotation_markers` (checked
  mechanically). Markers a, b and c bind to `schedule_cell` locations on the exact header columns they are
  printed on — (row 1, column 3), (row 2, columns 4 / 5 / 6), (row 1, column 7) — per the §6 header-cell
  rule, and not to the `schedule_property` row.
- **Markers referenced but not defined: none.** Every marker printed in the table (a–s) has its text
  printed on page 18, and every footnote letter printed on page 18 has at least one location in the table.
- **`unresolved` marker locations: none.**
- **Non-default methods recorded (§1e):**
  - `activity_schedule[].method = "raster_pixel_detection"` on the 15 arrow cells of rows 18 (3 cells),
    25 (6 cells) and 26 (6 cells) — arrows carry no text token, so their extent was read from the 200 dpi
    render.
  - `schedule_properties[row 2].structure_method = "inferred_from_layout"`.
  - `schedule_properties[row 2].property_name_source.synthesized = true`.
  - Nothing else: all X marks, all header labels and all footnote texts came from the default
    `pdftotext -bbox` text-layer read, bounded by the recovered rule lines.

## 10. Recommended spot-checks for the reviewer

1. Document page 17, rows `In-clinic` / `TeleVisit`, Randomization column — confirm the single X is shared
   by both rows (vertical merge) rather than belonging to `In-clinic` alone.
2. Document page 18 — confirm `Adverse Event Review` plus superscript s, not a plural "Reviews".
3. The three arrow rows and their spans (4:6, 2:7, 2:7).
