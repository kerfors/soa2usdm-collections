# NCT04677179 — SoA extraction uncertainty report

Study J1P-MC-KFAH. Prompt v3.7.2 §7, schema `soa-table-extraction` v1.0.
Source: `NCT04677179_soa.pdf`, 30 PDF pages = document pages 17–46 (PAGEMAP.md,
doc_page(pdf i) = 46 - (30 - i)). Four tables, extracted by four separate agents; this report is
written from all four JSON files plus the PDF, so it also carries the cross-table checks in §5.

All document-page numbers below are PAGEMAP document pages. The printed footers in this excerpt run
one **lower** than the document page (document page 37 prints `36`, document page 42 prints `41`);
no page number in the four JSON files was taken from a footer.

---

## 0. What the report author re-derived independently

Before summarising the four agents' own statements I re-derived four things from the PDF, so the
numbers below are checked rather than repeated:

* **(a) Mark census.** `pdftotext -bbox` on all 30 pages, counting every token whose text is exactly
  `X`, then subtracting the `Fasting visit` header-row marks (that row reprints on every
  continuation page and belongs to `schedule_grid`, not to the activity marks). No token matching
  ^[Xx][*a-zA-Z0-9]?$ other than a bare `X` occurs anywhere in this document — there are no
  footnoted marks such as Xa — and no lower-case `x` is used as a mark.
* **(b) Comment-column recount.** 200 dpi render per page (`pdftoppm -r 200`), vertical rules from
  column ink fraction, the two rightmost bounding the `Comment` column, horizontal rules taken
  **inside** that column's x-range, then every text-layer token assigned to the band its centre falls
  in (§1d). This yields the number of non-empty note cells per page independently of the extractions.
  It failed on the three short final pages (documents 32, 42, 46 — the table there is only a few rows
  tall, so a whole-page ink-fraction threshold finds no rules); those three pages have a clean text
  layer and were read directly instead.
* **(c) Glyph-spread diagnosis** per page, from the fraction of single-character tokens.
* **(d) Visual reads** of rendered crops: document 20 (dense Table 1 page, cell-for-cell), documents
  37 and 41 (the Stool-Samples redaction bar in both tiles of Table 3), document 43 (Table 4 top),
  and the header blocks of documents 33 and 46.

**Mark census result — all four tables reconcile exactly.**

| doc | `X` tokens | on Fasting row | activity marks | doc | `X` tokens | on Fasting row | activity marks |
|---|---|---|---|---|---|---|---|
| 17 | 33 | 2 | 31 | 32 | 10 | 1 | 9 |
| 18 | 24 | 2 | 22 | 33 | 43 | 1 | 42 |
| 19 | 14 | 2 | 12 | 34 | 15 | 1 | 14 |
| 20 | 35 | 2 | 33 | 35 | 35 | 1 | 34 |
| 21 | 19 | 2 | 17 | 36 | 8 | 1 | 7 |
| 22 | 27 | 2 | 25 | 37 | 16 | 1 | 15 |
| 23 | 20 | 2 | 18 | 38 | 41 | 1 | 40 |
| 24 | 36 | 1 | 35 | 39 | 16 | 1 | 15 |
| 25 | 24 | 1 | 23 | 40 | 24 | 1 | 23 |
| 26 | 13 | 1 | 12 | 41 | 13 | 1 | 12 |
| 27 | 16 | 1 | 15 | 42 | 10 | 1 | 9 |
| 28 | 39 | 1 | 38 | 43 | 26 | 0 | 26 |
| 29 | 16 | 1 | 15 | 44 | 20 | 0 | 20 |
| 30 | 21 | 1 | 20 | 45 | 8 | 0 | 8 |
| 31 | 14 | 1 | 13 | 46 | 0 | 0 | 0 |

* Table 1 (17–23): census 158 — JSON `activity_schedule` 158, and the **per-page** split agrees row
  for row (17:31, 18:22, 19:12, 20:33, 21:17, 22:25, 23:18).
* Table 2 (24–32): tile A (24–27) 85, tile B (28–32) 95, total 180 — JSON has exactly 85 marks in
  positions 2–11 and 95 in positions 12–21, total 180.
* Table 3 (33–42): tile A (33–37) 112, tile B (38–42) 99, total 211 — JSON has exactly 112 in
  positions 2–11 and 99 in 12–21, total 211.
* Table 4 (43–46): 54 — JSON 54. Document 46 carries zero marks (see §4).

**Comment-column recount result — all four annotation counts reconcile exactly.** Details per table
below.

**Glyph-spread diagnosis.** Letter-spaced pages: 18, 21, 22, 23, 24, 26, 27, 28, 30, 31, 33, 35, 36,
37, 38, 40, 41, 43, 44, 45. Clean pages: 17, 19, 20, 25, 29, 32, 34, 39, 42, 46. **Every one of the
four agents' glyph-spread page lists matches this exactly** (Table 1 claimed 18/21/22/23; Table 2
"Doc 24/26/27/28/30/31 are glyph-spread; 25/29/32 clean"; Table 3 "document pages 34, 39 and 42 are
clean, the other seven are letter-spaced"; Table 4 claimed 43–45 glyph, 46 clean). A representative
line of the raw layer, document 26: `F asti n g s a m pl es ar e pr ef err e d, b ut if` —
`p arti ci p a nt is n onf asti n g at ti m e of`.

---

## 1. Table 1 — `main_soa`, document pages 17–23

