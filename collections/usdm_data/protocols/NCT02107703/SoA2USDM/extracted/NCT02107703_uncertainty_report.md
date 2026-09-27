# NCT02107703 — SoA extraction uncertainty report

Source: `NCT02107703_soa.pdf` (7 PDF pages = document pages 72–78 per PAGEMAP.md; protocol I3Y-MC-JPBL, Attachment 1). No protocol markdown was provided. Prompt v3.8.1, single pass.

## Decisions needed (7)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | T1 p73–75, rows 7, 10, 14, 19, 27, 37, 40 | Each Procedure Category cell (Study Entry /Enrollment, Medical History, Physical Examination, Tumor Assessment, Lab/Diagnostic Tests, Study Drug, Health Outcomes) is emitted as its own level-0 grouping row with no marks, placed just above its procedures. The procedures are level 1. These grouping rows are not separately printed rows. | Treat the category column as a label column only: no grouping rows, and every procedure at level 0 (a flat table). | 3.2 |
| D2 | T1 p75, rows 38–39 (Fulvestrant / LY2835219 Therapy) | The dosing text cells are spread across columns 6–11: all on-treatment columns and both follow-up columns. The ruled merged cell runs to the right edge of the table. | Limit the text to the on-treatment columns 6–9, reading the extra width as layout only, since no drug is given in postdiscontinuation follow-up. | 3.3 |
| D3 | T1 p73, row 4 (Approximate Duration (days)) | This header row gets no hierarchy level (null), because it does not tell any two columns apart. "Relative day within a cycle" is level 4. | Give Duration level 4 and Relative day level 5, following the printed order. | 3.4 |
| D4 | T1 p73, row 2 (Cycle) | The row is typed as cycle, although its last two cells read "Short-Term Follow-Up" and "Long-Term Follow-Up", which are periods, not cycles. | Type the row `other`, or treat the follow-up cells as period labels. | 3.4 |
| D5 | T1 p73, subtitle | The subtitle "Perform procedure as indicated." is kept only in `table_metadata.notes`. It is not an annotation. | Emit it as a table-scope footnote with a synthesised marker on header row 1. | 3.5 |
| D6 | T2 p78 | The extension-period schedule is typed `track` with `track_label` "Extension period". It covers patients who continue treatment after study completion and has its own visits (501–5XX, 901). | Type it `main_soa`, as an independent schedule, with no track label. | 4.1 |
| D7 | T2 p78, rows 9–10 | The dosing text for Fulvestrant and LY2835219 is spread across both Day 1 and Day 15 (columns 4–5), matching the ruled merged cell. | Anchor the text to Day 1 only (column 4). | 4.2 |

## Recorded, not open (14)

- §5 label columns: both tables have L = 3 label columns (Procedure Category, Procedure, Protocol Reference). The header-row labels (Cycle, Visit, …) sit in column 3. The first data position is 4. T1 data columns are 4–11 and T2 data columns are 4–6.
- §6 abbreviations: both abbreviation blocks (p75 for T1, p78 for T2) yield zero annotations. None of their terms is printed as a marker.
- §6 inline/column references: every Protocol Reference entry became a `source_note`, de-duplicated by text. Rows with two references (e.g. "Section 7.1 / Attachment 4") were split into separate notes. The synthesised markers pr1–pr20 are numbered once across the study, so "Section 10.3" is pr9 in both tables. Each location has `method: synthesized` and the marker is also in the row's `annotation_markers`.
- §8 bare pointer: footnote m ("See Pharmacokinetic Sampling Schedule (Attachment 7).") is typed `source_note`. Footnote p ends with "Refer to Section 9.4.1.1.2." but also explains, so it stays a `footnote`.
- §6 header-cell footnotes: T1 marker a (Short-/Long-Term Follow-Upᵃ) is placed on the Cycle-row grid cells for columns 10 and 11. Marker p (1ᵖ) is placed on the Relative-day grid cells for columns 8 and 9. T2 marker a is on Cycle-row column 6.
- §5 merged marks: these were spread across their span from the raster rule geometry. The spans are listed in 3.3 and 4.2.
- §1b de-duplication: the header reprinted on p74 and p75 was de-duplicated, and header values are taken from p73. On p74–75 the text layer prints "≤" as the Symbol-font private-use glyph U+F0A3, so those tokens read "28" / "14". The render shows ≤28 / ≤14, the same as p73.
- §4 non-activity row: the "Procedure Category / Procedure / Protocol Reference" column-label band (row 6 in both tables) is not emitted as an activity.
- §4 page coverage: pages 76 and 77 of T1 hold only footnotes b–q and contribute no rows (see 3.1).
- §3 synthesised name: the empty-label epoch row is named "Study period" in both tables.
- Type definitions, one printed table: T1 is one table spanning p73–77, not a set of continuations. The "(continued)" / "(concluded)" captions on p76–77 caption the footnote pages and have no table number of their own.
- §1 literal transcription: Survival Information has no baseline mark, although footnote d says survival information is "collected at baseline". The grid was transcribed as printed.
- §1 literal transcription: some text differs between label and footnote, and some text looks defective. All of it was kept as printed. The BPI row label reads "BPI, EORTC QLQ-C30, EORTC BR23, EQ-5D 5L", while footnote k says "mBPI-sf … EORTC QLQ-BR23". Footnote h has no final period. T2 note c reads "ending hour after" with the "1" missing.
- §1c/§1e text repair: three glyph or line-break repairs are recorded as `annotation_text_source.method: visual_transcription` on notes a, b and j (see 5.2).

