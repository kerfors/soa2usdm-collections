# NCT05324124 — SoA extraction uncertainty report

Source: `/root/ph3/blind/NCT05324124/NCT05324124_soa.pdf` (4 PDF pages = document pages 9–12 per
`PAGEMAP.md`; no excerpt page falls beyond the declared SoA range, so there is no back matter here).
Prompt v3.7.3, schema `soa-table-extraction` v1.0.

---

## 1. Table inventory and classification

**One table.** `NCT05324124_Table_01_extraction.json`.

| field | value |
|---|---|
| table_number | 1 |
| table_type | `main_soa` |
| pages | 9–12 (document numbering) |
| label columns (L) | 1 → first data column = position **2** |
| data columns | 15 (positions 2–16) |
| excluded trailing column | source column 17, "Comments" (notes column) |
| header rows | 3 |
| activities | 19 |
| activity_schedule cells | 77 |
| annotations | 10 |

**Why `main_soa`:** it is the only activity × timepoint grid in the excerpt and the anchor for the
study timeline — a screening window, an inpatient treatment period Day −1 → Day 11 with an early
discontinuation column, and a follow-up phone call. There is no second population, no finer-timing
subsidiary grid and no non-activity table, so the `track` / `subsidiary` / `domain` / `reference`
discriminators do not fire.

**Why one table and not 1 + 3 continuations:** the three header rows and the `Procedure` label column
reprint verbatim on pages 10, 11 and 12 because a single Word table breaks across pages; the body rows
simply continue. Per §1b I de-duplicated the reprinted header rows and counted each activity once, and
declared one table with `page_start` 9 / `page_end` 12. Recorded in `table_metadata.notes`.

**Activity rows per page across the declared range** (every page contributes):

| document page | rows | activities |
|---:|---:|---|
| 9 | 6 | Informed consent; Inclusion and exclusion criteria; Demography; Participant admission to CRU; Participant discharge from CRU; Outpatient visit |
| 10 | 3 | Medical history; Medical assessment; Height and weight |
| 11 | 6 | Serum or urine pregnancy test …; Follicle-stimulating hormone …; Human immunodeficiency virus, hepatitis B and C screen; 12-lead ECG; Vital signs (supine), pulse rate, and blood pressure; Clinical laboratory tests |
| 12 | 4 | Genetic sample; Selpercatinib administration; Adverse event /Serious adverse event review; Concomitant medication review |

**Column layout.** One leading label column ("Procedure"), so data columns are numbered from 2 and are
not renumbered: 2 = the screening window `-28 to -2 days prior to Day 1`; 3–14 = Days `-1` … `11`;
15 = `ED`; 16 = the follow-up phone call, `within 7 to 10 days after last dose`. The right-hand
"Comments" column (source column 17) is a notes column: excluded from `schedule_grid` and
`activity_schedule`, and its eight non-empty cells emitted as annotations (§6).

**Empty data column — flagged, not repaired.** Column 16 (the Follow-Up phone call) carries **no mark
on any of the 19 activity rows**. This was confirmed mechanically (zero near-black pixels in that
column band for every body row) as well as by eye. Nothing was inferred into it.

---

## 2. Source defects (transcribed, never resolved)

1. **Full-width redaction block on page 12.** A black box spans y ≈ 222–363 pt across the whole table
   width, between the `Genetic sample` row and the `Selpercatinib administration` row. At ~141 pt it is
   tall enough to conceal several single-line rows (single-line rows in this table run ~17 pt). Nothing
   under it exists in the text layer — the only text-layer content in that region is the redaction's own
   large red overlay lettering, which `pdftotext` returns as two glyph tokens (`C` and `CI`).
   **This region may conceal activity rows and their marks.** No rows were
   invented for it; no row boundary was reconstructed inside it. Flagged in `table_metadata.notes` and
   `extraction_metadata.extraction_notes`.
2. **Truncated abbreviations line (page 12).** The line under the table reads (de-glyphed)
   `Abbreviations: CRU = clinical research unit; ECG = electrocardiogram; ED = early discontinuation; h = hour;`
   and is then cut off by a second, smaller redaction block. Whatever followed the final `;` is not
   recoverable.
