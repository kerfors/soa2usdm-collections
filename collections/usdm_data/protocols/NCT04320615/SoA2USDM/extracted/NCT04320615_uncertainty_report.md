# NCT04320615 — SoA extraction uncertainty report

Source: `NCT04320615_soa.pdf` (9 PDF pages = document pages 77–85 per PAGEMAP.md).
Protocol WA42380, Version 3 (Tocilizumab — F. Hoffmann-La Roche Ltd). Landscape letter pages,
clean vector text layer plus vector rule lines on every page. Deliverables: three table JSONs.

---

## 1. Structure decision — how many tables

The excerpt contains three appendices, each a Schedule of Activities with its own column
structure, and each split over a title page plus one or more "(Cont.)" pages:

| Table | Appendix | Document pages | Grid pages | Footnote-only pages |
|---|---|---|---|---|
| 01 | Appendix 1 — Days 1 and 2 | 77–80 | 77, 78 | 79, 80 |
| 02 | Appendix 2 — Days 3−28 | 81–83 | 81, 82 | 83 |
| 03 | Appendix 3 — After Day 28 | 84–85 | 84 | 85 |

**Continuation pages were folded into their parent table rather than emitted as separate
`continuation` tables.** On document page 78 the whole three-row header block of Appendix 1 is
reprinted verbatim and the body rows simply continue; likewise the two-row header of Appendix 2
is reprinted on page 82 above its single remaining body row. The prompt's §1b de-duplication rule — reprinted header and
`schedule_property` rows are to be counted once per table — presupposes one extraction per
logical table, and folding keeps
each appendix's footnote block inside the same file as the markers it defines — footnotes c–k of
Appendix 1 are printed on pages 79–80 but their markers sit on page 77. Splitting would have left
Table 01 with markers whose text lived in another file. This is the main judgement call in the
run; if the corpus convention is one file per physical page block, Table 01 splits at page 78 and
Table 02 at page 82, and nothing else in the extraction changes.

**All three are `main_soa`.** Applying the discriminators explicitly:

* *reference test* — every row is a procedure performed on subjects, so none is `reference`.
* *continuation* — rejected between appendices: the column structures differ completely
  (5 columns Screening/Day 1/Day 2 vs 26 day columns + completion vs Days 35/45/60).
* *domain* — rejected for the same reason: `domain` requires the **same** columns as the parent.
* *subsidiary* — rejected: none of the three gives finer timing for a single activity of another;
  they are consecutive stretches of one timeline, not a zoom on one row.
* *track* — rejected: the population question has one answer here. All three appendices schedule
  the same randomized patients moving forward through one continuous timeline (screening →
  Day 60); there is no responder/non-responder or arm split, and no separate phase with its own
  entry criteria. The taxonomy directs that where multiple independent schedules exist with
  different column structures, each is classified as `main_soa`. Consequently no `track_label` is set on
  any table. This is a genuine classification call and it is recorded in each file's
  `table_metadata.notes`.

---

## 2. Per-table summary

### Table 01 — Appendix 1, Schedule of Activities: Days 1 and 2 (pages 77–80)

* `table_type`: `main_soa`. Label columns **L = 1**; **first data column = 2**; 5 data columns
  (positions 2–6). 3 header rows (positions 1–3), 28 activities (positions 4–31), 56 schedule
  cells, 21 annotations.
* Activity rows per page: **page 77 → 17 rows** (positions 4–20), **page 78 → 11 rows**
  (positions 21–31), **page 79 → 0**, **page 80 → 0**. Pages 79 and 80 carry nothing but the
  continuation of the footnote block (footnotes b–m on 79, n–t on 80) — declared, not skipped.
* Column meaning: 2 = Screening (Day −2 to 0); 3 = Baseline / Day 1 / "0 Pre-dose (−4 hrs)";
  4 = Day 1 / "15 min After end of infusion (+1 hr)"; 5 = Day 2 / "24 hrs (±4 hrs)";
  6 = Day 2 / "36 hrs (±4 hrs)".

