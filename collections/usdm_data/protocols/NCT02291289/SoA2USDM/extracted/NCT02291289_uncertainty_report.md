# NCT02291289 — SoA extraction uncertainty report

Source: `NCT02291289_soa.pdf` (Bevacizumab — F. Hoffmann-La Roche Ltd., Protocol MO29112, Version 9),
26 PDF pages = document pages 169–194. Document page 194 is back matter beyond the declared SoA range
(Appendix 6, FOLFOX regimens) and was not extracted. All page numbers below are DOCUMENT pages, taken
from PAGEMAP.md; the printed footers on these pages happen to agree with the map but were not relied on.

## 1. Tables found and how each was classified

Five tables, one per protocol appendix:

| # | Table | Type | track_label | Pages | Data cols | Activities | Annotations |
|---|-------|------|-------------|-------|-----------|------------|-------------|
| 1 | Appendix 1 — Screening/Baseline and Induction, all patients | main_soa | — | 169–173 | 6 (positions 2–7) | 21 | 20 |
| 2 | Appendix 2 — Maintenance Phase (Cohort 1) | track | Cohort 1 | 174–178 | 4 (positions 2–5) | 25 | 24 |
| 3 | Appendix 3 — Maintenance Phase (Cohort 2) | track | Cohort 2 | 179–183 | 4 (positions 2–5) | 24 | 21 |
| 4 | Appendix 4 — Maintenance Phase (Cohort 3) | track | Cohort 3 | 184–188 | 4 (positions 2–5) | 22 | 21 |
| 5 | Appendix 5 — Maintenance Phase (Cohort 4) | track | Cohort 4 | 189–193 | 4 (positions 2–5) | 26 | 23 |

Label columns: **L = 1** in every table (the activity-name column only). First data column position is
therefore **2** in all five tables; no renumbering was applied.

**Table 1 = main_soa.** Rows are activities; it is the anchor grid the whole excerpt hangs off — every
maintenance appendix times its first column relative to it (`"Within 3 weeks of completing Induction
Treatment Phase"`).

**Tables 2–5 = track, not domain and not continuation.** They are not continuations (their column
structure differs from Table 1: `"Prior to randomization"` / `"Maintenance Treatment Phase"` replace
Screening/Baseline and Induction). They are not domains of each other even though Tables 2, 3 and 5 carry
almost byte-identical column labels, because they schedule mutually exclusive populations: footnote d of
each says patients are assigned to exactly one cohort — "these patients will be assigned to a maintenance
treatment cohort" — and footnote e says patients "found ineligible for any cohort will undergo a Study
Treatment Discontinuation Visit". Identical labels here mean the sponsor reused a layout, not that the
same patients attend. Classifying them as domain would silently null the `population_track` of every
column. The reasoning is also recorded in each file's `table_metadata.notes`.

`track_label` values are the source's own short population identifiers, taken from the appendix titles
("… MAINTENANCE PHASE (COHORT 1)" → `Cohort 1`), not composed descriptions.

## 2. Activity rows per page across each declared range

- Table 1: 169 → 12 rows, 170 → 7 rows, 171 → 2 rows. **172 and 173 contribute no activity rows** — they
  are footnote-only pages carrying footnotes f–t of this table. Their inclusion in the page range is
  deliberate (the annotations are part of the table).
- Table 2: 174 → 14, 175 → 10, 176 → 1. **177 and 178 are footnote-only** (footnotes k–x).
- Table 3: 179 → 14, 180 → 8, 181 → 2. **182 and 183 are footnote-only** (footnotes f–u).
- Table 4: 184 → 13, 185 → 9. **186, 187 and 188 are footnote-only** — this is the only table whose entire
  footnote block a–u sits on text-layer pages with no footnote text printed on the grid pages.
- Table 5: 189 → 16, 190 → 8, 191 → 2. **192 and 193 are footnote-only** (remainder of footnote g, then h–w).

No page in any declared range was skipped. There are no horizontally tiled tables in this document.

## 3. Image-based grid: method (§1a / §1d)

The grid pages carry **no text layer at all** — `pdftotext` returns 1 byte (a form feed) for pages 169–171,
174–176, 179–181, 184–185 and 189–191, while the footnote pages return 2.4–5.1 kB. The grid was therefore
recovered from the raster:

1. Pages rendered at 200 dpi (`pdftoppm -r 200`); ink = grey < 128.
2. **Vertical rules** = image columns black (< 15 grey) over the table's height. Table 1: x = 190, 590, 750,
   909, 1205, 1472, 1751, 2149 px → one label column plus six data columns. Tables 2–5: six rules → one
   label column plus four data columns.
3. **Horizontal rules** = image rows with > 50 % ink across the page width, read from the full table width
   (no redaction bars occur in this document, so no column had to be excluded from the band read).
4. **Marks** = count of near-black pixels (< 90 grey) inside each rule-bounded cell rectangle. A single
   lowercase mark scores 49–66; an uppercase mark 72–75; any text cell scores ≥ 590. Empty cells score 0
   with no intermediate values anywhere in the five tables, so the count threshold is unambiguous.
5. **Per-row merge test**: for every body row, each internal vertical rule was re-tested inside that row's
   band. Exactly one internal rule is missing anywhere in the document (Table 1, see §4).

**Detector validation against direct visual reads** (§1a requires at least one dense and one sparse row):

- Dense, page 169 row `Concomitant medications` — detector: cols 2,3,4,5,6 marked, col 7 empty. Visual
  read of a magnified crop: five `x` and an empty last cell. Match.
- Dense, page 170 row `Adverse events (including SAEs)` — detector: cols 2,3,4,6 single marks, col 5 text
  (719 px), col 7 text (1076 px). Visual: `x`, `x`, `x`, `Every cycle`, `x`, `x (as applicable)`. Match.
- Sparse, page 169 row `Informed consent` — detector: col 2 only. Visual: one `x`, five empty cells. Match.
- Sparse, page 175 row `INR, aPTT (select patients)` — detector: col 3 only (2295 px of text). Visual:
  `According to local standard of care`, three empty cells. Match.

**No cell disagreed between the mechanical matrix and the visual read** in any of the five tables. A
spot-check of the resolved grids against the rendered pages is still recommended, since every value in
these tables — marks and text alike — was read from pixels rather than from a text layer.

## 4. Merged cells and distributed marks

Header merges (Table 1 only; Tables 2–5 have no merged header cells):

- Row 1 (population band): columns 2:3 and 4:5 are merged and empty; columns 6:7 are merged and carry
  `"Patients who have PD during or at end of Induction Treatment Phase or who refuse Maintenance
  Treatment or who are not eligible for any study cohort"`.
- Row 2 (phase band): `"Screening / Baseline"` merged over 2:3, `"Induction Treatment Phase"` merged over
  4:5, then single cells for columns 6 and 7.

Body merge — exactly one in the whole document:

- **Table 1, `Study medication administration` (row 22, page 170)**: the internal vertical rule between
  columns 4 and 5 is absent in that row only (present in 100 % of the row's height everywhere else,
  15 % here). The cell holds `x` on one line and `Administered every 2 weeks` beneath it. Transcribed as
  `cell_value: "x Administered every 2 weeks"` on **both** columns 4 and 5 with `source_range: "4:5"`,
  rather than collapsed onto the visually centred column.

No arrows, no vertically merged marks, and no qualified marks needing column expansion occur anywhere.

## 5. Glyph case — the one genuinely uncertain read

Marks are lowercase `x` throughout except five cells that print a visibly larger glyph, transcribed as `X`:

- Table 3 (Cohort 2): `TSH, free or total T3, free or total T4 (Experimental Arm only)` col 4;
  `Pulse oximetry (Experimental Arm only)` col 4; `HIV, HBV, HCV serology` col 2.
- Table 5 (Cohort 4): `LVEF` col 2; `Ophthalmology exam` col 2.

Basis: at 200 dpi the ordinary marks measure 14–15 px tall × 11–12 px wide (49–66 near-black px); these
five measure 17–18 × 14–15 px (72–75 px). At 600 dpi the ratio is 54×46 vs 46×37 with **identical stroke
weight**, which fits a different character rather than a larger font size. Because there is no text layer,
the case cannot be confirmed from character codes — this is a size-based inference and is the least
certain call in the extraction. Note the internal inconsistency it exposes and that I did **not**
normalise away: Cohort 2 prints `X` for TSH / pulse oximetry / serology while Cohort 4 prints lowercase
`x` for the same three assessments, and Cohort 3 prints lowercase `x` for LVEF while Cohort 4 prints `X`.
Verify these five cells against the page.

