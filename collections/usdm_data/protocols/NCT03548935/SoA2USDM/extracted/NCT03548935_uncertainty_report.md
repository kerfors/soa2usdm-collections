# NCT03548935 — SoA extraction uncertainty report

Source: `/root/ph3/blind/NCT03548935/NCT03548935_soa.pdf` (6 PDF pages = document pages 8–13 per PAGEMAP.md).
Protocol NN9536-4373, Version 2.0, 21 December 2017 (Novo Nordisk). Prompt v3.7.3, schema
soa-table-extraction v1.0.

Output: one file, `NCT03548935_Table_01_extraction.json`.

---

## 0. Rotation — how it was established and handled

pdfinfo reports a page rotation of 0, so the page dictionaries claim no rotation; the table text is
drawn rotated instead. This was established from the text-layer geometry, not by eye: in the
pdftotext -bbox output every table token has a narrow x-extent (~9.5 pt, one line height) and a long
y-extent (the word length) — the token for the word Inclusion, for instance, spans x 338.12–347.61
but y 661.68–693.34. Successive words of a label run in the direction of *decreasing* y, and
successive table rows run in the direction of *increasing* x.

The transform used throughout was therefore

```
display_x = (792 - y_pdf) * 200/72      display_y = x_pdf * 200/72
```

i.e. the page read normally after a 90° **clockwise** rotation. Page rasters were rendered with
pdftoppm at 200 dpi greyscale and rotated 90° clockwise with PIL before rule-line detection, so pixel
and text coordinates share one frame. Two header labels ("Screening", "Randomisation") are drawn in
the *un*rotated orientation — they are the vertical labels inside the epoch band — and fall out of
the same transform without special-casing.

Sanity check on the transform: the header token for "V1" maps to display x 692–721, and the vertical
rule pair bounding column 2 sits at 667 / 745. Every one of the 25 visit labels lands inside exactly
one rule-bounded column.

## 1. Per table

| | |
|---|---|
| Table 01 | `main_soa`, doc pages 8–13 |
| Columns | 26 physical columns: **L = 1 label column**, first data column = **position 2**, data columns 2–26 (V1…V25) |
| Header rows | 4 (`schedule_properties` rows 1–4), de-duplicated across the six pages |
| Activities | 78 (`row_position` 5–82) |
| Marks (`activity_schedule`) | 428 |
| Annotations | 37 (7 `footnote`, 30 `source_note`) |

**Why `main_soa` and why one table, not six.** The section-2 heading "Flowchart" is printed
once, on doc page 8; doc pages 9–13 carry no title and reprint the identical four header rows above
rows that simply continue. The 27 vertical rules recovered from the raster are byte-identical on all
six pages (241, 667, 745, 822, 866, 910, 951, 996, 1036, 1081, 1121, 1177, 1230, 1286, 1339, 1394,
1447, 1503, 1556, 1612, 1664, 1705, 1742, 1784, 1835, 1949, 2008 px at 200 dpi). Rows are procedures
performed on subjects; there is no second column structure, no finer-timing block and no population
split anywhere in the excerpt, so `subsidiary`, `track`, `domain` and `reference` are all excluded and
this is the protocol's primary anchor grid. It is modelled as ONE table with `page_start` 8 /
`page_end` 13 rather than a parent plus five `continuation` files, because the split is purely a paper
break inside a single titled flowchart; the reasoning is also recorded in `table_metadata.notes`.

**Activity rows per page across the declared range — every page contributes:**

| doc page | activity rows | marks supplied |
|---:|---:|---:|
| 8 | 13 | 14 |
| 9 | 15 | 105 |
| 10 | 13 | 69 |
| 11 | 16 | 114 |
| 12 | 15 | 94 |
| 13 | 6 | 32 |
| **total** | **78** | **428** |

No page in the range contributed zero rows. The table is not horizontally tiled — all 25 visit columns
print on every page — so the §5 tiling exception does not apply and no row appears twice.

Row-band counts were cross-checked against pdftotext -layout line counts per page and agree
(13/15/13/16/15/6).

## 2. Merged-mark decisions