### Table 02 — Appendix 2, Schedule of Activities: Days 3−28 (pages 81–83)

* `table_type`: `main_soa`. **L = 1**, **first data column = 2**; 27 data columns (positions
  2–28): Day 3 → 2 … Day 28 → 27, Study Completion/Discontinuation → 28. 2 header rows,
  17 activities (positions 3–19), 220 schedule cells, 12 annotations.
* Activity rows per page: **page 81 → 16 rows** (positions 3–18), **page 82 → 1 row**
  (position 19, "Whole blood in PAXgene® tubes for RNA analyses"), **page 83 → 0** (footnotes
  g–k only).

### Table 03 — Appendix 3, Schedule of Activities: After Day 28 (pages 84–85)

* `table_type`: `main_soa`. **L = 1**, **first data column = 2**; 3 data columns (positions 2–4):
  Day 35, Day 45, Day 60/Study Completion. 2 header rows, 14 activities (positions 3–16),
  32 schedule cells, 8 annotations.
* Activity rows per page: **page 84 → 14 rows**, **page 85 → 0**. Page 85 is the footnote
  continuation page (footnotes b–h) and contributes no rows — declared, not skipped.

---

## 3. Mechanical mark-check

Method: **bbox column-binning (§1b)** as the primary check, cross-checked against a **200-dpi
raster rule-line scan (§1d)** for every merged span, and against a visual read of the 150-dpi
render of each grid page.

* `pdftotext -bbox` was run on PDF pages 1, 2, 5, 6 and 8 (document pages 77, 78, 81, 82, 84).
  Column x-centres were fixed from the header labels only (the day numbers 3…28 on page 81; the
  timepoint band on pages 77–78; the day-35/45/60 band on page 84), and
  every mark token was binned to the nearest centre. All marks in this document tokenise as a
  bare lowercase `x`; footnoted marks tokenise as a separate superscript token (`x` then `o`,
  `x` then `q` on the Serum PD / Serum PK rows of page 78) and were matched as such.
* Vertical rule positions per row band were recovered from the 200-dpi render by taking image
  columns whose ink fraction exceeded 0.8 across the band. Page 77 data-column boundaries came
  out at x ≈ 329.0 | 396.7 | 466.9 | 536.0 | 608.0 | 675.7 pt, page 81 at 182.9 | … | 664.2 |
  736.2 pt, page 84 at 403.9 | 491.0 | 579.2 | 664.6 pt — matching the header-derived centres.
* **No disagreement was found between the mechanical matrix and the visual read**, on any cell of
  any of the three tables. The marks are `x` (lower case) throughout; nothing was normalised.
* Repeated header rows on the continuation pages (78 and 82) were de-duplicated: each header row
  is emitted once, each activity once, each mark once.
* Recommended spot-check for a reviewer: the two dense all-column rows of Table 02
  (Vital signs, SpO2, Ordinal scoring, Adverse events, Concomitant medications — 26 day marks +
  1 completion mark each) and the sparse Chest X-ray/CT scan row (Days 7, 14, 21, 28 + completion).

---

## 4. Merged marks and merged cells — every distribution decision

All merge extents below were confirmed from rule-line geometry (a missing internal vertical rule
between two adjacent column centres), never from where the glyph sits.

**Table 01**
* Activity row 15, `PaO2/FiO2`: the cell to the right of the Screening column carries no internal
  vertical rules between x = 396.7 and x = 675.7 pt. The printed content `← Optional →` was
  distributed to **columns 3, 4, 5 and 6**, each with `source_range: "3:6"`. The glyph cluster
  sits at x ≈ 502–570 pt, i.e. over columns 4/5 — collapsing it there would have fabricated a
  two-visit schedule.
* Header row 1: the cell right of "Baseline" is empty and spans columns 4–6
  (`merged_cell_range: "4:6"`, emitted on all three positions).