**Type.** `main_soa`, and the discriminators are clean: it is the primary anchor grid (V1–V9,
weeks −5 to 12, study days 1–85), nothing precedes it so it is not a `continuation`, no other table
shares its columns so it is not `domain`, and it gives no finer timing for another table's activity
so it is not `subsidiary`. It is also the only one of the four tables whose header block carries no
population restriction sentence — Tables 2 and 3 print one, Table 4 is defined by its encounter
columns. Header block on all seven pages, verbatim and byte-identical page to page (whitespace
normalised): `For procedures at an ETV, see ETV in Table 4.` / `For procedures at an unscheduled
visit, see V997 in Table 4.`

**Columns.** L = 1 leading label column; first data column is position **2**. V1…V9 occupy
positions 2–10 → **9 data columns**. The right-hand `Comment` column is source position 11 and is
excluded from `schedule_grid`/`activity_schedule` per §6; each non-empty cell became a `footnote`.
Physical column count on the page is therefore 11.

**Activities: 63.** Rows per page — **17:12, 18:8, 19:6, 20:13, 21:7, 22:9, 23:8**. Every page in the
declared range contributed rows; there is no §4 exception to declare for this table.

**Marks: 158.** No merged marks, no arrows, no spanned text cells: every mark is a lone `X` in one
column, and `source_range` is set nowhere in the file. The two-row redaction bar on document 23 is a
merged *label* cell, not a merged mark — each of the two rows carries its own marks.

**Mechanical mark-check.** Declared method: raster rule-line cell geometry at 200 dpi plus bbox
column-binning, cross-checked cell-for-cell against a near-black-pixel detector, zero disagreements.
My independent census agrees at every page (§0), and my cell-for-cell visual read of document 20
agrees on all 11 body rows: PGI-C [10], QIDS-SR16 [3,10], Clinician-Administered Questionnaires
(Paper) [], PGA [2,10], C-SSRS [2], Self-Harm Supplement [2], Self-Harm Follow-Up [2],
Laboratory Tests and Sample Collections [], Hematology and Clinical chemistry [2…10] (all nine),
Lipid panel [3,10]. **No cell where the mechanical matrix disagreed with the visual read.**

**Row bands read from the Comment column, not the label column.** Correct and important here: the
label cells of the six redacted rows are solid black bars that read as horizontal rules and would
have merged the two redacted rows on document 23 into one (§1d).

**Synthesised.** No `property_name` is synthesised — all five header rows have printed labels
(`Visit number`, `Weeks from randomization`, `Study day`, `Visit interval tolerance (days)`,
`Fasting visit`). **All 32 annotation markers are synthesised**: `hn1`, `sn1`, `sn2` on the header
block (bound to `schedule_property` row 1, method `synthesized`) and `c1`–`c29` for the Comment-column
notes (bound to their activity rows, method `synthesized`). No footnote marker of any kind is printed
anywhere in this table.

**Annotation-text integrity.** 18 of the 32 annotations carry
annotation_text_source.method = "deglyph_reconstruction" — exactly the notes that sit on the
letter-spaced pages 18/21/22/23. My Comment-column recount finds **30 non-empty note cells** across
the seven pages (17:5, 18:5, 19:2, 20:4, 21:5, 22:6, 23:3), of which two on document 22 carry
identical text; 30 − 1 duplicate = **29 distinct Comment notes + 3 header-block notes = 32**, which is
exactly the file's annotation count.

*Overlapping / contained annotation pairs — all three re-verified on the page, all three
source-faithful, none is a split cell:*

1. **`c9` ⊂ `c8`** (document 18). `c8` is the Physical examination cell ending `…including
   peripheral lymph nodes. See Section 8.2.2.`; `c9` is the Symptom-directed row's own cell whose
   whole content is `See Section 8.2.2.` In the document-18 text stream the two note texts are not
   adjacent: the activity label `Symptom-directed physical examination, including assessment for
   signs and symptoms of tuberculosis (TB), including peripheral lymph nodes` and that row's two
   marks are interleaved between them, which is only possible if they are two cells on two
   consecutive rows. **Genuinely printed that way.**
2. **`c10` ⊂ `c11`** (documents 18 → 19). `c10` is the ECG cell, whole content `Locally performed.`;
   `c11` is the Chest x-ray cell on the *next page*, ending `Lateral view may also be taken. Locally
   performed.` Different pages, so not a split cell by construction. **Genuinely printed that way.**
3. **`c16` ⊂ `c17`** (documents 20 → 21). `c16` is the Serum pregnancy cell, whole content `Perform
   only for women of childbearing potential and women with a history of tubal ligation.`; `c17` is
   the Urine pregnancy cell on the next page, which opens with that same sentence and continues `At
   dosing visits which have pregnancy testing…`. Verified on both rendered pages. **Genuinely printed
   that way** — this is the serum/urine pair the §8 checklist warns not to merge away.

**`c23` — one note, two locations.** The text `Collect samples before dosing, if dosing is
scheduled…` prints in two separate Comment cells on document 22 (the PK-samples row and the redacted
row beneath it). My rule-line recount independently finds it twice on that page. Correctly deduped to
one annotation with two `marker_locations` (§6), with `c23` present on both rows'
`annotation_markers`.

**Low-confidence calls.**

* **`Fasting visit` modelled as a `schedule_property`, property_type: "modality",
  hierarchical_level: null, structure_method: "inferred_from_layout".** It is a bold row in a grey
  band inside the reprinted header block, i.e. it is styled exactly like the grey section-header
  activity rows (`Stool Samples`, `Randomization and Dosing`). Treating it as a property is
  defensible and all four tables did so — but Table 1 is the only one that typed it `modality`
  (Tables 2–4 typed it `condition`) and the only one that recorded a `structure_method`. See §5.
