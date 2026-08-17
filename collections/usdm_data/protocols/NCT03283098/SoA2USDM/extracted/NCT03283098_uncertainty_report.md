# NCT03283098 — SoA extraction uncertainty report (prompt v3.7.3)

Source: `/root/ph3/blind/NCT03283098/NCT03283098_soa.pdf`, 5 PDF pages = document pages 32–36
(per PAGEMAP.md; the printed footers happen to agree here — "Page 32 of 73" … "Page 36 of 73" —
but the PAGEMAP numbering was used throughout regardless). No page in the excerpt is marked as
falling beyond the declared SoA range.

The excerpt contains **three** SoA tables, printed under one overall heading
"Table 1. Schedule of Assessments":

| file | source table | table_type | pages | label cols (L) | first data col | data cols | activities | marks | annotations |
|---|---|---|---|---|---|---|---|---|---|
| `NCT03283098_Table_01_extraction.json` | Table 1a. Schedule of Non-laboratory Assessments | `main_soa` | 32–33 | 1 | 2 | 20 (2–21) | 14 | 94 | 12 |
| `NCT03283098_Table_02_extraction.json` | Table 1b. Schedule of Laboratory Assessments | `domain` | 34–35 | 2 | 3 | 20 (3–22) | 14 | 65 | 14 |
| `NCT03283098_Table_03_extraction.json` | Table 1c. Schedule of Pharmacokinetic Assessments | `domain` | 36 | 2 | 3 | 20 (3–22) | 7 | 23 | 10 |

## 1. Per-table classification and page coverage

**Table 1a → `main_soa`.** Rows are procedures performed on subjects; it is the primary anchor grid
of the Table 1 set.

**Table 1b and Table 1c → `domain`.** All three tables carry the *same* 20 data columns — the day
row is identical token for token (Screening, -2, 1, 2, 3, 6, 8, 10, 13, 15, 17, 20, 22, 24, 27, 28,
29/ET, 34, 41, 55, with `(HD)` on the same 16 of them) — and they schedule the *same* participants;
only the assessment category changes (non-laboratory / laboratory / pharmacokinetic). No population
split is stated anywhere, so `track` does not apply, and neither table is a page split of another
(1a closes with its own bottom rule and its own footnote page, and 1b/1c each restate the whole
header), so `continuation` does not apply.

**The close call is 1c: `domain` vs `subsidiary`.** Table 1c breaks PK sampling into within-visit
timepoints ("Pre HD", "SDA + 10 min" … "SDA + 18 to 30 hr"), which reads like the PK-sampling
subsidiary case in prompt §2. It was classified `domain` because (a) its **column structure is not
finer** — the 20 columns are identical to Table 1a's, not per-hour columns — and (b) "PK" is not an
activity row of Table 1a or 1b that this table decomposes; the finer timing lives in a leading
row-label column, not in the grid. The call is recorded in `table_metadata.notes` of Table 3.

**Activity rows per page across the declared range**

- Table 1 (1a): page 32 → 14 rows; **page 33 → 0 rows**. Page 33 carries only the abbreviation line
  and footnotes a–i; page 32 ends with "Footnotes displayed on the next page". Expected, not a
  skipped page.
- Table 2 (1b): page 34 → 14 rows; **page 35 → 0 rows**. Same pattern: page 35 is the footnote
  block for 1b (footnotes a–g), page 34 ends with "Footnotes displayed on the next page".
- Table 3 (1c): page 36 → 7 rows. Single page; the abbreviation line is on the same page.

No horizontally tiled tables in this excerpt.

## 2. Structure decisions

**Header rows.** Each table has exactly **two** header bands, confirmed from the raster rule lines
(pdftoppm -r 200): a top band holding the merged banner `Study Visit (Day)`, and a second band
holding the day numbers. There is **no rule line between the day number and its `(HD)` line** — e.g.
"1" and "(HD)" occupy one cell — so each is transcribed as a single `cell_value` such as `1 (HD)`,
not split into a separate modality row. On document page 32 the banner/day rule is at y = 514 px and
the next rule is at y = 669 px (the top of the "Informed consent" row); the intervening band is one
cell tall.

