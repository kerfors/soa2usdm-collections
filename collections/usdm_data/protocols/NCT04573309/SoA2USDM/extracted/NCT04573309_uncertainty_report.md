# NCT04573309 — SoA extraction uncertainty report

Source: NCT04573309_soa.pdf (4 PDF pages = document pages 14–17 per PAGEMAP.md; Protocol Amendment 3.1 (US), ALXN1840-WD-204, 18 Mar 2022). No protocol markdown was available, so all text comes from the PDF text layer.
Prompt version 3.8.1. Two tables extracted.

## Decisions needed (4)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | Table 1, p.14, header rows 1–2 | The top header band (Screening / C-I / UNS / EOS or ET) is typed as study phase (epoch, level 1) and the second band (Inpatient Period 1 / OP / Inpatient Period 2) as sub-periods (period, level 2). Both names are synthesised because the only label cell reads "Study Procedures". | Type the top band as visit categories (visit), since Screening, Check-in, Unscheduled and EOS/ET read like visit names, and keep the second band as epoch. | 2.2 |
| D2 | Table 1, p.14, rows 23–24 (Discontinue chelation therapy / Discontinue zinc therapy) | The arrow after each X is spread as "→" over every following column through the EOS column (chelation: X at Day -4 through -1, → over columns 8:25; zinc: X at Day -21, → over 4:25), including the UNS column, because the drawn arrowhead ends inside the EOS column. | Stop the span at Day 40 (column 23) and treat the arrowhead running into UNS/EOS as drawing slack. Or keep only the X and read the arrow as "stays discontinued for the rest of the study", with no per-column entries. | 2.3 |
| D3 | Table 1, p.15, row 31 (PD: Plasma total and PUF-Cu, LBC, ceruloplasmin, ceruloplasmin-bound Cu) | The row is transcribed with no marks, exactly as printed. | Treat it as the second half of the PK blood-sampling row (row 30), split by the page break (Table 2 prints PK and PD in one cell that shares its marks), and copy row 30's marks onto it: Days 1, 4-7, 25, 26-28, 29, 30-35, 36, 37-38, 39 and 40, with footnote p at Days 1, 25, 29 and 39. | 2.4 |
| D4 | Table 2, p.17, rows 2–3 | The single body cell, which lists the PK sampling label and the PD label on separate lines under one set of marks, is split into two activities that match the two Table 1 rows, and every mark is emitted on both. | Keep one combined activity ("Blood sampling for PK … / PD: …") that carries the marks once. | 3.2 |

## Recorded, not open (9)

- §2 classification: Table 1 = main_soa. Table 2 = subsidiary: it gives hour-level timing for the Table 1 PK/PD sampling activities on Days 1, 25, 29 and 39, and Table 1 footnotes o and p point to it.
- §2 / type definitions (a page break inside one printed table is not a continuation): Table 1 runs from p.14 to p.15 under a reprinted header. It is extracted as one table and the header is de-duplicated.
- §5 label columns: L = 1 in both tables. The first data column is position 2. Table 1 uses data positions 2–25 and Table 2 uses 2–12.
- §5 merged header cells: row 1 "Screening" 2:3, blank 5:23; row 2 "Screening" 2:3, "Inpatient Period 1" 5:12, "Inpatient Period 2" 15:23. All spans come from the raster rule lines.
- §6 header-cell footnotes: a (Screening, on both covered cells 2 and 3), b (C-I, col 4), d (UNS, col 24) and e (EOS or ET, col 25) sit on row-1 grid cells. c (OP, col 13) sits on a row-2 grid cell.
- §6 abbreviations: the abbreviation blocks under Tables 1 and 2 have no term printed as a marker, so no abbreviation annotations were emitted.
- §6 the "Note:" paragraph under Table 2 has no printed marker. It is emitted as footnote n1 (a synthesised marker), bound table-wide to the time-point property row with method "synthesized".
- §3 literal header: in row 2, the Day 23 cell (col 14) is blank and not part of "Inpatient Period 2", although footnote v puts Period 2 at Day 23 to Day 40. It is transcribed as printed.
- §5 vertical merge: the Table 2 marks sit in one cell that spans both activity lines, so each mark is emitted on both activities. See D4 for the split itself.

## 1. Method

- Both pages have a normal text layer and vector rule lines. The text is not glyph-spread, so no §1c reconstruction was needed.
- Column boundaries came from vertical rules detected in a 200 dpi raster (identical on pp.14 and 15). Horizontal spans and the Table 2 single body cell came from the raster rules too.
- **Mechanical mark-check (§1b):** pdftotext -bbox X-tokens were binned to the rule-line columns and compared with the delivered matrix. Table 1 has 212 X marks (including the 2 X marks where the arrows start), and the bbox and delivered matrices agree exactly (0 differences). Table 2 has 8 X positions, bbox-binned with no difference.
- Footnoted marks tokenise as a separate small glyph ("X" + "g"). The markers were attached from those glyphs and from the visual read.
- The arrows are vector lines, so their extents were measured from the raster (method `raster_pixel_detection` on the "→" cells). Chelation line: x 349–711 pt. Zinc line: x 235–711 pt. The EOS column spans 692.8–742.3 pt.

