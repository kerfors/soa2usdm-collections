# NCT03637764 — Tables 02 and 03 (flow charts): uncertainty report

Prompt: PDF_TO_JSON_PROMPT v3.8.1. Source: NCT03637764_soa.pdf. Page numbers are document pages from PAGEMAP.md (PDF 5-7 = document 22-24).
Scope: 'Pharmacokinetics and Immunogenicity Flow Chart' (Table 02, pp. 22-23) and 'Exploratory Biomarker Flow Chart' (Table 03, p. 24) only. Table 01 (SoA, pp. 18-21) was read for context and not extracted. Decision ids continue from Table 01 and start at D7.

## Decisions needed (7)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D7 | p.22, Table 02 header rows 4-7 | The four drug-specific rows 'Sample RNT (h)' and 'Sample time window' are header rows. The RNT rows are typed timepoint at levels 4 (atezolizumab) and 5 (isatuximab). The window rows are typed window with no level. | Treat the two RNT rows as parallel scales at the same level, or as per-drug timing attached to the activities, because neither is the parent of the other. | 2.3 |
| D8 | p.22, Table 02 row 10 (Isatuximab infusion) | Isatuximab infusion marks go in the columns their drawn cells cover: Cycle 1 D1 bar in cols 4-5, Cycle 2 bar in 12-13, Cycle 3 'X' in 15, Cycle 4 bar in 17-18, Subsequent 'X' in 21. From Cycle 2 on, this is one column right of the isatuximab 'SOI' column. | Realign each isatuximab infusion mark to start at the isatuximab SOI column of its cycle (cols 2, 11, 14, 16, 20). | 2.4 |
| D9 | p.22, Table 02 rows 12-13 | PK rows are named with the full two-line label: 'Atezolizumab Sample ID', 'Isatuximab Sample ID'. | Name them 'Atezolizumab' / 'Isatuximab' and read 'Sample ID' as a caption saying the cells hold sample identifiers. | 2.5 |
| D10 | p.22 Table 02 rows 2-3 cols 22-23; p.24 Table 03 rows 1-2 col 4 | A header cell merged vertically over two header rows is recorded on the upper row only: 'EOT' and 'FUP (90±7 days)' on the Cycle row, and 'Screening' on the Study phase row. The lower row is left empty in those columns. | Repeat the vertically merged value on each header row it covers. | 2.2 / 3.2 |
| D11 | p.24, Table 03 rows 4, 11, 13, 14, 16, 18 | The three label columns are turned into a hierarchy. Each Sample Collection label is a mark-free level-0 grouping row. Each 'Core n' is a level-1 row. The Biomarker Analysis text is the activity that carries the marks (level 1, or level 2 under a Core). row_position is renumbered in reading order (header 1-3, body 4-19), so it is not the physical row. | Keep one activity per physical row (10 body rows), named by the Biomarker Analysis text only, with the Sample Collection and Core labels kept in cell_text or joined into the name. | 3.3 |
| D12 | p.24, Table 03 | Table typed subsidiary: a finer per-cycle breakdown of the single main-SoA row 'Tumor Biopsy, Archival Tumor Tissue Collection, Biomarker Blood Draw — See Biomarker Flow Chart'. | Type it main_soa, as an independent schedule with its own columns. Its columns are not a strict refinement of the main SoA: it adds Cycle 2 / Cycle 3 / 'Cycle 5 and Beyond' and has no D8/D15. | 3.1 |
| D13 | p.24, Table 03 rows 15, 17, 19, note n4 | The Notes cell beside the three biopsy-core rows is one rule-bounded cell with two paragraphs. It is extracted as one note bound to all three core analysis rows. | Split it into two notes: 'Refer to I 02 for details.' on Core 1 only, and 'At least 1 tumor core is required …' on Core 2 (or on all three cores). | 3.5 |

## Recorded, not open (9)

- Table 02 typed `subsidiary` (§2, PK-sampling note): the main SoA rows 'PK' and 'ADA' (p.21) read 'See Pharmacokinetics and immunogenicity Flow Chart'. This table gives per-sample timing for them.
- The abbreviation block on p.23 was not extracted (§6). Its terms (SOI, EOI, P, S, AP, AS, …) appear only as cell values, never as markers.
- '-' cells were transcribed literally as `cell_value "-"` (§1). Grey-shaded cells with no text have no entry.
- Merged marks and merged '-' cells were distributed across their span with `source_range` (§5). Spans are listed in 2.4 and 3.4.
- Qualified marks were kept literal (§5): 'X (odd cycles only)', 'X (if clinically feasible)', and 'X (only 1 on-treatment sample, see Study Reference Manual for details)'. The last one is distributed over cols 6:8, so each of the three cells carries the 'only 1 sample' qualifier.
- Footnote a of Table 02 ('Refer to the laboratory manual …') is typed `source_note`, because it is a bare pointer (§8 check).
- The notes-column cells in Table 03 have synthesised markers n1-n4 with `method: synthesized` (§6).
- Table 03 marker 'a' is printed in header cell 'D1 (-1 day) a'. It is bound to that `schedule_grid` cell (row 3, col 5), not to the property row (§6).
- Label columns: Table 02 has L=1, so data starts at col 2. Table 03 has L=3 (Sample Collection, Core sub-column, Biomarker Analysis), so data starts at col 4 (§5).

