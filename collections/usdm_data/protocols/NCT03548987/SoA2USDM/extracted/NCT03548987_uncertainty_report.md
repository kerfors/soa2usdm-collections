# NCT03548987 — SoA extraction uncertainty report

Source: `/root/ph3/blind/NCT03548987/NCT03548987_soa.pdf` (4 PDF pages = document pages 8–11 per PAGEMAP).
Prompt v3.7.2, schema `soa-table-extraction` v1.0. One output file:
`NCT03548987_Table_01_extraction.json`.

---

## 0. Rotation — how it was established and handled

`pdfinfo` reports `Page rot: 0` and a portrait letter page (612 × 792 pt), but the flowchart is
printed rotated: in `pdftotext -bbox` every word box is ~9.5 pt wide and tens of points tall, and
successive words of one line share an `xMin` while their `y` values *decrease*. Rotation was
therefore fixed from the geometry, not assumed:

- **one table row = one band of PDF x** (row order = increasing x, top of table to bottom);
- **one table column = one band of PDF y** (column order = decreasing y, left of table to right);
- a rendered page must be turned by `Image.ROTATE_270` to read.

Consequently the §1d rule-line recipe is transposed: on the 200 dpi render, **vertical image rules
are the table's row boundaries** and **horizontal image rules are the table's column boundaries**.
Both families were detected as pixel runs with ink fraction > 0.4 and converted to points by
dividing by 200/72. The 27 column boundaries came out byte-identical on all four pages
(lowest 63.9 pt, highest 709.4 pt), which is itself a check that one grid runs across the whole excerpt.
Every text-layer token was then assigned to the (row band × column band) cell its centre falls in.
No text was read by eye; the rendered pages were used only to confirm the recovered bands and the
merge spans.

---

## 1. Table inventory and classification

| # | type | pages (doc) | label cols L | first data col | data cols | header rows | activities | marks | annotations |
|---|------|-------------|--------------|----------------|-----------|-------------|-----------|-------|-------------|
| 1 | `main_soa` | 8–11 | 1 | 2 | 25 (positions 2–26) | 4 | 69 | 378 | 33 |

**One table, not four.** The excerpt carries a single continuous flowchart introduced as
"Flowchart" (section 2). It has no table number and no per-page title; the four-row header band
("Visit(V), Phone (P)", "Timing of Visit (Weeks)", "Visit Window (Days)" plus the unlabelled study-period
band) simply reprints at the top of each page while the activity rows continue. It is therefore
modelled as one table with `page_start` 8 / `page_end` 11, and the reprinted header rows are
de-duplicated to `schedule_property` rows 1–4 (§1b). The alternative — one `main_soa` plus three
`continuation` tables — was rejected because the source gives no independent table identity to
split on.

**Why `main_soa`:** rows are procedures performed on subjects (so not `reference`); it is the only
grid in the excerpt, so there is no parent to be a `continuation`/`domain` of and no second column
structure to be `subsidiary` to; and it schedules one undivided population across
Screening → Run-in → Randomisation → Maintenance → End of treatment / End of trial, so there is no
mutually-exclusive population split that would make it a `track`. `track_label` is correctly absent.

**Activity rows per page (whole declared range covered — no page contributed zero):**

| doc page | PDF page | activity rows |
|---|---|---|
| 8 | 1 | 17 |
| 9 | 2 | 19 |
| 10 | 3 | 19 |
| 11 | 4 | 14 |

Page 11 also carries the four footnote definitions below the grid; those became annotations, not rows.
The table is not horizontally tiled — all 25 data columns appear on every page — so the §4 tiling
exception does not apply here.

---

## 2. Column model

`L = 1` leading label column (the activity-name / property-label column), so **the first data
column is position 2** and the 25 data columns occupy positions 2–26. There is no notes/comments
column and no protocol-reference column: the section references are printed inline inside the
activity labels (see §5 below).

Column identity, for the record: 2 = V1 (week −1), 3 = V2 (0), 4 = P3 (2) … 13 = V12 (20, randomisation),
14 = V13 (24) … 24 = P23 (64), 25 = V24 (68, end of treatment), 26 = V25 (75, end of trial).

**`V20P21V22P23` — one text token, four columns.** In the visit row, poppler returns the four
labels for weeks 52/56/60/64 as the single token "V20P21V22P23" spanning y 137–194 pt, because the
glyphs abut with no space. The rule-line scan of that row band shows *all* internal column
boundaries present (137.5 / 151.2 / 165.6 / 179.3 / 194.0), i.e. four separate cells, and the week
row prints 52, 56, 60, 64 as four separate tokens in exactly those bands. The token was therefore
split in printed order into V20 → col 21, P21 → col 22, V22 → col 23, P23 → col 24, and confirmed
against the rendered page. This is a token-splitting decision, not a merge decision — flagged here
because no count-based check can see it.

