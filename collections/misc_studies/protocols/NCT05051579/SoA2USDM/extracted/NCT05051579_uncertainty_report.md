# NCT05051579 — SoA extraction uncertainty report

Prompt 3.8.1, single pass. Source: NCT05051579_soa.pdf (6 PDF pages = document pages 13-18 per PAGEMAP.md; printed footers read 12-17, i.e. one lower, and were ignored). No protocol markdown was available, so all text comes from the PDF text layer.

## Decisions needed (6)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.17, rows 67-69 (Pharmacokinetic (PK) sample / Predose / Postdose) | The PK row is split into a parent row with no marks and two child activities, "Predose" and "Postdose", each with its own X marks. The words Predose/Postdose sit where the Visit 1 (Screening) column is, but they are read as row sub-labels, so Visit 1 gets no PK entry. | Treat "Predose"/"Postdose" as the Visit 1 cell content, or emit two flat activities "Pharmacokinetic (PK) sample - Predose" / "- Postdose". | 4.2 |
| D2 | p.16, rows 49-51, note n11 | The note "Collect serum estradiol, FSH, and LH in women whose menopausal status needs to be determined..." is printed under the FSH row only. It is bound to FSH by position, and also to LH and Estradiol because it names those tests. | Bind the note only to the FSH row it sits under. | 4.4 |
| D3 | p.13, header row 1, note n1 | The sentence inside the "Study Period II Treatment Period" header cell ("For early terminations ... Shaded columns represent the dose escalation period Weeks 0-16.") is moved out of the header value and recorded as a note on each Treatment Period column (Visits 3-18). | Keep the sentence as part of the header text, or scope it to the whole table (or only to the ET column / shaded columns it mentions). | 4.4 |
| D4 | p.13, header row 6 (Fasting Visit) | "Fasting Visit" is recorded as a header row that marks which visits are fasting visits, not as an activity. | Record "Fasting Visit" as an activity row with X marks. | 4.1 |
| D5 | p.15, row 37 (Participant Survey), note n10 | The note "administer at early termination if it occurs before 26 weeks" is bound to the Participant Survey activity as a whole. | Bind it only to the Participant Survey ET cell (column 20), because it governs the ET mark. | 4.4 |
| D6 | p.13, header row 2 | The second header band ("Screening" over Visit 1, "Lead-In" over Visit 2) is typed as `period` (sub-phases of Study Period I) at level 2. | Type it as `epoch`, or as visit names. | 4.1 |

## Recorded, not open (6)

- One table, not continuations (taxonomy "a page break inside ONE printed table is not a continuation"): the header reprints on pages 14-18 with no new table number or caption.
- De-duplication (§1b): the header rows and the Fasting Visit row reprint on all 6 pages and are counted once.
- Leading label columns (§5): L = 1, so the first data position is 2. Visit 1-18 map to positions 2-19, ET to 20 and 801 to 21.
- Abbreviation list (§6): the "Abbreviations:" block under the table on p.18 has no in-grid markers, so it yields 0 annotations.
- Notes without printed markers (§6): full-width note sub-rows were given synthesised markers n1-n13 and bound with `method: synthesized` to the row they sit under.
- Section membership across page breaks (§4): "Vital signs" (p.14) continues the Physical Evaluation section and "C-SSRS" (p.16) continues Clinician-Administered Assessments. Neither page reprints a section header, so these rows are indentation 1 under the previous section.

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (Section 1.3), document pages 13-18.
- 20 data columns (positions 2-21) and 6 header rows (schedule_properties 1-6).
- 71 activity rows (row positions 7-77): 9 section headers at level 0, 60 activities at level 1, and 2 PK sub-rows at level 2 (D1).
- 372 X marks and 13 annotations.
- Activity rows per page: p13 = 12, p14 = 10, p15 = 11, p16 = 16, p17 = 14, p18 = 8. Every page in the range contributed rows.

## 2. Merged marks

- No body mark is merged across columns. Every X sits in its own ruled cell, and the per-column bbox binning matched the visual read.
- Merged spans appear only in the header:
  - Row 1: "Study Period I Screening/Lead-in" spans 2:3. It also extends into the label column, so the row name is synthesised.
  - Row 1: "Study Period II Treatment Period" spans 4:19.
  - Row 2: the blank cell spans 4:19.
- The ET column (20) has no study-period label in row 1. It is left empty.
- Grey shading of the Visit 4-12 cells (the dose-escalation period, per the header text) is presentation only. It is not recorded, and no cell content was changed because of it.

## 3. Synthesised items

- Synthesised property names:
  - Row 1 "Study Period" (epoch, level 1)
  - Row 2 "Screening/Lead-in sub-period" (period, level 2)
- Both rows carry `structure_method: inferred_from_layout`. Row 5 (window) is level 5. Row 6 (Fasting Visit, `other`) is level null.
- Synthesised annotation markers:
  - n1: Treatment Period header text, on grid cells row 1, columns 4-19
  - n2: Vital signs
  - n3: Symptom-directed physical examination
  - n4: 12-Lead ECG
  - n5: Return ABPM device
  - n6: Record ABPM measurements
  - n7: Explain diet and physical activity plan
  - n8: Discuss diet and physical activity progress
  - n9: Dispense study drug administration log
  - n10: Participant Survey
  - n11: FSH, plus LH and Estradiol by text_match (D2)
  - n12: PK note, bound to the Predose and Postdose rows
  - n13: Dispense study intervention capsules

## 4. Details

### 4.1 Header structure
- The header has five timing rows plus "Fasting Visit". Row 2 typing is D6.
- Fasting Visit is a schedule property (D4). It is X at every visit except Visit 2 (Lead-In).
- The "Lead -In" line wrap is normalised to "Lead-In".

### 4.2 PK sample row (p.17)
- The label cell "Pharmacokinetic (PK) sample" spans three sub-rows: Predose, Postdose, and a one-cell note. The raster check found no internal horizontal rules inside the 471-515 pt band, which confirms the note is one cell.
- Predose marks: Visits 3, 8, 10, 13, 18. Postdose marks: Visits 6, 8, 12, 15, ET.
- Modelling as parent plus children is D1.

### 4.3 Mechanical mark-check
- The PDF has a text layer and vector rules, and it is not glyph-spread.
- Marks were binned with `pdftotext -bbox`. Every X token was assigned to the nearest header Visit Number x-centre on each page, and every token fell well within one column.
- The bbox matrix (372 body marks, Fasting rows excluded) agrees cell-for-cell with the visual read of all six page renders. There were no disagreements.

### 4.4 Annotation text integrity and scope
- Each note was taken as the full text of its ruled full-width sub-row cell. No note pair overlaps or contains another.
- Notes n2 and n4 contain inline cross-references (Section 10.7, Section 8.2.3). They also carry instructions, so they stay `footnote`. No separate source_note was emitted.
- The capsule-dispensing note (n13) spans only the Visit 3-17 columns in the source. It is bound to its activity.
- Open scope calls: D2 (n11), D3 (n1) and D5 (n10).
- The paragraph above the table that refers to Section 10.11 (Appendix 11) is outside the table. It was not captured.

### 4.5 Orphan risk and provenance
- There are no orphans. Every annotation has at least one location, and every activity_name location's marker also appears in that row's `annotation_markers`.
- Non-default methods recorded:
  - `indentation_method: visual_estimate` on all activities, because hierarchy comes from the bold grey section bands and the PK sub-label layout.
  - `method: synthesized` on the n1-n13 locations.
  - `method: text_match` on the n11 locations for LH (row 50) and Estradiol (row 51).
- No `unresolved` locations.