---

## 1. Method (both tables)

- The pages have a text layer and vector rule lines, and are not glyph-spread. Text and marks come from `pdftotext -bbox`.
- Cell boundaries come from rule lines detected on a 200 dpi render (§1d): vertical rules per row band, and horizontal rules per column. The Notes column of Table 03 is included.
- pdfplumber was not used.

## 2. Table 02 — Pharmacokinetics and Immunogenicity Flow Chart

### 2.1 Type and counts
- `subsidiary` (see Recorded).
- 22 data columns (positions 2-23).
- 7 header rows and 9 activity rows (3 section headers, 6 activities).
- 105 activity_schedule cells, 152 schedule_grid cells, 10 annotations (9 footnote, 1 source_note).
- Rows per page: p.22 = 9. p.23 = 0, because p.23 holds only footnotes f-j and the abbreviation list; there are no body rows there.

### 2.2 Header structure
- Rows: Study Phase (epoch, L1), Cycle (cycle, L2), Day within the Cycle (study_day, L3), Sample RNT atezolizumab (timepoint, L4), window atezolizumab (window, null), Sample RNT isatuximab (timepoint, L5), window isatuximab (window, null).
- Horizontal merges: Treatment Phase 2:21 and Post-treatment 22:23. Cycle 1 2:10, Cycle 2 11:13, Cycle 3 14:15, Cycle 4 16:19, Subsequent Cycles 20:21. D1 2:8, D15 9:10, then D1 per cycle.
- RNT '-' spans: atezolizumab 5:10, 12:13, 17:19. Isatuximab 3:4. Window rows follow the same pattern.
- EOT and FUP are vertically merged over the Cycle and Day rows. The rule at the Cycle/Day boundary is absent in cols 22-23. See D10.
- Source oddity, transcribed as printed: the '72h' and '168h (SOI of Day 8)' columns sit under the Cycle 1 'D1' span.

### 2.3 Drug-specific timing rows (D7)
- Rows 4-7 hold relative nominal times and windows for each drug separately. Both RNT rows are needed to tell the columns apart. For example, cols 3 and 4 differ only in the atezolizumab row, and cols 5-8 differ only in the isatuximab row. So both RNT rows get a level.
- Stacking them as levels 4 and 5 follows print order only. The property_comment on each row says so.

### 2.4 Merged marks and the isatuximab infusion row (D8)
- Infusion bars 'X----X' were distributed over their span with the literal glyph string:
  - row 9: 2:3
  - row 10: 4:5, 9:10, 12:13, 17:18
- Merged '-' spans:
  - row 12: 5:10, 12:13, 17:19
  - rows 15 and 16: 3:10, 12:13, 17:19
  - row 16 also: 21:22
- The lower body rows (9-16) are drawn with vertical rules offset up to about 16 px at 200 dpi (about 6 pt) from the header rules. Examples: 1133 vs 1141, 1201 vs 1217, 1520 vs 1505, 597 vs 609.
- Each body cell was assigned to header columns by majority x-overlap. This matters in three places:
  - row 15/16 cell 1201-1369 → 12:13
  - row 9/10 cell 1445-1520 → col 15
  - row 10 cell 530-671 → 4:5
- D8: the isatuximab infusion cells in row 10 do not line up with the isatuximab 'SOI' header column. In Cycles 2, 3, 4 and Subsequent they are one column to the right. In Cycle 1 D1 the bar covers the atezolizumab 'EOI +30 min' column and the isatuximab 'EOI' column. This may be deliberate, since isatuximab may be given after the atezolizumab infusion. It conflicts with the isatuximab SOI sample P00 in cols 2, 11, 14, 16, 20. Transcribed by geometry, not realigned.

### 2.5 Activity names (D9)
- Section rows 8, 11 and 14 are bold, level 0, `indentation_method: font_signal`. They carry no marks. Rows 11 and 14 carry markers only (a,i and a,e,i).
- The name 'Atezolizumab' / 'Isatuximab' repeats under three sections. The parent row tells them apart.
- Raw marker spacing was kept in cell_text: 'Immunogenicity (ADA) a, e ,i' and 'Isatuximabg,h i'. The markers were normalised to 'a,e,i' and 'g,h,i'.

### 2.6 Annotations
- Footnotes a-j: a-e are on p.22, f-j on p.23. Each has at least one location. Counts: a 2, b 10, c 5, d 1, e 1, f 4, g 7, h 2, i 4, j 4.
- None of the note texts contains or overlaps another.
- Wrapped cell text was rejoined: '±10 mi n' → '±10 min', '±10 m in' → '±10 min', 'AP00 / b' → 'AP00' with marker b.

