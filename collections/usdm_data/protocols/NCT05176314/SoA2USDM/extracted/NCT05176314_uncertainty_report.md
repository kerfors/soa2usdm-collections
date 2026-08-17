# NCT05176314 — SoA extraction uncertainty report (§7)

Source: `/root/ph3/blind/NCT05176314/NCT05176314_soa.pdf` (3 PDF pages = document pages 10, 11, 12).
Prompt v3.7.3, schema `soa-table-extraction` v1.0.
Deliverable: `NCT05176314_Table_01_extraction.json` (one table).

---

## 1. Structure discovery — how many tables

The excerpt contains **one** SoA grid, printed under the section heading
`Schedule of Activities (SoA)` (section 1.3) and running across document pages 10 and 11. The
source prints **no table number and no table title**; `table_title` was taken from the section
heading and this is recorded in `table_metadata.notes`.

Document page 12 carries only the abbreviation line and the footnote block for that grid. PAGEMAP.md
declares page 12 to be beyond the SoA range, so it is **not** included in `page_start`/`page_end` —
see §7 below for how its text was nevertheless used.

### Per table

| | |
|---|---|
| table_number | 1 |
| table_type | `main_soa` |
| pages | 10–11 |
| label columns (L) | **1** (`Study Procedure`) — **first data column is position 2** |
| data columns | 21 (positions 2–22) |
| total columns | 22 |
| header rows (`schedule_properties`) | 2 |
| activities | 22 |
| `activity_schedule` cells | 122 |
| annotations | 8 (7 `footnote`, 1 `legend`) |

**Activity rows per page across the declared range** (§4 coverage check):

| document page | activity rows |
|---:|---:|
| 10 | 15 (`Informed consent` … `Oral body temperature`) |
| 11 | 7 (`12-lead ECG` … `Concomitant medication review`) |

Every page in the declared range 10–11 contributes rows. No page was skipped. The table is not
horizontally tiled, so the §4 tiling exception does not apply.

### Why `main_soa`, and why one table rather than main_soa + continuation

Applying the §2 discriminators in order:

* **reference test** — the rows are procedures performed on participants (`Informed consent`,
  `12-lead ECG`, `Rosuvastatin administration`), not sample specifications or notes. Not `reference`.
* **domain / track / subsidiary** — there is no second grid to be a domain of, no second population,
  and no finer-granularity timing table. One population, one timeline.
* It is the primary anchor grid → `main_soa`.