## 2. Table 1 — Schedule of Activities (main_soa), pp.14–16

### 2.1 Counts and page coverage
- 25 columns (1 label + 24 data columns: 22 day columns, UNS, EOS/ET). 3 schedule_property rows.
- 44 activity rows (rows 4–47), including 8 mark-free section headers: Eligibility, Study Administration, Enrollment, Administration of Study Intervention, PK/PD Analyses, Safety Assessments / Laboratory Analyses, Balance assessments, Other.
- Activity rows per page: p.14 = 27 (rows 4–30); p.15 = 17 (rows 31–47); **p.16 = 0**. Page 16 carries only footnotes i–x and the abbreviation list, which is why page_end = 16.
- 24 footnote annotations (a–x). All are footnotes, since none is a bare "See …" pointer.

### 2.2 Header typing (D1)
The label column for header rows 1–2 is one merged cell reading "Study Procedures", so "Study Phase" (row 1) and "Period" (row 2) are synthesised names (`structure_method: inferred_from_layout`). Row 3 is labelled "Days" (study_day). The UNS column has an empty Days cell and is told apart only by row 1.

### 2.3 Arrows (D2)
Two rows carry arrows:
- Row 23: X at col 7, then "→" over 8:25.
- Row 24: X at col 3, then "→" over 4:25.

In the zinc row the arrow line is drawn over the X marks of other rows' columns with slight vertical drift, but it runs continuously from col 3 to col 25.

### 2.4 PK/PD rows across the page break (D3)
The last row on p.14 is the PK sampling row, which carries marks. The first row on p.15 is the PD row, which has none. Table 2 prints the same two labels inside one cell that shares its marks, so the Table 1 PD row may originally have been part of the same row. Only what is printed was transcribed.

### 2.5 Activity labels and markers
- Markers were stripped from the labels: "Discharge from unit" (f); "Follicle-stimulating hormone (post-menopausal females only)" (h, printed inside the parenthesis); "Medical history/demographics" (i); "WD history" and "Prior WD treatment" (j); "Physical examination" (k); "Height, weight, and BMI" (l, printed after "Height"); "Chemistry, hematology, Coagulation" (q); "Retained serum sample (safety)" (t); "Vitals sign measurements" (u, spelling as printed); "Cu/Mo-controlled meals" (v); "24-hour urine for Cu and Mo" (w); "Feces for Cu and Mo" (x). Footnote m sits on the section header "Administration of Study Intervention" and footnote o on "PK/PD Analyses".
- **Marker s:** bbox confirms that the final "s" in "Urine/serum pregnancy tests" and "menstruation checks" is a separate superscript glyph. It is therefore footnote s, and the names are "Urine/serum pregnancy test" and "… menstruation check". Footnote s covers both topics, which supports this reading.
- **Cell markers:** g on row 8/col 13, row 33/col 13 and row 34/col 13. n on row 28/col 13. p on row 30 at cols 8, 16, 18 and 22. r on row 33 at cols 7 and 17.
- Indentation comes from the font signal: section headers are bold on grey shading (level 0) and the rest are level 1.
- Section references appear only inside footnote text (Section 8 in note a, Section 10.2 in note s), not in activity labels, so no source_note annotations were emitted.

## 3. Table 2 — PK/PD Assessments on Days 1, 25, 29, 39 (subsidiary), p.17

### 3.1 Counts
- 12 columns (1 label + 11 time points: −0.5 to 24 h). 1 schedule_property row ("Time point (hours)", timepoint, marker a).
- 2 activity rows (p.17 = 2), with 8 marks each at −0.5, 2, 4, 5, 6, 8, 12 and 24 h. The 24 h mark carries marker b.
- 3 annotations: a, b and n1 (synthesised).
- The 0, 1 and 3 h cells are empty and were left empty.

### 3.2 Combined cell (D4)
There is no horizontal rule between the PK and PD labels in any data column (raster rules found only at y=130 and y=181 pt), so this is one cell. The labels match the separate Table 1 rows 30 and 31.

## 4. Synthesised values
- Property names: T1 row 1 "Study Phase" and T1 row 2 "Period".
- Annotation marker: T2 "n1" (the Note paragraph).

## 5. Annotation text integrity
- The text layer is clean, not glyph-spread. Footnote text is joined from the line-wrapped text layer.
- No annotation's text is contained in another's. Footnotes o and p both reference Table 2, but their wording is distinct and both are source-faithful.
- Every footnote is bounded by its printed marker, so no proximity bounding was used.

## 6. Orphan risk
- None. Every annotation has at least one marker_location, and every location's marker also appears on its element's annotation_markers (checked programmatically).
- No marker is referenced without a definition.

## 7. Method provenance (non-default)
- T1 rows 23 and 24: the "→" cells use `method: raster_pixel_detection` (arrow extents measured from the raster).
- T1 and T2 activities: `indentation_method` is `font_signal` (T1) and `assumed_flat` (T2).
- T1 schedule_property rows 1–2: `structure_method: inferred_from_layout`.
- T2 annotation n1: its marker_location uses `method: synthesized`.
- There are no `unresolved` locations.