* **Six redacted rows** (documents 18, 21, 22 ×2, 23 ×2), all carrying marks, transcribed as
  activity_name: "CCI" and never repaired. The two on document 23 sit under **one** bar with `CCI`
  printed once, so row 65 has cell_text: "" and activity_name_source.method:
  "visual_transcription". Consistent with what the text layer shows (document 23 carries exactly one
  `CCI` label).
* **The `c12` diary note is a flattened bullet list.** The source prints it as a lead-in plus eight
  bullet lines (`Check compliance for` / `Stool Frequency` / `Rectal Bleeding` / `Nocturnal Stools` /
  `Bristol Stool Scale` / `Urgency NRS` / `Abdominal Pain NRS` / `Fatigue NRS, and` / `Patient Global
  Rating of Severity (PGR-S).`). The extraction joins them with single spaces, giving `…Bristol Stool
  Scale Urgency NRS Abdominal Pain NRS Fatigue NRS, and…`, which does not read as running prose. No
  text was invented or dropped, but a resolver should not treat this as a sentence. The same
  flattening appears in Tables 2 and 3 (§5).

**Orphan risk: none.** All 32 annotations have ≥1 `marker_locations` entry, and every marker in a
location also appears in that row's `annotation_markers` (checked mechanically over all four files).
The 15-term abbreviation block on document 23 yielded **zero** annotations, which is the §6 outcome:
no term appears as a marker on a grid cell, a header cell or an activity label.

**Method provenance (non-default).** activity_name_source.method: `glyph_reconstruction` ×31,
`visual_transcription` ×1 (row 65). indentation_method: "font_signal" ×63 (indentation here is
carried by grey shading and bold, not whitespace). annotation_text_source.method:
"deglyph_reconstruction" ×18. marker_locations[].method: "synthesized" ×33 (every location).
schedule_property.structure_method: "inferred_from_layout" ×1 (Fasting visit). No cell-level
`method` recorded — marks were read from the text layer and pixel-checked, which is the default.
**No location_type: "unresolved" anywhere.**

---

## 2. Table 2 — `track` "Responders", document pages 24–32

**Type.** `track` with track_label: "Responders". Supported on the table's own face: `This schedule
applies to study participants who are responders at Week 12.` prints under the title on all nine
pages, and Table 3 prints the exact complement for nonresponders. The two tables carry numerically
identical V10–V29 / weeks 14–52 / days 99–365 columns, which is precisely the reused-numbering trap
in §2 — mutually exclusive populations make each a `track`, not `domain`.

**Columns.** Horizontally **tiled**: tile A = V10–V19 (documents 24–27), tile B = V20–V29 (documents
28–32, title `Table 2 (continued). Schedule of activities for the Maintenance Treatment Period of
Study J1P-MC-KFAH`, note the lower-case *activities*). Unioned into one 20-column logical table,
positions 2–11 = V10–V19, 12–21 = V20–V29. L = 1, first data column 2. Each printed tile is 12
physical columns (label + 10 visits + Comment). The `Comment` column has **no** `column_position` in
the unioned model; the file records this divergence from the agreed structure explicitly rather than
silently, which is the right call.

**Activities: 40.** Rows per page — **24:10, 25:8, 26:6, 27:10, 28:1, 29:3, 30:0, 31:2, 32:0**.

**Declared §4 exception (correct).** Documents **30 and 32 contribute no activity rows**, because
every row printed there also prints in tile A and was first read there. They still supply marks:
document 30 supplies 20 activity marks (the V21–V29 labs block: Hematology, Clinical chemistry,
Lipid panel, Urinalysis, Urine pregnancy (local), the redacted row between them, HBV DNA) and
document 32 supplies the nine V20–V28 `Dosing` marks plus the `No dosing at V29.` note. My census
confirms 20 and 9 respectively. This is a declared tiling exception, not a skipped page.

**Marks: 180**, tile A 85 + tile B 95, matching the census exactly. No merged marks, no arrows, no
`source_range` anywhere.

**Per-tile row differences — verified on the page.** Six rows print in tile B only, and I confirmed
each by searching both tile spreads: `Review modified Mayo score (MMS)`, `Diary return`,
`Clinician-Administered Questionnaires (Paper)`, `Physician's Global Assessment (PGA)`, `Endoscopy`,
`Colon biopsy sample collection`. Zero rows are tile-A only. The endoscopy absence is explained in the
source itself: the tile-A `Endoscopic Procedure` row carries the Comment `Not applicable for
responders during the time period of V10 through V19` (printed with no closing full stop, and
transcribed that way). All six tile-B-only rows carry marks only in positions 12–21, as they must.

> **Finding (notes vs data, Table 2).** table_metadata.notes says `33 of the 40 body rows print in BOTH tiles…` and `Seven rows print in only one tile:` and
> then names **six**. The data are right (34 rows in both tiles, 6 tile-B-only — the JSON's own
> `source_page` distribution gives 6 rows on tile-B pages); the prose count is off by one. Prose
> only; no cell is affected.

> **Finding (notes vs data, Table 2).** table_metadata.notes says `eight body rows are redacted to a black bar labelled 'CCI'…`. The file contains **six** such rows (row_positions
> 14, 31, 34, 37, 42, 43), which is what the page shows: five `CCI` labels print in tile A and five in
> tile B, and one of those five in each tile is a two-row-tall bar. Six is correct and matches
> Tables 1, 3 and 4; the word "eight" is wrong. Prose only.

