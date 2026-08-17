# NCT02107703 (Protocol I3Y-MC-JPBL, LY2835219) — SoA extraction uncertainty report

Source: `NCT02107703_soa.pdf`, 7 PDF pages = document pages 72–78 (per PAGEMAP.md).
Prompt v3.7.3, schema `soa-table-extraction` v1.0. Two tables emitted.

---

## 1. Structure of the excerpt

| Doc page | Content | Belongs to |
|---|---|---|
| 72 | Divider page: `Attachment 1. Protocol JPBL Study Schedule` — no grid | neither table |
| 73 | `Study Schedule, Protocol I3Y-MC-JPBL` — header + first body block | Table 1 |
| 74 | same header reprinted + body continues | Table 1 |
| 75 | same header reprinted + body ends; abbreviations block + footnote a | Table 1 |
| 76 | `Study Schedule, Protocol I3Y-MC-JPBL (continued)` — footnotes b–h only | Table 1 |
| 77 | `Study Schedule, Protocol I3Y-MC-JPBL (concluded)` — footnotes i–q only | Table 1 |
| 78 | `Study Schedule for the extension period only, Protocol I3Y-MC-JPBL` | Table 2 |

Pages 73–75 reprint an identical header block and the same
Procedure Category / Procedure / Protocol Reference label band; the rows simply continue, so this
is one logical table (page split), extracted as a single file with `page_start` 73 / `page_end` 77
and the repeated header rows counted once (§1b de-duplication), not as three files with
`continuation_of`.

PAGEMAP.md flags no page as beyond the declared SoA range; printed footers happen to agree with the
document pages here, but document pages from PAGEMAP.md were used throughout regardless.

---

## 2. Per table

### Table 01 — `main_soa`
- Title: `Study Schedule, Protocol I3Y-MC-JPBL`. Pages 73–77.
- **Label columns L = 3** (Procedure Category, Procedure, Protocol Reference) → **first data column is
  position 4**; data columns run **4–11** (8 data columns).
  Columns: 4 = Baseline / relative day ≤28, 5 = Baseline / ≤14, 6 = Cycle 1 / day 1,
  7 = Cycle 1 / day 15±3, 8 = Cycle 2–3 / day 1, 9 = Cycle 4 and Beyond / day 1,
  10 = Short-Term Follow-Up (Visit 801), 11 = Long-Term Follow-Up (Visit 802 - 8XX).
- 5 schedule properties, 40 schedule-grid cells, **31 activities**, 117 activity_schedule cells,
  37 annotations.
- **Activity rows per page:** 73 → 13 rows (row_position 7–19); 74 → 12 rows (20–31);
  75 → 6 rows (32–37); **76 → 0 rows; 77 → 0 rows**. Pages 76 and 77 are inside the declared range
  but carry only the "(continued)" / "(concluded)" footnote blocks — they contribute footnotes b–h
  and i–q respectively and no body rows. This is declared, not a skipped page (§4).
- Row_position 6 is the repeated label band that reprints the three label-column headings. It
  is neither an activity (§4: repeated column-label band) nor a schedule property (its data cells are
  blank/shaded and it distinguishes no column), so row 6 is intentionally unused; activities start at 7.

### Table 02 — `track`, `track_label` "Extension Period"
- Title: `Study Schedule for the extension period only, Protocol I3Y-MC-JPBL`. Page 78 only.
- **L = 3**, first data column **4**, data columns **4–6** (4 = cycle X-Y day 1, 5 = cycle X-Y day 15,
  6 = Extension Period Follow-Up, visit 901).
- 5 schedule properties, 15 grid cells, **3 activities** (all on page 78), 6 activity_schedule cells,
  6 annotations.
