# NCT03693430 — SoA extraction uncertainty report

Source: NCT03693430_soa.pdf (4 PDF pages = document pages 9-12 per PAGEMAP.md), protocol NN9536-4378 v3.0, 29 June 2018, section "2 Flowchart".
Output: NCT03693430_Table_01_extraction.json (one table). Prompt 3.8.1.

## Decisions needed (4)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.11 (and p.12), Visit Window row, V33 column | V33 (End of treatment) window recorded as ±3 days, as printed on pages 9 and 10 | Record ±5 days, as printed on the reprinted header of pages 11 and 12 | 3.1 |
| D2 | p.11 row 44 "Breast neoplasms follow-up"; p.12 row 63 "Training in trial product, pen-handling" | Rows printed above the reprinted Visit header are kept as ordinary activity rows in reading order (Breast neoplasms follow-up under SAFETY; Training under REMINDERS) | Place them in a different section (e.g. Training under TRIAL MATERIAL) or order them differently | 3.2 |
| D3 | p.10 row 37 Control of Eating Questionnaire (CoEQ) | Marks transcribed as lower-case "x", exactly as printed (smaller glyph than every other mark) | Normalise to "X" as a typesetting inconsistency | 3.3 |
| D4 | p.9 row 1 (unlabelled period band) | All six band values typed epoch, including single-column Randomisation (V2), End of treatment (V33), End of trial (V34) | Treat the single-column cells as visit-level labels and keep only Screening / Dose escalation / Maintenance as epochs | 3.4 |

## Recorded, not open (8)

- §1a/§1d image-based: the PDF has no text layer and no vector rules (each page is one 1753x1239 raster at 150 ppi). Rules recovered from the raster; marks by near-black pixel count; all text visually transcribed. Methods recorded in the JSON (see §6).
- §5 label columns: L = 1 (activity-name column); first data column is position 2; columns 2-35 = V1..V34.
- §1b/§4 header de-duplication: the period band and Visit / Timing / Window rows reprint on pages 10, 11 and 12; each is emitted once (rows 1-4).
- §4 section headers (SUBJECT RELATED INFORMATION AND ASSESSMENTS, EFFICACY, SAFETY, TRIAL MATERIAL, REMINDERS) and group rows (Body measurements, Vital signs, Vital Signs, Administration of trial product) carry no marks — confirmed by pixel counts of 0 in every cell.
- §6 inline cross-references stripped from activity names into 26 `source_note` annotations (pr1-pr26, deduplicated by text, split where one label cites several). Sections printed as bare numbers "(6.1)" are rendered "See Section 6.1."; appendix links "See Appendix n.".
- §6 no abbreviation list printed with the table; none emitted.
- §3 Period band property name synthesised ("Period", label cell empty); structure_method inferred_from_layout.
- Hyphenation artefacts cleaned in names only: "INFOR-MATION" -> "INFORMATION" (row 5, raw kept in cell_text), "Randomisa-tion" -> "Randomisation" (period band).

## 1. Table summary

Table 1 — "Flowchart", `main_soa`, document pages 9-12. One printed table running across four pages under a reprinted header; no own caption on the later pages, so extracted as one table (not continuations).

- Columns: 34 data columns (V1, V2, P3, V4, ... P31, V32, V33, V34); weeks -1, 0, 2-20 (step 2), 24-100 (step 4), 104, 111.
- Header rows: 4 (Period band, Visit (V) Phone(P), Timing of Visit (Weeks), Visit Window (Days)).
- Activities: 63 rows (5 section headers, 4 group headers, 54 scheduled activities); 470 scheduled cells.
- Rows per page: p.9 = 19 (rows 5-23); p.10 = 20 (rows 24-43); p.11 = 19 (rows 44-62); p.12 = 5 (rows 63-67). Every page in the declared range contributed rows.
- Annotations: 30 (4 footnotes a-d, 26 source_notes).