**No body row carries a merged mark.** This was tested mechanically, not assumed: for every row band
on every page, each of the 27 vertical rule positions was probed for ink over that band's height. On
all 78 activity rows every internal boundary is present (ink fraction ≥ 0.6 in all cases), so every
mark sits in a single-column cell. No `source_range` is set anywhere in `activity_schedule`.

The only merges in the table are in the top header row (`row_position` 1, the epoch band), where the
same probe found boundaries 5–11 and 13–24 absent (ink fraction 0.00–0.06):

| cells | value | `merged_cell_range` |
|---|---|---|
| 4–11 (V3–V10) | `Dose escalation period` | `4:11` |
| 12–24 (P11–P23) | `Maintenance period` | `12:24` |

`Screening` (2), `Randomisation` (3), `End of treatment` (25) and `End of trial` (26) are single
cells. `is_merged_cell` / `merged_cell_range` are recorded on each covered position, in numeric
`column_position` notation (not A1).

## 3. Synthesised values

**Synthesised `property_name` — one.** `row_position` 1 (the epoch band) has an empty label cell; the
name `Trial Period` was synthesised, `property_name_source.synthesized: true`, and
`structure_method: "inferred_from_layout"` recorded because both `property_type` and
`hierarchical_level` for that row come from the spanning geometry rather than a printed label.

**Synthesised annotation markers — 30 (`pr1`…`pr30`).** Per §6, every section / appendix reference
printed inline in an activity label was stripped out of `activity_name`, kept in
`activity_name_source.cell_text`, and emitted as a `source_note` deduplicated by text, with a
synthesised marker added to every citing activity's `annotation_markers` and a
`marker_locations` entry carrying `method: "synthesized"`. Numbered in printed order of first
occurrence:

```
pr1 Appendix 3   pr2 Appendix 5   pr3 6.1     pr4 6.2      pr5 6.3      pr6 9.4
pr7 9            pr8 6.4.1        pr9 7.7     pr10 7.1     pr11 7.6     pr12 9.1.1
pr13 Appendix 2  pr14 9.8         pr15 9.4.3  pr16 9.1.2   pr17 9.4.2   pr18 9.4.5
pr19 9.4.4       pr20 9.2         pr21 Appendix 4          pr22 9.2.9   pr23 9.4.6
pr24 9.4.1       pr25 9.5         pr26 Appendix 7          pr27 7.5     pr28 8.1
pr29 7.1.2       pr30 7.1.1
```

Labels carrying more than one reference were split into separate notes, e.g. the source cell
"Trial Product Compliance (7.1, 7.6)" yields `pr10` + `pr11` on row 20, and
"Biomarkers (9.8, Appendix 2)" yields `pr14` + `pr13` on row 34. Parenthesised acronyms were **not**
treated as references and stay in `activity_name` — `(SF-36)`, `(IWQoL-Lite for CT)`, `(PGI-S)`,
`(PGI-C)`, `(SPS-6)`, `(ICIQ-UI-SF)`, `(PHQ-9)`, `(C-SSRS)`.

No `abbreviation` annotations were emitted: the excerpt has no abbreviation block, and no term is
printed as a marker in a grid, header or label cell.

## 4. Mechanical mark-check

Method (this is a text-layer table, §1b, with cell geometry recovered from the raster, §1d):

1. 200 dpi grey render, rotated 90° clockwise; ink = pixel < 128.
2. **Vertical rules** = image columns with ink fraction > 0.35 over the table height → 27 boundaries →
   1 label column + 25 data columns, identical on all six pages.
3. **Horizontal rules** = image rows with ink fraction > 0.5 of page width → row bands per page. Row
   boundaries were read from the full-width rules; there are no redaction bars inside the grid, so the
   §1d redacted-column caveat did not bite.
4. Every pdftotext -bbox token was mapped into display space and binned into the (row band ×
   column band) rectangle containing its centre. Marks were not matched by a glyph regex — the whole
   cell content was taken, so a footnoted mark (an X carrying a superscript e or g) could not be
   dropped.
5. Superscript markers were separated by glyph height (17 px vs 26 px for body text) rather than by
   pattern-matching the alphabet.

