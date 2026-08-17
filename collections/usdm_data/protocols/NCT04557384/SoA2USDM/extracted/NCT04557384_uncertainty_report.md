# NCT04557384 — SoA extraction uncertainty report (prompt v3.7.2, schema v1.0)

Source: `NCT04557384_soa.pdf`, 9 PDF pages = document pages 16–24 (per PAGEMAP.md).
Protocol identifier printed in every page header (recorded as `document_version`): `I4T-MC-JVDU(e)`.
Document pages are used throughout; printed page footers were ignored (they happen to agree
with the page map here, but were not relied on).

**The PDF has no text layer at all** (`pdftotext` returns an empty string for every page).
The entire extraction is therefore the §1a/§1d image path: pages rendered with
`pdftoppm -r 200 -png`, rule lines recovered from the raster, marks detected by near-black
pixel COUNT inside rule-bounded cells, every result checked against a direct visual read of
the rendered page. No markdown was available, so there is no PDF/markdown disagreement to report.

---

## 1. Tables found and classification

| # | Title (as printed) | Type | Doc pages | Label cols (L) | First data col | Data cols | Activities | Annotations |
|---|---|---|---|---|---|---|---|---|
| 1 | Screening, On-Study, and Post-Treatment Schedule of Activities | `main_soa` | 16–22 | 1 | 2 | 2–16 (15) | 25 | 23 |
| 2 | Continued Access Schedule of Activities | `track` (`track_label`: Continued Access) | 23 | 1 | 2 | 2–3 (2) | 3 | 4 |
| 3 | Pharmacokinetic Sampling Schedule | `subsidiary` | 24 | 1 | 2 | 2–6 (5) | 15 | 1 |

Table 1 also has a right-hand **Instructions** notes column at column position 17; Table 2 has one
at column position 4. Per §5 these are not schedule columns: they carry no `schedule_grid` /
`activity_schedule` entries, and each non-empty cell became a `footnote` (or `source_note`) annotation.

**Classification reasoning.**

- **Table 1 → `main_soa`.** Rows are procedures performed on participants; it is the protocol's
  primary anchor grid.
- **Table 2 → `track`, not `domain`/`continuation`.** Different column structure entirely (2 columns,
  visits `501-5XX` and `901`) and it schedules a different *study phase* — participants who have moved
  into the continued-access period — rather than a different activity category on Table 1's timeline.
  `track_label` "Continued Access" is taken from the table's own title.
- **Table 3 → `subsidiary` (low-confidence call, see §6).** Its rows read `1`…`14` / `End of treatment`
  under a `Sample #` label column, so the literal reference test ("are the rows activities performed on
  subjects?") reads **no**. The §2 PK-sampling note governs: it provides finer timing for activities that
  already exist in Table 1 (`PK` and `IG`, whose grid cells read "See Section 1.3.1"), so it is classified
  by function as `subsidiary`. Recorded in `table_metadata.notes` as well.

## 2. Activity rows per page across each declared page range

| Table | Page | Activity rows |
|---|---|---|
| 1 | 16 | 6 (Informed consent … Physical examination) |
| 1 | 17 | 2 (Vital signs, AE collection) |
| 1 | 18 | 7 (ECOG PS … Pregnancy test) |
| 1 | 19 | 3 (Thyroid panel, Radiologic imaging…, Injection site assessments (solicited)) |
| 1 | **20** | **0 — see below** |
| 1 | 21 | 5 (Injection site assessments (spontaneous) … IG) |
| 1 | 22 | 2 (Administer ramucirumab, Administer combination medications…) |
| 2 | 23 | 3 |
| 3 | 24 | 15 |

**Page 20 contributes no activity rows, and this is deliberate (§4).** Page 20 carries exactly one
table band, and that band has an empty activity-label cell and an entirely empty grid; its only content
is the continuation of the Instructions text of the *Injection site assessments (solicited)* row, whose
row was split by the page 19/20 break. Two independent pieces of evidence: (a) the label cell and all
15 data cells are empty, and (b) the band reproduces the solicited-ISR row's shading pattern cell for
cell (columns 2 and 3 grey, columns 4–6 merged, 7–15 separate, column 16 grey). Per §4 an
instruction-overflow row that only carries footnote text is not an activity, so no row was created.

The same situation occurs at the page 18/19 break for **Pregnancy test**: the first band on page 19 has
an empty label, an empty grid, and reproduces the Pregnancy test row's shading (column 2 grey, column 3
white, 4–15 merged, column 16 white). It carries only the tail of that row's Instructions text. Page 19
still contributes 3 activity rows of its own, so no page-coverage gap arises there.

