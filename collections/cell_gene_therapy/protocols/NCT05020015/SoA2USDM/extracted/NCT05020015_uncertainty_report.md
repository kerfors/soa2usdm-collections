# NCT05020015 — Uncertainty report (Layer 1 extraction)

Takeda TAK-007-2001, Protocol Incorporating Amendment No. 5 (03 June 2024). Appendix A Schedule of Events, document pages 98-105. Prompt 3.9.4, schema soa-table-extraction 1.1.

## Decisions needed (4)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | Table 1 (A1), p.99, row 20 Virus tests (CMV, HBV, HCV, HIV) | The three-paragraph instruction in this row is one merged cell drawn over every column from Conditioning (D -5, -4, -3) to M6; its whole text is recorded as the cell value of each of those 13 columns (span 3:15). | Treat the first paragraph (HBV DNA / HCV RNA at D28/M1, M2, M3, M4 and M6) as text naming its own columns and place it on those 5 columns only; keep the rest on the full span or move it to a note on the row. | 4.2 |
| D2 | Table 1 (A1), p.99, row 28 Lymphoma tissue collection, marker k | Footnote k (lymphoma biopsy at screening; optional Day 7-14 biopsies) is printed below the table, but its marker appears nowhere in the grid. It is bound to the Lymphoma tissue collection row because its text names that procedure (bound by word match, not by a printed marker). | Leave footnote k unbound to any row (location recorded as unresolved), because the source never prints the k marker. | 4.4 |
| D3 | Table 1 (A1), p.100, unmarked note g2 | The sentence "Day 0 will be defined as the actual day of first TAK-007 administration, hence the reference of all subsequent dates." is kept as a table-wide note, because it defines the reference point for every date in the table. | Bind it to the D0 header cell (column 4) only, reading it as a statement of when the D0 visit occurs. | 4.4 |
| D4 | Table 3 (A3), p.104, header row 2 | The unlabelled header band "M12, M18, and M24" / "Annually after M24 until 15 years" is typed as a visit row (the month visits each column stands for). | Type it as "other" (a visit-timing / recurrence description), because its values are month labels and an annual recurrence rather than named visits. | 6.1 |

## Recorded, not open (21)