3. **Undefined in-grid mark `P`.** `P` is used as a scheduling value on Day 1 and Day 8 for four
   activities, but no legend defines it anywhere in the excerpt — most likely a casualty of defect 2.
   Per §6 (marker referenced but not defined) it is transcribed where it appears and carries a `legend`
   annotation whose text states plainly that the definition is not printed in the source. **No meaning
   was guessed** (in particular it was *not* expanded to "predose").
4. **Internally inconsistent cell text.** The vital-signs row prints the Day 1 cell as `P, 1, 2h` and
   the Day 8 cell as `P, 1,2h` (different spacing). Both are transcribed literally; not normalised.
5. **Missing terminal punctuation.** The pregnancy-test comment ends without a full stop —
   `… urine pregnancy test at all other time points`. Transcribed as printed.

---

## 3. Merged cells and distributed values

**Header merges (`schedule_grid`).** Confirmed from vertical-rule geometry, not from glyph position:
in header rows 1 and 2 the only internal rules are at x = 158.2 and 211.3 pt, so
- row 1 `Treatment Period` → merged across columns **3:15**, one entry per covered column;
- row 2 `Days` → merged across columns **3:15**, one entry per covered column;
- row 1 `Screening` (col 2) and `Follow-Up phone call` (col 16) are single cells.

**Vertical header merge, not expressible in the schema.** In column 16 there is no horizontal rule
between header rows 2 and 3 (the rule at y = 170.3 pt is present in columns 1, 2 and 17 but absent in
column 16), so `within 7 to 10 days after last dose` is one cell spanning header rows 2–3. The schema
has no vertical merge notation, so the value is recorded once, on row 3 (the row that gives each column
its timing), with `is_merged_cell: true` and **no** `merged_cell_range` — the merge is vertical, and
`merged_cell_range` is a column range. Flagged here so the flag is not misread as a column span.

**Distributed marks (`activity_schedule`).** Two rows, both on page 12:

| row | activity | value | source_range | covered columns |
|---:|---|---|---|---|
| 21 | Adverse event /Serious adverse event review | `↔` | `4:14` | Day 1 … Day 11 |
| 22 | Concomitant medication review | `↔` | `4:14` | Day 1 … Day 11 |

Basis: in these two rows *only*, every internal vertical rule between x = 239.6 pt and x = 555.5 pt is
absent — those two row bands yield 8 vertical rules (at x ≈ 72.2, 157.9, 211.1, 239.6, 555.5, 586.6,
629.3, 719.5 pt) against the 18 that every other body row yields. So the arrow sits in one cell merged
across columns 4–14 and is
distributed to all 11, not collapsed onto the columns where the glyph happens to be drawn (the drawn
extent is only x ≈ 272–526 pt, which would have wrongly read as Day 2 … Day 10).

**Arrow glyph transcription.** The mark renders as a double-headed horizontal arrow (a line with a
head at each end). Its text layer is not a real arrow character — `pdftotext` returns a run of `=`
signs flanked by two symbol-font glyphs. `cell_value` is recorded as `↔`, with
`method: "visual_read"`, because the arrowheads are vector graphics and no faithful character is
present in the text layer. This is the one place a glyph was normalised; flagged per §5.

No other merged marks, no qualified marks, no vertically merged marks anywhere in the table.

---

## 4. Synthesised names and markers

**Synthesised `property_name` values** (all three header rows; the label column carries no timing-row
names):

| row | synthesised name | label-cell content | reasoning |
|---:|---|---|---|
| 1 | `Study Period` | empty | row carries only spanning phase labels (`Screening`, `Treatment Period`, `Follow-Up phone call`) |
| 2 | `Days` | empty | row's only content is the word `Days` merged across columns 3–15 |
| 3 | `Study Day` | `Procedure` | the column-1 cell labels the *activity* column, not this timing row, so the name was taken from the row's own values |

All three carry `property_name_source.synthesized: true` and `structure_method: "inferred_from_layout"`.

**Synthesised annotation markers.** The Comments column carries no printed markers, so markers `n1`–`n8`
were synthesised in printed order and each linked by `marker_locations` (`method: "synthesized"`) to the
activity row whose Comments cell it is; the same marker is written into that activity's
`annotation_markers` so `resolve` can bind it:

