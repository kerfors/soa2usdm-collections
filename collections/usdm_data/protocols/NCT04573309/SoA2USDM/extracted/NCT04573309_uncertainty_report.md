# NCT04573309 — SoA extraction uncertainty report

Source: `NCT04573309_soa.pdf` (4 PDF pages), "Protocol Amendment 3.1 (US)", "18 Mar 2022",
sponsor study "ALXN1840-WD-204". Document pages taken from `PAGEMAP.md`
(PDF 1-4 = document pages 14-17); the printed footers happen to agree with the map on this
excerpt, but the map was used throughout.

Two tables were found and extracted:

| File | Table | Type | Doc pages | Label cols (L) | First data col | Data cols | Activities | Annotations |
|---|---|---|---|---|---|---|---|---|
| `NCT04573309_Table_01_extraction.json` | Table 1 "Schedule of Activities" | `main_soa` | 14-16 | 1 | 2 | 24 (positions 2-25) | 44 | 24 |
| `NCT04573309_Table_02_extraction.json` | Table 2 "Schedule of Pharmacokinetic and Pharmacodynamic Assessments on Days 1, 25, 29, and 39" | `subsidiary` | 17 | 1 | 2 | 11 (positions 2-12) | 2 | 3 |

---

## 1. Per table

### Table 1 — `main_soa`, document pages 14-16

Primary anchor grid: rows are procedures performed on participants, columns are the study's
own day/period timeline. No competing anchor exists, so `main_soa` needs no further argument.

**Activity rows per page across the declared range**

| Doc page | Activity rows |
|---|---|
| 14 | 27 (rows 4-30, `Eligibility` … `Blood sampling for PK: Plasma total Mo and PUF-Mo`) |
| 15 | 17 (rows 31-47, `PD: Plasma total and PUF-Cu, LBC, ceruloplasmin, ceruloplasmin-bound Cu` … `Concomitant medication and non-pharmacologic therapy/procedure`) |
| 16 | **0 — declared exception.** Page 16 carries no grid at all: it is the tail of the footnote block (markers i through x) plus the abbreviation block, beginning "Parameters include age and sex." and ending "…WD = Wilson disease." It is inside the page range because the table's footnotes run onto it. |

The three header rows are **reprinted verbatim** at the top of page 15. They were counted once
(row positions 1-3) and are not duplicated in `schedule_properties` / `schedule_grid`.

**One table or two?** The page-15 grid repeats the identical header and simply continues the
row list; the source labels only one "Table 1". It is emitted as ONE table spanning pages 14-16
rather than as a parent plus a `continuation` table. Either modelling reconstructs the same
consolidated grid; flagging it because the choice is visible in the file count.

### Table 2 — `subsidiary`, document page 17

`subsidiary` rather than `reference` or `track`: its rows are assessments performed on
participants (so not `reference`), it schedules the same participants in the same treatment
period (so not `track`), and its columns are a finer timing grid — hours post-dose — for
activities that Table 1 carries as whole rows. Table 1 footnote o points at it explicitly:
"…See Table 2 for PK/PD sampling on Days 1, 25, 29," and footnote p repeats the pointer. This is
the PK-sampling ambiguity named in prompt §2 and is recorded in `table_metadata.notes`.

Page 17 contributes both activity rows.

---

## 2. Structure decisions that changed the row/column count

### 2a. Table 2 — one printed cell, two emitted activities (biggest single judgement call)

Rule-line recovery on page 17 finds only three horizontal rules (table top, below the header,
table bottom): the body is **one** rule-bounded row. Its label cell holds four text lines naming
two assessments — "Blood sampling for PK: Plasma total Mo and PUF-Mo" and
"PD: Plasma total and PUF-Cu, LBC, ceruloplasmin, ceruloplasmin-bound Cu" — with a single row of
marks vertically centred across all four lines (which is why the text layer prints the marks
*between* the two names).

