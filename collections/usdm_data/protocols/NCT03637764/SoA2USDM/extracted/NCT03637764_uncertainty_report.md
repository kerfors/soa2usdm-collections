# NCT03637764 — SoA extraction uncertainty report

Prompt 3.8.1, single pass. Source: NCT03637764_soa.pdf (7 PDF pages = document pages 18-24 per PAGEMAP.md; declared SoA range 18-21). No protocol markdown available.

## Decisions needed (6)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.22-24 (flow charts) | Only the SoA table on pages 18-21 (the declared SoA range) was extracted. The "Pharmacokinetics and Immunogenicity Flow Chart" (p.22-23) and "Exploratory Biomarker Flow Chart" (p.24), which the SoA's PK / ADA / Tumor Biopsy rows point to, were not extracted. | Extract both flow charts as extra tables (Table 02, Table 03), typed `subsidiary` because they give finer timing for the PK, ADA and biomarker activities of Table 01. | 3.1 |
| D2 | p.21, rows 28-30 (PK, ADA, Tumor Biopsy/Biomarker) | The merged text "See Pharmacokinetics and immunogenicity Flow Chart" / "See Biomarker Flow Chart" is kept as a cell value in every data column 2-11 (source_range 2:11). The printed cell also runs into the Notes column, which is not a data column. | Treat these as cross-references (`source_note` annotations on the activity) and leave the three rows' grid cells empty. | 3.3 |
| D3 | p.18, header row 2 | Header row 2 (Cycle 1 / Cycle 2 and Beyond / Safety follow-up Period / Survival follow-up) is typed `period`, because it mixes treatment cycles with follow-up sub-periods. | Type it `cycle` or `other`. | 3.5 |
| D4 | p.20-21, rows 17-19, 24-32 | Blood Typing, Serology and Urinalysis (top of p.20) sit under "Laboratory Assessments" (indent 1) across the page break. The bold rows Isatuximab/Atezolizumab Administration and all p.21 rows are top-level (indent 0), not children of "Disease Assessment". | Make the p.20 lab rows top-level, and/or make the rows after the Disease Assessment block its children. | 3.5 |
| D5 | p.20, row 24, marker c | Footnote c ("evaluation not applicable for Cohort E") is printed in the Notes cell beside Isatuximab Administration. It is bound to every cell with a printed superscript c (the D8/D15 marks of 8 rows, 16 cells), not to the Isatuximab row alone. | Also bind c to the whole Isatuximab Administration row, or restrict it to that row's cells. | 3.4 |
| D6 | p.20, rows 17, 21-23, column 7 | Qualified entries in the "Cycle 2 and Beyond D1" column ("Cycle 2 Day 1 only"; "X (Weeks 9, 18, 27, and then every 12 weeks)"; "X (Weeks 6, 12, 18, 24, and then every 9 weeks)") are kept literally in that one column, because the weeks they name are not columns of the table. | Change them to a bare "X" and move the timing into a synthesised footnote on the cell. | 3.3 |

## Recorded, not open (9)

- §5 label columns: L = 1 ("Evaluation"), so the first data column is position 2. Data columns are 2-11. The Notes column (12) is excluded from the grid.
- §6 Notes column: each non-empty note became an annotation bound to the row it sits beside, using a synthesised marker (n1, n2, pr1-pr6) with `method: synthesized`.
- §6 bare pointers ("Section 8.2.1", "See Section 8.2.2", "See Section 8.2.3", "See Section 10.3 Table 12", "Section 10.3", "Section 8.1") are typed `source_note` and de-duplicated by text. "Section 10.3 Before each transfusion." both points and explains, so it is a `footnote` (n2).
- §5 merged marks are spread across their spans, with spans taken from raster rule lines (see 3.3).
- §5 qualified marks that name a condition ("X (<7days prior to first dose)", "X (within 7 days prior to first dose)", "X (if necessary)", etc.) are kept literally in `cell_value`.
- §6 header-cell footnote a is on the "Treatment Phase" header cell. It is encoded on the `schedule_grid` cells of row 1, columns 4-7 (the merged span), not on the property row.
- §4 section-header rows "Laboratory Assessments" and "Disease Assessment" carry no marks and are level 0.
- §4/§1 grey-shaded cells hold no content. They are treated as formatting and no cell is emitted for them.
- Types (§2): one printed table over four pages with the header reprinted on each page is extracted as ONE table (pages 18-21), not as continuations. The reprinted header rows are de-duplicated.

## 1. Table summary

**Table 01 — "SCHEDULE OF ACTIVITIES (SOA)"**, `main_soa`, document pages 18-21.
- Header rows: 3 (epoch / period / study_day). Data columns: 10 (positions 2-11).
- Activities: 29 (including 2 section headers). Schedule cells emitted: 159. Annotations: 11 (footnote 5, source_note 6).
- Activity rows per page: p.18 = 6, p.19 = 7, p.20 = 9, p.21 = 7. Every page in the range contributed rows.
- Pages 22-24 are outside the declared range and were not extracted (D1).

