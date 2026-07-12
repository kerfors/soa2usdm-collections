# NCT03548987 — SoA Extraction Uncertainty Report

Protocol NN9536-4376 (Novo Nordisk), v2.0, 21 December 2017. Semaglutide 2.4 mg once-weekly in
subjects with overweight/obesity who have reached target dose during a 20-week run-in period.
Source: `NCT03548987_soa.pdf` (4 pages = printed Document pages 8–11), Section 2 "Flowchart".

## Table(s)

One table — `NCT03548987_Table_01_extraction.json`, type **`main_soa`**.

- One logical flowchart spanning Document pages 8–11; the header (phase → Visit → Weeks → Window)
  repeats at each page break and is encoded **once**.
- Counts: 69 activity rows (58 with marks + 11 header/sub-header rows), 25 visit columns, 100
  header-grid cells, 378 activity×visit marks, 4 footnotes. Schema-valid; all markers resolve;
  no marks land on header/sub-header rows.

## SOURCE ISSUE — none

- `_soa.pdf` contains exactly the 4 flowchart pages (Document 8–11). No missing page. The full
  flowchart is present. **No `_soa.pdf` regeneration needed.**

## Presentation — landscape / rotated (method note)

The flowchart is printed **landscape (rotated 90° CCW)**: visits are columns, activities are rows.
The standard row-clustering bbox helper does not apply. Marks were reconstructed by:

- clustering PDF words into **page-x strips** (each strip = one activity row);
- defining **25 visit y-centres** from the week-number positions (weeks −1…75);
- assigning each `X` to the nearest visit y-centre (grid col = visit index + 1).

The transposed markdown flowchart (visits rendered as rows) was **not** used for marks.

## Column model

25 visit columns; grid col = visit index + 1 (V1→col 2 … V25→col 26).

| grid cols | epoch | visits (weeks) |
|----|----|----|
| 2 | Screening | V1 (−1) |
| 3–12 | Run-in (merged) | V2(0) P3(2) V4(4) P5(6) V6(8) P7(10) V8(12) P9(14) V10(16) P11(18) |
| 13 | Randomisation | V12 (20) |
| 14–24 | Maintenance period (merged) | V13(24) V14(28) P15(32) V16(36) P17(40) V18(44) P19(48) V20(52) P21(56) V22(60) P23(64) |
| 25 | End of treatment | V24 (68) |
| 26 | End of trial | V25 (75) |

'V' = clinic visit, 'P' = phone contact. Windows: V1 = "−7 to 0", V2 = "0", V3–V24 = "±3",
V25 = "0 to +5".

## Header model / synthesized property names

Four header rows: (1) Study Phase — epoch, **synthesized** (col-1 blank); (2) Visit(V), Phone (P)
— visit; (3) Timing of Visit (Weeks) — week; (4) Visit Window (Days) — window. Rows 2–4 take their
`property_name` directly from the source label column.

## Merged-cell decisions

- "Run-in" distributed across grid cols 3–12 (`merged_cell_range "3:12"`).
- "Maintenance period" distributed across grid cols 14–24 (`"14:24"`).
- Screening, Randomisation, End of treatment, End of trial each occupy a single column.

## Hierarchy / indentation

Three levels:

- **indent 0** — five grey full-width section headers: SUBJECT RELATED INFORMATION AND
  ASSESSMENTS, EFFICACY, SAFETY, TRIAL MATERIAL, REMINDERS.
- **indent 1** — direct activities and the white sub-headers (Body measurements, Vital Signs,
  Clinical Outcome Assessments, Administration of trial product).
- **indent 2** — leaf rows under a sub-header (Height/Body Weight/Waist; Systolic/Diastolic BP;
  Pulse; SF-36/WRSSM/PGI-S/PGI-C/SPS-6; PHQ-9/C-SSRS; Dispensing visit/Drug accountability).

Note: "Vital Signs (6.4.2, 9.4.3)" and "Clinical Outcome Assessments" each appear **twice** — once
under EFFICACY and once under SAFETY — as printed in the source (kept as separate rows).

## Cell- vs activity-level markers

- Footnotes a–d are activity-level (superscripts on activity names), captured with
  `marker_locations` at `activity_name`:
  - **a** (Demography definition) → Informed consent and Demography.
  - **b** ("For all female subjects") → Childbearing potential, History of Breast Neoplasm,
    Breast neoplasms follow-up.
  - **c** ("If subjects not fulfil randomisation criteria see Section 6.3.2") → Randomisation
    criteria and randomisation.
  - **d** ("Smoking is defined as…") → Tobacco Use.
- No cell-level (per-X) footnotes and no header-cell footnotes in this flowchart. All cells are
  plain "X" (no timepoint text, no legend symbols).

## Low-confidence calls / flags for review

1. **Narrow left-column reads visually re-verified** (V1 vs V2 vs P3), because these columns are
   thin: confirmed ECG at **V2** (Screening empty), Physical examination at **V1**, and
   "First date on trial product" at **P3** (week 2) via 220 dpi zoom. The clinically-surprising
   P3 placement is what the source shows.
2. **"Evaluation of …" rows land at V13** (week 24, first maintenance visit) + V24 — not V12
   (Randomisation). Confirmed by zoom (the wide Randomisation column is empty for these rows).
3. **SPS-6 label** normalized from wrapped source "SPS- 6" to "Stanford Presenteeism Scale
   (SPS-6)".
4. **PHQ-9 name** contains an en-dash "– 9"; kept verbatim. A stray numeric "9" token from the
   name was correctly excluded from cell values.
5. **Duplicate sub-headers** "Vital Signs" and "Clinical Outcome Assessments" retained under both
   EFFICACY and SAFETY as printed.

## Orphan risk

None. Footnotes a–d are all referenced; every activity marker resolves to a defined annotation.
No orphan annotations.