* Header row 2 (`Study Day`): the value `1` spans columns 3–4 (`source_range` / `merged_cell_range` `"3:4"`), the value `2` spans columns 5–6 (`"5:6"`);
  each value emitted on both covered positions.

**Table 02**
* Header row 1: `Days 3−28` spans **columns 2–27** (`merged_cell_range: "2:27"`), emitted on all
  26 positions, each carrying `annotation_markers: "a"`. `Study Completion/Discontinuation`
  occupies column 28 alone and is additionally merged *vertically* into the Study Day row — the
  schema has no vertical header merge, so it is recorded once on header row 1 and the Study Day
  cell at column 28 is emitted empty. Flagged as an interpretation.
* Activity row 5, `PaO2/FiO2`: `← Optional →` distributed to **columns 2–27**
  (`source_range: "2:27"`, 26 entries); a separate, unmerged `Optional` sits in column 28 and is
  emitted as its own cell with no `source_range`.

**Table 03**
* Header row 1: the empty cell over the Day 35 and Day 45 columns spans **columns 2–3**
  (`merged_cell_range: "2:3"`); `Study Completion` occupies column 4 alone.
* No merged marks in the body.

**Cell values kept literal.** The Optional cells are transcribed as printed, arrows included
(`← Optional →`), rather than reduced to the word "Optional"; a reviewer who prefers the bare word
can strip the arrows downstream, but inventing a normalisation here would have been inference.

---

## 5. Synthesised values

**Synthesised `property_name` (3, one per table)** — in all three tables the header rows' own
label cell is blank on the top band:

* Table 01, header row 1 → `"Study Period"` (`synthesized: true`, `structure_method:
  inferred_from_layout`). The row carries "Screening" and "Baseline".
* Table 02, header row 1 → `"Study Period"` (same flags). The row carries "Days 3−28" and
  "Study Completion/Discontinuation".
* Table 03, header row 1 → `"Visit"` (same flags). The only printed value is "Study Completion",
  which is a named encounter rather than a phase, hence a different synthesised name and
  `property_type: visit` rather than `epoch`.

**Synthesised annotation markers (2)** — the unmarked table-scope "Note:" lines:

* Table 01, marker `tn1`: "Note: On treatment days, all assessments should be performed prior to
  dosing, unless otherwise specified."
* Table 02, marker `tn1`: "Note: For patients who have been discharged, all assessments should be
  performed within ±3 days of the scheduled onsite visit."

Both are printed under the grid with no marker and apply to the whole table, so each is anchored
to header row 1 with a single `schedule_property` `marker_location` carrying
`method: "synthesized"`, and — per §6's table-scope rule — the marker is deliberately **not**
written into any element's `annotation_markers`. Table 03 has no such Note line.

No other markers were synthesised: every remaining annotation is a printed superscript letter.
Table 03 has no inline section/appendix references in any activity label, so no `pr*` source-notes
were created in any table.

---

## 6. Annotation text integrity

* **The text layer is not glyph-spread.** Words come back whole; no word re-segmentation was
  needed and no `deglyph_reconstruction` was recorded.
* **Symbol-font private-use code points were mapped to Unicode.** The document sets `−`, `+`, `±`,
  `←`, `→` and `®` in a Symbol font, so `pdftotext` returns them as private-use characters
  (`U+F02D`, `U+F02B`, `U+F0B1`, `U+F0DF`, `U+F0E0`, `U+F0E2`). Each was mapped to its Unicode
  equivalent and the mapping was confirmed against the 150-dpi render: `U+F02D → −` (U+2212),
  `U+F02B → +`, `U+F0B1 → ±`, `U+F0DF → ←`, `U+F0E0 → →`, `U+F0E2 → ®`. This affects the header
  cells ("−2 to 0", "(−4 hrs)", "(+1 hr)", "(±4 hrs)", "(±3 days)", "Days 3−28"), the two
  `← Optional →` cells, two activity names containing "PAXgene®", and four annotation texts
  (Table 01 footnotes `o`, `q` and `t`; Table 02 `tn1` and `k`). Because the characters *are* in
  the text layer (just in a private-use range), this is a character mapping rather than a change
  of read method, so no `annotation_text_source.method` was recorded. The en dash in "8–24 hours"
  (Table 01 footnote `n`) is a real U+2013 in the text layer and was left alone.