| marker | page | bound to row | activity |
|---|---:|---:|---|
| n1 | 9 | 8 | Participant discharge from CRU |
| n2 | 10 | 11 | Medical assessment |
| n3 | 10 | 12 | Height and weight |
| n4 | 11 | 13 | Serum or urine pregnancy test (women of childbearing potential only) |
| n5 | 11 | 16 | 12-lead ECG |
| n6 | 11 | 17 | Vital signs (supine), pulse rate, and blood pressure |
| n7 | 11 | 18 | Clinical laboratory tests |
| n8 | 12 | 20 | Selpercatinib administration |

Each note cell is bounded by the row's own horizontal rules (each Comments cell occupies exactly one
activity row here — no note spans two rows and no row's note is split), so the bindings are positional,
not word-overlap. `method: "synthesized"` records only that the *marker* is invented, not the position.

---

## 5. Mechanical mark-check

Both surfaces were built and diffed.

- **Text layer (§1b):** `pdftotext -bbox`; column x-centres fixed from the header day/visit labels
  (`-1` @223–231, `1` @253–258, … `11` @537–547, `ED` @564–578 pt), mark tokens binned to the nearest
  centre. Header rows de-duplicated across the four pages.
- **Raster rule lines + dark-pixel counts (§1a/§1d):** pages rendered at 200 dpi; horizontal rules read
  from the *text-bearing* Procedure column (never from the redacted band, per §1d) to fix row bands;
  vertical rules re-detected **per row band** (they shift by up to 2 pt between bands, and using one
  page-wide boundary set produced false positives where a rule fell inside a cell); each cell then
  scored by counting pixels below intensity 90.

**Result: zero disagreements.** Every cell the pixel detector scored above threshold corresponds to a
text-layer token in the same column, and vice versa, on all 19 rows × 15 data columns. Representative
dense/sparse rows were also read by eye against the render (`Vital signs …` — 10 marks; `Demography` —
1 mark) and agreed.

Two detector artefacts were identified and discarded rather than transcribed, both caused by a
mis-placed column boundary letting a rule line into a cell: an apparent mark in column 9 of
`Genetic sample`, and apparent marks in columns 13–14 of `Medical assessment`. Re-detecting boundaries
per row band removed both. The arrow spans in rows 21–22 score above threshold in columns 5–13 for the
same "ink in cell" reason; those are the arrow itself and are handled as a merge (§3), not as nine
separate marks.

Worth a spot-check on the resolved HTML, because they look irregular but are what the page shows:
- `12-lead ECG` has a mark at Day 10 and Day 11 but **not** Day 9;
- `Clinical laboratory tests` has a mark at Day 4 and Day 11 but **not** Day 3 and **not** Day 10,
  while `Vital signs …` has marks at Day 3, Day 4 **and** Day 10;
- `Adverse event /Serious adverse event review` and `Concomitant medication review` carry marks at
  screening, Day −1 and ED *in addition to* the Day 1–11 arrow.

---

## 6. Annotation text integrity

- **Glyph-spread source (§1c): page 12 only.** Pages 9–11 have an ordinary text layer; page 12 returns
  one glyph per token — 325 word tokens for visibly less content than page 11's 165, and the layout dump
  of that page renders the header as letter-spaced text (`S c r e e ni n g`, `T r e at m e nt P e ri o d`).
  Reconstructed fields on that page: the four activity names (`Genetic sample`,
  `Selpercatinib administration`, `Adverse event /Serious adverse event review`,
  `Concomitant medication review` — `activity_name_source.method: "glyph_reconstruction"`), the n8 note
  text and the `ED` abbreviation text (`annotation_text_source.method: "deglyph_reconstruction"`), and
  the abbreviations line quoted inside the `P` legend. The header text on page 12 is also glyph-spread
  but was **not** reconstructed — it is identical to the header on pages 9–11, which is clean, and the
  clean text was used. Every reconstructed string was re-read as running prose against the 300 dpi
  render before delivery.
- **Containment pairs: none.** No annotation's text is contained in another's, and no two notes share a
  long run at a boundary. Every Comments cell is a single-row cell delimited top and bottom by that
  activity row's own horizontal rules, so there is no split-note or merged-note risk here.
- **Notes bounded by geometry, not proximity.** No annotation uses `proximity_bounded`.
- **Text completeness.** Each note starts at its cell's first word and ends at its last; multi-line
  cells were joined with single spaces. The typographic apostrophe in
  `… at physician’s discretion.` is preserved as printed (U+2019).