In both cases the split Instructions cell was transcribed as **one** annotation spanning the page break,
not two — the row is one logical row whose notes cell flowed across the page boundary:

- `n14` (Pregnancy test) = "• Applies only to women of childbearing potential." (page 18) +
  "• Note:  During study treatment, perform monthly or as required per local regulations and/or institutional guidelines. See Appendix 2." (page 19)
- `n17` (Injection site assessments (solicited)) = "Prior to each injection and at the designated timepoints, …" through "1.Cycle 1 collection:" (page 19) +
  "a.C1D1:  5-15 min after injection is complete…" through "See Section 8.2.5 for details." (page 20)

## 3. Merged / distributed cells

All spans below were taken from the **rule-line geometry recovered from the raster** — i.e. from which
internal vertical rules are absent in that row's band — never from where the glyph sits. `source_range`
is set on every distributed cell, in `column_position` numbering.

Table 1, body rows:

| Row | Activity | Value | Span |
|---|---|---|---|
| 10 | Concomitant medication | `X` | 4:15 |
| 12 | Vital signs | `See instructions` | 4:6 |
| 13 | AE collection | `X` | 4:15 |
| 15 | ECG | `See instructions` | 4:15 |
| 20 | Pregnancy test | `See instructions` | 4:15 |
| 21 | Thyroid panel | `See instructions` | 4:15 |
| 22 | Radiologic imaging and measurement of palpable or visible lesions | `See instructions` | 4:15 |
| 23 | Injection site assessments (solicited) | `See instructions` | 4:6 |
| 24 | Injection site assessments (spontaneous) | `See instructions` | 4:15 |
| 25 | Participant diary | `See instructions` | 4:15 |
| 27 | PK | `See Section 1.3.1` | 5:16 |
| 28 | IG | `See Section 1.3.1` | 5:16 |
| 29 | Administer ramucirumab | `See instructions` | 4:15 |
| 30 | Administer combination medications… | `See instructions` | 4:10 **and** 12:15 |

Two of these need comment:

- **Row 30 (Administer combination medications, if applicable (Cohorts B and C only)).** The DX column
  (position 11) is a *shaded, empty* cell sitting between the two "See instructions" spans, so the row
  carries two separate merged cells rather than one. The shaded block's right edge is flush with the
  DX column's right rule, but its left edge sits ~45 px (0.22 in) to the right of the D22/DX rule — this
  row's cell widths do not line up exactly with the header's. The shading was read as covering the DX
  column, and the first span was therefore bounded at 4:10. This is the one place in Table 1 where a
  cell boundary had to be reconciled against the header grid rather than read off it.
- **Rows 27/28 (PK, IG).** The merged "See Section 1.3.1" cell begins at column **5** (C1D8), not column 4:
  there is a real vertical rule between column 4 and the merged cell, and column 4 (C1D1) is an empty
  white cell. This is transcribed as printed and **not** repaired, but it looks like a source formatting
  quirk: Table 3 schedules Sample 1 at Cycle 1 / Day 1 / -1 hr (predose), i.e. PK *is* collected on C1D1.
  Flagged for page verification.

Table 1, header rows — merged spans recorded with `is_merged_cell` / `merged_cell_range`:
row 1: `Screening` 2:3, `On-Treatment` 4:15; row 2: `Screening` 2:3,
`Cycle = 21 days` 4:6, `Cycle = 21 days (or 28 days for Cohorts B & C per combination regimen)` 7:15;
row 3: `(Day Relative to C1D1)` 2:3, `Cycle 1` 4:6, `Cycle 2` 7:10, `Cycle 3-n` 12:15;
row 4: `(Day Relative to C1D1)` 2:3, `(±3 days)` 4:6 / 7:10 / 12:15.

**Vertical merges in the Table 1 header.** Three header cells are merged *vertically*, which the schema
cannot express: `Screening` and `Post-Treatment` span header rows 1–2, and `(Day Relative to C1D1)` spans
header rows 3–4 (confirmed by the absence of a horizontal rule at the row boundary within those column
ranges). Following the §5 vertical-merge convention, the value is emitted on **each covered row**, so
`Screening`, `Post-Treatment` and `(Day Relative to C1D1)` each appear twice in `schedule_grid`. This is
intentional duplication, not a double-read.