1. §2 / taxonomy decision tree: Table 1 (A1) is `main_soa`, the anchor schedule for all patients.
2. §2 / taxonomy **domain** ("SAME column structure"): Table 2 (A2, Biomarker Sampling) is `main_soa`, not `domain`. Its columns differ from A1: it has no Screening column, a single "D -5" column instead of "D -5, -4, -3", and M2, M3, M4 and M6 merged into one column. It is not finer timing, so not `subsidiary`, and every participant passes through it, so not a `track`.
3. §2 **track vs main_soa**: Table 3 (A3, from Month 6 onwards) is `main_soa`. The title says "All Patients" and the table is the sequential part of the timeline after Month 6 (secondary, then long-term follow-up). Sequence alone never makes a track.
4. Taxonomy **continuation** ("a page break inside ONE printed table is not a continuation"): each of A1 (pp.98-101), A2 (pp.102-103) and A3 (pp.104-105) is one table across its pages. No continuation tables.
5. §5 **Leading label columns**: L = 1 in all three tables; the first data column is position 2 (A1 2-15, A2 2-11, A3 2-3).
6. §1b **De-duplicate repeated rows** / §4 `row_position`: the reprinted header on pp.99, 100, 101, 103 and 105 is not emitted, and its rows keep their numbers (A1 rows 16-18 on p.99; A2 15-18; A3 12-14). The A1 band "Safety assessments (continued)" (row 19) reprints the row-12 group band and is not emitted.
7. §4 **Hierarchy — printed signals only**: A1's full-width bold bands "Safety assessments" (row 12), "Study Intervention" (24) and "Disease assessments" (27) are level-0 groups, and their rows are level 1. Rows 4-11 come before the first band and stay at level 0. The Safety group stays open across the p.98/99 break to row 23. A2 and A3 are flat (`assumed_flat`).
8. §3 **Header bands — typing** (printed row label wins): A1 "Days" is `study_day`, although D28/M1 and M2-M6 are month labels.
9. §3 qualifier inside one header cell: A1 "Conditioning Phase LDC" (row 1, col 3) and A2 "D -5 (before LDC Administration)" (row 3, col 2) keep their printed qualifiers.
10. §3 synthesised `property_name`: A2 rows 1-3 are "Epoch", "Period" and "Study Day". The label cell spans the three rows and prints "Procedure", which is kept as `cell_value` on row 2. A3 rows 1-3 are "Epoch", "Visit" and "Window", with "Procedure" kept on row 3. All carry `structure_method: inferred_from_layout`.
11. §3 `hierarchical_level`: the Visit window / Window rows of all three tables are `null`.
12. §5 **Merged marks / Spanned values**: A1 row 10 Pregnancy tests "As clinically indicated" 4:15; row 21 Concomitant medications and row 22 AE arrows 2:15; rows 28 and 31 "(Range of D7-D14)" 7:9 (the text names D7-D14, which are exactly the three covered columns); row 30 BM biopsy "Mandatory for iNHL …" 11:15; row 31 BM aspirate "Recommended for iNHL …" 11:15. A3 row 4 Adverse events arrow 2:3. Every span was confirmed from the per-row vertical rule geometry.
13. §5 **Qualified marks**: A2 col 11 values "M2 and M6", "M3 and M6" and "M2, M3 and M6" stay in the single combined column. "X (before TAK-007 administration)" stays in A2 col 3. A1 col 3 "Day -5 (before LDC)" and "D -5 to D -3" stay as printed. A3 "M12 and M24", "M12" and "As clinically indicated until M12" stay in col 2.
14. §5 **Empty cells**: in A1 row 23 Survival follow-up, cols 2-12 are one merged empty cell, so no entries. Only M3 (13) and M6 (15) carry X.
15. §6 **Binding by what a note names** (names no element → table-wide `g`): A1 g1 ("Tests and procedures…") and g3 ("After the Day 28 visit, each month will count as 30 days."); A2 g1 ("Unless otherwise noted…") and g2 ("After the Day 28 visit…"); A3 g1 ("Tests and procedures…") and g2 ("Each month will count as 30 days."). Day 28 is named only as a reference point, so it is not bound.
16. §6 **annotation_type** (pure cross-reference → `source_note`): A1 footnote i ("Details on monitoring of AEs … are summarized in Section 10.3."). A3 a and b point to Section 10.3 and also explain, so they are `footnote`.
17. §6 **Header-cell footnotes**: A1 a is on the "Screening Phase" cell (row 1, col 2) and b on the "D0" cell (row 2, col 4). A2 a is on the "M2, M3, M4 and M6" cell (row 3, col 11).
18. §6 **Abbreviations are not annotations**: the abbreviation blocks on pp.100, 103 and 104 are not emitted.
19. §6 **Deduplicate by text**: A1 c is one annotation with 16 cell locations; A2 c is one with 7.
20. §6 **Text above or below the frame**: A2 "Unless otherwise noted, … medical monitor." is one printed paragraph and is kept as one note.
21. §6 **Synthesised markers — prefix and numbering**: `g1`-`g3` (A1), `g1`-`g2` (A2) and `g1`-`g2` (A3) are numbered in reading order below each table. No `n` or `pr` markers were needed: there are no inline section references in labels and no notes column.

---

## 1. Source and method

- 8-page excerpt; document pages from PAGEMAP.md (the printed footers run one lower: "Page 97 of 144" is doc p.98).
- Text layer present on every page; vector rule lines present on every page (no raster recovery needed); no images.
- A diagonal watermark ("Property of Takeda: For non-commercial use only and subject to the applicable Terms of Use") crosses every page. Its glyphs were identified by text direction in the PDF (rotated lines) and removed from the token stream before binning. 259 fragments were dropped. A few fragments that overlap upright glyphs survived, such as "th", "ap", "no" and "m". They were ignored as known residue, and none is an X or a marker.
- Text: footnote and note wording comes from the PDF text layer, cross-checked against the markdown. Structure and marks come from the PDF.

## 2. Mechanical mark-check (§1b)

- Method: `pdftotext -bbox` tokens. Column x-centres were fixed from the header day/visit labels (A1 "Days" row, A2 study-day row, A3 "M12…" row). Each mark token matching `^[Xx][*a-zA-Z0-9]?$` was binned to the nearest centre. Merged spans were read from per-row vertical rule segments, and marks were distributed over every covered column. Superscript markers were taken from glyphs of 8 pt or smaller and assigned to their ruled cell. The result was diffed cell for cell against the JSON.
- Result: **0 disagreements.**
  - A1: 96 X cells / 145 non-empty cells / 28 markers.
  - A2: 44 / 50 / 10.
  - A3: 11 / 14 / 7.
  - Nearest-centre binning and the ruled cell agreed for every mark.
- The marker diff has two explained residuals, neither of them an error:
  - The subscript "2" in "SpO2" (A1 row 14) is not a marker.
  - Footnote k on A1 row 28 is bound by word match, not by a printed marker (D2).

## 3. Annotation text integrity