* **Note-cell bounding is not at issue here:** there is no right-hand Notes/Instructions column in
  any of the three tables. Every annotation is a numbered footnote paragraph in the footnote block
  below the grid, bounded by its own marker; none was bounded by proximity, so no
  `proximity_bounded` methods appear anywhere in the extraction.
* **Containment / overlap pairs.** No containment pair exists *within* any single file. Two
  cross-table pairs exist and were re-verified against the pages; **both are source-faithful**,
  not a split note cell:
  * Table 01 footnote `h` (page 79) is a strict prefix of Table 02 footnote `b` (page 82).
    Appendix 2 reprints the same vital-signs note and appends two sentences that only make sense
    after discharge: "…Following hospital discharge these parameters should be recorded once at
    each return visit to the clinic. Vital signs and oxygen saturation will not be recorded if
    follow-up visits are conducted by telephone." Confirmed by reading both pages.
  * Table 01 footnote `l`/`m` (page 79) versus Table 03 footnote `g`/`h` (page 85): the Appendix 3
    versions append "Hematology labs will not be performed if follow-up visits are conducted by
    telephone." / "Chemistry labs will not be performed if follow-up visits are conducted by
    telephone." Again a deliberate reprint-plus-extension, verified on both pages.
  Nothing was merged, truncated or dropped to remove these pairs.
* **Abbreviation blocks deliberately dropped.** Pages 78, 82 and 84 each carry an abbreviation
  list ("CRP = c-reactive protein; CT = computed tomography; …"). None of those terms appears as a
  *marker* on a grid cell, header cell or activity label — they only occur inside running text and
  inside activity names such as "Serum PD (CRP, IL-6, sIL-6R)". Per §6 these would be orphan
  standalone-list entries bound only by word overlap, so **zero `abbreviation` annotations** were
  emitted for any table.
* **Source defects transcribed, not repaired.** "Patents" for "Patients" appears twice in
  Table 01 (footnotes `o` and `p`) and is kept verbatim. The page-82 abbreviation list ends
  "saturation.." with a doubled period and adds "BAL = bronchoalveolar lavage" which the page-78
  list omits — recorded here only, since the abbreviation lists are not emitted. The page-85
  running head reads "After Days 28" where page 84 reads "After Day 28"; the `table_title` follows
  the page-84 title page and the discrepancy is noted in `table_metadata.notes` of Table 03.
  Table 03's antibody row is spelled "Serum SARS-Cov-2 antibody titer" (lower-case v) where
  Tables 01 and 02 print "SARS-CoV-2"; the Table 03 spelling is transcribed as printed.

---

## 7. Low-confidence calls

1. **`main_soa` × 3 versus `track` for Appendices 2 and 3** (§1 above). The decision tree read
   literally would send any non-anchor table with different columns and no finer granularity to
   `track`; the `main_soa` definition covering multiple independent schedules, together with the
   absence of any population split, sends it to `main_soa`. I took the second
   reading because the population question — who attends — has the same answer for all three
   tables. If the corpus convention treats consecutive study phases as tracks, Table 02 would take a
   `track_label` of Days 3−28 and Table 03 one of After Day 28.
2. **Folding "(Cont.)" pages into the parent table** rather than emitting `continuation` tables
   (§1 above).
3. **Table 03 header row 2 `property_type`.** The label cell reads "Study Day (Assessment Window)"
   and each cell contains both a day number and a window ("35" over "(±3 days)"). Classified
   `study_day`, with the window kept inside `cell_value` because the source gives it no row of its
   own. A `window` property row could equally have been synthesised; I did not, to avoid inventing
   structure. The same pattern in Table 01 header row 3 ("Time Post Initial Treatment (Assessment
   Window)") is classified `timepoint` for the same reason.