## 6. Synthesised values

**Property names.** No table prints a label in the header row's own label cell — that cell is empty
navy in all five tables. Every `property_name` is therefore synthesised, flagged with
`property_name_source.synthesized: true` and `structure_method: "inferred_from_layout"`:

- Table 1: `Population qualifier` (row 1), `Study phase` (row 2), `Visit timing` (row 3).
- Tables 2–5: `Study phase` (row 1), `Visit timing` (row 2).

**Annotation markers.** None were synthesised. Every annotation in this document carries a printed
letter marker, and every printed marker resolves to a printed footnote. There is no notes/comments
column in any of the five tables.

## 7. Property-type calls (low-confidence)

- `Study phase` → `epoch`: the values are the protocol's own phase names, so this is safe.
- `Visit timing` → **`other`**: the row mixes concepts in a single band — screening windows
  (`"≤ 28 days"`, `"≤ 7 days"`), a cycle day (`"Day 1 Cycle 1"`), frequencies (`"Every 2 cycles (every 4
  weeks)"`, `"Every 3 months"`) and a post-dose window (`"(≤ 30 days after last dose of study
  treatment)"`). `window`, `study_day` and `timepoint` would each be right for part of the row and wrong
  for the rest, so `other` is the honest classification. This is the main property-type judgement call.
- Table 1 row 1 → `condition` with `hierarchical_level: null`, per §3: it qualifies who attends columns
  6–7 rather than telling column 6 from column 7.
- Hierarchical levels: Table 1 = epoch 1 / timing 2 (the null condition band is skipped in the count);
  Tables 2–5 = epoch 1 / timing 2.

## 8. Annotations

**Types.** All footnotes are explanatory except four bare pointers, typed `source_note` per §8: the
footnote `"See Appendix 8."` in Table 2 (marker m), Table 3 (marker i), Table 4 (marker i) and Table 5
(marker i). Everything else explains as well as points (e.g. Table 1 marker i: `"ECOG status assessed
within 7 days prior to Day 1 of Cycle 1 (Induction Treatment Phase) for eligibility determination. See
Appendix 8."`) and stays `footnote`. `by_type` is not degenerate — it is 20 / 23 / 20 / 20 / 22
footnotes for Tables 1–5, plus exactly one `source_note` in each of Tables 2–5.

**No abbreviation or legend annotations were emitted.** The tables carry no abbreviation block and no
in-grid legend; the only in-grid symbol is the mark itself, which the source never defines. Nothing was
bound by `text_match` or `synthesized`.

**Marker locations.** Markers a, b and c are printed on header cells, so per §6 they are recorded as
`schedule_cell` locations on the exact header column they sit on, not on the `schedule_property` row:

- `a` on the `Maintenance Treatment Phase` / `Induction Treatment Phase` header cell (Table 1: both
  columns 4 and 5, because that header cell is merged over 4:5);
- `b` on the `Study Treatment Discontinuation Visit` header cell;
- `c` on the `Post-Treatment Follow-Up Phase` header cell **and** on the two activity rows that cite it in
  their labels, `Subsequent anti-cancer therapies (see [c])` and `Patient survival (see [c])` — one
  annotation with three locations, not three annotations.

Markers d onwards are printed on activity labels and are recorded as `activity_name` locations. Every
marker in every `marker_locations` entry also appears in that row's / cell's `annotation_markers`
(checked mechanically; zero mismatches). No annotation has empty `marker_locations`. No marker is used
without a definition and no footnote is defined without a use, in any of the five tables.

**Containment check.** No annotation's text is contained in another's, within any table. (Across tables,
several footnotes are near-duplicates — e.g. footnote e is worded identically in Tables 2, 3 and 5 — but
they are separate tables and each is emitted once in its own file.)

