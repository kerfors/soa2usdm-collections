# NCT03402841 — SoA extraction uncertainty report

Prompt 3.8.1, single pass. Source: NCT03402841_soa.pdf (5 PDF pages = document pages 40–44 per PAGEMAP.md). No protocol markdown was available, so all text comes from the PDF text layer.

## Decisions needed (2)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p41, Table 1, row 16 (Tumour assessment), marker n1 | The unmarked line "Note: MRI/ CT scan more than 28 days prior to Day 1 may be acceptable, please consult with AstraZeneca." printed directly under footnote e is kept as its own footnote (synthesised marker n1). It is attached to the Tumour assessment row because it sits next to footnote e (method proximity). | Treat the Note as the end of footnote e (add its text to e and drop n1), or leave it as a table-level note not attached to any row. | 2.3 |
| D2 | p40/p42, Tables 1 and 2 | Table 1 (screening) and Table 2 (on-study) are both typed main_soa: two independent schedules for the same participants, with different columns. | Type Table 1 as the only main_soa and Table 2 as a track for the treatment/follow-up phase, or the reverse. | 1 |

## Recorded, not open (6)

- §5: there is L = 1 label column in both tables, so data columns start at position 2. Table 1 uses positions 2–3 and Table 2 uses positions 2–8.
- §1b / types doc: the header rows reprinted on p41 (Table 1) and p43 (Table 2) are removed as duplicates. Each table is one printed table that runs over a page break, so neither is typed `continuation`.
- §6 header-cell footnotes: marker a in Table 2 is placed on grid cells (row 1 col 5 "…safety visits^a", row 2 col 4 "…thereafter^a"), not on the property rows.
- §6: footnotes b and d (both tables) end with "please refer to Section 5.2.1", but they also give instructions, so they stay `footnote` and are not `source_note`.
- §4: all rows have indentation_level 0 with `indentation_method: assumed_flat`, because both tables are flat and have no grouping headers.
- §6: no abbreviation or legend annotations were emitted, because the source prints no abbreviation list or legend for these tables.

## 1. Tables

| Table | Title | type | pages | data cols | activities | rows per page |
|---|---|---|---|---|---|---|
| 1 | Study Schedule – Screening (Visit 1) | main_soa | 40–41 | 2 | 18 | p40: 14, p41: 4 |
| 2 | Study Plan Detailing the Procedures | main_soa | 42–44 | 7 | 15 | p42: 11, p43: 4, p44: 0 |

- Classification (D2): the protocol text says "The schedule of assessments for the screening visit is shown in Table 1. On-study assessments are shown in Table 2." The two tables have completely different columns and cover different phases for the same participants. The taxonomy says separate Screening and Treatment tables with different column structures are each main_soa. This reasoning is also recorded in `table_metadata.notes`.
- Page 44 has no activity rows. It holds only the Table 2 footnotes g, h, j, k and l (the table body ends on p43). It stays in the declared range because those footnotes belong to the table.
- Only non-empty body cells are written to `activity_schedule`. Empty header cells (Table 2 Day cols 6–8, Visit Window col 8) are written as empty strings in `schedule_grid`.

### Schedule properties
- Table 1: row 1 "Day" is `study_day` (values "Before screening period", "-28 to -1"), level 1.
- Table 2: row 1 "Visit Number or type" is `visit` (L1), row 2 "Day" is `study_day` (L2), and row 3 "Visit Window" is `window` (L3). All three labels are printed, so none are synthesised.
- Low confidence: the Table 2 visit row mixes visit numbers (2, 3), recurring visit groups (V4+, V5+) and event-driven visits (treatment discontinued, 30-day FU, long-term FU). It is typed `visit` as a whole.

## 2. Table 1 details

### 2.1 Merged marks
- None. Every mark sits in a single rule-bounded cell.

### 2.2 Notable cells
- Row 15 "Confirmed as having non-Germline BRCA Mutated ovarian cancer": X under "Before screening period" (col 2). The glyph sits a little low in the cell, but it is clearly inside that row's rules.
- Row 19 "Archival or fresh tumour biopsy sample…": X in both col 2 and col 3.
- The marker on "Haematology / clinical chemistry" is "b,f". In the `-layout` text dump it prints between the Vital signs and Haematology lines, but the bbox y (641.6) matches the Haematology row, so it is attached to Haematology.
- The marker on "Tumour assessment" (p41) is "e" according to the text layer and the 300 dpi render. A low-resolution render looked like "c", which was an artefact.

### 2.3 Unmarked note (D1)
- The "Note: MRI/ CT scan…" line is printed flush left between footnotes e and f and has no marker of its own. I synthesised marker `n1` and gave it one `activity_name` location on row 16 with `method: proximity`. It is flagged for page verification.

## 3. Table 2 details
- Merged marks: none. The wide V4/V5 columns are single columns, not merged spans.
- In-cell markers: X^b in col 2 on rows 4 (Physical examination), 5 (Vital signs), 7 (Haematology) and 8 (Urinalysis); X^j in cols 3 and 4 on row 16 (Olaparib dispensed/returned).
- Activity text is kept exactly as printed, including the source typo "Blood sample for restrospective gBRCA test".
- The marker sequence skips "i" (a, b, c, d, e, f, g, h, j, k, l). No "i" is printed anywhere, so nothing is missing.
- Footnote e mentions "Table 2", which is a self-reference and not a cross-reference; it is kept as a footnote.

## 4. Synthesised items
- Property names: none.
- Annotation markers: `n1` (Table 1, see D1).

## 5. Mechanical mark-check
- Method: `pdftotext -bbox`. Column x-centres were fixed from the header labels (T1: 470, 525; T2: 220, 246, 327, 447, 547, 639, 728). X tokens matching `^[Xx][*a-zA-Z0-9]?$` were binned to the nearest centre and the y-band of their row. The resulting matrix was compared cell by cell with visual reads of 300 dpi renders.
- Result: no disagreements. Table 1 has 19 marks and Table 2 has 45.
- The pages have vector rule lines and a normal (not glyph-spread) text layer, so no raster or glyph reconstruction was needed.

## 6. Annotation text integrity
- The text layer is not glyph-spread, so no reconstruction was done (§1c).
- Each footnote was checked from its first word to its last against the page.
- Source quirks kept as printed: "collection,shipping" (no space) in T2 g, "Day1" in T1 g, and no final full stop on T1 f and T2 a.
- Containment/overlap pairs: none. Footnote b in Table 1 and footnote d in Table 2 share the sentence about coagulation and Section 5.2.1, but they are separate notes in separate tables and are faithful to the source.

## 7. Orphan risk
- None. Every annotation has at least one marker_location, and every location's marker is also present in that row's or cell's `annotation_markers` (checked by script). Both files validate against the schema.

## 8. Method provenance
- All activities: `indentation_method: assumed_flat` (flat tables).
- T1 n1: one marker location with `method: proximity` (D1).
- No `unresolved` locations, no `annotation_text_source` methods and no non-default cell methods.