- The text layer is not glyph-spread (§1c), and nothing was reconstructed. There are no private-use or U+2011 code points.
- No annotation text is contained in another within a table. A1 g3 and A3 g2 share "each month will count as 30 days", but they are in different tables and are distinct printed notes.
- Every note is bounded by its own printed line(s) below the frame, and each footnote starts at its marker. Footnotes l-n continue onto p.101, below the reprinted header.
- PDF/markdown disagreements (the PDF was used in each case):
  - The markdown runs "Secondprimarymalignancyevaluation" together.
  - It writes "Blood for DNA(CK)" and "Blood(HLA and KIR typing)" without the space the PDF prints.
  - It writes "Section10.3"; the PDF has "Section 10.3".
  - It duplicates the A2/A3 footnote sentences.
  - It does not give the Virus tests cell or footnotes l, m and n as clean text.

## 4. Table 1 — Table A 1 Schedule of Events for All Patients up to Month 6 (main_soa)

### 4.1 Shape and page coverage
- 15 columns: 1 label column plus 14 data columns (Screening, D -5/-4/-3, D0, D1, D3, D7, D10, D14, D21, D28/M1, M2, M3, M4, M6).
- 3 header rows: Phase (epoch, level 1), Days (study_day, level 2), Visit window (window, null).
- 24 activities, 3 of them group headers: 145 `activity_schedule` entries, 96 of them X cells; 17 annotations (16 footnote, 1 source_note).
- Activity rows per page: **p.98: 12** (rows 4-15); **p.99: 12** (rows 20-31); **p.100: 0**; **p.101: 0**. Pages 100 and 101 carry only the reprinted header plus the abbreviations, notes and footnotes a-k (p.100) and l-n (p.101). They have no body rows; this is not a skipped page.

### 4.2 Merged-mark decisions
- Row 20 Virus tests: col 2 is X; cols 3-15 are one ruled cell holding three paragraphs, distributed in full to 3:15 (D1). The cell's last sentence prints "Not required for a patients …", transcribed as printed.
- The other spans are listed under Recorded item 12.
- The arrows in rows 21 and 22 are recorded as "<--- X --->". The printed dash run is shortened, and the X and arrowheads are kept.

### 4.3 Synthesised
- No synthesised property names in A1.
- Synthesised annotation markers g1, g2 and g3 (table-wide, below the frame on p.100).

### 4.4 Annotations — orphan risk and binding
- Footnote k is defined (p.100) but its marker is printed nowhere in the table. A zoomed render of the Lymphoma tissue collection label shows the text filling the cell to its right rule with no superscript. It is bound to row 28 by `text_match` and "k" is added to that row's `annotation_markers` (D2).
- g2 (Day 0 definition) is kept table-wide (D3).
- All other markers are printed and bound to where they are printed.

### 4.5 Low-confidence calls
- None beyond D1-D3.

## 5. Table 2 — Table A 2 … Biomarker Sampling (main_soa)

### 5.1 Shape and page coverage
- 11 columns: 1 label column plus 10 data columns (D -5, D0, D1, D3, D7, D10, D14, D21, D28/M1, "M2, M3, M4 and M6").
- 4 header rows: Epoch (synthesised, level 1), Period (synthesised, level 2), Study Day (synthesised, level 3), Visit window (null).
- 10 activities: 50 entries, 44 of them X cells; 6 annotations.
- Activity rows per page: **p.102: 10** (rows 5-14); **p.103: 0**. P.103 carries only the reprinted header and the abbreviations, notes and footnotes a-d.

### 5.2 Notes
- Row 9 col 3 prints "X (before TAK-007 administration" with no closing parenthesis; transcribed as printed.
- Typing reasoning (main_soa vs domain) is recorded in `table_metadata.notes` and Recorded item 2.

## 6. Table 3 — Table A 3 … from Month 6 onwards (main_soa)

### 6.1 Shape and page coverage
- 3 columns: 1 label column plus 2 data columns (Secondary Follow-up, Long Term Follow-up).
- 3 header rows: Epoch (synthesised, level 1), Visit (synthesised, level 2, D4), Window (synthesised, null).
- 8 activities: 14 entries, 11 of them X cells; 8 annotations.
- Activity rows per page: **p.104: 8** (rows 4-11); **p.105: 0**. P.105 carries only the reprinted header and footnotes e-f.
- Row 2 typing: §3 gives no rule for month labels or recurrence wording, so `visit` was chosen as the closest fit (D4).

## 7. Method provenance (non-default methods)

- `structure_method: inferred_from_layout`: A2 rows 1, 2 and 3; A3 rows 1, 2 and 3 (synthesised header bands).
- `indentation_method: font_signal`: all A1 activities (levels from the bold full-width group bands). `assumed_flat`: all A2 and A3 activities.
- `marker_locations[].method: synthesized`: A1 g1-g3, A2 g1-g2 and A3 g1-g2, each on the top header row (table-wide convention).
- `marker_locations[].method: text_match`: A1 footnote k on row 28 (D2).
- No `unresolved` locations; no `proximity` or `proximity_bounded` methods; no raster or visual-read cell methods. Every mark comes from the text layer.