**Annotation text integrity.** The text layer is *not* glyph-spread; nothing needed de-glyph
reconstruction. Footnote wording was taken from the text layer wherever one exists. The footnotes printed
on scanned grid pages were transcribed by eye and carry
`annotation_text_source: {"method": "visual_transcription"}`:

- Table 1 a–e (page 171), Table 2 a–j (page 176), Table 3 a–e (page 181), Table 5 a–f (page 191).
- Table 4 has **no** visually transcribed footnotes.
- **Table 5 footnote g is a join**: its opening runs to the bottom of scanned page 191 and continues on
  text-layer page 192. The two halves were joined at "For subsequent infusions, vital" / "signs will be
  collected within 60 minutes before the infusion", and the `annotation_text_source.note` records that.
  This is the one annotation in the document whose text comes from two different reading methods.
- Footnotes read from a text layer carry no `annotation_text_source` (default). They are running
  paragraphs beneath the table, not rule-bounded cells, but a direct text-layer read is still the
  default method and none of the four enumerated alternatives describes it better.

## 9. Source oddities transcribed, not repaired

- `Physical examination[h]` (Table 1, page 169) has no space before its marker; `Cohort- specific informed
  consent` (Tables 2–5) has a space *after* the hyphen. Both kept verbatim in `activity_name_source.cell_text`.
- Table 2 page 175 prints `Cycles 1, 2, 4, 6, 8, 10, 12,14 and every 2 cycles thereafter` — missing space
  after "12," — where Tables 3, 4 and 5 print `12, 14`. Kept as printed.
- Table 5 page 190 prints `Experimental arm only` for LVEF and `Experimental Arm only` for the
  ophthalmology exam in the same column. Kept as printed.
- Table 4 page 185 prints `Experimental arm only: Cycle 3, 6, 10, 14, 18, 22, 26 and every 4 cycles
  thereafter` — singular, where the sibling tables use the plural, e.g. Table 5's `Experimental arm only:
  Cycles 4, 10, 16, 22, 28, 34, 40 and every 6 cycles thereafter`. Kept as printed.
- Several activity cells carry a status sentence appended below the label, e.g. `Stool sample` followed by
  `Supplemental Biomarker Program closed as of May 2018. Collection of these samples has been
  discontinued.` The full cell text is preserved in `activity_name_source.cell_text`; `activity_name` is
  the procedure label alone. Reviewers who expect the status sentence in the name should look there.
- The header cells of Tables 1, 3 and 4 embed an appendix pointer, `Every 3 months until May 31, 2019 (see
  Appendix 19)`. §6's inline-reference rule is written for activity labels; rather than invent a marker on
  a header cell, the pointer was left inside the header cell value. Flagging it here in case downstream
  wants it as a `source_note`.
- Table 1's `Metastatic tumour tissue for exploratory biomarker assessment` row prints
  `No sample collection` alone in column 2 but `No sample collection Supplemental Biomarker Program
  CLOSED` in column 5 — an asymmetry in the source, not a transcription slip.

## 10. Method provenance recorded (§1e)

- Every `activity_schedule` cell: `method: "raster_pixel_detection"` for the bare marks (`x` / `X`),
  `method: "visual_read"` for text-bearing cells. Every `schedule_grid` cell: `method: "visual_read"`.
- Every activity: `activity_name_source.method: "visual_transcription"` and
  `indentation_method: "assumed_flat"` — all five tables are flat, with no section-header rows and no
  visual indentation to read, so every activity is level 0 and every level-0 row legitimately carries marks.
- Every `schedule_property`: `structure_method: "inferred_from_layout"` (label cells empty; type and level
  read off the band geometry and the printed values).
- `annotation_text_source.method: "visual_transcription"` on the 27 footnotes listed in §8.
- **No `location_type: "unresolved"` anywhere, and no guessed targets** — every marker in this document is
  printed at a location that could be read directly.

## 11. What a reviewer should check first

1. The five `X` vs `x` cells in §5 — the only case call made from glyph size rather than character codes.
2. The single merged cell, Table 1 `Study medication administration`, distributed across columns 4:5.
3. The synthesised property names and the `other` property-type on the timing row (§6, §7).
4. Table 1's row-1 population band recorded as `condition` with `hierarchical_level: null`.
5. Any text cell in the five grids: all of it was read from pixels, none of it from a text layer.