**Merged label cell.** Under `Stool Samples`, one two-row-tall bar covers rows 42 and 43; the file
records `Both rows carry X at V15, V21 and V29` and the JSON has both rows at positions [7, 13, 21].
Row 43 is given activity_name_source.method: "visual_transcription" but keeps cell_text: "CCI",
where Tables 1 and 4 give the label-less row cell_text: "". Minor provenance inconsistency (§5).

**Synthesised.** No synthesised `property_name`. All 19 markers synthesised: `h1`–`h6` on the header
block (all bound to `schedule_property` row 1) and `c1`–`c13` on activity rows. No printed markers.

**Annotation-text integrity.** 11 of 19 annotations carry `deglyph_reconstruction`. My Comment-column
recount finds 12 note cells in tile A and 12 in tile B, collapsing to 12 distinct texts, plus
`No dosing at V29.` on document 32 = 13 Comment notes, + 6 header-block notes = **19**, exactly the
file's count.

*Overlapping pairs — both re-verified against the pages, both source-faithful:*

1. **`h2` / `h3`** — `For procedures at an ETV, see ETV in Table 4.` (tile A, documents 24–27) versus
   `For procedures at an early termination visit, see ETV in Table 4.` (tile B, documents 28–32). I
   diffed the whitespace-stripped header block of document 24 against document 28 and of document 27
   against document 32: the sponsor really does re-word this line in the continued spread. **Genuinely
   printed that way** — a source variant pair, not one note split across rows.
2. **`h5` / `h6`** — the remote-visit note, identical except that tile A prints
   `…or a combination thereof), if written approval…` and tile B prints `…or a combination thereof) if
   written approval…`. Same diff, same conclusion. **Genuinely printed that way.**

   Keeping both variants is source-faithful and is the safer of the two options, but a resolver will
   see four near-duplicate header notes on one `schedule_property` row; they should be treated as two
   notes with two printings, not four requirements.

**Low-confidence calls.**

* `Fasting visit` → `schedule_property`, property_type: "condition", no `structure_method`. Its two
  marks (V15 at position 7, V29 at position 21) are in `schedule_grid`, not among the 180 activity
  marks — correct, and confirmed against the page.
* This is the only file that emits `schedule_grid` cells with cell_value: "" for the unmarked
  columns of the Fasting row (20 cells, 18 of them empty). Harmless but inconsistent with the other
  three (§5).
* `Genetics sample` (row 36) carries the `c10` note but **no marks** in either tile. That is what the
  page shows (the note says the sample `can be obtained any time at or after V2`), not a dropped mark.

**Orphan risk: none.** All 19 annotations have locations; all markers are present on their rows.
Abbreviation block on document 32 → zero annotations (§6).

**Method provenance (non-default).** `glyph_reconstruction` ×28, `visual_transcription` ×1 (row 43);
indentation_method: "font_signal" ×40; `deglyph_reconstruction` ×11; marker_locations[].method:
"synthesized" ×20 (every location). **No unresolved locations.**

---

## 3. Table 3 — `track` "Nonresponders", document pages 33–42

**Type.** `track` with track_label: "Nonresponders"; `This schedule applies to study participants
who are nonresponders at Week 12.` prints under the title on all ten pages. Same reasoning as Table 2,
mirrored.

**Columns.** Tiled: tile A = V10–V19 (documents 33–37), tile B = V20–V29 (documents 38–42), unioned
to positions 2–21. L = 1, first data column 2, `Comment` excluded as a notes column. 12 physical
columns per printed tile.

**Activities: 40.** Rows per page — **33:11, 34:7, 35:7, 36:8, 37:6, 38:0, 39:1, 40:0, 41:0, 42:0**.

**Declared §4 exception (correct).** Documents 38, 40, 41 and 42 contribute no rows; document 39
contributes exactly one. Marks supplied by those pages: 40, 23, 12 and 9 respectively (document 42's
nine are the V20–V28 `Dosing` marks), and document 39 supplies 15 marks while contributing its single
row. Census agrees.

**Marks: 211**, tile A 112 + tile B 99. No merged marks, arrows or spanned text; the file states
explicitly `Marks are single-cell throughout - no merged or arrow-spanned marks exist in this table,
so no source_range is set anywhere.`

**Per-tile row difference — verified.** `Diary return` (row 18) prints only in tile B (document 39),
with a single mark at V29 (position 21). I confirmed by searching both spreads: tile A has no
`Diary return` row, tile B has one. Every other body row prints in both tiles — including
`Endoscopy`, `Colon biopsy sample collection`, `Review modified Mayo score (MMS)`,
`Clinician-Administered Questionnaires (Paper)` and `Physician's Global Assessment (PGA)`, all of
which are tile-B-only in Table 2. That asymmetry is real (see §5).

**Mechanical mark-check.** Declared: 200 dpi rule-line bands × bbox token binning, diffed against
visual reads of rendered crops for named dense and sparse rows, `no disagreement was found`. My census
agrees per tile. I additionally read the Stool-Samples block of documents 37 and 41 visually: two
mark rows under one bar in **both** tiles, marks at V12/V16 (positions 4, 8) in tile A and V22/V29
(positions 14, 21) in tile B — the file has rows 42 and 43 each at [4, 8, 14, 21], which matches.

**Synthesised.** No synthesised `property_name`. All 17 markers synthesised: `t1`, `t2`, `t3`, `pr2`,
`pr3` on the header block, `pr1` and `n1`–`n11` on activity rows. `t2` (the V16 multi-day note) is
bound to the V16 header cell — `schedule_cell` row 1, `column_position` 8 — by method: "text_match",
which is the §6 header-cell treatment and is right: the note names V16 and the grid confirms V16 is
position 8 (`Fasting visit` also marks V16 in this table).