## 1. Method

- **Text layer:** present on every page, with real words, so the text is not glyph-spread (§1c does not apply). Two exceptions: Symbol-font characters come through as private-use glyphs (U+F0B1 = ±, U+F0A3 = ≤), and pdftotext drops line-break hyphens.
- **Vector layer:** not relied on. Rule lines were recovered from the 200-dpi raster (§1d): vertical rules are pixel columns that are more than 90% ink inside each row band. The same method was used on every page.
- **Mechanical mark-check (§1b):** I ran `pdftotext -bbox` and matched X tokens with `^[Xx][*a-zA-Z0-9]?$`. I binned each token to a column using the raster column boundaries (269.2 / 297.2 / 323.2 / 346.6 / 377.2 / 413.2 / 471.7 / 534.8 / 606.8 pt on T1), then grouped tokens by row y. On T1 this gave 30 bbox mark rows (p73: 13, p74: 13, p75: 4). They match the 30 marked activity rows cell for cell. The only difference is expected: tokens sitting on a column border (x ≈ 295 at the 4|5 border, x ≈ 350 at the 6|7 border) belong to merged cells whose internal rule is missing in the raster. They were spread across the span rather than binned. On T2, the X tokens at x = 377 (column 4) and x = 524 (column 6) match the visual read. **Mechanical and visual reads disagree in no cell.**

## 3. Table 1 — Study Schedule, Protocol I3Y-MC-JPBL (main_soa, doc pages 73–77)

### 3.1 Counts

- 8 data columns (4–11): Baseline ≤28 | Baseline ≤14 | C1 D1 | C1 D15±3 | C2–3 D1 | C4+ D1 | Short-Term FU (801) | Long-Term FU (802–8XX).
- 5 schedule properties.
- 38 activity rows: 7 category/grouping rows and 31 procedures. 117 activity_schedule entries. 36 annotations: 17 lettered footnotes a–q, including one `source_note` (m), plus 19 protocol-reference `source_note`s (pr1–pr19).
- Activity rows per page: p73 = 17, p74 = 13, p75 = 8, p76 = 0, p77 = 0. Pages 76–77 are footnote-only pages (notes b–h on p76, i–q on p77), so they have no activity rows. No page was skipped.

### 3.2 Hierarchy (D1)

The Procedure Category column is a vertically merged label column. To keep the grouping, each category became a level-0 organisational row with no marks, and its procedures are level 1. The row positions of these grouping rows are placed before their first child, so they are not separate printed rows. Three rows print their name across the merged Category+Procedure cells and carry marks: Survival Information, Adverse Event Collection/CTCAE Grading, and Concomitant Medications (with analgesics). These are standalone level-0 activities. Indentation came from the column position, with category cells bold except "Health Outcomes", which is printed in regular weight. This is recorded as `indentation_method: visual_estimate` on every row. The category "Medical History" and the procedure "Medical History" (row 11) are distinct printed cells, and both were kept.

### 3.3 Merged-mark decisions

| rows | span | evidence |
|---|---|---|
| 8 Informed Consent Form signed (Xᵠ) | 4:5 | no 297 pt rule in the row band |
| 20–23 Tumor measurement, Radiologic imaging, Bone Scintigraphy, X-ray/CT/MRI (Xᶜ/Xᵇ/Xⁱ/Xʲ) | 4:5 | no 297 pt rule |
| 25 Adverse Event Collection (Xᶠ), 26 Concomitant Medications (X) | 6:7 | no 346.6 pt rule (rows 28–29 and 32–36 do have it, so their Cycle 1 marks are split) |
| 38 Fulvestrant Therapy, 39 LY2835219 Therapy (dosing text with ᵍ) | 6:11 | only rules at 269.2 / 323.2 / 606.6 pt in the band. The cell runs to the table edge (D2) |

Shaded empty merged cells (for example Cycle 1 in the tumor rows) carry no value and were not emitted.

### 3.4 Header properties (D3, D4)

The rows are: Study period (epoch, level 1, name synthesised), Cycle (cycle, 2), Visit (visit, 3), Approximate Duration (days) (other, null), and Relative day within a cycle (study_day, 4). Header cells are merged as follows: Baseline 4:5, Patients on Study Treatment 6:9, Postdiscontinuation Follow-Up 10:11. Cycle "BL", Visit "0" and Duration "28" span 4:5, and Cycle 1, Visit 1 and Duration 28 span 6:7. The Relative-day cells in columns 10–11 are shaded and blank, and were emitted as empty grid cells.

### 3.5 Annotations

