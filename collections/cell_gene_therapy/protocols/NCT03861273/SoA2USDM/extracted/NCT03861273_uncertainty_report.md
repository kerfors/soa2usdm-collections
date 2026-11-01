# NCT03861273 — Uncertainty report (prompt 3.9.4, single-pass)

Pfizer C0371002 (BENEGENE-2), Final Protocol Amendment 3, 29 June 2022. Section 1.3 Schedule of Activities, doc pp.13-28.
Two tables: Table 1 = Year 1 (doc pp.13-24), Table 2 = Year 2 to Year 6 (doc pp.25-28).

## Decisions needed (5)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | T1 doc p.13 (reprinted every page), header notes cell beside header rows 1-3; markers n1-n5, *, g2-g4 | The shaded notes cell next to the header is a bulleted list. Each bullet is its own note, bound to what it names: Screening (n1), Screening + Baseline (n2), IP Infusion (n3), Visits 1-3 (n4), Day 1 (n5), Visit 4 (printed *). The three bullets that name no specific visit (unscheduled visits, mobile phlebotomy, early discontinuation) apply to the whole table (g2-g4). | Treat the whole header notes cell as one note that applies to the whole table, without binding parts of it to individual header cells. | 3.2 |
| D2 | T1 doc p.14, row 10 Physical Exam, Weight, and Vital Signs; n9/n10/n11 | The first sentence ("Complete Physical Exam at all clinic visits (Height is required to be collected at Screening Visit).") stays on the whole row, because its main clause covers all clinic visits. The two Weight-at-Screening sentences go to the Screening cell (n10). The Day 1 vital-signs list goes to the Day 1 cell (n11). | Also move the first sentence to the Screening cell, because its parenthesis names Screening Visit. Or keep the whole notes cell as one note on the row. | 3.3 |
| D3 | T1 doc p.21, row 58 Patient-Reported Outcomes; n38/n39 | The two sentences about the baseline visit ("Every effort ... especially the baseline visit. Baseline PRO assessments need to be done prior to start of infusion on Day 1 if not done at baseline visit.") are split off and bound to the Baseline cell (col 3). The rest stays on the row. | Keep the first of the two sentences on the row, since its main clause covers all protocol timepoints, and bind only the "Baseline PRO assessments ..." sentence to the Baseline cell. Or keep all the unmarked text on the row. | 3.3 |
| D4 | T1 doc p.23 below the frame (g5, g6); T2 doc p.27 below the frame (g5) | The definition paragraphs printed under the frames ("Note: Study Site: ...", "Infusion Center: ..." in Year 1; "Study Site: ..." in Year 2-6) are emitted as table-wide notes. They describe site roles, and the Year 1 one adds a rule ("Some study sites may be approved to carry out infusions"). | Treat them as term definitions, like the abbreviation list that follows them, and emit no annotation for them. | 3.4 |
| D5 | T1 doc p.13 title; T2 doc p.25 title | Table 1's title is the section heading printed directly above its frame ("1.3. Schedule of Activities (SoA)"). Table 2 has no heading above it, so its title is empty. | Give both tables an empty title, because the heading belongs to the section rather than the table. Or use the corner-cell wording ("Year 1" / "Year 2 to Year 6") to tell the two apart. | 2 |

All five calls belong to Table 1's `review_items` array. D4 concerns both tables, so it goes in the first table's array. Table 2's `review_items` is `[]`.

## Recorded, not open (24)