**Banner distribution (merged header cell).** `Study Visit (Day)` spans the data columns only — the
label column(s) to its left form a separate tall cell (rule at y = 425 px absent inside the Time and
Assessment columns on pages 34 and 36, present in every data column). It is emitted as one
`schedule_grid` entry per covered column with `is_merged_cell: true` and
`merged_cell_range: "2:21"` (Table 1) / `"3:22"` (Tables 2 and 3).

**Label columns.** Table 1a has L = 1 (the activity-name column, headed "Assessment") → first data
column 2. Tables 1b and 1c have L = 2 ("Time" and "Assessment") → first data column 3. Data columns
were **not** renumbered.

**Vertically merged "Pre HD" cell (Table 1b).** The Time column on document page 34 has horizontal
rules only at y = 579, 613 and 1224 px — one single cell from 613 to 1224 spanning **all 13**
assessment rows — while the Assessment column beside it has 13 separate bands in the same interval.
So "Pre HD" applies to every laboratory assessment, not just the Hematology row it sits beside.

**The Time column is not a schedule column and not an activity.** Following prompt §6's notes-column
rule (a row-labelling column whose cells are notes bound to rows), each Time cell is emitted as one
`footnote` annotation with a synthesised marker and one `marker_location` per covered activity row:

- Table 2: `t1` = "Pre HD", 13 locations (rows 4–16), `method: "synthesized"` on each.
- Table 3: `t1`…`t6` = "Pre HD", "SDA + 10 min", "SDA + 30 min", "SDA + 60 min", "SDA + 90 min",
  "SDA + 18 to 30 hr", one location each (rows 4–9).

This is an interpretation, flagged here because the alternative — folding the Time text into
`activity_name` — would have composed labels the source never prints contiguously. **Consequence
worth a reviewer's eye:** in Table 3 every Assessment cell reads "PK", so all six body rows carry
`activity_name: "PK"` and are told apart only by `row_position` and by markers t1–t6.

**Section-header rows.** "Laboratory Assessments" (Table 2, row 3) and "Central Laboratory"
(Table 3, row 3) are full-width bands spanning the label columns and the whole grid; both are kept
as `indentation_level: 0` organisational rows carrying no marks, with the rows beneath them at
level 1. Table 1a is flat: all 14 rows at level 0.

## 3. Merged / distributed marks

**No body mark was distributed across a span** in any of the three tables — every `X` sits in a
single-column, single-row cell, confirmed against the rule-line geometry. `source_range` is
therefore unused in `activity_schedule`. The only distributions are the header banner (above) and
the vertically merged Time cell in Table 1b, which is carried as an annotation with 13 locations
rather than as grid values.

## 4. Mechanical mark-check

Method: §1b bbox binning, with the column and row boundaries taken from the **raster rule lines**
(§1d) rather than from header-token proximity — vertical rules are image columns with high ink
fraction over the table height, horizontal rules are image rows with >85% ink within one column's
x-range, converted to PDF points by px × 0.36. Every `pdftotext -bbox` token matching
`^[Xx][*a-zA-Z0-9,]?$` was binned to the cell its centre falls in.

Grid self-check: binning the *header* tokens with the same boundaries reproduces the printed day
sequence exactly (`(HD)` lands on the 16 columns that print it and on no others; "29/", "ET" and the
superscript "a" all land on the same column), which anchors the column grid independently of the
body marks.

Diff against the visual read of the rendered pages: **no disagreement in any of the 182 marks.**
Two rows were re-checked at 2× zoom because their right-hand tails are easy to misread — Albumin
(marks at 22, 27, 29/ET, 34, 41, 55) and Phosphorus (marks at 22, 27, 34, 41, 55; **no** mark at
29/ET) — and the crop confirms the mechanical matrix. A spot-check of the resolved grid for
Table 1b rows 6–8 is still recommended.

Cell-level markers found by the same pass: superscript `b` on the Day -2 cell of Albumin,
Phosphorus, Calcium (cCa), Serum or Urine Pregnancy and iPTH in Table 1b (transcribed as
`cell_value: "X"` + `annotation_markers: "b"`).