The two-band column header is reprinted **verbatim** at the top of page 11 (identical tokens:
`Treatment Period (Study Days)`, `Screening`, the day labels `-1 … 18`, `FU/`…`EDb`) and the body
rows simply continue where page 10 stopped. Per §1b ("header and `schedule_property` rows … reprint
on every continuation page; count each activity and each mark once") this was extracted as **one
table spanning pages 10–11 with the reprinted header de-duplicated**, not as a `main_soa` +
`continuation` pair. This is a judgement call and is the single largest structural decision in the
run; it is recorded in `table_metadata.notes`. A reviewer who prefers the two-file form would split
rows 18–24 into Table 02 with `continuation_of: 1` — the marks, spans and column numbering would be
unchanged.

---

## 2. Header rows and synthesised names

Two header bands, confirmed from the vector rule geometry (see §5):

* **row 1 — `Study Period`, `property_type: epoch`, level 1.** Cells: column 2
  `Screening (D-42 to -2)`; columns 3–21 `Treatment Period (Study Days)`; column 22 `FU/ED`.
* **row 2 — `Study Day`, `property_type: study_day`, level 2.** Cells: `-1`, `1`, `2` … `18` in
  columns 3–21 and `24 (± 2 days)` in column 22.

**Synthesised `property_name` values (2):** `Study Period` and `Study Day`, both with
`property_name_source.synthesized: true` and `cell_value: ""`. Neither header band has a printed row
label — the leftmost header cell reads `Study Procedure`, which labels the activity column, not the
property rows. `property_type` for both comes from printed values, so no `structure_method` was set:
`Screening` / `Treatment Period` / `FU/ED` are printed phase names (epoch), and the parent band
prints `(Study Days)` over the number row (study_day).

**Header merges** (`is_merged_cell` recorded on every covered position):

* `Treatment Period (Study Days)` is one cell spanning **columns 3–21** — 19 `schedule_grid` entries,
  each with `merged_cell_range: "3:21"`. Verified from the rule geometry: in the top header band
  (y 333–407 px at 200 dpi) the only vertical rules are at x = 200, 416, 495, 1893, 1999; every
  internal rule between x = 495 and x = 1893 is absent. Note this span **includes the `-1` column**
  (check-in day), which sits under "Treatment Period" and not under "Screening".
* `Screening (D-42 to -2)` (column 2) and `Study Procedure` (column 1) are merged **vertically**
  across both header bands — neither column has the horizontal rule at y = 407 that every other
  column has. The schema has no vertical-merge notation and `merged_cell_range` is defined as a
  *column* range (§5), so the Screening cell is emitted once, on header row 1, with
  `is_merged_cell: true` and **no** `merged_cell_range`. Column 2 therefore has no row-2 entry. Flagged
  here because it is the one place `is_merged_cell` appears without a range.

---

## 3. Merged-mark decisions in the body

**None.** Every internal vertical rule is present in every one of the 24 body row-bands (checked
mechanically, all 23 rule x-positions present in all bands), so there are no merged body cells, no
distributed marks, no `source_range` values, and no arrows. Multi-column text such as
`P, 0.5, 1, 1.5, 2, 2.5, 3, 4, 5, 6, 8, 12 h` is *not* merged — it is repeated independently in
columns 4, 9 and 16, each inside its own ruled cell.

---

## 4. Redacted content — source defect, two kinds

### 4a. Two full-width redaction boxes conceal activity rows (document page 11)

Two solid black boxes, each overprinted with a red `CCI` label, cross the **entire width** of the
table on page 11:

| box | pixel band (200 dpi) | x extent | sits between |
|---|---|---|---|
| 1 | y 708–825 (117 px tall) | x 174 → 1999 (past the table's right rule at 1999) | `Genetic blood sample for screening` and `Rosuvastatin PK samples` |
| 2 | y 1037–1119 (82 px tall) | x 180 → 2030 | `Rosuvastatin PK samples` and `Adverse event review` |

For scale, single-line rows in this table are 37–38 px tall, two-line rows 73–95 px and three-line
rows ~109 px. **Each box therefore conceals at least one activity row together with every mark in
it, and the number of concealed rows cannot be determined** — box 1 could be one three-line row or a
two-line plus a one-line row. The text layer carries nothing under the boxes: the only tokens in
those bands are the `CCI` overlay labels, which are printed at x ≈ 181 px, i.e. **outside** the
table's left rule at x = 200 px, so `CCI` is a redaction overlay and not a cell value.

Decision: **no activity rows were emitted for the concealed bands** and no placeholder names were
invented. `row_position` runs contiguously (… 21 `Genetic blood sample for screening`,
22 `Rosuvastatin PK samples`, 23 `Adverse event review` …) with the two hidden bands falling between
21/22 and 22/23. This is flagged in `table_metadata.notes`. **A reviewer with an unredacted copy
should expect this table to have more than 22 activities.**

Box 1 clips the descender of the last line of the row above it: the `screening` line of
`Genetic blood sample for screening` is partly overpainted. The word is still legible on the render
and the text layer confirms it, so the activity name is transcribed in full.

### 4b. Three footnote definitions are redacted (document page 12)

The footnote block prints `a`, `b`, `c`, then a redaction box, then `e`, then a second, much larger
redaction box:

| footnote page region | y band (200 dpi) | content |
|---|---|---|
| after footnote `c` | 509–598 | black box, `CCI` — the `d` slot |
| after footnote `e` | 678–911 | black box, `CCI` — the `f` and `g` slots |

Markers `d`, `f` and `g` **are** printed in the table (`Supine vital signs (PR and BP)ᵈ`, the `Pf` in
the Day 6 PK cell, `Rosuvastatin PK samplesᵍ`), so all three annotations are emitted with the marker
transcribed where it appears and no text fabricated. Their `annotation_text` states the defect
plainly. The `d` annotation reads, verbatim from the JSON:

`[Footnote d is not printed in the source. In the footnote block (document page 12) the slot immediately after footnote c is covered by a solid black redaction box overprinted with the label "CCI"; no text is legible. Remainder redacted in source.]`

No probable cross-reference was offered for any of them: there is no same-assessment equivalent
elsewhere in the excerpt to point at (§6).

---

## 5. Mechanical mark-check (§1b)

Method: **bbox column-binning against rule-line-recovered cell rectangles.**

1. `pdftotext -bbox` for every token (no pdfplumber).
2. Column x-centres fixed from the **header** day labels, then confirmed against the 23 vertical
   rules recovered from the 200 dpi raster at x = 200, 416, 495, 554, 672, 731, 789, 849, 908, 1009,
   1076, 1142, 1209, 1275, 1361, 1420, 1538, 1608, 1679, 1749, 1819, 1893, 1999. Every header label
   falls inside the expected inter-rule band.
3. Row bands recovered from horizontal rules read inside a **text** column, not a redacted one (§1d):
   the notes-free right-hand column 22 and column 16 both yield the same band list, and the label
   column yields the same list minus the y = 407 rule (the vertical header merge, §2).
4. Every token assigned to the band its centre falls in; marks matched with `^[Xx][*a-zA-Z0-9]?$`
   so footnoted marks are not dropped.
5. The resulting matrix was diffed cell-for-cell against a visual read of both page renders.

**Result: no disagreement.** All 122 non-empty body cells agree between the mechanical matrix and the
visual read, including the sparse rows (`Discharge from CRU` — one mark, column 21) and the dense
rows (`Adverse event review` and `Concomitant medication review` — 21 marks each, columns 2–22).

Glyph case: every mark in this table is an upper-case `X`; no lower-case x, tick or bullet glyph occurs, and nothing
was normalised.

Spot-checks worth a human eye:
* `Pirtobrutinib administration` carries marks in columns 9–20 (Days 6–17) and **not** in column 21
  (Day 18) — a real gap, not a dropped mark; footnote `a` says participants are discharged on Day 18.
* `Rosuvastatin PK samples` has **no** cell in column 15 (Day 12) between the 120 h cell of column 14
  (Day 11) and the predose series of column 16 (Day 13). Verified twice: the Day 12 cell is ruled and
  empty.
* `Supine vital signs (PR and BP)` column 5 / 10 / 17 read `24 h`; the `24` and the `h` are on
  separate text-layer lines within the one cell and were rejoined.

---

## 6. Annotations

8 annotations: `a`–`g` as `footnote`, plus one `legend`. `by_type` is not degenerate and is not
all-`source_note`.

**No `source_note` was emitted** — the table contains no reference column and no inline section or
appendix citation in any activity label.

**Legend `P`.** The abbreviation line defines `P = predose`, and `P` appears as a standalone in-grid
scheduling mark (`12-lead ECG` and `Clinical laboratory tests`, columns 4, 9, 16) as well as leading
the compound cells `P, 2 h` and `P, 0.5, …`. Per §5 the `P` **stays in `cell_value`** — it was not
stripped — and per §6 it is additionally emitted as a `legend` with a `marker_location` on each of
the 12 cells that use it (rows 16, 18, 19, 22 × columns 4, 9, 16). To satisfy the §6/§8 agreement
rule, `P` was also added to those 12 cells' own `annotation_markers`. This is the one place in the
file where a marker in `annotation_markers` is *also* part of `cell_value`; it is deliberate, and it
is why cell (row 22, column 9) carries `annotation_markers: "f,P"`.

**Abbreviations deliberately NOT emitted.** The block opens
`Abbreviations: BP = blood pressure;` and closes `PR = pulse rate.` — 10 terms. Per §6, an abbreviation's marker must be
the term *printed as a marker*, and word overlap is not a marker, so none of `BP`, `CRU`, `D`, `ECG`,
`ED`, `FU`, `PK`, `PR` was emitted: each occurs only inside an activity label
(`Supine vital signs (PR and BP)`, `Rosuvastatin PK samples`, `12-lead ECG`) or as the content of a
header cell (`FU/ED`, `Screening (D-42 to -2)`), never as a marker. Binding them would have required
`method: "text_match"` or `"synthesized"`, which §6/§8 forbid. **Borderline call:** `h = hours` …
`postdose` was also dropped although it appears in many grid cells (`24 h`, `120 h`, `12 h`) — unlike
`P` it never appears as a standalone cell value, so it reads as a unit inside the cell's text rather
than as a scheduling symbol. A reviewer may reasonably want `h` promoted to a second `legend`; if so,
its locations would be the 23 cells in rows 16 and 22 that end in `h`.

**Header-cell footnotes (§6).** Markers `a` and `b` sit on specific header cells and were bound to
those cells, not to the `schedule_property` row: `a` → `schedule_cell` row 2, column 21 (the `18a`
day cell); `b` → `schedule_cell` row 1, column 22 (`FU/`…`EDb`). This keeps each footnote resolving
to its own column rather than collapsing to the whole header row.

**Cell-level footnote.** `f` is bound to `schedule_cell` row 22, column 9 only — it is printed on the
`Pf, 0.5,` of the Day 6 PK series, not on the Day 1 or Day 13 series, and not on the row label.

**Marker agreement.** Checked mechanically: every marker in every `marker_locations` entry also
appears in that element's `annotation_markers`, and every `annotation_markers` value on every
element resolves to an emitted annotation. Zero failures both directions. No annotation has empty
`marker_locations`.

**Annotation text integrity.** The text layer is **not** glyph-spread — tokens are whole words — so
no `deglyph_reconstruction` was needed and no `annotation_text_source.method` is recorded anywhere.
All footnote text was read from the page-12 text layer, each footnote bounded by its own printed
marker (`a`, `b`, `c`, `e`), not by proximity, so no `proximity_bounded` note exists.

**Containment check.** One near-duplicate pair exists and it is **not** a split source cell: the
placeholder texts for `f` and `g` differ only in the marker letter, because both are *my* synthesised
statements of the same redaction defect, not source content. No pair of genuine source-text
annotations overlaps. Footnote `b`'s text is long and is transcribed complete, opening
`Participants will attend an outpatient follow-up visit 7 (± 2) days after the final dose of study intervention.`
and closing `should be performed on the day of discontinuation (or as close to it as possible).`

---

## 7. Page-map interactions

* Document page numbers come from PAGEMAP.md, not from footers. In this document the printed footers
  happen to agree (page 10 prints `10`, page 11 prints `11`, page 12 prints `12`), but the mapping
  used is the PAGEMAP one.
* **Page 12 is declared beyond the SoA range.** It carries no activity rows — only the abbreviation
  line and footnotes `a`–`g`. `page_end` is therefore **11**, and page 12 contributes nothing to the
  §4 page-coverage check. Its footnote text *was* transcribed into the annotations, because markers
  `a`–`g` are printed inside the table on pages 10–11 and leaving them textless would have asserted
  "not printed in the source" about text that is plainly printed. **This is a judgement call worth a
  reviewer's attention**: if the intent of the page-map flag is that page-12 content be excluded
  entirely, annotations `a`, `b`, `c`, `e` would lose their text (and `d`, `f`, `g` would be
  unaffected, being redacted anyway).
* The running header `Protocol number J2N-MC-JZNW (a)` appears on all three pages. The trailing
  `(a)` is a protocol amendment identifier in the page header, **not** a table footnote marker, and
  was not treated as one.

---

## 8. Low-confidence calls, ranked

1. **One table vs. `main_soa` + `continuation`** for the page 10/11 split (see §1). Recorded in
   `table_metadata.notes`.
2. **Redacted rows omitted entirely** rather than emitted as placeholder activities (see §4a). The
   activity count of 22 is a floor, not the true count.
3. **Page-12 footnote text used** despite the page being outside the declared range (see §7).
4. **`h = hours`…`postdose` dropped** while `P = predose` was kept (see §6).
5. **`Screening (D-42 to -2)` placed on header row 1 only**, with `is_merged_cell: true` and no
   range, because the merge is vertical (see §2). Column 2 consequently has no row-2 study-day value.
6. **`24 (± 2 days)` kept as one `study_day` cell** rather than split into a day value and a `window`
   property. The source prints it as a single cell in the day band; no separate window row exists.
7. **`Treatment Period (Study Days)` spans columns 3–21, i.e. it covers Day −1.** Read from rule
   geometry, not from where the caption glyphs sit. Worth confirming by eye, since a reader might
   expect Day −1 (check-in) to belong to screening.

---

## 9. Method provenance (§1e) — everything non-default, one line each

* `activity_name_source.indentation_method: "assumed_flat"` on **all 22 activities.** The table is
  genuinely flat: every row is an individual procedure, there are no grouping/section-header rows,
  and no row is indented, shaded or bolded relative to another. All `indentation_level` = 0.
* **No** `annotation_text_source.method` recorded anywhere — all annotation text was read from
  rule-line-bounded text-layer cells (default).
* **No** `method` recorded on any `schedule_grid` or `activity_schedule` cell — all values are bbox
  text-layer reads (default). The raster was used only to recover rule lines, never to detect marks.
* **No** `activity_name_source.method` recorded — all names are direct text-layer reads.
* **No** `schedule_property.structure_method` recorded — `property_type` and `hierarchical_level` for
  both header rows come from printed labels.
* **No** `marker_locations[].method` recorded — every one of the 20 marker locations is a position
  where the marker is actually printed. Nothing was bound by `text_match`, `proximity` or
  `synthesized`.
* **No `location_type: "unresolved"` entries**, and no synthesised annotation markers: every marker
  in the file (`a`, `b`, `c`, `d`, `e`, `f`, `g`, `P`) is printed in the source.

---

## 10. Schema conformance

Validated against `soa-table-extraction.schema.json` with `jsonschema` — **valid**.
`schema_name: "soa-table-extraction"`, `schema_version: "1.0"`,
`extraction_status: "ready_for_resolution"`. Every `property_comment` is populated; every annotation
has ≥ 1 `marker_locations` entry; no `merged_cell_range` or `source_range` uses A1 notation
(the only range in the file is `"3:21"`); `track_label` is absent (not a `track` table).
