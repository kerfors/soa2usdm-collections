# NCT02291289 — SoA extraction uncertainty report

Source: NCT02291289_soa.pdf (Protocol MO29112, Version 9, Appendices 1–5). 26 PDF pages map to document pages 169–194 (PAGEMAP.md). Page 194 (Appendix 6, FOLFOX regimens) is outside the SoA range and was not extracted. The printed footers match PAGEMAP on every page checked.
Prompt version 3.8.1. There are 5 tables and 5 JSON files.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p170 rows 17, 20 (Table 1); Tables 2/3/5 "(Experimental Arm only)" rows; Tables 2–5 Metastatic tumour tissue / Stool sample rows | Explanatory sub-lines printed inside an activity label cell were moved out of `activity_name` into footnote annotations with synthesised markers `n1`/`n2`, bound to that activity. Examples are "Collection of these samples discontinued as of May 2018", the Supplemental Biomarker Program closure sentence and "(Experimental Arm only)". The raw text stays in `cell_text`. | Keep the sub-line as part of the activity name, e.g. "Pulse oximetry (Experimental Arm only)". No synthesised note is then needed. | 3.3 |
| D2 | p169 row 3 (Table 1); row 2 of Tables 2–5 | The per-column timing header row is typed `other`. It mixes windows ("≤ 28 days"), an anchor ("Day 1 Cycle 1"), recurrence frequencies ("Every 2 cycles…", "Every 3 months…") and a post-dose window. | Type the row as `visit` (one column = one encounter) or as `window`. | 3.2 |
| D3 | p174 (Tables 2–5) | Appendices 2–5 (maintenance schedules for Cohorts 1–4) are each typed `track`, with track_label "Cohort 1" … "Cohort 4". Each schedules a mutually exclusive, biomarker-assigned cohort. Appendix 1 (induction for all patients) is `main_soa`. | Type Tables 2–5 as independent `main_soa` tables. The identical column labels of Cohorts 1 and 4 could also invite a `domain` reading, but the populations differ. | 3.1 |

## Recorded, not open (10)

- §1a: all grid pages are full-page raster images with no text layer or vector rules. Cells are therefore visual reads (`method: visual_read`), and labels have `activity_name_source.method: visual_transcription`.
- §1d: merged cells were confirmed from raster rule lines (200 dpi). The only body merge is Table 1, row 22, cols 4:5.
- §3: the condition band in Table 1 (row 1, cols 6:7, "Patients who have PD…") has `property_type: condition` and `hierarchical_level: null`.
- §3: every `property_name` is synthesised, because the label column of the header rows is empty.
- §4: all tables are flat. `indentation_method: assumed_flat` and every row is level 0.
- §5: there is one label column (L = 1), so the first data column is position 2.
- §5: qualified marks are kept literally: "x (as applicable)", "x (sites using 2 consent forms only)" and "x Administered every 2 weeks".
- §5: glyph case is kept as printed. Capital `X` appears only in Table 3 (rows 15, 16 col 4; row 18 col 2) and Table 5 (rows 19, 20 col 2). Everywhere else the mark is lower-case `x`.
- §6: header-cell footnotes [a]/[b]/[c] sit on the epoch-row `schedule_grid` cells they are printed on. In Table 1, [a] is on both covered positions 4 and 5.
- §6: bare-pointer notes are typed `source_note`. These are "See Appendix 8." (Table 2 m; Tables 3/4/5 i) and the synthesised `pr1` "See Appendix 19". `pr1` comes from the header text "(see Appendix 19)", which was stripped from the follow-up column header in Tables 1, 3 and 4.

## 1. Method

- **Text layer:** pages 169–171, 174–176, 179–181, 184–185, 189–191 are single 1682×1192 RGB images. `pdftotext` returns nothing on them. Pages 172–173, 177–178, 182–183, 186–188 and 192–193 are footnote continuation pages with a real text layer. Footnote wording on those pages was taken from the text layer. Two line-break hyphens were re-joined: "protocol-mandated" (T1 s) and "post-treatment" (T4 c). "3- week" is kept as printed.
- **Mechanical check (§1a/§1d):** each grid page was rendered at 200 dpi. Vertical and horizontal rules were recovered from ink fractions, and each internal column rule was tested for presence in every body band.
  - Results: the true column rules were found on every page; T1 has 8 rules (7 columns) and T2–T5 have 6 rules (5 columns). The only missing internal rule in a body band is T1 p170 "Study medication administration" at the col 4|5 boundary, which confirms the merge 4:5. No other body merges exist.
  - The matrix of which cells hold ink agrees with the visual read. The single-row pages 176, 181 and 191 were checked by eye only, because the automatic band detector needs at least two body rows.
- **Glyph-spread:** not applicable. There is no text layer on the grid, and the footnote text layers are normally spaced.

## 2. Per table

