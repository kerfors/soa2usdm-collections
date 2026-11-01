# NCT04323098 — Uncertainty report (prompt 3.9.4)

BioMarin 270-303 Amendment 2 (25 May 2021), SoA excerpt doc pp. 311-325. Six SoA tables extracted (Tables 9.1.1-9.1.6 → Table_01 … Table_06).

## Decisions needed (2)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | Table 9.1.4, doc p. 321, header row 1 (and ETV header, col 9) | The top header band ("Years 2-5*" over the three routine-visit columns, "End of Year Visit" over the four annual visits, plus the ETV cell) is typed as a **visit** band, and the ETV header (which spans all three header rows) is recorded on it. | Type the band as a **period** band (Years 2-5 is a period name) or **other**; ETV would then be recorded on the year row (row 2), where its text is printed. | 4.1 |
| D2 | Table 9.1.6, doc p. 325, whole table | Typed **main_soa**: the prophylactic corticosteroid course is given to every participant (prophylactic corticosteroids are part of this study's design) on its own week grid starting at infusion Day 1, so it is treated as a schedule every participant passes through. | Type it **subsidiary**: footnote a calls it an *example* course, and its test rows (FVIII, liver tests, hepatitis B / HCV) are "also reflected in the Schedule of Events", so it is supporting detail linked to Tables 9.1.2-9.1.4. | 6.1 |

## Recorded, not open (19)

1. §2 **track vs main_soa** — Tables 9.1.2, 9.1.3, 9.1.4 typed `main_soa`: sequential post-infusion follow-up that every participant passes through, each printed as its own numbered table.
2. §2 **track vs main_soa** — Table 9.1.5 typed `track`: the optional liver biopsy is a branch only some participants take.
3. §2 **track_label** — Table 9.1.5 `track_label` = "Optional Liver Biopsy" (from the table title).
4. Taxonomy **continuation** ("A page break inside ONE printed table is not a continuation") — Tables 9.1.1-9.1.4 each run over two pages with the header reprinted and no own number: each is one table spanning both pages.
5. §4 **row_position** — header rows reprinted on the second page of Tables 9.1.1-9.1.4 keep their row numbers (T1 rows 22-24, T2 rows 20-22, T3 rows 22-24, T4 rows 24-26) and are not emitted.
6. §3 **Header cells over several rows** — T1: Smart Rescreening and Baseline (rows 2-3) and BMN 270 Infusion Visit (rows 1-3) recorded on the visit row 3; T4: ETV (rows 1-3) recorded on row 1 (the visit-typed row, see D1); T6: D1 (rows 2-3) recorded on row 3, where it is printed.
7. §3 **Header cells over several rows** (qualifier stays in the cell) — T1 visit values keep their day windows, e.g. "Baseline (Day -7 to Day -1)".
8. §3 **property_name synthesis** — names synthesised: T1 Epoch/Period/Visit; T2 and T3 Epoch/Week; T4 Visit/Period; T5 Visit; T6 Period/Week 1/Week 2. Where the label cell prints "Assessment" (the activity-column heading), that text is kept in `property_name_source.cell_value`.
9. §3 **hierarchical_level** — the Study Day rows of T2 and T3 restate the week row (Day = 7 × Week + 1): level null. The T6 "Week" unit band does not tell columns apart: level null.
10. §3 **Header bands — typing** — T4 header row 1 is printed above the period row (visit-type labels above periods); print order kept for levels and the mix stated in `property_comment` (the type itself is D1).
11. §5 **Leading label columns** — L = 1 in every table (the Assessment column); the first data column is position 2.
12. §5 **Merged marks — distribute, never centre** — T4: in every body row the four End of Year Visit columns are one merged cell (no internal vertical rules); its value is distributed to cols 5-8 with `source_range` "5:8" (24 rows, 96 entries).
13. §5 **Spanned values** — T4 "Optional liver biopsy" and "Liver ultrasound": the drawn cell covers cols 5-8 only; ETV (col 9) has its own empty ruled cell, so no ETV entry.
14. §5 **Qualified marks** / **Glyph case** — T4 "(X)" kept literally as the cell value (marker e cleaned out).
15. §5 **Empty cells** — `schedule_grid` complete for every header row × data column with "" for blank or covered cells; `activity_schedule` holds non-empty cells only.
16. §6 **annotation_type** — T5 footnote b ("Refer to Table 9.7.8.2.1 … and to Table 9.7.8.4.1 for liver tests.") is a bare pointer: `source_note`. T1 g, T2/T3 b, T4 c and T6 b point *and* explain: `footnote`.
17. §6 **Header-cell footnotes** — markers on header cells are bound to those `schedule_grid` cells: T1 *, m, n, o; T2/T3 f; T4 g (Q4W, Q6W) and * (merged "Years 2-5*" cell, cols 2-4); T6 b (merged cell cols 2-21) and c (cols 22-26). On merged header cells the marker is recorded on every covered column. Markers on the Study Day / Study Week label are bound to the `schedule_property` row.
18. §6 **Text above or below the frame** / **Binding by what a note names** — T6 footnote a has a printed marker that appears nowhere in the table. It names no single element (it covers the whole example course), so it is table-wide: one `schedule_property` location on row 1 with `method: "synthesized"`, and its marker is on no element's `annotation_markers`. The printed letter "a" is kept as the marker (no `g` synthesised, because the source prints a marker).
19. §6 **Deduplicate by text** — each table has its own footnote list. Notes printed again under a later table with the same letter (e.g. "Visit windows are ± 48 hours." for T2 and T3) are emitted once per table, verbatim.

---

## Method

- **Text and vector layers:** all 15 pages have a real text layer (pdftotext -bbox, no glyph spreading, no private-use code points). Every table page has vector rule lines (`page.get_drawings()`: 173-1225 items per table page), so no raster rule recovery was needed (§1a/§1d not applicable).
- **Cell geometry:** for each page, the vertical and horizontal rule lines were clustered to give column and row bounds. For each row band, an internal column boundary counts as present only where a vertical segment covers the band's mid-height. That gives every merged cell, both horizontal (T4 cols 5-8, merged header cells) and vertical (header cells covering several header rows, seen as a missing top rule). Tokens were assigned to cells by their bbox centre. Superscript markers were detected by token height (< 10 pt against ~12.2 pt body text).
- **Text source:** the pymupdf4llm markdown is badly degraded for these pages: words run together ("FastingFibroTest", "ImmunogenicityAAV5", "StudyDay", "exploratorybiomarkers"), cells are misaligned and stray glyphs appear. Activity names, header values and footnote text were therefore taken from the PDF text layer, which matches the rendered page. Its wording was checked against the markdown wherever the markdown is readable. Footnote paragraphs were bounded from line-initial superscript markers. "2 nd" was restored to "2nd" (superscript), and "Follow-" + "Up" at a line break was joined as "Follow-Up" (T4 c).
- **Page 311:** this excerpt page carries only the T1 title and its frame; no narrative text from the preceding section appears on it.

## Mechanical mark-check (§1b)

The check was independent of the cell assignment. Column centres were fixed from the header column bounds. Each mark token matching `^\(?[Xx]\)?[*a-zA-Z0-9]?$` was binned to the nearest centre within its activity's row band. The resulting matrix was then diffed against the JSON `activity_schedule`:

| Table | JSON mark cells | bbox-binned glyph cells | JSON-only | bbox-only | explanation |
|---|---|---|---|---|---|
| T1 | 67 | 67 | 0 | 0 | exact match |
| T2 | 157 | 157 | 0 | 0 | exact match |
| T3 | 149 | 149 | 0 | 0 | exact match |
| T4 | 148 | 76 | 72 | 0 | all 72 are the extra columns of the 5:8 merged End of Year cells (24 rows × 3); each span holds its binned glyph |
| T5 | 16 | 16 | 0 | 0 | exact match |
| T6 | 54 X (+20 dose values) | 54 | 0 | 0 | exact match; dose cells ("40 mg" …) are text values, not marks |

Visual read: every page was rendered and read row by row. No cell disagreed with the mechanical matrix. Rows checked closely include T1 AAV5 TAb Assay (CDx) (cols 2, 3, 4, 6), T2 Plasma/PBMC (weeks 1, 4, 8, 12, 18), T2 Complement Panel (weeks 1, 2, 4, 8, 12, 16, 20; the week 1 X is printed raised in its cell), T3 Physical examination and Weight, and T6 FVIII/liver rows (D1 to week 23; week 32 empty).

**PDF/markdown disagreements (PDF used):** the markdown places marks in the wrong columns in several rows, among them T1 AAV5 TAb Assay (CDx), BMN 270 Infusion and Hypersensitivity; T2 Serum for exploratory biomarkers, EQ-5D-5L and PROBE (p. 316); T4 WPAI+CIQ:HS and PROBE (p. 322: marker a split into its own cell). It also prints the T6 footnote a as a fragmented table row. In footnote o of T1 the markdown loses the marker letter; the text layer prints it.

---

## 1. Table 9.1.1 — Schedule of Events-Screening and Infusion (Table_01)

- `main_soa`; doc pp. 311-314; 5 data columns (positions 2-6); 3 header rows (Epoch / Period / Visit).
- Activities: 37 (p. 311: 18, p. 312: 19, p. 313: 0, p. 314: 0). Pages 313-314 carry footnotes c-r only (no activity rows).
- Marks: 67. Annotations: 19 (*, a-r), all `footnote`.
- No merged body cells.
- Low-confidence (not raised as decisions): the top band "Prior to BMN 270 Infusion" is typed `epoch` and the "Screening" band `period`. Both are phase bands; a reviewer could also type both as epoch.

## 2. Table 9.1.2 — Schedule of Events-Post-Infusion Follow-Up (Week 1-20) (Table_02)

- `main_soa`; doc pp. 315-317; 20 data columns (positions 2-21, Weeks 1-20); header rows Epoch / Week / Study Day.
- Activities: 23 (p. 315: 16, p. 316: 7, p. 317: 0 — footnotes d-g only).
- Marks: 157. Annotations: 8 (*, a-g), all `footnote`. Footnote f is on 13 week header cells; g is on the hepatitis reactivation cells at Weeks 6 and 20.

## 3. Table 9.1.3 — Schedule of Events – Post-Infusion Follow-Up (Weeks 21-52) (Table_03)

- `main_soa`; doc pp. 318-320; 16 data columns (positions 2-17); header rows Epoch / Week / Study Day.
- Activities: 25 (p. 318: 18, p. 319: 7, p. 320: 0 — footnotes e-i only).
- Marks: 149. Annotations: 10 (*, a-i), all `footnote`.
- Footnote a reads "…should be performed Additional physical exams…" (the period is missing in the source); transcribed as printed.

## 4. Table 9.1.4 — Schedule of Events-Post-Infusion Follow-Up (Year 2-Year 5) (Table_04)

- `main_soa`; doc pp. 321-323; 8 data columns (positions 2-9: Q12W, Q4W, Q6W, W104, W156, W208, W260, ETV); header rows Visit / Period / Study Week.
- Activities: 24 (p. 321: 20, p. 322: 4, p. 323: 0 — footnotes f-j only).
- Marks: 148 entries (76 printed cells; 72 more from distributing the 5:8 merged End of Year cells). Annotations: 11 (*, a-j), all `footnote`.
- **Merged-mark decisions:** every body row (rows 4-23, 27-30) has its End of Year value in one cell spanning cols 5-8, distributed with `source_range` "5:8". Values: "X", "X" + marker a/c/j, "(X)" + marker e.
- The ETV column is a conditional visit inside this sequential schedule. It stays part of this `main_soa` table; it is not split into a track.

### 4.1 Header row 1 typing (D1)
The row holds "Years 2-5*" (period name, cols 2-4), "End of Year Visit" (visit type, cols 5-8) and the ETV cell (col 9, spanning rows 1-3). Both of the first two values span several columns, so §3's dominant-content test does not settle the type by itself. I typed the band `visit` because two of its three values are visits, and the wider span is a visit type. Under that typing, ETV, a visit, is recorded on this row (§3 **Header cells over several rows**: the row whose type matches its content). If the band were `period`/`other`, no row would match ETV's content, and it would sit on row 2, where its text is printed.

## 5. Table 9.1.5 — Schedule of Events-Optional Liver Biopsy (Table_05)

- `track` ("Optional Liver Biopsy"); doc p. 324; 3 data columns (positions 2-4); one header row (Visit).
- Activities: 11 (p. 324: 11). Marks: 16. Annotations: 6 (*, a, c, d, e `footnote`; b `source_note`).
- The header row is typed `visit`. Footnote * itself calls the columns the "Day -7 and biopsy day visits".

## 6. Table 9.1.6 — Suggested Schedule of Events-Prophylactic Corticosteroids (Table_06)

- `main_soa` (D2); doc p. 325; 25 data columns (positions 2-26: D1, Weeks 1-23, Week 32); header rows Period / Week 1 (unit band, level null) / Week 2.
- Activities: 5 (p. 325: 5). Schedule entries: 74 (54 X marks + 20 dose values "40 mg" … "5 mg" on the dose row; value and unit are printed on two lines and joined with one space). Annotations: 4 (a-d), all `footnote`.
- The top band is typed `period`, not `epoch`. These are periods of the corticosteroid regimen and run alongside the post-infusion follow-up epoch.

### 6.1 Table type (D2)
Rows are activities; the columns differ from every other table; the table is not the anchor. Is the timing finer? Not really: it is weekly, the same grain as Table 9.1.2. Does every participant pass through it? The study design gives prophylactic corticosteroids to all participants, so I chose `main_soa`. The arguments for `subsidiary`: the title says "Suggested", footnote a calls it "an example of a prophylactic corticosteroid course", and the monitoring rows repeat tests that Tables 9.1.2-9.1.4 already schedule. Nothing in this excerpt states outright that every participant receives the course.

---

## Synthesised
- Property names: see Recorded item 8.
- Annotation markers: none synthesised. T6 footnote a keeps its printed letter and has one synthesised table-wide location (Recorded item 18).

## Annotation text integrity
- Not glyph-spread; no reconstruction needed.
- Overlap pairs, each re-checked on the page, all **source-faithful** (separate footnotes, each printed with its own marker line):
  - T1 f / m: the same two SARS-CoV-2 sentences are printed in both footnotes (doc p. 313).
  - T1 j / k and T2/T3 b / e: shared closing sentence on exploratory samples.
  - T2/T3/T4 b-c / c-d: a shared phrase on adjusting assessments after discussion.
  - T4 b / c: a shared phrase on the second Q12W visit.
  - T6 a / d: shared sentence ending "…also reflected in the Schedule of Events for the study."
- No containment pairs (no note fully contained in another). Every note was bounded by its printed marker line; no note was bounded by proximity.

## Orphan risk
- T6 footnote a: marker printed in the footnote list but used nowhere in the table (source defect). It is bound table-wide with `method: "synthesized"` and is not an orphan.
- No marker is used in a table without a printed definition.

## Method provenance
- T6 annotation a: `marker_locations[0].method = "synthesized"` (table-wide anchor on schedule_property row 1).
- No other non-default methods. Indentation: all labels are left-aligned at the same x (no hierarchy signals; every row is level 0 from the text layer). Cells, spans and text all come from the bbox text layer and vector rule lines.
- No `unresolved` locations.
- No redactions in the excerpt.