**`Rando-` / `misation`.** The randomisation epoch cell is printed on two lines with a soft hyphen;
`cell_value` is de-hyphenated to `Randomisation`. Same treatment for the wrapped activity label
"Stanford Presenteeism Scale (SPS-" + "6)" → `Stanford Presenteeism Scale (SPS-6)`.

---

## 3. Merged cells and merged marks

Merge spans were resolved per row from the rule-line geometry, by testing each row band for the
presence of every one of the 26 column boundaries.

- **Only the study-period (epoch) header row has merges.** Missing internal boundaries there give:
  - `Run-in` → merged across columns **3:12** (V2 … P11)
  - `Maintenance period` → merged across columns **14:24** (V13 … P23)
  - `Screening` (col 2), `Randomisation` (col 13), `End of treatment` (col 25) and `End of trial`
    (col 26) are single cells — `Screening` in particular covers **V1 only**, not V1+V2.
  Each covered position carries `is_merged_cell: true` and `merged_cell_range` in numeric
  `"first:last"` form.
- **No body row has a merged mark.** All 69 activity rows returned the complete set of 25 internal
  column boundaries, so every `X` sits in a single-column cell. No `source_range` is set anywhere in
  `activity_schedule`, and no mark was distributed. There are no arrows, no vertically-merged marks,
  no qualified marks and no spanning text cells of the see-instructions kind in this table.

---

## 4. Mechanical mark-check

Two independent passes over the same recovered cell rectangles:

1. **bbox column-binning (§1b, primary).** `pdftotext -bbox` tokens matched against
   `^[Xx][*a-zA-Z0-9]?$` and assigned to the cell their centre falls in.
2. **raster near-black-pixel count (independent check).** On the 200 dpi render, a cell was called
   marked when it contained more than 20 pixels below intensity 90, using an absolute count, not a
   fraction.

**Result: 69 body rows compared, 378 marks, zero disagreeing cells.** No cell needed a visual
tie-break. Every mark in this table is a literal capital `X`; no `x`, `✓`, `•` or arrow occurs, and
no mark carries an annotation marker, so `annotation_markers` is empty on every `activity_schedule`
entry.

Spot-checks read directly off the rendered pages and agreeing with both passes:
`First date on trial product` carries its single X under **P3 (col 4)**, not under V2;
`Criteria for discontinuation (8.1)` runs V2 → P23 (cols 3–24) and stops before V24/V25;
`Trial product compliance (7.1, 7.6)` starts at P3 (col 4), one column later than
`Concomitant medication (7.7)` which starts at V1 (col 2).

---

## 5. Annotations

33 annotations: **4 `footnote`** (the printed a/b/c/d block on document page 11) and
**29 `source_note`** (deduplicated inline protocol cross-references).

**Footnotes** — text taken from the text layer of page 11, each bound to the activity row(s) whose
label carries the printed superscript:

| marker | rows | text |
|---|---|---|
| a | 6 | "Demography consists of date of birth, sex, ethnicity, and race (according to local regulation)" |
| b | 7, 15, 49 | "For all female subjects" |
| c | 11 | "If subjects not fulfil randomisation criteria see Section 6.3.2" |
| d | 19 | "Smoking is defined as smoking at least one cigarette or equivalent daily" |

Marker b binds three rows: `Childbearing potential`, `History of Breast Neoplasm` and
`Breast neoplasms follow-up`. Superscript position was taken from the glyph's own y-offset inside
the label cell (e.g. in "Breast neoplasms follow-up (9.4)" the `b` glyph sits between "follow-up"
and "(9.4)", so `cell_text` is `Breast neoplasms follow-upb (9.4)`).

Footnote c is typed `footnote`, not `source_note`, because it states a condition as well as
pointing ("If subjects not fulfil randomisation criteria see …").

**Source notes — synthesised markers `pr1` … `pr29`.** Almost every activity label in this protocol
ends in an inline, hyperlinked protocol reference — "Inclusion criteria (6.1)",
"Vital Signs (6.4.2, 9.4.3)", "Pregnancy test (9.4.5, Appendix 5)", "Trial product compliance (7.1, 7.6)".
Per §6 these were stripped out of `activity_name` (retained verbatim in
`activity_name_source.cell_text`), split when one parenthesis carries several references, and
emitted as one `source_note` per **distinct** reference with a synthesised marker added to every
citing row's `annotation_markers`. `annotation_text` is the reference exactly as printed inside the
parentheses, without the parentheses, so that deduplication is exact: `6.1`, `9.4`, `Appendix 2`, …

