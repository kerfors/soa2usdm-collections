# NCT04320615 — SoA extraction uncertainty report

Source: NCT04320615_soa.pdf (9 PDF pages = document pages 77–85 per PAGEMAP.md; printed footers happen to agree). No protocol markdown was available. Prompt v3.8.1, single pass.

## Decisions needed (6)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p78, T1 rows 25–31 (also T2 rows 13–19, T3 rows 13–16) | Rows printed below the bold "Central Labs" header, down to the end of the table, are recorded as its children (level 1). All other rows are level 0. The source prints no visual indent. | Treat each table as flat: every row at level 0, with "Central Labs" as a mark-free row that groups nothing | 4.1 |
| D2 | p77, T1 row 15 PaO2/FiO2 (also p81, T2 row 5) | The "← Optional →" cell drawn across merged columns is recorded as the value "← Optional →" on every covered column (T1 cols 3–6; T2 cols 2–27). T2 col 28 has its own separate "Optional". | Treat "Optional" as a condition on the activity, not as a scheduled value, and leave those cells unscheduled | 4.2 |
| D3 | p77, T1 header row 1 | The top band (Screening / Baseline / blank over cols 4–6) is typed epoch, with a synthesized name "Study Period". The blank span is left empty. | Type it as visit ("Baseline" is a visit name), or read the blank span as an unlabelled treatment epoch | 4.3 |
| D4 | p81 / p84, T2 and T3 | Appendix 2 and Appendix 3 are each typed main_soa: they are independent, sequential schedules for the same participants, like separate Screening and Treatment tables | Type T2 and/or T3 as track (same participants, different study phase, own visit structure), with labels "Days 3–28" / "After Day 28" | 5.1 |
| D5 | p81, T2 header row 1 | The top band ("Days 3–28" over the daily columns, "Study Completion/Discontinuation" over col 28) is typed period, with a synthesized name "Period" | Type it as epoch, or as visit | 5.2 |
| D6 | p84, T3 header row 1 | The top band (blank over Days 35/45, "Study Completion" over Day 60) is typed visit at level 1, with a synthesized name "Visit" | Type it as epoch, or give it hierarchical_level null, since Study Day alone already tells the columns apart | 6.1 |

## Recorded, not open (9)

- §5 leading label columns: each table has L = 1 label column, so the first data column is position 2 in all three tables.
- §5 merged header cells: T1 row 1 cols 4:6 (blank), T1 row 2 "1" 3:4 and "2" 5:6, T2 row 1 "Days 3–28" 2:27, T3 row 1 blank 2:3. All are confirmed from raster rule lines.
- §6 header-cell footnotes: T1 "Screening a, b" goes on grid cell (1,2). T2 "Days 3–28 a" goes on every covered grid cell (row 1, cols 2–27, 26 locations). T3 "35 a" and "45 a" go on grid cells (2,2) and (2,3).
- §6 table-wide "Note:" paragraphs (T1 and T2) have no printed marker. Each gets the synthesized marker `tn1`, anchored to header row 1 with `method: synthesized`, and the marker is also added to that row's annotation_markers.
- §6 abbreviation blocks (T1 p78, T2 p82, T3 p84) are dropped. No term is printed as a marker anywhere, so they yield zero annotations.
- §6 footnotes that end with "(see Section 5.6)" (T1 j, T2 e, T3 e) are typed footnote, not source_note, because they explain as well as point.
- §4 header and "Study Day" rows reprinted on continuation pages p78 and p82 are de-duplicated.
- §1b symbol-font glyphs in the text layer (U+F02D, U+F02B, U+F0B1, U+F0DF/F0E0, U+F022) are mapped to what the rendered page shows: − / – (minus in header windows, en dash in ranges "0–4", "8–24", "3–28"), +, ±, ← / →, and ®. Subscripts in PaO2/FiO2 and SpO2 are flattened.
- §5 glyph case: every mark is a lowercase "x" in the source and is kept as "x".

## 1. Table inventory

| Table | Title | type | pages | data cols | activities | scheduled cells | annotations |
|---|---|---|---|---|---|---|---|
| 1 | Appendix 1 Schedule of Activities: Days 1 and 2 | main_soa | 77–80 | 5 (pos 2–6) | 28 | 56 | 21 |
| 2 | Appendix 2 Schedule of Activities: Days 3–28 | main_soa | 81–83 | 27 (pos 2–28) | 17 | 220 | 12 |
| 3 | Appendix 3 Schedule of Activities: After Day 28 | main_soa | 84–85 | 3 (pos 2–4) | 14 | 32 | 8 |

Activity rows per page:
- T1: p77 17 rows, p78 11 rows. p79 and p80 contribute none: they hold only footnotes b–m and n–t.
- T2: p81 16 rows, p82 1 row (Whole blood in PAXgene®). p83 contributes none: footnotes g–k only.
- T3: p84 14 rows. p85 contributes none: footnotes b–h only.