- §2 **track vs main_soa**: both tables are `main_soa`. Every participant goes through Year 1 and then Years 2-6 in sequence, and sequence alone does not make a track.
- Taxonomy **continuation** ("A page break inside ONE printed table is not a continuation"): Year 1 is one table across pp.13-24 with its header reprinted on each page. Year 2-6 has different columns (V14-V20) and its own header notes cell, so it is a separate table, not a continuation.
- §3 **synthesised names**: row 1 of both tables is named `Epoch`. Its label cell prints the activity-column heading "Procedures" plus "Year 1" / "Year 2 to Year 6", which is kept in `cell_value`.
- §3 **Header bands — typing**: row 1 is `epoch` because its multi-column values are period names. The single-column cells (Screening Period, Baseline Period, IP Infusion, EOS Week 312) stay values of that band. Row 3 is `week` because week values dominate, even though "Day 1" also appears.
- §3 **hierarchical_level**: levels 1/2/3 for the epoch, visit and week rows in both tables.
- §6 **a sentence in a header cell** (`n` prefix): the label cell of T1 row 3 also prints "The schedule of assessments should be based on Day 1". It is emitted as n6 on that `schedule_property`.
- §4 **row_position**: the three header rows reprint on every page and keep their numbers. T1 body rows are 4-6, 10-12, 16-20, 24-28, 32-33, 37-38, 42-47, 51-52, 56-58, 62-64, 68, and rows 69-71 are the header-only p.24. T2 body rows are 4-12, 16-21, 25-32, and rows 33-35 are the header-only p.28.
- §4 **Hierarchy — printed signals only**: no group headers, indents or bands, and every row carries its own marks. All activities are level 0 with `indentation_method: assumed_flat`.
- §4 **Several procedures in one label cell**: not applied. In every multi-line label the later lines continue the name ("FIX Infusion," / "Accountability" — the IP row prints the same label on one line; "HBcAB, HCV, CD4," / "human immunodeficiency" ...).
- §4 **Whitespace**: "nAb Assay to adeno-" + "associated virus" becomes "adeno-associated". In merged cells, "phlebotomy/" at a line end is joined with no space. The T2 header "26" / "0" (narrow column) becomes "260".
- §4 **source_page**: doc pp.24 and 28 contribute no rows. Each holds only the reprinted header rows plus the tail of the abbreviation list (see §2 below).
- §5 **Leading label columns**: L = 1 (Procedures) in both tables, so the first data position is 2. T1 data columns are 2-14 (V1-V13) and T2 data columns are 2-8 (V14-V20). The notes column is not a data column.
- §5 **Empty cells**: `schedule_grid` is complete (3 x 13 for T1, 3 x 7 for T2). The empty merged cell T2 row 32 cols 2-7 (Return eDiary) has no entries.
- §5 **Merged marks — distribute** / **Spanned values**: every multi-column cell is distributed with `source_range` taken from the rule lines (list in §4 below).
- §5 **Spanned values** (cross-reference exception): not applicable. No spanned cell is a bare pointer, and "Ongoing ..." / "As needed" are timing.
- §5 **Spanned values** (text naming its own columns): not applied. T1 row 32 cols 10:14 names "Week 13 to Week 18" and "Week 19 to Week 52", which together cover all of V9-V13. T2 row 26 "Ongoing" keeps the drawn span 6:8 (V18-V20). The note's "After Week 156" agrees with that.
- §6 **Notes / Instructions / Comments column**: each ruled note cell is one annotation, bounded by its horizontal rules (label-column and notes-column rules coincide on every body row). The cell is split only along printed markers (*, #, $) and under **Binding by what a note names**.
- §6 **Binding by what a note names**, splits applied: T1 r27 "ABO blood group (CL) (at Screening if unknown)." to cell col 2 (n19). T1 r33 first sentence ("...beginning of the Baseline Period...") to cell col 3 (n22). T1 r51 first sentence ("...up to 3 days prior to Day 1/ Visit 3...") to cell col 4 (n32). T1 r52 first sentence ("On Day 1 ...") to cell col 4 (n34). T2 r32 "...returned during the Week 312 visit." is a whole note naming one cell, so it is bound to cell col 8 (n20) and not split. The judgement calls among these splits are D2 and D3.
- §6 **Binding by what a note names**, not split because the sentence names more than one cell: T1 r32 "For first 12 weeks ..." (V4-V8), T1 r37 / T2 r17 "collected at Baseline / End of Study and if the corticosteroid treatment is needed" (one visit plus the conditional span), T1 r42 "Week 1 and Week 2".
- §6 **Binding by what a note names**, visits named only as reference points are not bound: T1 r46 "starting at Visit 1", T1 g3 (Day 1/Visit 3), T1 g4 and T2 g4 ("procedures for Week 312 visit"). T2 g3 (Weeks 53 to 312) covers the whole table, so it is table-wide.
- §6 **Text above or below the frame**: "Alternative study measures during the COVID-19 Pandemic ... is described in Appendix 13." is printed above both frames. It is a bare pointer, so `source_note` g1 in each table.
- §6 **Abbreviations are not annotations**: the abbreviation blocks under both frames (doc pp.23-24, 27-28) yield zero annotations.
- §6 **marker_locations** / printed markers: the source reuses `*` and `#` row by row for different notes (T1: 12 notes with `*`, 5 with `#`; T2: 3 with `*`, 4 with `#`, 1 with `$`). They are transcribed literally, each note with the exact cells or labels where its marker is printed. No element carries the same marker for two different notes.
- §1c **Symbol-font glyphs**: the text layer has U+F061 where the page shows α ("α-Fetoprotein (CL)", both tables), U+F02A where it shows * (header "*Visit 4"), and U+F0B7 for bullets. The JSON holds "α", "*" and "•". No private-use code point remains (checked).

## 1. Method summary

- PDF has a full text layer and vector rule lines (thin black filled rectangles) on all 16 pages. Not image-based, not glyph-spread.
- Column boundaries come from the vertical rules (T1: 15 boundaries, giving 13 data columns; T2: 9 boundaries, giving 7 data columns). Row boundaries come from the horizontal rules in the label and notes columns. Tokens come from `pdftotext -bbox` and are binned to each ruled cell by their centre.
- Merged spans come from per-row coverage of each internal vertical rule. A boundary with no rule segment across the row band means a merged cell.

## 2. Per table

**Table 1 — Year 1** (`main_soa`, doc pp.13-24, title "1.3. Schedule of Activities (SoA)" — see D5)
- 13 data columns (V1-V13). Header rows: Epoch / Study Visit / Weeks after IP administration.
- 35 activities, 175 `activity_schedule` entries (94 single-cell X marks + 81 entries distributed from merged text cells), 66 annotations (65 footnote, 1 source_note).
- Activity rows per page: p.13: 3, p.14: 3, p.15: 5, p.16: 5, p.17: 2, p.18: 2, p.19: 6, p.20: 2, p.21: 3, p.22: 3, p.23: 1, **p.24: 0**. p.24 is genuinely header-only: it prints the reprinted header rows and the last two lines of the abbreviation list, with no body row.

**Table 2 — Year 2 to Year 6** (`main_soa`, doc pp.25-28, title empty — see D5)
- 7 data columns (V14-V20). Header rows: Epoch / Study Visit / Weeks after IP administration.
- 23 activities, 126 `activity_schedule` entries (69 single-cell X + 57 distributed), 33 annotations (32 footnote, 1 source_note).
- Activity rows per page: p.25: 9, p.26: 6, p.27: 8, **p.28: 0**. p.28 is header-only plus the abbreviation-list tail.

## 3. Annotations

### 3.1 Synthesised markers
- T1: g1 (above frame), n1-n5 and g2-g4 (header notes cell bullets), n6 (row-3 label sentence), n7-n43 (unmarked notes-column text in reading order), g5-g6 (below frame). No number is skipped.
- T2: g1 (above frame), g2-g4 (header notes cell bullets), n1-n20 (notes-column text), g5 (below frame).
- `g` notes sit on `schedule_property` row 1 with `method: synthesized`, and their marker is on no element. `n` notes bound to the row they sit beside use `activity_name` + `synthesized`. `n` notes bound to a named cell use `text_match`.

### 3.2 Header notes cell (D1)
The shaded cell to the right of the three header rows is one ruled cell with nine bullets, the sixth of which carries the printed `*` matching "4*" in the Study Visit row. I treated the bullets as separate notes because they print as distinct list items and they govern different elements. Binding: n1 → row 1 col 2; n2 → row 1 cols 2-3; n3 → row 1 col 4; n4 → row 2 cols 2-4; n5 → row 3 col 4; `*` → row 2 col 5 (printed). g2-g4 are table-wide. T2's header cell holds three bullets (g2-g4), all table-wide.

### 3.3 Sentence-level splitting inside note cells (D2, D3)
T1 r10 and r58 are the two places where a sentence names one visit only inside a parenthesis or as emphasis. I kept those sentences on the row (D2) or moved them to the Baseline cell (D3) as described. The other splits listed under Recorded follow the rule directly.

### 3.4 Text below the frames (D4)
"Note: Study Site: ..." and "Infusion Center: ..." (T1 p.23), and "Study Site: ..." (T2 p.27), are printed between the frame and the abbreviation list. They are emitted as `g` footnotes.

## 4. Merged-mark decisions (all spans from rule-line geometry)

- T1 header row 1: "Week 1-12 (± 2 days)" 5:9; "Week 13-52 (±1 week)" 10:14.
- T1 r32 FIX Activity...: "Collect 2-3 times every week starting from Day 1. Utilize mobile phlebotomy/home health services as needed." 5:9; "Week 13 to Week 18: Collect once every week. ... Week 19 to Week 52: Collect at site visits." 10:14.
- T1 r37 Immunology (PBMC): "Only if corticosteroid treatment is needed ..." 5:14.
- T1 r47 Review the eDiary: "Ongoing (including unscheduled visits)" 3:14, marker `#` on every covered entry.
- T1 r62 Vector Shedding: "Weekly, as needed Utilize mobile phlebotomy / home health services, as needed" 5:14, marker `#`.
- T1 r63 Any Additional Safety Tests: "Ongoing (as needed)" 2:14. T1 r64 Adverse Events and r68 Concomitant Medications: "Ongoing (including unscheduled visits)" 2:14.
- T2 header row 1: "Week 53-156 (±2 weeks)" 2:5; "Week 157-260 (±3 weeks)" 6:7.
- T2 r5 Electrocardiogram: "As needed" 2:8.
- T2 r16 FIX Activity...: "Starting at Week 53, monitor once every 3 months" 2:5 (`$`); "Starting at Week 157, monitor once every 6 months" 6:7 (`#`).
- T2 r17 Immunology (PBMC): "Only if corticosteroid treatment ..." 2:7.
- T2 r26 Phone call: "Ongoing" 6:8.
- T2 r27 Vector Shedding: "Weekly, as needed Utilize mobile phlebotomy/ home health services as needed" 2:8 (`#`).
- T2 r28 "Ongoing" 2:8. T2 r29 / r30 / r31 "Ongoing (including unscheduled visits)" 2:8.

## 5. Mechanical mark-check

- Method: §1b bbox column-binning on the text layer (`pdftotext -bbox`), with per-row vertical rule coverage for spans. All 12 T1 pages and 4 T2 pages were processed. Reprinted header rows were de-duplicated, and each activity and mark was counted once.
- I diffed the mechanical matrix against an independent visual read of the 100-dpi renders of every page. **0 disagreements** (T1: 94 X cells; T2: 69 X cells). Footnoted marks tokenise as "X*", "X#", "X*#", "X*,#" and are all captured.
- Places where the mechanical read overturns the **markdown** (structure taken from the PDF):
  - T1 p.13 r5: "X#" sits in **col 4 (V3, IP Infusion)**, not under Baseline as the markdown suggests.
  - T1 p.15 r18: the HBcAB/HCV/... label is **one** ruled row. The markdown splits it into two.
  - T1 p.21 r58 PROs: marks are X* at V2 and X at V8, V10 and V13. The markdown misplaces the "*" into its own column.
  - T1 p.23 r68: "Ongoing ..." spans 2:14 (from V1). The markdown leaves the first column empty.
  - The markdown misaligns the header columns on several pages ("3 4*", "Day1 1" merged), and fragments the T2 p.25 header ("(±3 week" / "s)").

## 6. Annotation text integrity

- Not glyph-spread, and no word reconstruction was needed. The markdown glues words in places ("seeSections", "testingnolaterthan", "arenot availableinthe"), so wording was taken from the PDF text layer. That is the same text with correct word breaks, and the substance is the same.
- Every note was checked by script: each sentence of each note occurs verbatim and in order in its ruled cell, and the notes of a cell together cover exactly its words, with no gaps or duplicates.
- Source typos are kept verbatim: "infuion" (T1 r56), "aPTT ... D-dimer, These specimens" (comma, T1 r45), "consecutive specimen test" (T2 r27).
- Containment pairs: none. No annotation text is contained in another within either table.
- Cross-table near-duplicates (X-ray/MRI notes, PRO notes, Immunology PBMC notes) differ in wording between Year 1 and Year 2-6, and each is transcribed as printed.

## 7. Low-confidence calls

- Row 1 typed `epoch` rather than `period`. The values mix named periods (Screening, Baseline), an event (IP Infusion) and week windows. The rules decide it, but a reviewer may prefer `period` for the "Week x-y" bands.
- T1 r46 Issue the eDiary: "starting at Visit 1" is read as a reference point for diary entries, so the note is on the row. Read as a condition on V1, it would go to cell col 2.
- T1 r47 `#` ("Infusion centers are not required to perform this activity on Visit 3") is printed on the merged "Ongoing" cell. It is bound to all 12 covered cells as printed, although its text concerns V3 only.

## 8. Orphan risk

- None found. Every annotation has at least one `marker_location`, and every element marker has a matching location (checked).
- Markers that print without a definition: none. Every printed `*`, `#` and `$` has its text in the same row's notes cell, or in the header cell for V4.
- Reuse of `*` and `#` across rows (see Recorded, §6 marker_locations) relies on the locations to tell the notes apart. No single element carries a marker shared by two notes.

## 9. Method provenance

- `activity_name_source.indentation_method: assumed_flat` on all 58 activities (flat tables).
- `marker_locations[].method: text_match`:
  - T1 n1, n2 (2 locations), n3, n4 (3 locations), n5 — header cells.
  - T1 n10 (r10 c2), n11 (r10 c4), n19 (r27 c2), n22 (r33 c3), n32 (r51 c4), n34 (r52 c4), n39 (r58 c3).
  - T2 n20 (r32 c8).
- `marker_locations[].method: synthesized`: all other `n` notes (on their row) and all `g` notes (on `schedule_property` row 1).
- No `annotation_text_source` (all note text read from rule-bounded text-layer cells), no cell `method` (all bbox), no `structure_method`, no `unresolved` locations.
- No redaction anywhere in the 16 pages.