- All lettered notes are in the page-bottom footnote block. They were bound through their printed markers (activity label and cell), so no proximity binding was used and there is no notes column.
- Markers are placed as follows:
  - a: header columns 10–11
  - b: row 21 and columns 4, 5, 8–11
  - c: row 20 and columns 4, 5, 8–11
  - d: row 24 and columns 10–11
  - e: row 35 and columns 5, 6, 7, 9, 10
  - f: row 25 and columns 5–11
  - g: rows 38–39 and columns 6–11
  - h: row 36 and column 6
  - i: row 22 and columns 4, 5, 9–11
  - j: row 23 and columns 4, 5, 8–11
  - k: row 41 and columns 5, 8, 9, 10
  - l: row 42 and columns 5, 8–11
  - m: row 32 and columns 6–8
  - n: row 30 and column 5
  - o: row 31 and column 5
  - p: header columns 8–9
  - q: row 8 and columns 4–5
- The subtitle "Perform procedure as indicated." is not an annotation (D5).

## 4. Table 2 — Study Schedule for the extension period only (track, doc page 78)

### 4.1 Classification (D6)

The table is typed `track` with `track_label` "Extension period". The title limits it to the extension period, which footnote a says "begins after study completion and ends at the end of trial". It has its own visit numbers (501–5XX, 901) and its own columns. This fits the type definition's example of a post-study schedule for participants continuing treatment.

### 4.2 Counts and spans (D7)

- 3 data columns (4–6): Day 1 | Day 15 | Extension Period Follow-Up (901).
- 5 schedule properties, with the same row structure as Table 1.
- 4 activity rows, all on p78: 3 procedures and 1 grouping row (Study Drug). 6 activity_schedule entries. 5 annotations: footnotes a, b, c, plus pr9 (Section 10.3) and pr20 (Section 8.1.2).
- The AE row has X in column 4 and column 6. Column 5 is shaded and has its own rule at 409.6 pt, so it is not merged.
- The Fulvestrant and LY2835219 dosing text spans 4:5: the band has rules only at 343.4 / 471.9 pt. The follow-up column for both rows is shaded and empty.
- "Patients on Study Treatment", "X-Y", "501-5XX" and "28" are merged header cells spanning 4:5.

## 5. Cross-cutting checks

### 5.1 Synthesised items

- Property name "Study period" (row 1, both tables).
- Protocol-reference markers pr1–pr20:
  - pr1 Section 8.1
  - pr2 Section 7
  - pr3 Section 12.2.3
  - pr4 Section 7.1
  - pr5 Attachment 4
  - pr6 Section 10.1.1
  - pr7 Attachment 5
  - pr8 Section 10.1
  - pr9 Section 10.3
  - pr10 Section 9.6
  - pr11 Attachment 2
  - pr12 Attachment 7
  - pr13 Section 10.4.2.2
  - pr14 Section 10.4.2.3
  - pr15 Section 10.3.2.1
  - pr16 Section 10.4.2.1
  - pr17 Section 9.1
  - pr18 Section 12.2.11
  - pr19 Section 12.2.11.4
  - pr20 Section 8.1.2 (T2 only)
- The Medical History procedure row has no reference.

### 5.2 Annotation text integrity

- The text is not glyph-spread. Three notes were repaired against the render, and each carries `annotation_text_source.method: visual_transcription` with a note explaining the repair:
  - Note a: the private-use glyph U+F0B1 was mapped to "±", giving "(± 14 days)".
  - Note b: the second "Day -28 to Day -1)". pdftotext had turned the line-break "Day -⏎1)" into "Day 1)".
  - Note j: "post-⏎baseline" was restored to "post-baseline".
- Overlap pairs:
  - T1 notes b, c and j share a long closing sentence ("For patients who discontinue study treatment without objectively measured progressive disease (PD) … overall study completion."). I re-checked this against pp76–77. **It is source-faithful:** these are three separately lettered notes, each printed in full, and none is a split cell.
  - T2 note b is the opening sentence of T1 note f, and T2 note c overlaps T1 note g. These pairs are in different tables and are also source-faithful, since the extension table reprints shortened versions.
- No note's boundary is uncertain: each footnote starts at its printed letter and ends before the next letter.

### 5.3 Orphan risk

None. Every annotation has at least one `marker_location`, and each such marker also appears in the target row's or cell's `annotation_markers`. I checked this by script. Every marker printed in either table has a printed definition.

### 5.4 Method provenance

- `annotation_text_source.method = visual_transcription`: T1 notes a, b and j (see 5.2).
- `marker_locations[].method = synthesized`: every pr1–pr20 location, anchored to the row whose Protocol Reference cell prints it.
- `activity_name_source.indentation_method = visual_estimate`: every activity in both tables (level from the category-column layout, see 3.2).
- No `proximity`, `text_match` or `unresolved` locations. No `structure_method` values: the header rows all have printed labels, except the epoch row name, which is flagged as synthesised.
- Cell values come from the bbox text layer, the default, so there is no cell `method`. Merged spans come from the raster rules (§1d).