Decision: emitted as two activities (`row_position` 2 and 3) with the centred marks applied to
both, per the vertically-merged-mark convention (§5). Rationale: those are two separate activity
rows in Table 1, so a fused single name would match neither parent activity downstream. **This is
an extraction decision, not a printed row split** — a reviewer who prefers strict row fidelity
should collapse rows 2 and 3 into one. Recorded in `table_metadata.notes`.

### 2b. Table 1 — the PK/PD mark row split by the page break (marks left where printed)

Row 30 "Blood sampling for PK: Plasma total Mo and PUF-Mo" is the last body row on page 14 and
carries 10 marks. Row 31 "PD: Plasma total and PUF-Cu, LBC, ceruloplasmin, ceruloplasmin-bound Cu"
is the first body row on page 15 and carries **none**. In Table 2 the same two assessments share
one merged mark cell, so this is probably the same vertical merge, broken by the page break — but
the page break puts the two cells on different pages, so the merge cannot be confirmed from
rule-line geometry, and every internal vertical rule is present in both bands.

Decision: **transcribed as printed** — the marks stay on row 30 and row 31 is empty. If a reviewer
resolves this as a vertical merge, row 31 should receive the same 10 marks
(columns 8, 10, 16, 17, 18, 19, 20, 21, 22, 23). Recorded in `table_metadata.notes`.

---

## 3. Merged cells and distributed marks

### Header merges (Table 1) — all confirmed from per-band rule-line presence, not from glyph position

| Row | Cell | Columns | `merged_cell_range` |
|---|---|---|---|
| 1 | "Screening" (marker a) | 2-3 | `2:3` |
| 1 | *empty* | 5-23 | `5:23` |
| 2 | "Screening" | 2-3 | `2:3` |
| 2 | "Inpatient Period 1" | 5-12 | `5:12` |
| 2 | "Inpatient Period 2" | 15-23 | `15:23` |

`C-I` (col 4), `OP` (col 13), `UNS` (col 24) and `EOS or ET` (col 25) are single-column cells.

**Transcribed, not repaired:** in header row 2 the Day 23 column (position 14) has an **empty**
period cell — "Inpatient Period 2" starts at Day 24 (col 15) — while footnote v states the diet
runs "…throughout both the inpatient Period 1 (Day -8 to Day 9) and inpatient Period 2 (Day 23 to Day 40); during this time participants will be strongly encouraged to…"
and footnote c says participants may be "…readmitted on Day 22 with all procedures starting on Day 23."
The empty cell is what the rules and the page render both show; it was left empty.

### Body merges (Table 1)

**None.** For every one of the 44 body bands on both grid pages, all 24 internal vertical rules
are present at ≥85% contiguous coverage of the band. No mark was distributed across columns by
merge.

### Arrows (Table 1) — distributed per §5

Two rows carry a drawn horizontal arrow (vector graphics, invisible to the text layer); extents
were measured from the raster and confirmed on a rendered crop.

| Row | Activity | Printed X | Arrow columns | `source_range` | `cell_value` |
|---|---|---|---|---|---|
| 23 | "Discontinue chelation therapy" | col 7 (Days -4 through -1) | 8-25 | `8:25` | `→` |
| 24 | "Discontinue zinc therapy" | col 3 (Day -21) | 4-25 | `4:25` | `→` |

Both arrowheads terminate **inside** the last column (25, `EOS or ET`); both lines cross the
`UNS` column (24) continuously, so column 24 carries the arrow as drawn. Cells method =
`visual_read` (40 cells).

### Table 2

The single centred mark row was applied to both emitted activity rows (see 2a). No column-level
merge exists in Table 2; marks are at hours −0.5, 2, 4, 5, 6, 8, 12, 24 (columns 2, 5, 7, 8, 9,
10, 11, 12). Hours 0, 1 and 3 are **unmarked in the source** and were left unmarked.

---

## 4. Synthesised values

**Synthesised `property_name` (Table 1, 2):** header rows 1 and 2 have no printed row label — the
label column carries "Study Procedures" merged over both. Names synthesised as **"Visit"**
(row 1: `Screening`, `C-I`, `UNS`, `EOS or ET`) and **"Study Period"** (row 2: `Screening`, `C-I`,
`Inpatient Period 1`, `OP`, `Inpatient Period 2`), both with
`property_name_source.synthesized: true` and `structure_method: "inferred_from_layout"`.
Row 3's label "Days" is printed, so it carries no synthesis flag.