**Diff against visual read: zero disagreements.** All six rendered pages were read by eye against the
mechanical matrix (page 8 whole-page; pages 9–13 as full-width crops). Every row's mark set matched,
including the dense rows (`Adverse event` 24 marks, `Concomitant medication` 24, `Criteria for
discontinuation` 21, `Technical complaint` 22) and the sparse ones (`Height` 1, `Hand out ID card` 1).

One binning fix was needed and is recorded here because it is invisible in the output: pdftotext
emits the three adjacent header labels V20/P21/V22 as a **single token**, `V20P21V22`, spanning display
x 1665–1785. The three rule-bounded columns underneath are 1664–1705, 1705–1742, 1742–1784, so the
token was split on those boundaries, giving V20 → column 21, P21 → column 22, V22 → column 23. The
split is corroborated independently by the week row, which does emit three separate tokens: 52 → col
21, 56 → col 22, 60 → col 23, and by the visit-window row (three separate `±3`).

## 5. Annotation text integrity

- The source text layer is **not** glyph-spread. Tokens are whole words; no `deglyph_reconstruction`
  was needed and no `annotation_text_source` method is recorded on any annotation.
- All seven footnote texts (a–g) were read from the text-layer footnote block on doc page 13, which is
  ordinary horizontal-in-display running prose below the grid, each note starting at its own marker.
  No note is bounded by proximity; there is no notes/comments column in this table.