### 2.7 Mechanical mark-check
- 98 body tokens and 96 header tokens were binned to the header column bands and diffed against the JSON. Result: 0 mismatches, and no JSON cell is left without a token.
- On the first pass the diff caught one extraction error of mine: the atezolizumab infusion 'X' marks at cols 11, 14, 16 and 20 (row 9) had been omitted. They are now included.
- Spans were confirmed from rule geometry, not from where the glyphs sit.

## 3. Table 03 — Exploratory Biomarker Flow Chart

### 3.1 Type and counts (D12)
- `subsidiary`. The reason is in table_metadata.notes. Same participants with different columns rules out domain, and it is not a separate timeline, so not track.
- 7 data columns (positions 4-10).
- 3 header rows, 16 activity rows (3 level-0 groups, 3 'Core n' rows, 10 mark-carrying analyses).
- 34 activity_schedule cells, 20 schedule_grid cells, 5 annotations (all footnote).
- Rows per page: p.24 = 16.
- Source oddity: there is no 'Cycle 4' column. The columns jump from Cycle 3 to 'Cycle 5 and Beyond'. Transcribed as printed.

### 3.2 Header structure
- The label columns hold only the column captions 'Sample Collection' and 'Biomarker Analysis', merged over all three header rows. So the three property names are synthesised: 'Study phase' (epoch, L1), 'Cycle / visit' (cycle, L2), 'Day / timing' (study_day, L3).
- Row 2 mixes cycles with EOT / Follow-up. Row 3 mixes study days with 'n (±7) days after last IMP admin'. Both are noted in property_comment.
- 'Screening' is vertically merged over rows 1-2. There is no rule at that boundary in col 4. See D10.

### 3.3 Label-column hierarchy (D11)
- The physical body has 10 rows. The left label 'Peripheral Blood' spans 6 of them, 'Archival …' spans 1, and 'Baseline or on treatment tumor biopsy core …' spans 3. Within the biopsy block, 'Core 1/2/3' sit in their own sub-column.
- The schema has one activity-name column, so these became separate grouping rows with sequential row_position. `indentation_method: visual_estimate` records that the levels come from column geometry, not whitespace.
- As a result, rows 15 and 17 have the same name ('CD38 and PD-L1 expression, immune contexture (IHC analyses)'). They are told apart only by their parent Core row.
- The archival group has a single child, the 'If feasible, biomarker analyses listed below …' row, which carries the Screening X.

### 3.4 Merged and qualified marks
- One horizontal merge: row 9 (Adaptive immunity), 'X (only 1 on-treatment sample, see Study Reference Manual for details)', over cols 6:8. The rule is absent between cols 6-7 and 7-8 in that row only.
- Note on meaning: distributing that cell puts it in three columns, while the text says one sample in total. The qualifier travels with each cell.

### 3.5 Notes column (D13)
- Cell text was bounded by the Notes-column horizontal rules. Rules were found at 546, 625, 695, 741, 785, 881, 977, 1099 and 1387 px. The body cells beside Immune genetic markers and Tumor mutational profile are empty.
- n1 'Only for Phase 2 Stage 2 participants' → rows 7, 8. These are two separate cells with identical text, deduplicated.
- n2 'Only Phase 2 Stage 2 participants, if sufficient clinical response …' → rows 9, 10. Also two separate cells, deduplicated.
- n1 and n2 share a long run of text. This is source-faithful: they sit in separate rule-bounded cells with different wording ('Only for …' vs 'Only … , if …'). It is not a split cell.
- n3 (archival note) → row 12. It is bound to the mark-carrying analysis row, not the group row 11.
- n4 → rows 15, 17, 19. It is one cell spanning the three core rows, with no rule between them. See D13.
- The header-row Notes cell holds the definition of footnote 'a'.
- Unresolved cross-reference: n4's text is 'Refer to I 02 for details.' In the PDF, 'I 02' is a blue italic link whose target is not in the excerpt. It was transcribed literally. I don't know which protocol section or appendix it points to.

### 3.6 Mechanical mark-check
- All X / qualified-X tokens were binned to the column bands 752/903/992/1081/1195/1396/1548/1712 and diffed against the JSON. Result: 0 mismatches.

## 4. Cross-table

- Synthesised:
  - Table 03 property names on rows 1-3.
  - Table 03 note markers n1-n4.
  - No synthesised markers in Table 02.
- Method provenance (non-default):
  - `indentation_method: font_signal` on all Table 02 activities.
  - `indentation_method: visual_estimate` on all Table 03 activities.
  - `marker_locations.method: synthesized` on n1-n4.
  - No `unresolved` locations.
  - No proximity-bounded or raster-read cell values; all text is from the text layer.
- Orphan risk:
  - None. Every annotation has at least one location, and every location's marker is on its row's or cell's `annotation_markers`.
  - Only open point: the 'I 02' target in n4.
- Validation: both files validate against soa-table-extraction.schema.json (Draft-07) with 0 errors. The review_items arrays (Table 02: D7-D10; Table 03: D11-D13) match the Decisions-needed block one-to-one.