**Synthesised annotation marker (Table 2, 1):** the unmarked paragraph beginning
"Note: Windows for PK/PD time points will be defined as ±10% of the nominal time point." carries no
printed marker. Marker synthesised as **`note1`**, bound to `schedule_property` row 1 with
`method: "synthesized"`, and mirrored in that property's `annotation_markers` so `resolve` can
link it.

**No synthesised markers in Table 1** — every one of its 24 annotations has a printed marker
(a-x).

---

## 5. Mechanical mark-check

Method (both tables, one method for the whole document):

1. **Rule-line geometry from the raster (§1d)** — pages rendered at 200 dpi (`pdftoppm -r 200`),
   ink = pixels below 50% grey. Vertical rules = image columns with ink over ≥50% of the table
   height; horizontal rules = image rows with ≥85% ink **within the label column's x-range**
   (a text column, per §1d) applied across the row.
   * Pages 14 and 15 give the **identical** 26 vertical rules → 1 label column + 24 data columns,
     which is itself the check that the page-15 header is the same grid.
   * Page 17 gives 13 vertical rules → 1 label + 11 timepoint columns, and 3 horizontal rules.
2. **Column identity fixed from the header** (`Days` row / `Time point (hours)` row), never from
   body marks.
3. **Mark matrix from `pdftotext -bbox`** — every token's box centre binned to the column band it
   falls in and to the row band it falls in. A trailing single-letter token inside the same cell
   (e.g. the `p` of `X`+`p`) is taken as an annotation marker, not as part of `cell_value`, so
   footnoted marks are never dropped.
4. **Per-band merge test** — for every row band, each internal vertical rule is tested for a
   contiguous ink run covering ≥85% of the band. This is what produced the header merge table in
   §3 and what established that no body cell is merged.
5. **Diff against visual reads** of the full-page renders and of two header crops.

**Disagreements between the mechanical matrix and the visual read: none.** Spot checks that were
resolved in favour of the mechanical read (my first eyeball estimate of the column was off by one
in each case, the bbox x-centre was right): `Vitals sign measurements` Day 9 (col 12, x-centre
1193.5 px, band 1174-1212) and `Chemistry, hematology, Coagulation` Day 8 (col 11, x-centre
1156.3 px, band 1137-1174).

Totals: Table 1 — 212 `X` cells + 40 arrow cells = 252 `activity_schedule` entries; Table 2 — 16.

---

## 6. Annotation text integrity

* **The text layer is not glyph-spread.** Words come out whole from `pdftotext`; no
  de-glyphing was needed and no `deglyph_reconstruction` provenance is recorded. Footnote text was
  read from the text layer and joined across printed line wraps only.
* **Containment / overlap check: clean.** No annotation's text is contained in another's, and no
  pair shares a run of ≥6 words, in either table.
* **One boundary needed care.** In the text layer footnote x is immediately followed, with no
  blank line, by the abbreviation block
  ("Abbreviations: AE = adverse event; BMI = body mass index; C-I = check-in;…"). Footnote x was
  cut at its own last word — "For fecal samples, each individual sample will be independently collected with record of date, time, and weight of the sample." — and the abbreviation list was
  **not** appended to it. Verified against the page 16 render.
* **Abbreviation blocks yielded zero annotations, in both tables** (§6): no term in either block
  is printed as a *marker* on a grid cell, a header cell or an activity label. Terms such as
  `C-I`, `OP`, `UNS` are header cell *contents*, and those columns already carry their own printed
  footnote markers (b, c, d); binding the abbreviation entries to activities whose names contain
  the term would be exactly the `text_match` binding §6 forbids. Terms `AE`, `D` and `HR` do not
  appear in either grid at all.