Table 2 has one vertical merge: the column-1 label cell spans header rows 1–2 with the word `Visit`
printed on the row-2 line (see §4).

## 4. Synthesised values

**Synthesised `property_name` (Table 1).** The entire header block of column 1 is one blank merged cell,
so all five schedule-property rows needed a synthesised name (`property_name_source.cell_value: ""`,
`synthesized: true`): `Study Epoch` (row 1), `Cycle Length` (row 2), `Cycle` (row 3), `Visit Window`
(row 4), `Study Day` (row 5).

**Synthesised `property_name` (Table 2).** Row 1 (`Study Treatment` / `Follow-Up`) is named `Study Period`.
The label cell that would name it is merged vertically across header rows 1–2 and reads `Visit`, which
belongs to the visit-number row; row 2 therefore takes `Visit` from the source and row 1 is synthesised.

**Synthesised annotation markers.** The source prints exactly **one** marker in each of Tables 1 and 2
(superscript `a`), and none in Table 3. Every other annotation is an unmarked Instructions-column cell or
an unmarked note, so a marker was synthesised and linked positionally:

- Table 1: `n1`–`n22` in printed order (`n1` = the "Note:" paragraph above the table; `n2` = the day-row
  Instructions cell; `n3`–`n22` = the body rows' Instructions cells).
- Table 2: `m1`–`m3` for the three body rows' Instructions cells.
- Table 3: `gi1` for the "General Instructions" block printed above the table.

Two annotations are anchored to content printed **outside the table frame** and are flagged as judgement calls:

- `n1` (Table 1) — "Note:  For applicable participants who have their combination therapy held while SC weekly ramucirumab is continued, procedures noted in column DX are to be performed at the time of the additional ramucirumab doses." It is printed between the section heading and the table, and it names a column of the table explicitly, so it was bound to the `DX` header cell (`schedule_cell`, row 5, column 11) with `method: "synthesized"`.
- `gi1` (Table 3) — the "General Instructions" block, which ends "…at the same time as a scheduled PK time point (see table below), then only the scheduled sample will be taken." Bound table-scope to the single `schedule_property` row with `method: "synthesized"`.

A reviewer who considers out-of-frame prose out of scope should drop these two; nothing else depends on them.

## 5. Mechanical mark-check

**Method.** For each page: render at 200 dpi; vertical rules taken as image columns with high ink fraction
over the table height (Table 1: x = 215, 393, 469, 537, 607, 674, 749, 811, 874, 949, 1024, 1198, 1260,
1323, 1398, 1473, 1599, 2016 → 17 columns); horizontal row bands taken from the ink runs **inside the
right-hand Instructions column and the activity-label column** (both always contain text, never shading —
§1d's warning about reading bands from a filled column applies here to the grey "not applicable" shading,
which fills a cell edge to edge exactly like a redaction bar). Then, per body band, the *internal* vertical
rules were re-detected within that band only, giving each row's real cell segmentation and hence every
merged span; each resulting cell was scored by counting pixels below intensity 90.

**Separation achieved.** A single `X` glyph yields 60–77 near-black pixels; an empty cell yields 0; a
grey-shaded cell yields 0 near-black but thousands of mid-grey (150–238) pixels; a merged
"See instructions" cell yields 460–500 near-black pixels spread over several column bands. The three
populations do not overlap anywhere in this document, so no threshold tuning was needed.

**Validation.** Every page was also read directly from the rendered image, and the detector output was
compared cell for cell. Dense rows checked explicitly: `Urinalysis` (14 marks, columns 3–16) and
`Vital signs` (11 marks + one 3-column merged text cell); sparse rows checked explicitly: `Coagulation`
(2 marks, columns 3 and 7) and `ECOG PS` (5 marks, columns 2, 4, 7, 12, 16). **No cell disagreed** between
the mechanical matrix and the visual read, in any of the three tables. The one place where the two methods
had to be reconciled is the DX shaded block in Table 1 row 30, described in §3.

Grey shading is treated as "not applicable" formatting, **not** as content: shaded cells are omitted from
`activity_schedule` exactly like empty white cells. This is a deliberate reading of the source's
convention and is worth a spot-check by a human, because it is the only place where the extraction
distinguishes "no mark" from "explicitly greyed out" and the schema has nowhere to record the difference.

## 6. Low-confidence calls

1. **Table 3 `subsidiary` vs `reference`** (§1). Rows are `Sample #` values, which the taxonomy lists as a
   `reference` example; the §2 PK-sampling note directs classification by function, which gives `subsidiary`.
   Recorded in `table_metadata.notes`.
2. **Table 3 label-column count.** `L = 1` (`Sample #` only), so `Study Cycle`, `Day within Cycle` and
   `Collection Time Point Relative to Ramucirumab Weekly Dose` are kept as **data columns 2–4** with their
   per-row values as `cell_value`s. The alternative reading (`L = 4`, data columns 5–6) is defensible —
   those three columns do label the row rather than schedule it — but it would silently discard every
   timing value in the table, so the data-column reading was chosen. If a reviewer prefers `L = 4`, every
   `column_position` in Table 3 shifts by 3.
3. **`property_type` of Table 1 header row 2** (`Cycle = 21 days` / `Cycle = 21 days (or 28 days for Cohorts B & C per combination regimen)`).
   Typed `period` (cycle-length definitions delimiting sub-phases of On-Treatment). `epoch` is arguable
   because the vertically merged `Screening` and `Post-Treatment` cells also occupy this row.
4. **`property_type` of Table 3's single header row.** Typed `other`: the row mixes per-row timing labels
   with two sample-type labels and matches no single enum value.
5. **`hierarchical_level: null` on Table 1 header row 4** (`(±3 days)`). Every cell in the row carries the
   same value, so removing the row would not make any two columns indistinguishable (§3). `structure_method:
   "inferred_from_layout"` recorded.
6. **Indentation of `ECOG PS` (Table 1 row 14).** Its label is indented to exactly the same x-offset as
   `PK` and `IG` (26 px in from the other labels), but unlike PK/IG it has no group-header row above it —
   the preceding row is the flush-left activity `AE collection`. It is recorded as
   `indentation_level: 0`, because `1` would assert a parent-child relationship with `AE collection` that
   the page does not support. `PK` and `IG` keep `indentation_level: 1` under the `Sample collection`
   section-header row (row 26), which is fully shaded across all data columns and carries no marks.
7. **Rows not emitted.** The table's own title bar row (spanning all 17 columns) is captured as
   `table_metadata.table_title` rather than as a property row; the grey `Procedure` band that reprints on
   every page of Table 1 and once in Table 2 is the activity column's label and is emitted neither as an
   activity (§4) nor as a schedule property (it defines nothing temporal). Likewise the word `Instructions`
   printed in the notes column of the header is a column label, not a schedule value.
8. **Sentence spacing.** The source sets two spaces after most sentence-ending periods. This was
   transcribed as seen where the render shows the wider gap; because there is no text layer, spacing is a
   visual judgement and may differ by a single space in places.

## 7. Annotation text integrity

The source is **not** glyph-spread — there is no text layer at all, so every annotation, activity name and
header label in all three files was read visually from the 200-dpi render. No field was reconstructed from
a glyph stream.

**Containment pairs (§7/§8) — re-verified against the page.** Table 1 annotation `n11`
("See Appendix 2.") is textually contained in four others: `n12`, `n13`, `n14` and `n15`. **All four are
source-faithful, not a split note cell.** Verified by re-reading each Instructions cell against its own
rule-bounded band on the rendered page:

- Hematology (row 16) and Clinical chemistry (row 18) each contain exactly `See Appendix 2.` and nothing
  else — these are the two locations of the single `n11` annotation (deduplicated by text per §6).
- Coagulation (row 17) opens with the same sentence and continues: `See Appendix 2.  Perform at baseline, C2D1, D1 of every other cycle afterwards (C4D1, C6D1, etc.), and as clinically indicated.`
- Urinalysis (row 19): `See Appendix 2.  In addition, perform as clinically indicated.`
- Thyroid panel (row 21): `See Appendix 2.  Starting C1D1, then every 3 months thereafter.`
- Pregnancy test (row 20) *ends* with `See Appendix 2.` after the page-break continuation.

Nothing was merged, truncated or dropped to remove the containment.

**Typing of pointer-only notes (§8).** Two Table 1 annotations whose entire text is a bare cross-reference
are typed `source_note`: `n11` (`See Appendix 2.`) and `n20` (`See Section 1.3.1 for PK and IG.`).
Everything else points *and* explains, so it stays `footnote`. `by_type` is therefore
21 `footnote` / 2 `source_note` for Table 1 — not degenerate.

**Abbreviation blocks deliberately yield zero annotations (§6).** Table 1 (page 22) prints a 22-term
abbreviation block, Table 2 (page 23) a 5-term block and Table 3 (page 24) a 2-term block. None of those
terms is printed as a *marker* on a grid cell, a header cell or an activity label — they appear only inside
running text or inside longer labels (e.g. `PK` inside `Ramucirumab PK Collection`), which §6 says is word
overlap and not a marker. All three blocks were dropped rather than bound by `text_match` or `synthesized`.

**Notes bounded confidently.** Every Instructions cell was bounded by its own horizontal rules recovered
from the raster; no note was bounded by proximity, so no annotation carries
`annotation_text_source.method: "proximity_bounded"`. The only two cells whose extent required judgement
are the two that cross a page break (§2), and in both cases the shading fingerprint of the continuation
band settled it.

## 8. Orphan risk

- No annotation has an empty `marker_locations` array in any of the three files.
- Every marker recorded in a `marker_locations` entry also appears in that row's / cell's / property's
  `annotation_markers` string (checked mechanically).
- No `unresolved` marker locations were needed: every note sits in a rule-bounded cell beside exactly one
  activity row, or (for `a`, `n1`, `n2`, `gi1`) has an identifiable header target.
- The only markers *printed* in the source are `a` in Table 1 (superscript, on the header cell whose
  cleaned value is `Short-term follow-up`, column 16 of header row 3 — recorded on that column's
  `schedule_grid` cell per §6, not on the property row) and `a` in Table 2 (superscript, on the header
  cell whose cleaned value is `Follow-Up`, column 3 of header row 1). Both definitions
  **are** printed below their tables, so there is no referenced-but-undefined marker anywhere in this
  excerpt. Note that the two `a` footnotes are different texts in different tables (short-term follow-up
  vs continued-access follow-up); they are not duplicates.

## 9. Method provenance recorded (§1e)

Because the document has no text layer, non-default methods are recorded on essentially every interpreted
value rather than exceptionally:

| Field | Value | Where |
|---|---|---|
| `activity_name_source.method` | `visual_transcription` | all 43 activities in all three tables |
| `activity_name_source.indentation_method` | `visual_estimate` | all 25 Table 1 activities |
| `activity_name_source.indentation_method` | `assumed_flat` | all Table 2 and Table 3 activities (no hierarchy present) |
| `activity_schedule[].method` | `raster_pixel_detection` | every `X` mark (Table 1, 2, 3) |
| `activity_schedule[].method` | `visual_read` | every text cell (`See instructions`, `See Section 1.3.1`, and the Table 3 cycle/day/timepoint values) |
| `schedule_grid[].method` | `visual_read` | every header cell in all three tables |
| `annotation_text_source.method` | `visual_transcription` | all 28 annotations, each with a `note` recording that the cell extent came from raster-recovered rule lines |
| `schedule_property.structure_method` | `inferred_from_layout` | Table 1 header row 4; Table 2 header row 1 |
| `marker_locations[].method` | `synthesized` | all 27 unmarked-note locations (`n1`–`n22`, `m1`–`m3`, `gi1`) |
| `marker_locations[].method` | (absent = printed) | `a` in Table 1 and `a` in Table 2 |

No `proximity`, `proximity_bounded`, `text_match`, `glyph_reconstruction`, `deglyph_reconstruction` or
`unresolved` values were used.

## 10. Recommended spot-checks

1. Table 1 rows 27/28 (`PK`, `IG`): confirm on page 21 that the merged "See Section 1.3.1" cell really
   starts at C1D8 and that the C1D1 cell is empty (§3).
2. Table 1 row 30: confirm on page 22 that the DX column is the shaded gap between the two
   "See instructions" spans (§3).
3. The grey-shading convention: confirm that shaded cells are intended as "not applicable" and that
   omitting them from `activity_schedule` is correct (§5).
4. Table 3's classification and label-column count (§6.1, §6.2).
5. The two out-of-frame annotations `n1` and `gi1` (§4).