- **Classification reasoning.** Not `continuation` (different header block, own footnote set a–c, own
  abbreviations line). Not `domain` (column structure differs entirely: 3 data columns, cycles "X-Y",
  visits "501-5XX" and "901" against Table 1's BL/1/2–3/4+/801/802-8XX). It is a separate study phase
  attended by the subset of patients who continue: footnote a reads
  "The extension period begins after study completion and ends at the end of trial." → `track`.
  `track_label` "Extension Period" is taken from the source's own wording
  ("Study Schedule for the extension period only…", "Extension Period Follow-Up").

---

## 3. Method — how the grid was read

The PDF has a real text layer (`pdftotext` returns full words) and **no raster images at all**
(`pdfimages -list` is empty), so this is a vector table, not a scan. pdfplumber is forbidden (§1), so
the rule lines were recovered from the **raster** at 200 dpi per §1d and used for every page:

- Vertical rules (page 73–75): px 184 | 338 | 587 | 749 | 824 | 899 | 962 | 1049 | 1149 | 1312 | 1487 | 1687
  → 3 label columns + 8 data columns.
- Vertical rules (page 78): px 186 | 420 | 661 | 955 | 1137 | 1312 | 1600 → 3 label columns + 3 data columns.
- Horizontal rules taken from full-width ink rows; per-band re-scan of each vertical rule x tells
  which internal boundaries a given row actually draws — that is what identifies the merges below.
- Cell text then came from `pdftotext -bbox` tokens binned into those rule-bounded cells.
- The bbox matrix was diffed cell-for-cell against a visual read of each rendered page
  (73, 74, 75, 78) plus zoomed crops of the Baseline block on page 73. **No mark disagreed** between
  the mechanical matrix and the visual read.
- One systematic bbox-only artefact worth recording: a footnoted mark tokenises as X
  plus a separate superscript-letter token, and in a Baseline-merged row the two tokens straddle the
  (undrawn) 4/5 boundary — column binning alone would file the `X` in column 4 and the marker in
  column 5. The rule-line geometry resolves it: those cells are merged 4:5 and the mark is
  distributed, not centred (§5).
- Within multi-line cells the text layer's word order is scrambled (the ECOG label's two printed
  lines come back out of order); line order for every activity name and header label was re-established
  against the rendered page. The words themselves are text-layer reads, so no
  `activity_name_source.method` was recorded.

### Text-layer glyph encoding (normalised against the render)
- Some maths glyphs are encoded in a Symbol font at private-use code points rather than as their
  Unicode characters, which makes them look like dropped characters in a text dump. Page 73 carries
  both less-than-or-equal signs of the relative-day header as U+2264; page 74 carries one as U+2264
  and one as U+F0A3; page 75 carries both as U+F0A3. A 200-dpi crop of page 75 shows both signs
  printed, so the header cells are transcribed `≤28` and `≤14` on all three pages.
- Same for footnote a: the plus-minus in the 12-week follow-up interval is U+F0B1 in the text layer.
  The render shows it printed, so it is transcribed as `±` — my JSON reads
  "every 12 weeks (± 14 days) for the duration of this period."
  (The `15±3` header cell, by contrast, uses a plain U+00B1 in the source.)
- Two line-break rejoins verified on the render: footnote b prints
  "…is performed locally at baseline (Day -28 to Day -" / "1)" → transcribed "(Day -28 to Day -1)";
  footnote j prints post- / baseline across a line break and is rejoined as "post-baseline".
  The hyphen-collapsing raw (non-layout) pdftotext rendering of these two spots was not used.
- Not glyph-spread (§1c): tokens are whole words, so no deglyph reconstruction was needed anywhere,
  including `annotation_text`.

---

## 4. Merged cells — every distribution decision

Confirmed from per-row rule-line geometry, never from where the glyph sits.

**Table 1 — header (merged_cell_range):** row 1 `Baseline` 4:5, `Patients on Study Treatment` 6:9,
`Postdiscontinuation Follow-Up` 10:11; rows 2, 3 and 4 merge 4:5 and 6:7 (`BL`/`0`/`28` and
`1`/`1`/`28`). Row 5 (`Relative day within a cycle`) is the only header row that draws both internal
boundaries — it is what splits Baseline into two columns and Cycle 1 into two columns.

**Table 1 — body marks distributed (`source_range` set):**

