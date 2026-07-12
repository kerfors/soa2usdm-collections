# NCT03421379 — SoA extraction uncertainty report

Protocol: LY900018 (nasal glucagon) vs IM glucagon, I8R-JE-IGBJ(a) Clinical Pharmacology Protocol,
Amendment (a), Approval 05-Dec-2017. Two-period crossover, insulin-induced hypoglycemia.

## Per table
- **Table 01 — `main_soa`.** Title "Study Schedule Protocol I8R-JE-IGBJ" (Section 2). Document pages 11–18.
  8 data columns (positions 2–9); 36 body rows = 3 section headers + 33 activities; 80 schedule cells
  (56 `X`, 24 timing-text); 27 annotations.
- **One logical table, header repeated per page.** The two-row header repeats on each of pages 11–18.
  Per our settled practice this is modeled as a single `main_soa` (header encoded once), not 8
  `continuation` entries.
- **Completeness:** confirmed complete against the `.md`. Section 2 runs pages 11–18 and ends at the
  Abbreviations/footnote block immediately before Section 3 (page 19). No missing pages/tables.
- **Orientation:** portrait, non-rotated. Column x-boundaries confirmed by `pdftotext -bbox` on page 1;
  every body mark lands cleanly in its expected sub-column (Day -1 vs Day 1 are well separated).

## Column model
Row 1 (level 1, `epoch`): Screening | Period 1 (merged 3:4) | Wash out | Period 2 (merged 6:7) |
Follow-up/ED | Additional Follow-up for TE ADAᵃ.
Row 2 (level 2, `study_day`): Days -28 to -2 | Day -1 | Day 1 | 3 to 14 days | Day -1 | Day 1 |
Within 28±2 days after last study treatment | (col 9 blank).

## Merged marks
- **None in the body.** No body mark spans the two sub-columns of a period; each `X`/timing-text sits in a
  single Day -1 or Day 1 column, so no distribution/`source_range` was needed. `source_range` is "" on all
  cells.
- Header merges only: Period 1 (cols 3:4) and Period 2 (cols 6:7) `is_merged_cell` on the epoch row.

## Synthesised
- **Property names (2):** row 1 → "Period" (label cell empty); row 2 → "Study Day" (col-1 cell literally
  reads "Procedure", which is the activity-column header spanning both header rows, so the timing-row name
  was synthesised). Both flagged `synthesized: true`.
- **Comments-column markers (c1–c25):** the right-hand "Comments" column (source physical col 10) is a
  notes column, excluded from the grid; each distinct note became a synthesised annotation. c5 and c15 are
  cross-references → `source_note`; the rest are `footnote`.
- **Dedup-by-text:** c15 "See Appendix 2, Clinical Laboratory Tests, for details." links rows 22 + 24
  (Clinical Serology Tests, HbA1c). c19 "Additional tests can be done at the discretion of the investigator."
  links rows 27 + 28 (Ethanol Testing, Urine Drug Screen). (Note: Clinical Lab Tests row 23 has a *longer*
  variant of the Appendix-2 text with fasting instructions, kept separate as c16.)
- **note1 (synthesised):** the table-wide bottom Note ("If multiple procedures take place at the same time
  point … ECG, vital signs, and venipuncture …").

## Low-confidence / judgement calls (please review)
1. **Row-1 `property_type` = `epoch`.** The row mixes true epochs (Screening, Wash out, Follow-up/ED,
   Additional Follow-up) with the two crossover **treatment periods** (Period 1, Period 2). Chose `epoch`
   as the topmost column-distinguishing row; `period` is defensible. Flagged.
2. **note1 placement/scope.** It is genuinely table-wide. Its `marker_locations` anchor is
   `schedule_property` row 2 (timing row) for traceability, but because no element carries the marker,
   `resolve` will classify it **`annotation_scope: table`** — which is the honest scope. Confirm you're happy
   with table scope rather than linking it to the specific procedures it names (ECG / Vital Signs / PK /
   Plasma Glucose).
3. **Footnote 'a' scope.** Marker sits on the "Additional Follow-up for TE ADA" epoch cell (grid row 1
   col 9); it will resolve **column-scoped** to col 9 (same pattern as prior extractions). Text is the
   immunogenicity-sampling footnote.
4. **Mixed footnote vs source_note.** c16 (Clinical Lab Tests) and c23 (Anti-glucagon Antibodies) each
   contain an embedded section reference ("See Appendix 2 …", "Refer to Section 9.7.1 …") but are primarily
   explanatory, so classified `footnote` (not split). c5 ("Refer to Section 9.5.5.1.") and c15 ("See Appendix
   2 … for details.") are pure references → `source_note`.
5. **Abbreviations block NOT captured.** The Abbreviations list (CRU, ECG, ED, FSH, HbA1c, IMG, min, PD, PG,
   PK, TE ADA) was **not** emitted as `abbreviation` annotations — the terms carry no in-grid markers, so
   they would be orphans (fails the ≥1-marker_location rule). Consistent with prior extractions. Flag if you
   want them captured (would require linking each to the label rows where the term appears).

## Orphan risk
None. All 27 annotations have ≥1 `marker_location`; every element marker (`a` on grid col 9; c1–c25 on
activity rows) resolves to a defined annotation. `note1` is table-scoped by design (see call #2).

## Timing-text cells (not `X`)
Many cells carry post-dose timepoint lists relative to study-treatment administration (0 min) rather than
`X` — e.g. PK (Glucagon) and Plasma Glucose for PD ("…Predose, 5, 10, 15, 20, 25, 30, 40, 50, 60, 90, 120,
240 min"), Vital Signs / Triplicate ECG / questionnaires ("Pre-hypoglycemia induction, …"), Physical Exam
("240 min"). All transcribed verbatim as `cell_value`.

**STOP — awaiting your verification before running the pipeline.**