---

## 7. Low-confidence calls

1. **Header row 2 (`Days`) typed `other` with `hierarchical_level: null`.** It is a unit label merged
   across columns 3–15 and assigns no per-column value; removing it leaves every column still
   distinguishable, so it does not earn a level. An alternative reading types it `study_day` at level 2
   and pushes the numeric row to level 3. Low impact but noted.
2. **Header row 3 typed `study_day`.** Mixed content: twelve day numbers plus a screening window, an
   `ED` visit label and a follow-up window. Day numbers dominate, so `study_day`; a reviewer could
   argue `visit`.
3. **`Comments` treated as a notes column, not a schedule column.** It has no timing value in the
   header and its cells are prose. Firm call, but it is what removes source column 17 from the grid.
4. **`ED` emitted as the only `abbreviation`.** `ED` is printed *as* a header cell — the cell's entire
   content is the term — so its `marker_location` is a printed position (default method), which is the
   §6-permitted case. `CRU`, `ECG` and `h` were **dropped**: `CRU` and `ECG` occur only inside the
   running text of activity labels (`Participant admission to CRU`, `12-lead ECG`) and `h` only inside
   composite cell values (`2h`, `24h`); binding any of them would be the `text_match` standalone-list
   binding §6 forbids.
5. **`P` legend text is an extraction statement, not source wording.** Its `annotation_text` says the
   definition is not printed and quotes the truncated abbreviations line as context. Nothing is asserted
   as source content. Note that `P` is deliberately both the `cell_value` (§5: a legend-defined in-grid
   scheduling mark stays in the grid) and the `annotation_markers` value on those eight cells (§6:
   locations and markers must agree) — that duplication is intentional, not an uncleaned cell.
6. **n7 typed `source_note`.** `See Appendix 10.2, Clinical Laboratory Tests, for details.` is a bare
   pointer with nothing explained (§8). The other seven Comments notes explain and stay `footnote`.
7. **No full-protocol markdown was supplied**, so there is no PDF/markdown disagreement to report; the
   PDF is the sole authority for both structure and text.

---

## 8. Orphan risk

- Every annotation has ≥ 1 `marker_locations` entry, and every location's marker is present in the
  corresponding row's / cell's `annotation_markers` (checked mechanically both directions — no
  unmatched markers in either).
- No `unresolved` marker locations were needed.
- **Residual risk:** the `P` legend is definitionally incomplete (see §2.3) — resolvable and correctly
  bound to its eight cells, but it points at an absent definition. Anything hidden by the page-12
  redaction (§2.1) is unrecoverable and therefore unrepresented: if that block conceals rows, both those
  activities and any Comments notes beside them are missing from this extraction.

---

## 9. Method provenance (every non-default value recorded)

| where | field | value | why |
|---|---|---|---|
| schedule_properties rows 1, 2, 3 | `structure_method` | `inferred_from_layout` | type and level read off spanning/merge geometry; no printed row labels |
| schedule_properties rows 1, 2, 3 | `property_name_source.synthesized` | `true` | label column empty (rows 1–2) or carrying the activity-column header `Procedure` (row 3) |
| activities rows 4–22 (all 19) | `activity_name_source.indentation_method` | `assumed_flat` | flat table — every label starts at the same x; no grouping headers, no indentation evidence |
| activities rows 19–22 | `activity_name_source.method` | `glyph_reconstruction` | page 12 text layer is one glyph per token |
| annotation n8, `P`, `ED` | `annotation_text_source.method` | `deglyph_reconstruction` | same, for text read from page 12 |
| activity_schedule rows 21 & 22, columns 4–14 (22 cells) | `method` | `visual_read` | arrow is a vector graphic with no faithful text-layer character |
| annotations n1–n8 | `marker_locations[].method` | `synthesized` | Comments column prints no markers; markers invented, positions read from rule-line geometry |
| — | `location_type: "unresolved"` | none | no marker's target was indeterminable |

No `proximity`, `proximity_bounded`, `text_match`, `visual_transcription`, `raster_band_cells` or
`raster_pixel_detection` values were used: rule lines were recoverable everywhere outside the redaction,
and the text layer supplied every cell value except the two arrows.