## 2. Synthesised

- Property names: all three header rows have an empty label column (the "Evaluation" header spans all header rows), so their names were synthesised: "Study Phase", "Cycle / Follow-up Period", "Study Day / Visit Timing".
- Annotation markers: n1 (Informed consent note, row 4), n2 (Blood Typing note, row 17), pr1 "Section 8.2.1" (rows 6, 7), pr2 "See Section 8.2.2" (rows 8, 9), pr3 "See Section 8.2.3" (row 10), pr4 "See Section 10.3 Table 12" (row 13), pr5 "Section 10.3" (rows 14, 15, 16, 18, 19), pr6 "Section 8.1" (rows 21, 22, 23).

## 3. Details

### 3.1 Scope (D1)
PAGEMAP marks pages 22-24 as beyond the declared SoA range. They hold two further tables that the SoA refers to: PK/immunogenicity (p.22, with footnotes and abbreviations continuing on p.23) and exploratory biomarkers (p.24). Both would classify as `subsidiary`. They were left out to respect the declared range. If they are wanted, the PK chart is a dense grid with sample IDs, time windows and many footnotes (a-j), and should be extracted as a separate pass.

### 3.2 Mechanical mark check
- The source is a text-layer PDF, not glyph-spread. `pdftotext -bbox` X tokens (regex `^[Xx][*a-zA-Z0-9]?$`, with superscript "c" as separate tokens) were binned to column bands. The bands came from vertical rule lines found in a 200-dpi raster.
- Result: every X and every superscript c landed in the expected column. Merged-cell glyphs sit under a single column, as expected (for example, screening X's under column 3), and were spread by rule-line geometry. The mechanical matrix and the visual read agreed in every cell.
- On pages 19 and 20 the column-3/4 rule is offset by about 4 px, so the automatic boundary probe reported it as "missing" in some rows. A visual check of the crops confirmed separate cells there. No span was taken from those probe results.

### 3.3 Merged spans spread (D2, D6)
- Rows 4, 5, 7, 8, 9, 10, 18, 21, 26: X over screening, 2:3.
- Row 27: "X (within 14 days prior to first dose)" 2:3.
- Rows 10 and 16: "As clinically indicated" 7:8.
- Row 19: "As clinically indicated" 5:7.
- Rows 21-23: "X (until PD is confirmed if no PD documented & confirmed)" 10:11 (Safety FU 90-day plus Survival).
- Rows 26-27: "Continuously throughout period" 4:8.
- Row 26: "X (ongoing related AEs, ongoing SAEs at EOT and new related AE/SAEs)" 9:11.
- Row 27: "X (related to AE/SAEs listed above)" 9:11.
- Rows 28-30: "See … Flow Chart" 2:11. The printed cell also covers the Notes column (D2).
- Header: Screening 2:3, Treatment Phase 4:7, Post Treatment Follow-up Phase 9:11, row-2 blank 2:3, Cycle 1 4:6, Safety follow-up Period 9:10.

### 3.4 Annotations
- a (header, "A cycle is 21 days"): printed in the Notes header area on every page. Emitted once.
- b (Pregnancy test activity label): the text wraps "administra / tion." in the source. It was rejoined as "administration."; no other reconstruction was needed.
- c (16 cells): see D5.
- Containment pair: "Section 10.3" (pr5) is contained in "Section 10.3 Before each transfusion." (n2) and in "See Section 10.3 Table 12" (pr4). Checked against page 20 (and page 19): these are separate Notes cells on different rows, so the overlap is **source-faithful**, not a split note.
- Every Notes cell is bounded by rule lines in the text layer. No proximity-bounded text.

### 3.5 Low-confidence structure (D3, D4)
- Row 2 type: see D3.
- Hierarchy: the table has no visual indentation. Levels come from font signals (bold full-width section rows), recorded as `indentation_method: font_signal`. "Height (at baseline only)" is partly bold in the source; it is still a level-0 activity.

## 4. Orphan risk
None. Every annotation has at least one marker_location. Every location's marker also appears in that row's or cell's `annotation_markers`. No marker is used without a printed definition.

## 5. Method provenance
- `indentation_method: font_signal` on all 29 activities.
- `method: synthesized` on the marker_locations of n1, n2 and pr1-pr6 (Notes-column notes with no printed marker).
- Cell spans come from raster rule lines (§1d); cell text comes from the text layer (default). No `unresolved` locations.
- Spot-check recommended: the page-21 "See … Flow Chart" rows and the 10:11 PD spans on page 20.