Marker order is printed order of first occurrence: pr1 = Appendix 3, pr2 = Appendix 5, pr3 = 6.1,
pr4 = 6.2, pr5 = 6.3.1, pr6 = 6.3.2, pr7 = 9.4, pr8 = 9, pr9 = 7.7, pr10 = 7.1, pr11 = 7.6,
pr12 = 9.1.1, pr13 = Appendix 2, pr14 = 6.4.2, pr15 = 9.4.3, pr16 = 9.1.2, pr17 = 9.4.2,
pr18 = 9.4.5, pr19 = 9.4.4, pr20 = 9.2, pr21 = Appendix 4, pr22 = 9.2.9, pr23 = Appendix 6,
pr24 = 9.4.1, pr25 = 7.5, pr26 = 8.1, pr27 = 7.1.2, pr28 = 7.1.1, pr29 = 6.4.1.
The most-cited are pr7 (`9.4`, 8 rows), pr8 (`9`, 6 rows), pr10 (`7.1`, 3 rows) and pr13
(`Appendix 2`, 3 rows).

Parentheticals that are **instrument acronyms, not references**, were deliberately kept inside
`activity_name`: `Short Form-36 (SF-36)`, `Weight Related Sign and Symptom Measure (WRSSM)`,
`Patient Global Impression of Severity (PGI-S)`, `Patient Global Impression of Change (PGI-C)`,
`Stanford Presenteeism Scale (SPS-6)`, `Patient Health Questionnaire – 9 (PHQ-9)`,
`Columbia Suicide Severity Rating Scale (C-SSRS)`.

**No `legend` and no `abbreviation` annotations.** The table prints no legend defining `X` and no
abbreviation block, so there is nothing to emit; per §6 no abbreviation was manufactured from terms
occurring only in running text.

**`by_type` distribution** is 29 `source_note` / 4 `footnote` — not degenerate, but heavily skewed to
`source_note`. That is what the source is: this protocol hyperlinks a section number into nearly
every activity label, and there is no notes/comments column to yield footnotes.

**Containment pairs (§8).** 19 pairs where one `annotation_text` is a substring of another were
found and re-verified against the pages. **All are source-faithful, none is a split note cell:**
they are the trivial prefix relations between genuinely distinct section numbers (`9` ⊂ `9.4` ⊂ `9.4.3`,
`7.1` ⊂ `7.1.1`, `9.2` ⊂ `9.2.9`, …) plus `6.3.2` ⊂ the footnote c sentence
"If subjects not fulfil randomisation criteria see Section 6.3.2", where footnote c is a printed
footnote and `6.3.2` is the separate inline reference on the "Randomisation criteria and randomisation"
label. Nothing was merged, truncated or dropped.

**Orphan risk: none.** All 33 annotations have ≥ 1 `marker_locations` entry, every location is an
`activity_name` on a row that also carries that marker in its `annotation_markers`, and no marker is
`unresolved`. No marker appears in the grid whose definition is missing from the source.

---

## 6. Synthesised values

- **`property_name` for header row 1** — the study-period band's label cell is empty in the source
  (the leftmost column of that row is blank), while the row clearly carries period names spanning
  columns. Synthesised as `Study period`, with
  `property_name_source.cell_value: ""` and `synthesized: true`.
- **Annotation markers pr1–pr29** — the inline references carry no printed marker; markers were
  synthesised per §6 and each location carries `method: "synthesized"`.
- Nothing else was synthesised. No `unresolved` locations, no `proximity`/`text_match` bindings.

---

## 7. Low-confidence calls and judgement

1. **`Visit Window (Days)` given `hierarchical_level: 4`.** §3 says a header row needs a level when
   removing it would make two columns indistinguishable; strictly, removing the window row leaves
   every column still distinguishable by its visit label, which argues for `null`. It was given
   level 4 because it is a full-width structural band in the stacked header (epoch → visit → week →
   window), and dropping it out of the hierarchy would misrepresent the presentation. Reviewers who
   prefer the strict reading should change it to `null`; no other field depends on it.