## 5. Synthesised names and markers

- `property_name` **"Study Day"** is synthesised in all three tables (row 2). The label cell(s) of
  that row are not empty — they hold the headings of the label columns ("Assessment" in Table 1a;
  "Time" and "Assessment" in Tables 1b/1c) — but those name columns, not this header row, so
  `property_name_source.synthesized` is `true` and the raw cell text is preserved in `cell_value`.
- Annotation markers `t1`–`t6` (Time-column cells, Tables 2 and 3) — synthesised, see §2.
- Annotation markers `ab1`–`ab6` (abbreviations) — synthesised, assigned **study-wide and stable**
  rather than per-table printed order, so that the same term keeps the same marker in every table:
  `ab1` = HD, `ab2` = ET, `ab3` = cCa, `ab4` = SDA, `ab5` = Kt/V, `ab6` = URR. (This is the printed
  order of the longest list, on document page 35; Table 1a prints ET before HD.)
- Annotation marker `cn1` (Tables 1 and 2) — synthesised for the typesetting note "Footnotes
  displayed on the next page", typed `continuation_note` and anchored table-scope to the row-1
  `schedule_property` with `method: "synthesized"`; per §6 it is deliberately **not** added to any
  element's `annotation_markers`.

## 6. Abbreviations kept and dropped

Abbreviation entries were kept **only** where the term is printed as a marker in a grid, header or
activity-label cell, and each is bound by a printed location (no `text_match`, no `synthesized`
abbreviation bindings):

- `ab1` HD — printed in 16 header cells of every table ("(HD)"), bound to all 16.
- `ab2` ET — printed in the "29/ET" header cell of every table.
- `ab3` cCa — printed in the Table 1b activity label "Calcium (cCa)".
- `ab5` Kt/V and `ab6` URR — printed in the Table 1b activity label "Kt/V or URR".

**Dropped:** `SDA` in **Table 1b**. The abbreviation line on document page 35 defines it
("SDA = Study drug administration") but the term appears nowhere in Table 1b — not in a header, a
label or a cell — so it would be an orphan list entry and was omitted per §6. It is defined and used
in Table 1c and is carried there.

## 7. Orphan risk and undefined markers

- **Table 3, marker `a`.** The 29/ET header of Table 1c prints a superscript `a`, but document
  page 36 prints only the abbreviation line — no footnote text. The marker is transcribed where it
  appears and `annotation_text` states plainly that the definition is not printed in the source; the
  Table 1b footnote a is quoted inside it as a clearly-labelled *probable* equivalent, never asserted
  as this table's content.
- **Table 3, `ab4` (SDA).** The term is printed only in the Time row-label column, which has no
  modelled element to hang a marker on. Rather than invent a target, the location is recorded as
  `location_type: "unresolved"` with `row_position: 5` (the "SDA + 10 min" row, where the term first
  prints). It is consequently the one annotation with no matching `annotation_markers` string.
- No other annotation has an unplaced location; every annotation in all three files has ≥ 1
  `marker_locations` entry, and every non-unresolved, non-table-scope location has its marker echoed
  in the target row's `annotation_markers` (checked mechanically).

## 8. Annotation text integrity

- The text layer is **not** glyph-spread; no `deglyph_reconstruction` was needed. Footnote text was
  read from the text layer of the footnote pages (32→33, 34→35) and is complete from first word to
  last word of each note.
- **Source artifact, transcribed not repaired:** the Table 1b abbreviation line uses a full-width
  ideographic semicolon between two definitions — the text layer reads
  "SDA = Study drug administration；Kt/V = measure of dialysis adequacy;". The abbreviation
  annotations were split on it as printed; the character is not "corrected".