| Row | Activity | Value | Span |
|---|---|---|---|
| 7 | Informed Consent Form signed | X (marker q) | 4:5 |
| 16 | Tumor measurement (palpable or visible) | X (c) | 4:5 |
| 17 | Radiologic imaging according to RECIST | X (b) | 4:5 |
| 18 | Bone Scintigraphy | X (i) | 4:5 |
| 19 | X-ray or CT scan with bone windows or MRI | X (j) | 4:5 |
| 21 | Adverse Event Collection/CTCAE Grading | X (f) | 6:7 |
| 22 | Concomitant Medications (with analgesics) | X | 6:7 |
| 32 | Fulvestrant Therapy | "Days 1 and 15 of Cycle 1, then Day 1 of Cycle 2 and beyond" | 6:11 |
| 33 | LY2835219 Therapy | "Every 12 hours on Days 1 through 28 of every cycle" | 6:11 |

Rows 8–15, 23, 24, 25, 26, 30, 34, 35, 36, 37 **do** draw the 4/5 boundary and their baseline mark
sits in column 5 alone; rows 13–15, 23, 24, 27, 30 draw the 6/7 boundary and carry separate marks in
6 and 7. So the merges above are row-specific, not a table-wide pattern.

**Table 2:** header rows 1–4 merge 4:5; row 5 draws the 4/5 boundary (days 1 and 15). Body rows 8 and
9 carry merged text across 4:5 ("Days 1 and 15 of Cycle 1, then Day 1 of Cycle 2 and beyond" and
"Daily every 12 hours"); row 7 (Adverse Events Collection) draws the boundary and marks column 4 only.

**Deliberately empty cells worth a second look at resolution:** row 18 (Bone Scintigraphy) column 8 is
shaded/blank while rows 16, 17 and 19 carry a mark there; row 30 (Local ECG) column 8 is blank, which
matches footnote e (baseline, C1D1, C1D15, C4D1, short-term follow-up — no cycle 2–3 ECG). Both were
re-checked on a zoomed crop. Nothing was completed by pattern.

---

## 5. Hierarchy — the Procedure Category column

Table 1 groups its rows with a vertically merged **Procedure Category** column (Study Entry/Enrollment, Medical History, Physical Examination, Tumor Assessment, Lab/Diagnostic Tests, Study
Drug, Health Outcomes); Table 2 does the same with "Study Drug". That column is a leading label column
and is excluded from the grid (§5). Those category labels occupy **no physical row of their own** —
they are merged cells spanning several body rows — so no synthetic level-0 section rows were
fabricated for them. Every activity is therefore recorded flat at `indentation_level` 0 with
`indentation_method: "assumed_flat"`, and the grouping is documented in `table_metadata.notes`.
This is a real loss of grouping information at this layer and the main judgement call in this
extraction; the alternative (inventing 7 header rows that do not exist) would have broken the schema's definition of `row_position` as the physical row position in the source.

Three rows on page 74 (`Survival Information`, `Adverse Event Collection/CTCAE Grading`,
`Concomitant Medications (with analgesics)`) and one on page 78
(`Adverse Events Collection/CTCAE Grading`) merge the Category and Procedure cells; the activity name
was taken from that merged label. Note the two spellings across tables — "Adverse Event Collection"
(Table 1) versus "Adverse Events Collection" (Table 2) — transcribed as printed, not harmonised.

---

## 6. Annotations

**Table 1 — 37:** 16 `footnote` (a–l, n–q), 1 `source_note` from the footnote block (m),
19 `source_note` from the Protocol Reference column, 1 `legend`.
**Table 2 — 6:** 3 `footnote` (a–c), 2 `source_note` (reference column), 1 `legend`.
`by_type` is not degenerate in either table; no annotation has empty `marker_locations`.

- **Footnote m typed `source_note`,** not footnote: its entire text is a bare pointer —
  "See Pharmacokinetic Sampling Schedule (Attachment 7)." (§8). Footnote p also points
  ("Refer to Section 9.4.1.1.2.") but explains first, so it stays a `footnote`.