**Annotation-text integrity.** 10 of 17 carry `deglyph_reconstruction`. My Comment-column recount
finds 11 distinct note texts in tile A (33: four; 34: one; 35: two; 36: three distinct, one of them
printed in two adjacent cells; 37: one) and the same 11 in tile B, plus `No dosing at V29.` on
document 42 = 12 Comment notes, + 5 header-block notes = **17**, exactly the file's count.

**`n8` — one note, two locations.** `Collect samples before dosing…` prints in two adjacent Comment
cells on document 36 (PK-samples row and the redacted row below it) and again in two adjacent cells on
document 41. Deduped to one annotation with two locations. **Source-faithful repeat, not a split
cell** — verified: the rule-line recount finds the text twice on document 36 and twice on document 41,
i.e. four printings of one note across two tiles of two rows.

No containment pair exists in this table (checked mechanically: no annotation's text is a substring of
another's, and no pair shares a run of ≥40 characters).

> **Finding (provenance gap, Table 3).** The file states that no markers are printed in this table
> and that its markers were synthesised — but **13 of its 18 `marker_locations` carry no `method`
> field at all** (`pr1` and `n1`–`n11`, plus one more), where absent means "the default: a scope
> established by a printed marker" (§1e). Tables 1, 2 and 4 record method: "synthesized" on every
> such location. The bindings themselves are right (each marker also appears in its row's
> `annotation_markers`), so nothing resolves incorrectly; but the provenance is understated and a
> validator comparing tables will see Table 3 claiming printed markers that do not exist. `t1`, `t3`,
> `pr2`, `pr3` do carry `synthesized`, and `t2` carries `text_match`.

> **Finding (unflagged source variant, Table 3).** The tile-A and tile-B header blocks differ, and one
> difference is not recorded. Tile A ends the remote-visit note `…on-site study visit occurring.`; tile B ends it `…on-site study visit occurring` with no full stop (documents
> 38–42; verified by diffing the whitespace-stripped header blocks of documents 33 vs 38 and 37 vs
> 42). `t3` carries the tile-A form only. This is the mirror image of Table 2's handling, where both
> variants were kept as `h5`/`h6` (§5). The other tile difference — the `V16 procedures may be conducted over more than 1 day…` note printing only in tile A — **is** correctly captured, as `t2`.

**Low-confidence calls.**

* `Fasting visit` → `schedule_property`, property_type: "condition"; its X marks are at V16
  (position 8) and V29 (position 21). Verified visually on document 33; note this differs from
  Table 2's V15/V29 — a genuine difference between the two tracks, not a transcription slip.
* Rows 6–9 (`Concomitant medications`, `Adverse events`, `Review modified Mayo score (MMS)`,
  `Tobacco use`) are indentation_level: 0 **and carry marks**. §4 says level-0 rows are grouping
  headers that carry no marks, with an exception for flat tables. This table is not flat (it has
  eight mark-free grey section headers at level 0), so these four rows are level-0 rows with marks:
  they precede the first shaded section header and have no parent. Table 4 made the opposite choice
  for the same four rows (§5). Neither is wrong; the resolver needs to know they disagree.
* Six redacted `CCI` rows (14, 31, 34, 37, 42, 43), all carrying marks, transcribed as printed. Both
  of rows 42/43 are recorded with cell_text: "CCI" and method: "glyph_reconstruction", but on
  document 37 only **one** `CCI` label is printed, over a two-row-tall bar — I rendered the page to
  confirm. Tables 1, 2 and 4 all recorded the second row of the equivalent bar as
  `visual_transcription`. The row itself is real; only the provenance is optimistic (§5).

**Orphan risk: none.** Every annotation has ≥1 location and every marker is on its row. Abbreviation
block on document 42 → zero annotations.

**Method provenance (non-default).** `glyph_reconstruction` ×32; indentation_method: "font_signal"
×40; `deglyph_reconstruction` ×10; marker_locations[].method: `synthesized` ×4, `text_match` ×1,
**absent ×13** (see the finding above). **No unresolved locations.**

---

## 4. Table 4 — `track` "Early Termination / Unscheduled / Post-Treatment", document pages 43–46

**Type.** `track`. The four encounter columns — ETV, V997, V801, V802 — exist in no other table in
the excerpt; it is the shared destination for participants leaving any of Tables 1–3, so it is not
`domain` (different columns, different attendance) and not `subsidiary` (it is not finer timing for
one existing activity). `track_label` is four words and takes its wording from the table title
(`Table 4. Schedule of Activities for Early Termination, Unscheduled Visits, and Post-Treatment
Follow-Up Period`), which is what §2 asks for.

**Columns.** L = 1, first data column **2**: ETV=2, V997=3, V801=4, V802=5 → **4 data columns**.
The `Comment` column is source position 6 and is excluded from the grid; six physical columns. Not
tiled.

**Activities: 39.** Rows per page — **43:15, 44:12, 45:10, 46:2**. All four pages contributed rows.
Document 46 contributes the `Randomization and Dosing` band and the `Dosing` row, and carries **no
marks at all** — consistent with its own Comment cell, `No dosing at ETV, V997, or post-treatment
follow-up visits.` So `Dosing` is a row with a note and zero marks; that is data, not an omission.

**Marks: 54.** No merged marks, arrows or vertical merges: every mark is a lone `X` in one column,
and no `source_range` value is set (the key exists in the schedule objects but is never populated). My
visual read of document 43 agrees cell for cell with the file: Concomitant medications [2,3,4,5],
Adverse events [2,3,4,5], Review modified Mayo score (MMS) [2], Tobacco use [2], Physical
Evaluation [], Weight [2,4], Vital signs [2,3,4,5], Symptom-directed physical examination
[2,4,5].

**Mechanical mark-check.** Declared: bbox column-binning cross-checked cell-for-cell against
near-black pixel detection (<90) in raster row × column bands, `marked cells return 76-89 dark pixels
and empty cells return 0`, zero disagreements. My independent census agrees per page (26/20/8/0).
**No disagreement between mechanical and visual reads.**

**Synthesised.** No synthesised `property_name`. All 16 markers synthesised, `n1`–`n16` in printed
order. Two are bound by method: "text_match" to the header cell they name, which is the §6
header-cell rule applied correctly: `n1` (`V802 is only for randomized participants who were positive
for anti-HBc at screening. All other participants will have their final visit at V801 or ETV.`) to
`schedule_cell` row 1 column 5 = V802, and `n2` (`V997: Additional study procedures can be performed
at the discretion of the investigator.`) to row 1 column 3 = V997. Both column bindings are correct
against the printed header. `n3` and `n4` are bound to the `Weeks from randomization` and
`Fasting visit` property rows; the remaining 13 to activity rows with method: "synthesized".

**Annotation-text integrity.** 11 of 16 carry `deglyph_reconstruction` (documents 43–45), 5 carry
`raster_band_cells` (the header/property-band notes and the document-46 dosing note). My rule-line
recount over documents 43–45 finds 13 distinct note texts once the two notes that reprint on every
page (`n3`, `n4`) are counted once and the note printed twice (`n12`) is counted once; + `n1`, `n2`
from the header block + `n16` from document 46 = **16**, exactly the file's count.

**`n12` — one note, two locations.** `Collect samples before dosing…` prints in the PK-samples
Comment cell on document 44 and again in the redacted row's cell on document 45. My recount finds it
on both pages. Deduped correctly.

*Overlapping pair — re-verified, source-faithful:*

* **`n14` (Endoscopy) / `n15` (Colon biopsy sample collection)**, document 45. They share their
  opening sentence (`Recommended at ETV based on judgment of the investigator and after discussion with the sponsor’s medical monitor.`) and their closing sentence (`If not performed at ETV, this will not be considered a protocol deviation.`), with `n15` inserting the lab-manual and
  sigmoidoscopy sentences in between. Neither contains the other. In the document-45 text stream the
  two passages are separated by the label `Endoscopy` and its mark, and the second is broken by the
  label `Colon biopsy sample collection` and its mark — the row labels and marks interleave with the
  note text, which proves two distinct rule-bounded cells rather than one note wrapped across rows.
  The file also cites the rule lines (`y=342.2 / 394.6 / 486.4 pt`). **Genuinely printed that way,
  not a split note.**

**Low-confidence calls.**

* **Indentation.** Rows 6–9 are given indentation_level: 1 `because level 0 is reserved here for the
  shaded/bold organizational rows that carry no marks`. Tables 1–3 give the same four rows level 0.
  Both readings are defensible; they are not the same reading (§5).
* **`n6` merges two paragraphs into one annotation.** The Vital signs Comment cell on document 43
  prints `Blood pressure, body temperature, and pulse rate. Vital signs should be measured after
  participant has been sitting for at least 5 minutes.` and, on a new line, `V997: Vital signs
  collection is optional.` I confirmed on the rendered page that these are one rule-bounded cell, so
  one annotation is correct — but the second sentence is column-specific (V997) while the first is
  not, and a resolver may want to split it.
* **Six redacted rows** (14, 30, 33, 36, 41, 42). Rows 41/42 sit under one bar with `CCI` printed
  once; row 42 has cell_text: "" and `visual_transcription`. The two-row reading is supported by the
  Comment-column rules the file cites and by the text layer (document 45 prints three `CCI` labels for
  four redacted rows).
* `Fasting visit` → `schedule_property`, property_type: "condition", with **no** `schedule_grid`
  cells at all — the row's only content is the Comment `No fasting in this period`, captured as `n4`.
  Reasonable; note the other three tables emit grid cells for this row.

**Orphan risk: none.** All 16 annotations have locations; all markers are on their rows, including
the two `schedule_cell` bindings. Abbreviation block on document 46 → zero annotations.

**Method provenance (non-default).** `glyph_reconstruction` ×36, `visual_transcription` ×1 (row 42);
indentation_method: "font_signal" ×39; annotation_text_source.method: `deglyph_reconstruction`
×11, `raster_band_cells` ×5; marker_locations[].method: `synthesized` ×15, `text_match` ×2. **No
unresolved locations.**

---

## 5. Cross-table checks

### 5.1 Activities appearing in more than one table

35 distinct activity names appear in two or more of the four tables. **Every one of them is spelled
byte-identically in every table it appears in** — checked mechanically by comparing the raw strings
of all names that are equal after case/punctuation folding. That includes the awkward ones:
`Symptom-directed physical examination, including assessment for signs and symptoms of tuberculosis
(TB), including peripheral lymph nodes`, `Physician's Global Assessment (PGA)` (U+2019 apostrophe in
all four), `QIDS-SR16`, `Review modified Mayo score (MMS)`, `Pharmacokinetic (PK) samples`,
`12-lead electrocardiogram (ECG)`, `Urine pregnancy (local)`, `Clinician-Administered Questionnaires
(Paper)`. No harmonisation is needed for the row set.

Twenty-four names are unique to Table 1 (the screening-only rows: `Informed consent`, `Demographics`,
`Height`, `Chest x-ray (posterior-anterior view)`, `Serum pregnancy`, `Follicle-stimulating hormone
(FSH)`, `Tuberculosis (TB) test`, the HIV/HCV/HBV screening tests, `Stool culture`,
`C. difficile testing`, `Randomization`, and so on). Tables 2, 3 and 4 introduce no name that does not
also appear elsewhere.

**Structural disagreements on shared rows** (same row, different modelling):

| Item | Table 1 | Table 2 | Table 3 | Table 4 |
|---|---|---|---|---|
| `Fasting visit` `property_type` | `modality` | `condition` | `condition` | `condition` |
| `Fasting visit` `structure_method` | `inferred_from_layout` | absent | absent | absent |
| `Fasting visit` grid cells | 2 (marked only) | 20 (18 empty strings) | 2 (marked only) | 0 |
| Rows 6–9 (`Concomitant medications`, `Adverse events`, MMS, `Tobacco use`) | level 0, with marks | level 0, with marks | level 0, with marks | **level 1** |
| Label-less row of a two-row redaction bar | cell_text "" + `visual_transcription` | cell_text "CCI" + `visual_transcription` | cell_text "CCI" + **`glyph_reconstruction`** | cell_text "" + `visual_transcription` |
| marker_locations[].method on synthesised bindings | `synthesized` on all | `synthesized` on all | **absent on 13 of 18** | `synthesized`/`text_match` on all |

None of these changes a mark or a row; all four are pickable by a resolver, and all four are listed
here because a consolidated view will show them side by side.

### 5.2 Annotation markers reused across tables with different text

**Every synthesised marker in this study is table-local, and the same string means different things
in different tables.** This is the largest cross-table hazard in the set.

* `c1`–`c13` exist in **both** Table 1 and Table 2 with **completely different text** in each. For
  example `c5`: Table 1 = `For AESIs, additional data are collected (Section 8.3.6).`, Table 2 =
  `Check compliance for Stool Frequency Rectal Bleeding…`. All thirteen collide.
* `n1`–`n11` exist in **both** Table 3 and Table 4 with different text in each. For example `n1`:
  Table 3 = `For AESIs, additional data are collected (Section 8.3.6).`, Table 4 = `V802 is only for
  randomized participants who were positive for anti-HBc at screening…`.
* Conversely, **one note text carries four different markers**. `For AESIs, additional data are
  collected (Section 8.3.6).` is `c5` (T1), `c1` (T2), `n1` (T3), `n5` (T4). `See Section 8.2.2.` is
  `c9`, `c3`, `pr1`, `n7`. `Locally performed.` is `c10`, `c4`, `n3`, `n8`. `Only for participants who are positive for anti‑HBc at screening.` is `c22`, `c8`, `n7`, `n11`. `Collect samples before
  dosing…` is `c23`, `c9`, `n8`, `n12`. `Blood sample for DNA pharmacogenetics can be obtained any
  time at or after V2.` is `c24`, `c10`, `n9`, `n13`. Fifteen note texts in total are shared across
  tables under different markers.
* The prefix schemes also differ: Table 1 uses `hn`/`sn`/`c`, Table 2 `h`/`c`, Table 3 `t`/`pr`/`n`,
  Table 4 `n` only. Table 3 is the only one that uses the §6 `pr` convention (for `See Section
  8.2.2.`), where the other three treat the same note as an ordinary Comment-column note.

Since no marker is printed anywhere in this document, none of this is a transcription error — but
markers must **not** be treated as study-wide identifiers downstream, and any consolidation that keys
on marker strings will silently cross-link Table 1 to Table 2 and Table 3 to Table 4. §6 asks for
consistent naming across the tables of one study; that was not achieved here, and it cannot be fixed
by renaming without touching all four files together.

### 5.3 Wording variants of the "same" note across tables — all verified, all faithful

These look like transcription discrepancies and are not. Each was checked against the page:

| Note | Table 1 (documents 17–23) | Tables 2/3/4 |
|---|---|---|
| Vital signs | `…has been sitting at least 5 minutes.` (document 18) | `…has been sitting for at least 5 minutes.` (documents 24, 28, 33, 38, 43) |
| Lipid panel fasting | `…if participant is non fasting at time of collection…` (document 20, clean text layer) | `…nonfasting…` (documents 26, 30, 35, 40) |
| Pregnancy testing | `…designated on the SoA, if local laws…` (document 21, comma) | comma present in Table 2 (document 26), **absent** in Table 3 (document 35) and Table 4 (document 44) |
| `anti-HBc` hyphen | ASCII `-` on documents 21, 23 | **U+2011 non-breaking hyphen** on documents 22, 26, 30, 36, 40, 44; ASCII `-` again on documents 32, 42, 43, 45, 46 (and both forms occur on document 44) |

All four extractions transcribed what their page prints, including the U+2011, and Tables 2, 3 and 4
flagged the hyphen. One caveat for the reviewer: the `non fasting` / `nonfasting` distinction is
**only** securely readable on document 20, which has a clean text layer. On the letter-spaced pages
the glyph clusters read `p arti ci p a nt is n onf asti n g at ti m e of` — the de-glyph decision
there rests on cluster boundaries, not on a measurable space, and §1c's own warning applies. I regard
`nonfasting` as well supported (the run `onf` is one cluster) but it is the single most fragile
character-level call in the study.

### 5.4 Rows present in one table but conspicuously absent from a sibling

* **Table 2 vs Table 3 (responders vs nonresponders, the directly comparable pair).** At the unioned
  level the two row sets are **identical**: same 40 names, same multiset, including the six redacted
  `CCI` rows. Nothing is missing from either. The difference is *per tile*: in Table 2, `Review
  modified Mayo score (MMS)`, `Diary return`, `Clinician-Administered Questionnaires (Paper)`,
  `Physician's Global Assessment (PGA)`, `Endoscopy` and `Colon biopsy sample collection` print only
  in the V20–V29 tile, while in Table 3 only `Diary return` is tile-restricted. I verified this
  directly on both spreads of both tables. It is a real clinical difference (responders are not
  scoped or scoped endoscopically during V10–V19, and the source says so in the
  `Not applicable for responders during the time period of V10 through V19` comment), not a dropped
  row — but it means Table 2 has six rows whose marks all live in positions 12–21, and any check that
  expects marks in both halves will flag them.
* **Table 4 vs Tables 2/3.** Table 4 carries every row that Tables 2 and 3 carry **except
  `Diary compliance check`**, and instead carries `Diary return`. Verified on the page: document 43
  prints `Patient Diary (Electronic)` followed directly by `Diary return`, and the string
  `Diary compliance` does not occur anywhere on documents 43–46. Source-faithful.
* **Table 1 vs the rest.** Table 1's 24 unique rows are screening/baseline procedures and their
  absence from Tables 2–4 is expected. Two are worth a resolver's attention because they are *near*
  matches rather than absences: Table 1 has both `Physical examination` (full exam, row 22) and
  `Symptom-directed physical examination…` (row 23), where Tables 2–4 have only the symptom-directed
  row; and Table 1 has `Serum pregnancy` **and** `Urine pregnancy (local)`, where Tables 2–4 have only
  the urine row. Both splits are visible on the page and are the origin of the `c16`/`c17` containment
  pair.
* **`Dosing` marks.** Tables 1, 2 and 3 each carry a `Dosing` row with marks and a "no dosing at
  <visit>" note (`No dosing at V3.` / `No dosing at V29.` / `No dosing at V29.`). Table 4's `Dosing`
  row carries a note and **zero** marks. Consistent with `No dosing at ETV, V997, or post-treatment
  follow-up visits.` — flagged here only because a zero-mark row in a 54-mark table is the kind of
  thing an eye-check queries.

### 5.5 Study-wide conventions that were applied consistently (no action needed)

* All four tables: L = 1, first data column position 2, `Comment` column excluded from the grid and
  emitted as footnotes (§6). No table renumbered its data columns.
* All four: zero `abbreviation` and zero legend annotations, despite an abbreviation block on the
  last page of each table. This is the §6 outcome and it was reached the same way four times. The
  closest calls, for the record, are `ETV = early termination visit` (the string `ETV` *is* a header
  cell value in Table 4) and `QIDS-SR16` (the string is an activity label in all four tables); all
  four agents declined to bind them, which I agree with — a column label and a row label are not
  markers on those cells.
* All four: extraction_status: "ready_for_resolution", `schema_name`/`schema_version` correct,
  every annotation has ≥1 `marker_locations` entry, and **every marker recorded in a location also
  appears in that row's `annotation_markers`** (checked mechanically across all four files — zero
  mismatches, zero orphan markers in the reverse direction).