- **Containment pairs.** 22 pairs exist where one `annotation_text` is a substring of another — all of
  them among the bare section-number `source_note`s (`9` inside `9.4`, `9.4` inside `9.4.3`, `7.1`
  inside `7.1.1`, `9.2` inside `9.2.9`, …). These are **not** a split note cell: they are separate
  section identifiers that happen to be numeric prefixes of one another, each printed independently in
  a different activity label (e.g. "Weight History (9)" vs "Medical history/Concomitant illness
  (9.4)" vs "Vital Signs (9.4.3)"). Re-verified against doc pages 8–13; no merging or truncation was
  applied. None of the seven real footnotes overlaps another.
- Every annotation's text is complete against its source cell — start word to end word, no
  letter-spacing, no missing inter-word spaces.

## 6. Low-confidence calls

1. **`row_position` 4, `Visit Window (Days)` — `hierarchical_level: 4`.** Strictly, removing this row
   would not make any two columns indistinguishable (the visit row alone separates all 25), which by
   the §3 test would argue for `null`. It was given level 4 because it is a genuine fourth tier of the
   stacked header (epoch → visit → week → window) carrying per-column data, and `null` is reserved for
   purely presentational qualifiers. Flagging it as the most likely single point of disagreement in
   the header modelling.
2. **`row_position` 1 — `property_type: epoch`.** The band mixes true phases (`Dose escalation
   period`, `Maintenance period`) with a point event (`Randomisation`) and two end-of-study markers
   (`End of treatment`, `End of trial`). Taken together the row reads as the study's major phase
   decomposition, so `epoch`; `period` was the alternative considered.
3. **`row_position` 60–61 (`Patient Health Questionnaire–9 (PHQ-9)`, `Columbia-Suicide Severity Rating
   Scale (C-SSRS)`) — `indentation_level: 2` set from shading, not whitespace.** Text-layer indent for
   these two rows is 274.0/274.1 pt, the same as level-1 rows, while their parent
   `Clinical Outcome Assessments (9.4.1)` sits at 268.4 pt — i.e. the whitespace makes them look like
   siblings of their own parent. Cell shading, which is a clean signal everywhere else in this table
   (grey 170 = level 0 section band, 211 = level 1, 241 = level 2), gives these two rows 241, the same
   as `Pulse`, `Height`, `Body Weight`, `Systolic blood Pressure`, `Dispensing visit` and the six COA
   rows on doc page 10, all of which are unambiguously level 2. They are also semantically the
   clinical outcome assessments the parent row heads. `indentation_method: "visual_estimate"` is
   recorded on both. If a reviewer disagrees, these are the only two rows affected.
4. **The 30 `source_note`s.** Emitting one annotation per distinct inline section/appendix reference is
   what §6 prescribes, but it is a large interpretive footprint for this protocol (nearly every row
   cites at least one section). `annotation_text` holds the bare reference as printed
   (`9.4.1`, `Appendix 2`), not a composed sentence.
5. No full-protocol markdown was supplied, so there is no PDF/markdown text disagreement to report;
   all text comes from the PDF text layer.

## 7. Source defects transcribed, not repaired

- **Header inconsistency across the repeated header.** The Visit Window value for V2 prints as `±0` on
  doc pages 8, 9, 10, 12 and 13 but as a bare `0` on doc page 11. Verbatim from the text layer, doc
  page 8: `-7 to 0      ±0     ` — doc page 11: `-7 to 0         0   `. The de-duplicated header in
  the JSON carries `±0` (five pages against one); the discrepancy is recorded in
  `table_metadata.notes` and is not otherwise resolved.
- **Typo retained.** `row_position` 79 is transcribed exactly as printed: "Hand out and instruct in PK
  dairy (9)" — the source says *dairy*, not *diary*, while `row_position` 80 says
  "Collect, review and transcribe diaries". Not corrected.
- **Capitalisation retained.** "Systolic blood Pressure" / "Diastolic Blood Pressure" (rows 36–37) are
  inconsistent in the source and are transcribed as printed.
- **Redaction.** A black redaction bar sits in the running header of doc page 8, over the watermark
  text, which the text layer renders as a garbled token beginning `3URWRFRO`…`Y` and interleaved with
  control characters (present on all six pages). It is above the table title, outside the grid, and conceals no activity rows or marks — the
  table's top rule on that page is at display y 401, the bar ends near y 120. Nothing was reconstructed
  from it.
- **`Randomisation` appears twice** — once as a header-band label (grid row 1, column 3) and once
  inside an activity label ("Randomisation criteria and randomisation (6.3)", row 10). Both kept.

## 8. Orphan risk

None. All 37 annotations have ≥ 1 `marker_locations` entry, and a mechanical check confirms that for
every location recorded, the marker also appears in that element's own `annotation_markers` string
(78 activities + 428 cells checked, 0 mismatches). No annotation is bound only to a
`schedule_property`. No marker is printed in the table whose definition is missing from the source:
markers a–g are all defined in the footnote block on doc page 13, and every one of a–g is used at
least once in the grid.

Marker placement detail worth a reviewer's eye: footnotes **e** and **g** are printed on *cells*, not
on labels, and are encoded as `schedule_cell` locations, keeping the visit they govern —
`e` on row 45 (`DEXA scan`) column 2 (V1), `g` on row 82 (`Attend visit fasting`) column 26 (V25).
Footnote **b** ("For all female subjects.") is printed on four different activity labels (rows 7, 14,
44, 52) and is one annotation with four locations, not four annotations.

## 9. Method provenance — every non-default method recorded

| where | field | value | why |
|---|---|---|---|
| `schedule_properties` row 1 | `structure_method` | `inferred_from_layout` | epoch row's label cell is blank; type and level read off the merge geometry |
| `activities` rows 60, 61 | `activity_name_source.indentation_method` | `visual_estimate` | level taken from cell shading, whitespace contradicts (§6.3 above) |
| all 30 `source_note` annotations, every location | `marker_locations[].method` | `synthesized` | `pr*` markers are not printed in the source; created per §6 for inline references |

Nothing else carries a method field: activity names, header labels, footnote texts and all 428 marks
came from the default path (text-layer cell bounded by its rule lines), so `annotation_text_source`,
`activity_name_source.method` and the cell-level `method` fields are absent throughout.

**No `marker_location` is `unresolved`.** Every marker in this table has a determinable target.

## 10. Recommended spot-checks

- Doc page 11, `Visit Window (Days)` V2 cell — confirm the printed `0` vs `±0`.
- Doc page 11, rows `Patient Health Questionnaire–9 (PHQ-9)` and `Columbia-Suicide Severity Rating
  Scale (C-SSRS)` — confirm the level-2 call.
- Doc page 9, rows `Evaluation of lipid-lowering treatment (9)`,
  `Evaluation of antihypertensive treatment (9)`, `Evaluation of glycaemic status (9)` — all three
  carry their mid-trial mark on **P13 (week 24)**, not on V12 (week 20) where `HbA1c` sits. This was
  verified at 2× zoom against the header and is what the source prints, but a phone visit carrying
  those evaluations is the kind of thing a reader will want to see twice.