2. **Indentation is a three-level model.** Two signals agree and were used together: leading
   whitespace in the text layer (flush labels start at y = 706.66 pt, indented ones at 695.82 pt),
   and cell shading measured from the render (three clean tones — modal grey 170 for the all-caps
   section bands, 200 for ordinary rows, 241 for indented children). Level 0 = the five all-caps
   shaded bands (`SUBJECT RELATED INFORMATION AND ASSESSMENTS`, `EFFICACY`, `SAFETY`,
   `TRIAL MATERIAL`, `REMINDERS`), level 1 = ordinary activity rows, level 2 = indented children.
   The level-0 call comes from shading rather than whitespace, so those five rows carry
   `activity_name_source.indentation_method: "font_signal"`; levels 1 and 2 come from whitespace and
   record no method. All five level-0 rows carry zero marks, as §4 requires. Note that the level-1
   group parents `Body measurements`, `Vital Signs` (twice), `Clinical Outcome Assessments` (twice)
   and `Administration of trial product` also carry zero marks, their children carrying them instead.
3. **`Vital Signs` and `Clinical Outcome Assessments` each appear twice** (rows 34 and 53; rows 37
   and 55), once under EFFICACY and once under SAFETY, with different section references
   (`(6.4.2, 9.4.3)` both times for Vital Signs; `(9.1.2)` vs `(9.4.1)` for Clinical Outcome
   Assessments). These are two distinct printed rows and are kept as two activities each — not
   de-duplicated. Downstream consolidation should expect the repeat.
4. **`cell_text` whitespace and line wraps.** The text layer contains no space glyphs; leading
   indentation in `cell_text` reproduces `pdftotext -layout`'s rendering of the measured 10.8 pt
   indent, and cells that wrap over two printed lines are joined with a single space
   (e.g. `Administration of trial product (7.1, 7.5)`, printed as "Administration of trial product (7.1," /
   "7.5)"). `activity_name` is the cleaned form in every case.
5. **`First date on trial product` at P3.** Its single mark sits in column 4 (P3, week 2), a phone
   contact, rather than at V2 where run-in starts. Both mechanical passes and a direct look at the
   rendered page 10 agree. Transcribed as printed; not repaired.
6. **No protocol markdown was available**, so there is no PDF/markdown text comparison to report.
   All text is from the PDF text layer, which is clean (not glyph-spread: words come back as whole
   words, e.g. "Informed consent and Demography", so no §1c reconstruction was needed anywhere,
   including annotation text).

---

## 8. Source defects observed

- **Redaction bar outside the table.** Every one of the four pages carries a solid black rectangle in
  the top margin, above the page header line that carries the trial ID and well clear of the grid
  (it sits at PDF x ≈ 31 pt, whereas the table starts at x ≈ 144 pt). It conceals no activity row, no
  column and no footnote — the row-boundary scan for every page was taken from within the table
  proper, so the §1d failure mode in which a redaction bar is mistaken for a horizontal rule did not arise. Nothing was
  reconstructed from underneath it.
- **Footer page numbers disagree with the document pages**, as PAGEMAP warns: the printed footers read
  "8 of 94" … "11 of 94" while the PAGEMAP document pages are 8–11. They happen to coincide here, but
  the PAGEMAP values were used throughout regardless.
- **Grammatical defect in footnote c** — "If subjects not fulfil randomisation criteria see Section 6.3.2"
  is transcribed exactly as printed and was not repaired.

---

## 9. Method provenance recorded in the data (§1e)

| field | value | where | why |
|---|---|---|---|
| `activity_name_source.indentation_method` | `font_signal` | 5 rows (row_position 5, 25, 43, 58, 64) | level 0 derived from the all-caps + dark-shading band signal, not from whitespace |
| `marker_locations[].method` | `synthesized` | every location of pr1–pr29 (29 annotations) | the inline references carry no printed marker |
| `property_name_source.synthesized` | `true` | schedule_property row 1 | the study-period band's label cell is empty |

No `annotation_text_source.method` was recorded: all annotation text came from rule-line-bounded
text-layer cells (the footnote block on page 11 and the activity label cells). No
`activity_schedule`/`schedule_grid` `method` was recorded: all values came from the bbox text layer,
with the raster detector used only as a cross-check, not as the source of any value. No
`schedule_property.structure_method` was recorded: `property_type` and `hierarchical_level` follow
the printed header labels, except for row 1 whose type (`epoch`) follows from its printed values
(Screening / Run-in / Randomisation / Maintenance period / End of treatment / End of trial) — this is
noted in its `property_comment`.

**Recommended human spot-check:** the four columns V20/P21/V22/P23 (positions 21–24), where the
visit labels were split out of one text token; and the epoch row's two merge spans, 3:12 and 14:24.