* by_type is not degenerate in any table: 29 footnote / 3 source_note (T1), 15/4 (T2), 14/3 (T3),
  15/1 (T4). Every bare-pointer note (`See Section 8.2.2.`, `For procedures at an ETV, see ETV in
  Table 4.`, `For procedures at an unscheduled visit, see V997 in Table 4.`) is typed `source_note`
  in every table; every note that points *and* explains (`For AESIs, additional data are collected
  (Section 8.3.6).`) is typed `footnote` in every table.
* No table sets `source_range`, merged_cell_range, is_merged_cell or an arrow glyph anywhere.
  Confirmed against the pages: this document contains no horizontally merged marks and no arrows. The
  only merged cells in the whole excerpt are the label cells of the two-row redaction bars, which are
  handled as described above.

---

## 6. Summary of items a reviewer should look at first

1. **Table 2 table_metadata.notes contains two wrong counts** — `Seven rows print in only one tile`
   (six are named and six exist) and `eight body rows are redacted` (six exist). Data are right, prose
   is wrong; fix the prose, not the rows.
2. **Table 3 omits `method` on 13 of its 18 `marker_locations`**, so synthesised bindings read as
   printed-marker bindings. The other three tables record `synthesized` on every such location.
3. **Marker strings collide across tables** (`c1`–`c13` in Tables 1 and 2; `n1`–`n11` in Tables 3 and
   4) and one note text carries up to four different markers. Do not key on markers across tables.
4. **Table 3's tile-B header-block variant** (`occurring` with no full stop) is not recorded, where
   Table 2 kept its two equivalent variants as `h5`/`h6`.
5. **Table 3 records both rows of the document-37 redaction bar as `glyph_reconstruction`**, but only
   one `CCI` label is printed there; the second row's name was necessarily read visually.
6. **`Fasting visit` is typed `modality` in Table 1 and `condition` in Tables 2–4**, and rows 6–9 are
   level 0 in Tables 1–3 and level 1 in Table 4. Pick one of each before consolidation.
7. **`non fasting` vs `nonfasting`** is a genuine source variant, but the `nonfasting` side is only
   readable through the glyph reconstruction; it is the study's most fragile character-level call.
