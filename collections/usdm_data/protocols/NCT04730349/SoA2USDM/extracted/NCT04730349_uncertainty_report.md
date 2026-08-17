# NCT04730349 — SoA extraction uncertainty report (§7)

Source: `NCT04730349_soa.pdf`, 16 PDF pages = document pages 27–42 (per PAGEMAP.md).
Document page 27 is the section-2 lead-in text ("An overview of the schedule of major assessments in this study are provided in Table 2-1") and contains no table.
Three tables were found and extracted, in document order.

---

## 1. Per table

### Table 1 — `NCT04730349_Table_01_extraction.json`
- Title: "Table 2-1: Screening Procedural Outline (CA045020)"
- `table_type`: **main_soa**. Document pages **28–30**.
- Label columns **L = 1** (Procedure). First data column position = **2**. **1 data column** ("Screening Visit"). The right-hand Notes column is source column 3 and is excluded from the schedule grid.
- Activities: **21** (4 bold section-header bands at `indentation_level` 0, 17 procedure rows at level 1). Marks: **17** `activity_schedule` cells (one `X` per level-1 row). Annotations: **18**.
- Activity rows per document page: **p28 = 12, p29 = 6, p30 = 3**. Every declared page contributed rows.

### Table 2 — `NCT04730349_Table_02_extraction.json`
- Title: "Table 2-2: On-treatment Procedural Outline (CA045020)"
- `table_type`: **main_soa**. Document pages **31–38**.
- Label columns **L = 1** (Procedure). First data column position = **2**. **6 data columns** (positions 2–7: Cycle 1 Day 1 / Day 3 / Day 5 / Day 8, Cycle 2+ Day 1 / Day 3-5). Notes column is source column 8, excluded.
- Activities: **27** (7 section headers at level 0, 20 procedure rows). Marks: **83** `activity_schedule` cells. Annotations: **22**.
- Activity rows per document page: **p31 = 4, p32 = 4, p33 = 2, p34 = 4, p35 = 5, p36 = 6, p37 = 2, p38 = 0**.
  **p38 contributed no activity rows** — it carries only footnotes d and e ("Samples can be collected ± 3 hours…" and "For participants who require blood draw volume modifications…"). That is a declared exception, not a skipped page.

### Table 3 — `NCT04730349_Table_03_extraction.json`
- Title: "Table 2-3: Long-term Follow-up Period (CA045020)"
- `table_type`: **main_soa**. Document pages **39–42**.
- Label columns **L = 1** (Procedure). First data column position = **2**. **4 data columns** (positions 2–5). Notes column is source column 6, excluded.
- Activities: **19** (4 section headers, 15 procedure rows). Marks: **51** `activity_schedule` cells. Annotations: **15**.
- Activity rows per document page: **p39 = 7, p40 = 3, p41 = 3, p42 = 6**. Every declared page contributed rows.

### Classification reasoning (recorded in each `table_metadata.notes`)
All three are independent schedules with different column structures, applying to the **same single participant population** in three sequential study phases (screening → on-treatment → long-term follow-up). The taxonomy's `main_soa` definition names exactly this case — several independent schedules with different column structures, giving a Screening table and a Treatment table, each classified as main_soa — so all three are `main_soa` and no `track_label` is set anywhere.
This is a genuine judgement call, because the `track` definition also covers the same participants in a different study phase. It was resolved toward `main_soa` because (a) the explicit worked example in the `main_soa` definition matches this protocol's layout exactly, and (b) there is **no mutually exclusive population split** anywhere in the excerpt — the only sub-population wording found is inside cells and notes ("Part B only", "(Parts A and B)"), never as a separate table. **If the corpus convention is that only the treatment grid is `main_soa`, Table 1 and Table 3 should be re-typed `track` with labels such as "Screening" and "Long-term Follow-up"; that change would not alter any row, mark or annotation.**

---

## 2. Merged-mark / merged-text decisions

Every span below was read from **rule-line geometry** (a missing internal vertical rule inside the row band), never from where the glyph sits. Spans are given in `column_position` numbering.

**Table 2** (Day 1, column 2, is a *separate empty cell* in every row below — the rule at that boundary is present):
| Row | Activity | Value distributed | `source_range` |
|---|---|---|---|
| 7 | Adverse Events Assessment (including Serious Adverse Events) | "Continuously" | 3:7 |
| 8 | Concomitant Medication Use | "Continuously" | 3:7 |
| 13 | Body Imaging | bulleted imaging-schedule text | 3:7 |
| 14 | Brain Imaging | bulleted brain-imaging text | 3:7 |
| 15 | CSF - Solid tumors | "Performed at time of imaging assessments for disease response as clinically indicated." | 3:7 |
| 16 | Bone Marrow- Solid tumors | same text as row 15 | 3:7 |
| 17 | CSF - Leukemia | "Performed as clinically indicated." | 3:7 |
| 18 | Bone Marrow - Leukemia | "Performed as clinically indicated." | 3:7 |
| 20, 21, 22 | Bempegaldesleukin PK Plasma Samples / Nivolumab PK Serum Samples / Immunogenicity Samples | "See Section 9.5.1 for further details." | 3:7 |
| 24 | Patient-reported Outcomes (PRO) Version of the CTCAE | "See Notes" | 3:7 |

