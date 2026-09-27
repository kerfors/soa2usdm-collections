# NCT05259917 — SoA extraction uncertainty report

Source: NCT05259917_soa.pdf (2 PDF pages = document pages 17-18 per PAGEMAP.md). KVD900-301 KONFIDENT protocol, Version 4.1 (US Only), 04 May 2023. No protocol markdown was available; all text comes from the PDF text layer.
Output: NCT05259917_Table_01_extraction.json (one table).

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.17, rows 3-4 ("In-clinic", "TeleVisit") | These two rows are recorded as visit-type header rows (how each visit is held), not as activities; footnotes d/e attach to them. | Record them as two ordinary activity rows with their X marks, exactly as printed in the body. | 2.2 |
| D2 | p.17, rows 3-4, Randomization column (marker a) | The single X printed centred across the In-clinic and TeleVisit rows is recorded on both rows (Randomization visit may be either, per footnote a). | Put the mark on one row only, or record it as a single either/or condition. | 2.3 |
| D3 | p.17, header row 1 ("Visit") | Header row 1 is typed "visit", following its printed label; "Treatment Period" is a spanning group over the three attack visits. | Type header row 1 as "epoch" (Screening / Randomization / Treatment Period / Final). | 2.1 |

## Recorded, not open (6)

- §5 arrows: "Conventional on-demand treatment washout" arrow distributed across columns 4:6 as "↔" (source_range "4:6", method visual_read).
- §5 arrows: "Concomitant Medication Review" (p.17) and "Adverse Event Review" (p.18) arrows distributed across columns 2:7.
- §5 merged header: "Treatment Period" merged over columns 4:6 (merged_cell_range "4:6").
- §6 header-cell footnotes: a on Randomization (row 1, col 3), c on Final Visit/ET (row 1, col 7), b on each attack column (row 2, cols 4-6) placed on the grid cells, not the property rows.
- §6 abbreviations: the page-18 abbreviation block yields zero annotations (no term is printed as a marker).
- Type definitions: the page break before "Adverse Event Review" is one printed table (Table 1) with a reprinted header, extracted as one table; the reprinted header on p.18 is de-duplicated.

## 1. Table summary

- Table 1 "Schedule of Events", `main_soa`, pages 17-18.
- Label columns L = 1 (activity/"Visit" column); first data column position = 2. Data columns: 2 Screening, 3 Randomization, 4 1st eligible HAE attack, 5 2nd, 6 3rd, 7 Final Visit/ET (6 data columns).
- Schedule properties: 4 (rows 1-2 header, rows 3-4 modality). Activities: 22 (rows 5-26). activity_schedule entries: 54. Annotations: 19 (a-s, all footnote).
- Activity rows per page: p.17 = 21 (rows 5-25), p.18 = 1 (row 26 Adverse Event Review). Both pages contribute rows; p.18 otherwise holds the abbreviations and footnotes.
- Indentation: flat table, all activities level 0 (indentation_method assumed_flat); no grouping headers.

## 2. Judgement calls (detail)

2.1 Header row 1 property_type (D3). The label cell reads "Visit" and spans both header rows; values mix named visits (Screening, Randomization, Final Visit/ET — each vertically merged over rows 1-2) and a spanning period "Treatment Period" (cols 4-6). Row 2 ("1st/2nd/3rd eligible HAE attack") has an empty label cell, so its property_name "Eligible HAE attack" is synthesised (property_name_source.synthesized = true). Row 2 has no grid values in cols 2, 3, 7 (vertical merge from row 1 is not expressible in the schema).

2.2 In-clinic / TeleVisit (D1). These rows mark visit modality, the pattern the prompt describes as Telephone-visit bands (schedule_property). Recorded as property_type `modality`, hierarchical_level null, X values in schedule_grid: In-clinic cols 2, 3, 7; TeleVisit cols 3, 4, 5, 6.

2.3 Vertically merged Randomization mark (D2). The bbox y of this X (186) sits between the In-clinic (178) and TeleVisit (194) rows, and the page render shows no horizontal rule between the two rows in column 3. Per §5 (vertically merged marks) it is emitted on both rows.

## 3. Merged-mark decisions

- Row 18 Conventional on-demand treatment washout: arrow, cols 4:6.
- Row 25 Concomitant Medication Review: arrow, cols 2:7.
- Row 26 Adverse Event Review: arrow, cols 2:7.
- Rows 3-4, col 3: vertical merge (see 2.3).
Arrow extents confirmed visually on 130-dpi renders (arrowheads sit within the first and last covered columns); no other merged marks.

## 4. Synthesised

- property_name "Eligible HAE attack" (row 2).
- No synthesised annotation markers; all 19 footnote markers are printed.

## 5. Mechanical mark-check

pdftotext -bbox; column centres fixed from the header labels (x ≈ 339, 412, 493, 565, 634, 695 pt); X tokens binned to the nearest centre. The bbox matrix agrees cell-for-cell with the visual read (54 marks including arrows; arrows are vector graphics and were checked visually only). No disagreements.

## 6. Annotation text integrity

- The text layer is not glyph-spread; no reconstruction needed.
- Footnotes a-s transcribed from p.18, each bounded by its marker letter and the next one. No containment/overlap pairs.
- Footnote r cites "Table S1" (timed patient assessments through 48 h). Table S1 is not in this excerpt, so a subsidiary table may exist elsewhere in the protocol but could not be extracted. Footnote r is kept as a `footnote` (it explains and points; it is not a bare pointer).

## 7. Low-confidence calls

- D1 and D3 above. No PDF/markdown comparison was possible (no markdown).

## 8. Orphan risk

- None: every annotation has ≥1 marker_location, and every location's marker is also in that element's annotation_markers. Marker b sits on three header cells (row 2, cols 4-6); marker r sits on four activities (PGI-S, PGI-C, VAS, GA-NRS).

## 9. Method provenance

- activity_schedule `method: visual_read` on the 12 arrow cells (rows 18, 25, 26): arrows are invisible to the text layer.
- activity_name_source.indentation_method `assumed_flat` on all 22 activities.
- property_name_source.synthesized on row 2.
- No `unresolved` marker locations; no proximity-bounded or text_match bindings.