## 2. Merged cells

- Period band: "Dose escalation period" merged over columns 4:11 (P3-V10); "Maintenance period" merged over 12:33 (P11-V32). Extents from missing vertical rules in the raster band. Screening, Randomisation, End of treatment, End of trial are single-column cells.
- No merged marks, arrows or spanning text in the body: every internal vertical rule is present in every body band on all four pages.

## 3. Judgement calls (detail)

3.1 V33 visit window. Pages 9 and 10 print "±3" under V33; the reprinted header on pages 11 and 12 prints "±5". All other header values are identical across the four prints. Recorded ±3 (first print); not resolvable from this excerpt.

3.2 Rows above the reprinted header. On pages 11 and 12 the first body row of the page ("Breast neoplasms follow-up b (9.4)"; "Training in trial product, pen-handling (7.1.1)") is printed directly under the period band, before the Visit / Timing / Window rows reprint. Treated as the next activity in reading order (row 44 follows Technical complaint, row 63 follows Hand out directions for use). Breast neoplasms follow-up (V33, V34) sits naturally with "Colon neoplasms follow-up" in SAFETY. Training sits in REMINDERS by position.

3.3 CoEQ marks are a visibly smaller lower-case "x" (visibly smaller on the render; pixel counts 28-32, at the bottom of the 32-56 range of the other marks). Kept literal.

3.4 Period band typing: see D4.

3.5 Indentation: dark-grey caps rows = level 0; light-grey rows = level 1; white rows printed indented under a grey group row (Height, Body weight, Waist circumference, Systolic/Diastolic Blood Pressure, Pulse, Dispensing visit, Drug accountability) = level 2. indentation_method visual_estimate. Leading spaces in cell_text for level-2 rows represent the visual indent (no text layer exists).

3.6 Visit Window row given hierarchical_level null (does not distinguish any column); Visit = level 2, Weeks = level 3.

3.7 "Attend visit fasting (6.4.1)" is a body row with marks in the REMINDERS section, so it is an activity, not a schedule_property.

## 4. Mechanical mark-check

Image method (§1a/§1d): vertical rules = pixel columns with ink fraction > 0.5 over the table height (36 rules, identical x on all 4 pages); horizontal rules = pixel rows with ink fraction > 0.85 across the table width. Each cell inset 4 px; mark = count of pixels with intensity < 90. Distribution is cleanly bimodal: empty cells 0, marked cells 28-56; threshold 15. The detector matrix was compared with direct visual reads of every body row on all four pages (dense rows: Concomitant medication, Adverse event, Diet and physical activity counselling; sparse rows: Height, Evaluation of glycaemic status, First date on trial product). No disagreement. Recommend a spot-check of the resolved grid, as for any image-based source.

## 5. Annotation text integrity

- No text layer; footnotes a-d visually transcribed from page 12 below the table (annotation_text_source.method = visual_transcription). Not glyph-spread (no text layer at all).
- No overlapping or contained annotation texts.
- Footnote b is cited on four rows (7 Childbearing potential, 14 History of Breast Neoplasm, 40 Pregnancy test, 44 Breast neoplasms follow-up): one annotation with four locations.
- No markers without printed definitions; no redaction inside the table (the only redaction is in the page running header).

## 6. Method provenance

- schedule_grid: all header cells method visual_read.
- activity_schedule: all 470 cells method raster_pixel_detection.
- activities: activity_name_source.method visual_transcription and indentation_method visual_estimate on all 63 rows.
- annotations: annotation_text_source.method visual_transcription on all 30.
- marker_locations: the 26 source_note locations are method synthesized (markers pr1-pr26 are synthesised for inline references); footnote a-d locations are printed markers (default).
- schedule_property row 1: structure_method inferred_from_layout; property_name synthesised.
- No unresolved marker locations.

## 7. Orphan risk

None: every annotation has at least one marker_location, and every location's marker is present in that row's annotation_markers.