4. **Table 02 header row 1 `property_type: epoch`.** The row mixes a phase label ("Days 3−28",
   spanning 26 columns) with a visit name ("Study Completion/Discontinuation", one column).
   `epoch` fits the spanning band; the completion column's visit identity is carried by the cell
   value rather than by a separate visit row.
5. **Indentation hierarchy.** Activity labels are all set flush at the same x, so whitespace gives
   no hierarchy. "Central Labs" is bold and underlined (Tables 01, 02) or bold (Table 03) and its
   row is merged across the full table width with no marks — read as a section header at level 0
   (`indentation_method: "font_signal"`). The rows that follow it to the end of each table are set
   to level 1 (`indentation_method: "visual_estimate"`), which is an inference from position under
   the band rather than from any printed indent. All other activity rows are level 0 from the
   text layer's own flush-left geometry (no method recorded).
6. **"← Optional →" as `cell_value`** (§4 above) — literal transcription chosen over the
   normalised word.
7. No full-protocol markdown was supplied, so there are no PDF/markdown text disagreements to
   report; every text field comes from the PDF text layer.

---

## 8. Orphan risk

* **None.** Every annotation in all three files has at least one `marker_location`, and every
  location's marker also appears in that row's / cell's `annotation_markers` — with the single
  intentional exception of the two synthesised `tn1` table-scope notes, where §6 explicitly
  directs that the marker not be written onto any element.
* Every marker printed in the three grids has its definition printed within the same appendix, so
  no marker was left with its definition missing from the source and no probable cross-references were composed.
* Header-cell footnotes were attached to the exact columns they sit on, not to the property row:
  Table 01 `a` and `b` → header row 1, column 2 (the Screening cell); Table 02 `a` → header row 1,
  columns 2–27 (all 26 columns of the merged "Days 3−28" cell); Table 03 `a` → header row 2,
  columns 2 and 3 (the Day 35 and Day 45 cells only — Day 60 carries no marker).
* Cell-level footnotes: Table 01 `o` on the Serum PD marks at columns 3 and 4, `q` on the Serum PK
  marks at columns 3 and 4. The markers are cleaned out of `cell_value` (each is plain `"x"`).

---

## 9. Method provenance — every non-default value recorded

| File | Element | Field | Value | Why |
|---|---|---|---|---|
| Table 01 | header row 1 | `schedule_property.structure_method` | `inferred_from_layout` | label cell blank; type/level from spanning geometry and row order |
| Table 02 | header row 1 | `schedule_property.structure_method` | `inferred_from_layout` | same |
| Table 03 | header row 1 | `schedule_property.structure_method` | `inferred_from_layout` | same |
| Table 01 | activity 24 (`Central Labs`) | `activity_name_source.indentation_method` | `font_signal` | level 0 from bold + underline, not whitespace |
| Table 02 | activity 12 (`Central Labs`) | same | `font_signal` | same |
| Table 03 | activity 12 (`Central Labs`) | same | `font_signal` | bold only in this appendix |
| Table 01 | activities 25–31 | same | `visual_estimate` | level 1 inferred from position under the Central Labs band |
| Table 02 | activities 13–19 | same | `visual_estimate` | same |
| Table 03 | activities 13–16 | same | `visual_estimate` | same |
| Tables 01–03 | the two `tn1` notes | `marker_locations[].method` | `synthesized` | unmarked table-scope Note lines anchored to header row 1 |

No `unresolved` marker locations were needed: every printed marker's target is determinable.
No `annotation_text_source.method`, no `activity_name_source.method`, no `schedule_grid`/
`activity_schedule` `method` values were recorded — all cell values and all annotation texts come
from the vector text layer bounded by vector rule lines, which is the schema's default method.