No page was skipped. None of the tables is horizontally tiled.

## 4. Table 1

- **4.1 Hierarchy (D1).** "Central Labs" is bold and underlined and carries no marks. The rows under it have no indent in the source, so their level is inferred from the font signal (`indentation_method: font_signal` on all rows). The same call applies to T2 (underlined bold) and T3 (bold only).
- **4.2 Merged marks (D2).** PaO2/FiO2 has an "x" at Screening (col 2). "← Optional →" spans cols 3–6: the raster shows no internal vertical rules in that row. It is distributed as 4 entries with `source_range "3:6"` and `method: visual_read`, because the arrows are symbol-font glyphs read from the render.
- **4.3 Header typing (D3).** Row 1 is epoch (L1). Row 2 is Study Day (study_day, L2). Row 3 is "Time Post Initial Treatment (Assessment Window)" (timepoint, L3), and each timepoint cell keeps its window text, e.g. "0 Pre-dose (−4 hrs)".
- **4.4 Cell markers.** Serum PD "x o" at cols 3 and 4. Serum PK "x q" at cols 3 and 4, plus marker p on its label.

## 5. Table 2

- **5.1 Classification (D4).** The three appendices split one continuous timeline for the same participants into consecutive phases, each with its own column structure. The type definitions give "Screening table and Treatment table with different column structures" as main_soa, so main_soa was chosen. Track is the recorded alternative.
- **5.2 Header typing (D5).** Row 1 is period (L1, synthesized name). Row 2 is Study Day 3…28 (study_day, L2). Col 28 is blank in row 2.
- **5.3 Merged marks (D2).** PaO2/FiO2 "← Optional →" spans cols 2–27 (no internal rules, raster-confirmed): 26 entries with `source_range "2:27"` and `method: visual_read`. Col 28 holds "Optional" in its own cell.

## 6. Table 3

- **6.1 Header typing (D6).** Row 1 is visit (L1, synthesized). Row 2 is "Study Day (Assessment Window)" (study_day, L2), with values like "35 (±3 days)".
- The row "Serum SARS-Cov-2 antibody titer" is transcribed with the source's own casing ("Cov").
- The continuation heading on p85 reads "After Days 28 (Cont.)". This is source wording and is recorded in table notes only.

## Synthesised items

- property_name, all with `synthesized: true`: T1 row 1 "Study Period", T2 row 1 "Period", T3 row 1 "Visit".
- Annotation marker `tn1`: T1 ("Note: On treatment days, all assessments should be performed prior to dosing, unless otherwise specified.") and T2 ("Note: For patients who have been discharged, all assessments should be performed within ±3 days of the scheduled onsite visit.").

## Mechanical mark-check

- **Method.** This is a text-layer PDF, checked with `pdftotext -bbox` (§1b):
  - Column centres were fixed from the header day and timepoint labels, and the T2 completion column centre from its marks.
  - Every `^[Xx]…` token was binned to its nearest column and nearest label row.
  - Merged spans were confirmed by detecting vertical rules in the raster per row band (§1d).
- **Result.** The bbox matrix agreed cell-for-cell with the visual read in all three tables:
  - T1: 56 cells, including 4 "Optional" cells.
  - T2: 220 cells, including 27 Optional cells.
  - T3: 32 cells.
- **Disagreements.** None.
- **Multi-line labels.** Serum PD, Serum sample…, Serum SARS-CoV-2 antibody titer and Whole blood… carry one vertically-centred mark per row. Each was bound to its own single row, not merged across rows.

## Annotation text integrity

- **Glyph spacing.** The text layer is not glyph-spread, so no word reconstruction was needed. The only glyph fixes are the symbol-font mappings listed above.
- **Source typos kept literally.** "Patents receiving…" in T1 footnotes o and p.
- **Containment within a table.** No annotation's text is contained in another's within the same table.
- **Near-duplicates across tables (checked against the pages).**
  - T2 b = T1 h plus two extra sentences about discharge and telephone visits. This matches the source.
  - T3 g and h add a "will not be performed if follow-up visits are conducted by telephone" sentence, and T3 g drops "if the test is available at the site". This matches the source.

## Low-confidence calls / orphan risk / method provenance

- **Orphans.** None: every annotation has at least one marker_location, and every location's marker also appears on its element's annotation_markers.
- **Undefined markers.** None: every in-table marker has printed text.
- **Non-default methods recorded:**
  - `activity_schedule.method = visual_read` on the 4 T1 and 26 T2 "← Optional →" cells.
  - `indentation_method = font_signal` on all activities.
  - `marker_locations.method = synthesized` on the two `tn1` locations.
- **Unresolved locations.** None.
