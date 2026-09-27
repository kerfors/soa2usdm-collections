# NCT04004988 — SoA extraction uncertainty report

Prompt 3.8.1, single pass. Source: NCT04004988_soa.pdf (PDF pages 1-3 = document pages 9-11 per PAGEMAP.md). No protocol markdown available.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.11, general note "If multiple procedures take place at the same time point..." (marker g2) | Kept as one table-wide note, anchored to header row 1 for traceability only | Bind the note to the seven activities it names (Safety 12-lead ECG, Supine Vital Signs, PK Sampling, Clinical Laboratory Tests, Immunogenicity, Blood glucose monitoring, Pharmacogenetic Sample) | 4 |
| D2 | p.10, header row 2 | Row named "Study Day" (synthesised); the printed "Procedure" in its label cell is read as the heading of the activity-name column | Use the printed "Procedure" as the property name | 3 |
| D3 | p.10, header row 1, column 15 ("ED^c") | ED recorded as a value of the Epoch row, with an empty study-day cell below | Treat ED as a visit (early-discontinuation visit) rather than an epoch value | 3 |

## Recorded, not open (7)

- §2 / taxonomy "one table spanning both pages": the page-11 reprint of the header is one table across a page break, not a `continuation`. Header rows were de-duplicated.
- §2: `main_soa`. Periods 1 and 2 share one printed day grid for the same participants (a crossover), so they were not split into tracks.
- §5: L = 1 label column (Procedure). The first data column is position 2 and data columns run 2-15. The Comments column (16) is a notes column and is not in the grid.
- §6 notes column: the 12 non-empty Comments cells became `footnote` annotations with synthesised markers n1-n12, each bound to its own row (`method: synthesized`).
- §6 abbreviations: the abbreviation block (ADA, AE, CRU, D/d, ECG, ED, PK, TE ADA) produced no annotations. No term is printed as a marker, and binding by word overlap is forbidden.
- §6 header-cell footnotes: marker b sits on the study-day cells D-1, D1 and D36 (±1) (cols 3, 4, 14). Marker c sits on the ED cell (row 1, col 15).
- §6: n6 "See Appendix 2 for details. Day 1 predose sample is for baseline only." both points and explains, so it is typed `footnote` rather than `source_note`. n10 contains Section 9.4.6 and 9.7 references inside explanatory text, so it stays a `footnote`.

## 1. Per table

**Table 1: "Study Schedule Protocol I8F-MC-GPGS"**
- Type `main_soa`, document pages 10-11.
- 14 data columns (2-15): D-28 to D-2, D-1, D1, D2, D3, D4, D5, D6, D7, D8, D15, D21, D36 (±1), ED.
- 2 header rows and 20 activity rows (rows 3-22). The table is flat: every row is a level-0 activity with marks (`indentation_method: assumed_flat`).
- Activity rows per page:
  - p.10: 17 rows (rows 3-19).
  - p.11: 3 rows (rows 20-22).
  - p.9: 0 rows. It is outside the declared range: it carries only the section heading "2. Schedule of Activities", with no table content.
- 280 activity_schedule cells, 90 of them non-empty.
- Cell values that are timing text rather than marks were transcribed literally: "0 hour", "Predose", "Predose, 12", "Predose, 8, 12", "24, 36", and the hour values 24/48/72/96/120/144/168/336/480. They were not split or converted to X.

## 2. Merged-mark decisions

- No merged marks in the body. Every body cell has its own vertical rules; checked on a 250 dpi render.
- Header row 1 "Periods 1 and 2 Study Days – at least 35 days washout between Day 1 doses" is merged across columns 3:14. Its right boundary is the rule between D36 (±1) and ED, confirmed on the render.

## 3. Synthesised

- Property names: "Epoch" for row 1, where the label cell is empty. "Study Day" for row 2, where the label cell reads "Procedure" (see D2).
- ED placement: see D3.
- Annotation markers: n1-n12 (Comments column, one per row). g1 and g2 are the two unmarked general notes below the table.

## 4. General notes

- g1 ("Note: All sampling times ...") and g2 (order of procedures) are table-scope.
- Each has one `schedule_property` location on row 1 with `method: synthesized`, following the §6 table-scope convention. The marker is deliberately not placed on any element's `annotation_markers`.
- g2 names specific procedures, so it could be bound to them instead (D1).

## 5. Mechanical mark-check

- The PDF has a text layer; it is not glyph-spread.
- Every mark and timing token was binned to the nearest header column centre using `pdftotext -bbox` on pages 10-11.
- The result matches the visual read cell for cell, with no disagreements.
- Notable check: the Immunogenicity X on p.11 sits under D15 (x ≈ 437.5). In the plain `-layout` text dump it appeared shifted left, and the bbox check confirms D15.

## 6. Annotation text integrity

- Not glyph-spread, so no reconstruction was needed.
- Each Comments note was read whole from its rule-bounded cell. Each cell sits beside exactly one activity row; no note spans several rows.
- No annotation text is contained in another.

## 7. Low-confidence calls

- D2 and D3 (header naming and ED typing).
- Footnote b describes Period 2 omissions. Its scope is recorded only on the cells where it is printed (D-1, D1, D36 header cells). It was not extended to the individual activities it names (pregnancy test, physical examination, clinical laboratory).

## 8. Orphan risk

None. Every annotation has at least one marker location, and every marker is defined in the source.

## 9. Method provenance

- `indentation_method: assumed_flat` on all 20 activities.
- `method: synthesized` on the marker locations of n1-n12 (activity_name), and of g1 and g2 (schedule_property row 1).
- No `unresolved` locations.