- **Protocol Reference column → `source_note`s, deduplicated by text** (§6, "a dedicated reference
  column"). Table 1: pr1 `Section 8.1`, pr2 `Section 7`, pr3 `Section 12.2.3`, pr4 `Section 7.1`,
  pr5 `Attachment 4`, pr6 `Section 10.1.1`, pr7 `Attachment 5`, pr8 `Section 10.1`, pr9 `Section 10.3`,
  pr10 `Section 9.6`, pr11 `Attachment 2`, pr12 `Attachment 7`, pr13 `Section 10.4.2.2`,
  pr14 `Section 10.4.2.3`, pr15 `Section 10.3.2.1`, pr16 `Section 10.4.2.1`, pr17 `Section 9.1`,
  pr18 `Section 12.2.11`, pr19 `Section 12.2.11.4`. Table 2: pr1 `Section 10.3`, pr2 `Section 8.1.2`.
  Cells holding two references (ECOG: `Section 7.1` + `Attachment 4`; the three imaging rows:
  `Section 10.1.1` + `Attachment 5`) were split into separate notes. Every citing row carries the
  synthesised marker in its `annotation_markers`, and each location is flagged
  `method: "synthesized"` because the marker itself is not printed.
  `Medical History` (row 9) is the one activity whose reference cell is empty — no marker was invented.
- **Synthesised markers:** `pr1`–`pr19` (Table 1), `pr1`–`pr2` (Table 2), `lg1` (both tables).
- **Synthesised property names:** row 1 in each table is `Study Phase` — the label cell of the top
  header band is blank while the row carries `Baseline` / `Patients on Study Treatment` /
  `Postdiscontinuation Follow-Up` (Table 1) and `Patients on Study Treatment` /
  `Extension Period Follow-Up` (Table 2). `property_name_source.synthesized: true` on both.
- **`lg1` — "Perform procedure as indicated."** printed under each table title. It tells the reader
  what a grid mark means, so it is typed `legend`; it has no printed marker and no modelled element,
  so it is anchored table-scope to schedule_property row 1 with `method: "synthesized"` (§6, final
  bullet) and the marker is also recorded in that property's `annotation_markers` so resolve can link
  it.
- **Header-cell footnotes bound to their column, not the row** (§6): marker a sits on the Cycle row's
  "Short-Term Follow-Up" and "Long-Term Follow-Up" cells → `schedule_cell` (2,10) and (2,11) in Table 1, and (2,6)
  in Table 2; marker p sits on the two cells whose value is 1 in the Relative-day row → `schedule_cell` (5,8) and (5,9).
  Neither was put on the `schedule_property` row.
- **Zero `abbreviation` annotations.** The abbreviation block on page 75 (16 terms: BL, Temp, BP, CT,
  CTCAE, ECG, ECOG, GnRH, HR, IV, PK, MRI, RECIST, RR, SAEs, FSH) and on page 78 (CTCAE, PK, SAEs)
  define terms that appear only inside running activity names ("Vital Signs (Temp, BP, HR, RR)",
  "Local ECG", "Central pharmacokinetic (PK) sampling"). Binding those would be word overlap
  (`text_match`), which §6/§8 forbid. **Borderline call, recorded here:** `BL` *is* printed as a
  standalone cell value in the Cycle header row (columns 4–5), so it arguably has a printed marker
  location; it was still dropped, because tagging a cell whose entire `cell_value` is `BL` with an
  `annotation_markers` of `BL` would put the marker back inside the value it is supposed to be
  cleaned out of. GnRH, IV, SAEs and RECIST v1.1 appear only in footnote prose.

### Annotation-text integrity
- No annotation's text is contained in another's, in either table.
- **Long shared runs, re-verified against the pages and source-faithful — not a split note cell.**
  Footnotes b and c (page 76) and i and j (page 77) each end with the same boilerplate paragraph,
  "For patients who discontinue study treatment without objectively measured progressive disease
  (PD), continue to evaluate tumor response…"; b/j share 611 characters, b/c and c/j 451, and the
  pairs involving i 158–159. Each block is printed under its own marker letter, on a page that starts
  a new marker sequence, and the differing halves are assessment-specific (radiologic tests vs tumor
  assessments vs bone scintigraphy every 6 months vs radiologic tests). Nothing was merged, truncated
  or dropped to make the overlap go away.
- Every footnote was bounded by its printed marker letter at the left margin of pages 75–77 / 78, so
  no note needed `proximity_bounded`; there is no notes/comments column in either table.
- Two source defects transcribed as printed, not repaired: footnote h ends without a full stop
  ("…it will not constitute a protocol deviation"), and Table 2 footnote c reads
  "ending hour after taking study drug" where Table 1 footnote g reads "ending 1 hour after taking
  LY2835219" — the "1" is missing in the extension-period version.

---

## 7. Low-confidence calls

1. **`Approximate Duration (days)` / `Duration (days)` typed `other`, `hierarchical_level` null.**
   The row gives the nominal length of each cycle or period (28/28/28/28/30/Variable). No enum member
   fits a period-length row — it is not a visit `window` and not a `study_day` — and removing the row
   leaves every column still distinguishable via Cycle/Visit/Relative day. Consequently the
   `Relative day within a cycle` row is level 4 (fourth level-bearing row), not 5.
2. **`Cycle` row typed `cycle` although two of its values are follow-up periods**
   ("Short-Term Follow-Up" and "Long-Term Follow-Up", each carrying marker a). The printed row label
   is `Cycle`, and the
   remaining values are cycle identifiers, so the label was followed rather than reclassified.
3. **Table 2 as `track` rather than a second `main_soa`.** Both readings are defensible — it is an
   independent schedule with its own header block. `track` was chosen because it covers a distinct
   later phase of the same study for the continuing subset, which is what `track_label` exists to
   carry; a resolver that treats it as an independent anchor loses nothing but the phase label.
4. **Distributing the Baseline-merged marks across columns 4 and 5.** Semantically the Informed
   Consent / tumor-assessment marks probably belong to the ≤28-day baseline window only (footnote q:
   "Informed Consent Form is signed within 28 days prior to randomization…"; footnote c/i/j:
   "(Day -28 to Day -1)"). The cell is nonetheless physically merged across both baseline columns and
   §5 forbids collapsing a merged mark onto one column, so both columns carry the mark and
   `source_range` "4:5" records the span. Resolution may wish to narrow these using footnotes q, b, c,
   i and j.
5. No full-protocol markdown was attached, so there is no PDF/markdown text disagreement to report;
   all text is from the PDF text layer, with the four glyph/line-break exceptions listed in §3.

---

## 8. Method provenance (§1e) — everything non-default

- `activity_name_source.indentation_method: "assumed_flat"` on all 31 Table 1 activities and all 3
  Table 2 activities — flat tables; grouping lives in an excluded label column (see §5 above).
- `marker_locations[].method: "synthesized"` on: the 24 reference-column locations in Table 1
  (pr1–pr19) and 3 in Table 2 (pr1–pr2), and the single `lg1` schedule_property location in each
  table. All other locations are printed markers (no `method` recorded).
- **No `location_type: "unresolved"`** anywhere — every marker's target is printed.
- **No `annotation_text_source.method`** recorded: every note was read from a rule-line-bounded /
  marker-delimited text-layer block. In particular no `proximity_bounded` notes exist in this study.
- **No `activity_schedule` / `schedule_grid` cell `method`** recorded: all cell values are text-layer
  reads; only the *cell boundaries* came from the raster (§1d), which the schema does not treat as a
  different value-read method. The rendered pages were used for verification, and for the four glyph
  gaps in §3 (≤, ±, two hyphen rejoins) which affect header cell values `≤28`/`≤14` and footnote text
  only.

## 9. Orphan risk

None identified. All 37 + 6 annotations carry at least one `marker_locations` entry; every marker in
a `marker_locations` entry also appears in that row's / cell's `annotation_markers` (checked
mechanically). No marker is referenced in the grid without a definition printed in the source, and no
footnote definition is printed without a referencing marker: Table 1 defines exactly a–q and all
seventeen are used; Table 2 defines a–c and all three are used.