**Table 3** (spans cover all four data columns):
rows 13 (Body Imaging), 14 (Brain Imaging), 15 ("As clinically indicated."), 16 ("As clinically indicated"), 19 and 20 ("See Section 9.5.1 for further details.") — all `source_range` **2:5**.

**Header merges** (`is_merged_cell` / `merged_cell_range`, Table 2 only): "Cycle 1 Only" and "Cycle = 3 wks" over **2:5**; "Cycle 2 and Beyond" and "Each cycle = 3 wks" over **6:7**.

**Marks deliberately NOT distributed:**
- Table 2 row 6 column 7 = "X (Cycle 5 only)". The qualifier names a cycle, not a span of this table's columns (its columns are days inside "Cycle 2 and Beyond"), so it is kept literally in `cell_value` per §5.
- Table 2 row 30 column 4 = "X (Day 3-5)". **Low confidence — see §6 below.**

---

## 3. Synthesised names and markers

**Synthesised `property_name`** (in all three tables the header row's label cell holds the activity-column header "Procedure", not a property label, so `property_name_source.synthesized = true`):
- Table 1 row 1 → "Screening Visit"
- Table 2 row 1 → "Cycle"; row 2 → "Cycle Duration"; row 3 → "Day"
- Table 3 row 1 → "Follow-up Visit"

**Table 2 header split:** the top header band physically contains two printed lines per merged cell ("Cycle 1 Only" / "Cycle = 3 wks"). It is modelled as two `schedule_property` rows (1 = cycle identity, level 1; 2 = cycle length, level `null`, type `other`) because the two lines carry different concepts. This is a layout inference, recorded with `structure_method` = `inferred_from_layout` on rows 1–3 of Table 2 and on the single header rows of Tables 1 and 3.

**Synthesised annotation markers.** The right-hand Notes column is not a schedule column; each non-empty Notes cell became one `footnote`/`source_note` annotation with a synthesised marker `n1, n2, …` in printed order, bound to the activity row it sits beside (`location_type` = `activity_name`, `method` = `synthesized`), and the same marker added to that activity's `annotation_markers`:
- Table 1: n1–n17 (17 notes)
- Table 2: n1–n16 (16 notes) plus one printed marker, see below
- Table 3: n1–n12 (12 notes)

**Table 2, the printed `*`.** The Day-8 cell of row 5 prints "X*" and the same row's Notes cell ends with "*At Day 8 visits collect vital signs only.". The Notes cell was bounded by its rule lines and emitted as **one** annotation, given `annotation_marker` `*` (the marker the source actually prints) with **two** locations: `schedule_cell` row 5 column 5 (printed) and `activity_name` row 5 (`method` = `synthesized`). The alternative — a synthesised `n0` for the whole cell plus a separate `*` annotation carrying only the last sentence — was rejected because it would have manufactured a containment pair by splitting one rule-bounded note cell, which §6 forbids. Consequence to be aware of downstream: the `*` annotation is slightly over-scoped, since the vital-signs and nivolumab sentences in that cell are not specific to Day 8.

---

## 4. Mechanical mark-check

The SoA pages carry a real text layer (`pdftotext` returns full prose), so no OCR or pixel-count mark detection was used. Two independent mechanical passes were run and diffed:

1. **Rule-line cell assignment (primary).** Pages rendered at 200 dpi (`pdftoppm -r 200`); vertical rules recovered as image columns with a long contiguous ink run, horizontal rules as image rows with ≥80 % ink across the table width; each text-layer word assigned to the cell its centre falls in. Merged spans were determined per row band by testing whether the ink of each internal vertical rule is present over that band.
2. **bbox column-binning (cross-check, §1b).** `pdftotext -bbox` over the whole document; column x-centres fixed from the header day/visit labels; mark tokens matched with a regular expression for X-like glyphs plus an optional footnote letter, and binned to the nearest centre.

**Result: the two matrices agree cell-for-cell in all three tables — no disagreements to report.** The 24 binned mark tokens of Table 2 and the 24 of Table 3 land within 1 px of a column centre, except the single token of "X (Cycle 5 only)" (40 px off-centre because the qualifier follows it in the same cell) which the rule-line pass places in column 7 unambiguously.

**Detector validation against direct visual reads** (rendered crops, dense and sparse rows): document pages 30, 31 (rows 5 and 7), 32, 34, 36, 37, 39, 41 were read by eye and matched the mechanical result, including the empty Day-1 cell that precedes every merged text span in Table 2.

Two rule-detection artefacts worth noting, neither of which affects output:
- On pages carrying a full-width black redaction band the naive vertical-rule detector saturates; the bands were detected first (rows ≥97 % ink over ≥8 px) and masked before rule detection.
- The Table 1 section-header band "Laboratory Tests" (document page 28) keeps its first internal rule but not its second, unlike the other three section headers which span the full width. Section-header rows carry no marks, so nothing was emitted either way.

---

## 5. Annotation text integrity

- The text layer is **not** glyph-spread; words come back whole. No `deglyph_reconstruction` was needed and no `annotation_text_source.method` is recorded anywhere.
- **Normalisations applied (flagged per §5):** Symbol-font private-use glyphs U+F0B7 to a bullet, U+F0B1 to a plus-minus sign, U+F020 to a space; the superscript 18 re-joined into "[18F]fluorodeoxyglucose", which the text layer emits on its own line; intra-word line splits re-joined ("non-serious", "SARS-CoV-2", "anti-cancer", "PET-CT", "non-Hodgkin"). Wrapped lines inside a cell are otherwise joined with a single space.
- **Containment pairs — both re-verified against the page, both source-faithful, neither merged nor dropped:**
  - Table 2: **n3 is contained in n2**. n3 is the entire Notes cell of "Concomitant Medication Use" on document page 32 and reads "Record at each visit."; n2 is the Notes cell of "Adverse Events Assessment (including Serious Adverse Events)" on document page 31, whose second paragraph *begins* "Record at each visit. All AEs (SAEs or non-serious AEs), including those associated with SARS-CoV-2 infection…". Different pages, different rule-bounded cells — **source-faithful, not a split note.**
  - Table 3: **n8 is contained in n9**. n8 is the entire Notes cell of "Body Imaging" on document page 41 and reads "See Section 9.1.1 for further details."; n9 is the Notes cell of "Brain Imaging" on the same page, which *ends* with the same sentence after the glioma/MRI text. Two adjacent rule-bounded cells on one page — **source-faithful, not a split note.**
  - Near-miss pair worth a human glance (not a containment after the redaction suffixes): Table 2 n9 and n11 both start "Bone marrow biopsy/aspirate should be obtained in any participant with a previously positive bone marrow aspirate as part of response assessment"; n9 (document page 34) continues "(see Section 9.1)." and n11 (document page 35) stops there. Verified on both pages as printed.
- **Note bounding:** every note was bounded by its cell's horizontal rules, never by proximity. No annotation carries `proximity_bounded`.

### Redactions (source defect — transcribed, never repaired)
Black redaction boxes are present throughout. Where a box truncates a Notes cell the visible text was transcribed and "[remainder redacted in source]" appended:
- Table 1: n14 (doc p29), n15, n16, n17 (doc p30)
- Table 2: n8, n9, n10 (doc p34), n11 (doc p35)
Footnote e of Table 2 has an **interior** redaction, transcribed as "Omitted blood collection will likely be for [text redacted in source] pharmacokinetic assessments, since all required safety assessments must be performed."
The trailing abbreviation blocks on document pages 30, 37 and 42 are also partially redacted; no annotation depends on them (see §7).

### Regions that may conceal activity rows — REQUIRES PAGE VERIFICATION
Three **full-table-width** black bands sit inside the table body and may hide whole rows together with their marks. Nothing was invented for them; they are reported here only.
| Document page | Table | Band (200-dpi raster rows) | Position |
|---|---|---|---|
| 30 | 1 | 799–1090 | after the last visible row ("Bone Marrow and Buccal Swab - Leukemia"), to the bottom rule of the table |
| 35 | 2 | 1051–1406 | after "Immunogenicity Samples", to the bottom rule |
| 36 | 2 | 500–614 | between the day header row and the "Health Outcomes" section band |
| 40 | 3 | 753–1129 | after "Pregnancy Test", to the bottom rule |
Each band is roughly one to three row-heights tall.

---

## 6. Low-confidence calls

1. **Table 2 row 30, "Oral Hydration Follow-up", column 4 = "X (Day 3-5)".** Transcribed literally into the Day 5 column, where it is printed, with no distribution. §5's qualified-mark rule would allow distributing it over columns 3:4 (this table does have Day 3 and Day 5 columns), but the cell's rule lines are all present — it is *not* a merged cell — and the row's note describes a single contact "between Days 3 and 5 following infusion", i.e. a window, not two visits. "Day 3-5" is also verbatim the label of column 7. Alternative reading: `cell_value` "X" on columns 3 and 4 with a `source_range` of 3:4.
2. **`property_type` of the Table 1 header.** Recorded as `visit` — the cell "Screening Visit" names one encounter. An `epoch` reading of the word Screening is defensible; the deciding factor was that the printed cell also says Visit.
3. **Table 2 header split into two `schedule_property` rows** (cycle identity vs the printed line "Cycle = 3 wks"). One physical rule band, two printed lines. Recorded with `structure_method` = `inferred_from_layout`. If the convention is one property per rule band, rows 1 and 2 collapse into one whose cell value would concatenate the two printed lines of the merged header cell.
4. **`Cycle Duration` typed `other` with `hierarchical_level: null`.** It states a duration and does not by itself tell one column from another.
5. **Activity names broken across lines at a hyphen.** "Bone Marrow-" / "Solid tumors" (Table 2 row 16) and "Bone marrow aspirate -" / "solid tumors and leukemia" (Table 3 row 16) were joined with a single space, giving "Bone Marrow- Solid tumors" and "Bone marrow aspirate - solid tumors and leukemia". The source's own spacing at the wrap point is unrecoverable; the raw two-line text is preserved in `activity_name_source.cell_text`.
6. **Table 3 row 10 columns 3 and 4 read "If toxicities are present." and "If toxicities are present"** — the missing final full stop in column 4 is in the source and was not repaired.
7. **Table 3 note n10 ends "See Section 9.1"** with no full stop, as printed.
8. **No PDF/markdown comparison was possible** — no protocol markdown was supplied, so all text comes from the PDF text layer.
9. **Document version/date not recorded.** The page footers are damaged in the text layer ("ProtclAmendN.:01", "Date:28-Nov01" — characters are dropped in the rendering), so `source_document.document_version` and `document_date` were left unset rather than guessed. Footer page numbers were ignored entirely; PAGEMAP.md was used, and the two agree on the pages where the footer number is legible (e.g. 30, 37, 38, 42) while document pages 34 and 36 print "3" and "6".

---

## 7. Orphan risk

- **No orphan annotations:** all 55 annotations across the three files carry at least one `marker_locations` entry, and every marker in a location also appears in that row's/cell's `annotation_markers` — with one deliberate exception below.
- **Deliberate exception, Table 3 footnote c.** The marker is printed on the **Notes column header** ("Notes" with superscript c), which is not a modelled element. Per §6 it is table-scope: one `schedule_property` location on row 1 with `method` = `synthesized`, and the marker is **not** added to any element's `annotation_markers`. A mechanical marker/`annotation_markers` cross-check will flag this one row; it is intended.
- **No `abbreviation` or `legend` annotations were emitted.** Each table ends with an abbreviation block (CSF, CT, FDG, IRT, MIBG, MRI, MUGA, PET, SAE, SARS-CoV-2, AE, CTCAE, ECG, NCI, Ped, PK, PRO). None of those terms is printed as a *marker* on a grid cell, header cell or activity label — they only occur inside running text and inside activity names such as "Echocardiogram or Multigated Acquisition Scan (MUGA)". Binding them would require the `text_match` location method, which §6 forbids, so the blocks yield zero annotations. There is no in-grid legend — no symbol-definition line appears anywhere in the excerpt — so `X` is left as a bare `cell_value`.
- **No marker is referenced but undefined.** Every printed marker (a, b, c, d, e, `*`) has its text printed in the excerpt.

---

## 8. Method provenance (§1e) — every non-default value

- `activity_name_source.indentation_method` = `font_signal` on **all 67 activities** in the three files. The label column has no leading whitespace at all — section headers and procedure rows start at the same x — so hierarchy comes from the bold, full-width, ungridded section bands ("Eligibility Assessments", "Safety Assessments", "Laboratory Tests", "Tumor Assessment", "Efficacy Assessments", "Health Outcomes", "Study Drug", "Pharmacokinetic (PK)/Immunogenicity Assessments"). Section headers are level 0 and carry no marks; procedure rows are level 1.
- `schedule_property.structure_method` = `inferred_from_layout` on Table 1 row 1, Table 2 rows 1–3, Table 3 row 1 — `property_type` and `hierarchical_level` come from the header geometry, not from a printed property label.
- `property_name_source.synthesized` = true on the same five rows.
- `marker_locations[].method` = `synthesized` on every Notes-column annotation (n-markers, Tables 1–3), on the `activity_name` location of Table 2's `*` annotation, and on Table 3 footnote c.
- **No** `text_match` and **no** `proximity` location methods anywhere.
- **No** `unresolved` location type anywhere — every marker's target was determinable.
- **No** `activity_schedule` / `schedule_grid` `method` values and **no** `annotation_text_source` blocks: all cell values and all note text were read from the text layer inside rule-line-bounded cells, which is the default.