| Table | Title | Type | Pages | Data cols | Activities | Rows per page |
|---|---|---|---|---|---|---|
| 1 | Appendix 1 Schedule of Assessments for All Patients (Screening / Baseline and Induction Treatment Phase) | main_soa | 169–173 | 6 (pos 2–7) | 21 | 169: 12, 170: 7, 171: 2, 172: 0, 173: 0 |
| 2 | Appendix 2 … Maintenance Phase (Cohort 1) | track "Cohort 1" | 174–178 | 4 (pos 2–5) | 25 | 174: 14, 175: 10, 176: 1, 177: 0, 178: 0 |
| 3 | Appendix 3 … (Cohort 2) | track "Cohort 2" | 179–183 | 4 | 24 | 179: 14, 180: 8, 181: 2, 182: 0, 183: 0 |
| 4 | Appendix 4 … (Cohort 3) | track "Cohort 3" | 184–188 | 4 | 22 | 184: 13, 185: 9, 186–188: 0 |
| 5 | Appendix 5 … (Cohort 4) | track "Cohort 4" | 189–193 | 4 | 26 | 189: 16, 190: 8, 191: 2, 192: 0, 193: 0 |

- Every page that contributes no activity rows (172–173, 177–178, 182–183, 186–188, 192–193) holds only footnote text. They are included in the range because they carry the table's footnotes.
- Header rows reprinted on the continuation pages were de-duplicated.

## 3. Interpretation calls

### 3.1 Table types (D3)

- Table 1 covers every patient through screening and induction, so it is the anchor table.
- Tables 2–5 each cover one maintenance cohort. A patient is assigned to exactly one cohort by biomarker status (footnote d), so the four tables are separate timelines for mutually exclusive populations, which makes them tracks.
- The reasoning is also recorded in `table_metadata.notes`.

### 3.2 Header rows (D2)

- **Table 1:** row 1 is the condition band, row 2 is the epoch row (level 1) and row 3 is the timing row (level 2, typed `other`, `structure_method: assumed`).
- **Tables 2–5:** row 1 is the epoch row (level 1) and row 2 is the timing row (level 2, `other`).
- **Merged header cells in Table 1:**
  - "Screening / Baseline" spans 2:3.
  - "Induction Treatment Phase" spans 4:5.
  - The condition band spans 6:7.
  - The blank cells of the condition row span 2:3 and 4:5.
- In Table 4, the col 3 timing cell holds two arm-specific frequencies ("Control arm: every 2 two-week cycles Experimental arm: every 3-week cycle"). It is kept as one literal value.

### 3.3 Synthesised markers (D1)

| Table | Marker | Text | Bound to |
|---|---|---|---|
| T1 | n1 | "Collection of these samples discontinued as of May 2018" | row 17 |
| T1 | n2 | Supplemental Biomarker Program closure sentence | row 20 |
| T1 | pr1 | "See Appendix 19" | header row 3, col 7 |
| T2 | n1 | closure sentence | rows 21, 23 |
| T2 | n2 | "Experimental Arm only" | rows 9–12 |
| T3 | n1 | closure sentence | rows 20, 22 |
| T3 | n2 | "Experimental Arm only" | rows 15, 16 |
| T3 | pr1 | "See Appendix 19" | header row 2, col 5 |
| T4 | n1 | closure sentence | rows 18, 20 |
| T4 | pr1 | "See Appendix 19" | header row 2, col 5 |
| T5 | n1 | closure sentence | rows 22, 24 |
| T5 | n2 | "Experimental Arm only" | rows 15, 16 |

- All of these locations carry `method: synthesized`.
- `activity_name` "Cohort-specific informed consent" is normalised from the printed "Cohort- specific informed consent", which is kept in `cell_text`. This row has no footnote marker in any table.

## 4. Merged-mark decisions

- There is one distributed span: Table 1, row 22 "Study medication administration". "x / Administered every 2 weeks" spans cols 4:5 (`source_range` "4:5"). The span was confirmed by the missing rule line.

## 5. Annotation text integrity

- **Visual transcriptions (`annotation_text_source.method: visual_transcription`):**
  - T1 a–e (p171)
  - T2 a–j (p176)
  - T3 a–e (p181)
  - T5 a–f (p191)
  - T5 g, which starts on p191 as an image and continues on p192 in the text layer
  - all synthesised n1/n2/pr1 texts
- **Superscript:** in T1 c, the superscript "mut" in "BRAFmut/MSS" and "BRAFmut/MSI-H" is written inline as "BRAFmut".
- **Containment pairs, re-checked against the pages:** the n1 closure sentence ("Supplemental Biomarker Program closed as of May 2018. Collection of these samples has been discontinued.") is almost wholly contained in footnotes T2 u, T3 r, T4 r and T5 t, which add "(see Appendix 18)".
  - This is source-faithful, not a split note. One text is printed inside the label cell and the other in the footnote list.
- **Cross-table wording differences:** the cohorts' footnotes differ slightly in wording (e.g. d says "further enrolment" in T2 and "further accrual" in T3/T5). Each is transcribed as printed per table.

## 6. Low-confidence / orphan risk

- None of the footnote text could be taken from a text layer on the image pages. Visual transcriptions should get a spot-check against the rendered pages 171, 176, 181 and 191.
- No marker lacks its definition. Every in-grid or label marker in each table has printed footnote text in that table's own footnote list, and there are no orphan annotations.
- Glyph case (x vs X) was checked on zoomed renders.

## 7. Method provenance (non-default)

- **All tables:**
  - `schedule_grid` and `activity_schedule` cells use `method: visual_read`.
  - Activity labels use `method: visual_transcription` and `indentation_method: assumed_flat`.
  - The timing header rows use `structure_method: assumed`.
- **Footnote text:** visual transcriptions are listed in §5.
- **Synthesised marker locations:** listed in §3.3.
- **Unresolved locations:** none.