- **Source artifact:** the "Time" header cell of Table 1b tokenises as "TTime" in the text layer —
  a duplicated first glyph sitting on/left of the table's left rule; the page renders "Time". Since
  this is a label-column heading it does not reach the JSON, but it is noted so a reviewer who greps
  the text layer is not surprised. (Table 1c's equivalent cell tokenises cleanly as "Time".)
- **Source artifact:** the Table 1b activity label "Serum" … "Pregnancy (females only)" prints a
  double space between "Serum" and "or" (token gap ~5 pt vs ~2.5 pt elsewhere). Preserved verbatim
  in `activity_name_source.cell_text`, normalised to a single space in `activity_name`.
- **Source artifact:** the Table 1a label "Vital Signs (BP, HR, RR, TEMP)" starts 2.6 pt right of
  every other label. Preserved as a leading space in `cell_text`; read as a typo, **not** as an
  indentation level (Table 1a is flat).
- **Containment check.** No annotation's text is contained in another's within the same table. Two
  near-duplicates exist *across* tables and are source-faithful, printed on two different pages:
  Table 1a footnote a ("… to perform Day 29 assessments …") and Table 1b footnote a ("… to obtain
  day 29 samples …") differ in wording and each belongs to its own table. The one deliberate
  containment is in Table 3's marker-`a` annotation, which quotes Table 1b footnote a as a labelled
  probable equivalent (§7) — that is an extraction-authored cross-reference, not a split note cell.

## 9. Low-confidence calls (please review)

1. **`property_type: "other"` for the banner row** (`Study Visit (Day)`, row 1 of every table). The
   row is a single merged title over all 20 columns and carries no per-column value; `visit` and
   `epoch` were both rejected because either would manufacture one visit/epoch spanning the entire
   study including screening. `hierarchical_level` was set to **1** (topmost header row) with the
   day row at 2, even though the banner does not by itself distinguish columns; the alternative
   reading of §3 would make it `null`.
2. **1c as `domain` rather than `subsidiary`** — see §1.
3. **The Time column → annotations decision** (§2), and its consequence that Table 3 has six
   activities all named "PK".
4. **"Central Laboratory" as an activity row.** It is a full-width band naming where the samples are
   analysed rather than a procedure; it is kept as a level-0 organisational header (mark-free) so the
   six PK rows have a parent, consistent with "Laboratory Assessments" in Table 1b.
5. **`continuation_note` for "Footnotes displayed on the next page"** — a typesetting instruction
   rather than study logic; kept for completeness with a table-scope synthesised anchor. Safe to
   drop downstream if unwanted.
6. No full-protocol markdown was supplied, so there is no PDF/markdown text disagreement to report.

## 10. Method provenance recorded in the data (§1e)

| file | element | field | value | why |
|---|---|---|---|---|
| Table 01 | all 14 activities | `activity_name_source.indentation_method` | `assumed_flat` | flat table, no hierarchy evidence in whitespace or layout |
| Table 02 | all 14 activities | `activity_name_source.indentation_method` | `visual_estimate` | levels come from the full-width band row geometry, not from leading whitespace (child labels sit in column 2 for column reasons, not indentation) |
| Table 03 | all 7 activities | `activity_name_source.indentation_method` | `visual_estimate` | same |
| Tables 01–03 | row-2 `schedule_property` | `property_name_source.synthesized` | `true` | label cell holds the activity-column heading, not a row name |
| Table 02 | annotation `t1` | `annotation_text_source.method` | `raster_band_cells` | Time-column cell extent taken from the raster rule lines, not proximity |
| Table 03 | annotations `t1`–`t6` | `annotation_text_source.method` | `raster_band_cells` | same |
| Table 02 | `t1` locations (13) | `marker_locations[].method` | `synthesized` | the source prints no marker on the Time cell |
| Table 03 | `t1`–`t6` locations | `marker_locations[].method` | `synthesized` | same |
| Tables 01–02 | `cn1` location | `marker_locations[].method` | `synthesized` | table-scope note with no printed marker |
| Table 03 | `ab4` (SDA) location | `location_type` | `unresolved` | term printed only in an unmodelled label column; no target invented |

No `proximity_bounded`, `proximity`, `text_match`, `visual_transcription`, `glyph_reconstruction`
or `raster_pixel_detection` values were used anywhere: the text layer was usable throughout and all
cell boundaries came from rule-line geometry.