* All 27 annotations are typed `footnote`. Nothing is a bare pointer: footnote a explains and then
  cites ("…are detailed in Section 8."), and footnotes o and p both explain before pointing at
  Table 2. No activity label carries an inline section/appendix reference, so no `source_note` was
  emitted.

---

## 7. Low-confidence calls

1. **Table 2 = two activities from one printed cell** (§2a) — the single most reviewable decision.
2. **Table 1 row 31 left empty** (§2b) — probable cross-page vertical merge, deliberately not
   resolved.
3. **Page 16 inside Table 1's range** — the page has no grid; included because the footnote block
   continues onto it. Contributes 0 activity rows by design.
4. **Table 1 emitted as one table, not parent + `continuation`** (§1).
5. **`property_type` for the two synthesised header rows** — row 1 typed `visit` (its distinctive
   values `UNS` and `EOS or ET` are encounters; it is the only row that tells column 24 from its
   neighbours) and row 2 typed `period` (the source's own word: "Inpatient Period 2"). Both rows
   also repeat `Screening` and `C-I`, so an alternative reading types row 1 `epoch`. The
   hierarchy 1=Visit / 2=Period / 3=Days follows the printed stacking, which puts the visit-ish
   row above the period row.
6. **Arrows crossing the `UNS` column** — columns 24 and 25 receive the arrow because the drawn
   line crosses column 24 and the arrowhead lands in column 25. A reader who treats `UNS` as
   out-of-timeline would stop the span at 23.
7. **Source-internal inconsistencies transcribed as printed, not repaired** (no data changed):
   * Header row 2 leaves Day 23 outside "Inpatient Period 2" (§3) although footnote v and
     footnote c both put Day 23 inside it.
   * "ALXN1840 30 mg/day" carries no mark at Day 40 (col 23), though Day 40 is a dosing/discharge
     day elsewhere in the table.
   * "Cu/Mo-controlled mealsv" carries no mark at Day -8 (col 4) or Day 9 (col 12), though
     footnote v says the diet runs Day -8 to Day 9.
   * Footnote r reads "Laboratory assessment including chemistry, hematology, and coagulation parameters should be performed on Days -8, -1, 8, 23, and 28 only." while its two `X`+`r`
     cells sit on the ranged columns "-4 through -1" (col 7) and "26-28" (col 17); the un-footnoted
     `X`s on that row cover Days -8, 8 and 23. Consistent, but worth a reviewer's eye.
8. **No full-protocol markdown was supplied**, so no PDF/markdown text comparison was possible;
   all text comes from the PDF text layer, all structure from the PDF rule geometry.

---

## 8. Orphan risk

**None.** All 27 annotations (24 in Table 1, 3 in Table 2) carry ≥1 `marker_locations` entry, and
every location was verified programmatically to (a) point at an element that exists and (b) have
its marker present in that element's own `annotation_markers` string — the binding `resolve`
actually uses. No marker is referenced without a printed definition, and no footnote definition
is unused.

Marker a in Table 1 sits on a header cell merged across columns 2-3; it is recorded on **both**
covered `schedule_grid` positions. Per-column header markers (a on cols 2-3, b on col 4, c on
col 13, d on col 24, e on col 25) are on the grid cells, not on the `schedule_property` rows, so
each footnote keeps the column it governs.

## 9. Method provenance (every non-default method recorded)

| Where | Field | Value | Count |
|---|---|---|---|
| Table 1, header rows 1-2 | `schedule_property.structure_method` | `inferred_from_layout` | 2 |
| Table 1, all activities | `activity_name_source.indentation_method` | `font_signal` (bold + grey shading marks the 8 section headers as level 0; no leading whitespace exists in the text layer) | 44 |
| Table 1, arrow cells | `activity_schedule.method` | `visual_read` (vector arrows are absent from the text layer) | 40 |
| Table 2, both activities | `activity_name_source.indentation_method` | `assumed_flat` | 2 |
| Table 2, `note1` | `marker_locations[].method` | `synthesized` | 1 |

**`unresolved` marker locations: none.** No annotation target was guessed; every location is a
printed marker except the one synthesised `note1` anchor above.
