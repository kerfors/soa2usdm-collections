# NCT03548935 — SoA Extraction Uncertainty Report

Protocol NN9536-4373 (Novo Nordisk), v2.0, 21 December 2017. Semaglutide overweight/obesity trial
(sister trial to NN9536-4376 / NCT03548987). Source: `NCT03548935_soa.pdf` (6 pages = printed
Document pages 8–13), Section 2 "Flowchart".

## Table(s)

One table — `NCT03548935_Table_01_extraction.json`, type **`main_soa`**.

- One logical flowchart spanning Document pages 8–13; header (phase → Visit → Weeks → Window)
  repeats at each page break and is encoded **once**.
- Counts: 78 activity rows (6 grey section headers + 6 sub-headers + 66 leaf activities),
  25 visit columns, 100 header-grid cells, 428 activity×visit marks, 7 footnotes. Schema-valid;
  all markers resolve; no marks on header/sub-header rows.

## SOURCE ISSUE — none

- `_soa.pdf` contains exactly the 6 flowchart pages (Document 8–13); Section 3 (Introduction)
  starts page 14. No missing page. **No `_soa.pdf` regeneration needed.**

## Presentation — landscape / rotated

Same as NCT03548987: flowchart printed **landscape (rotated 90° CCW)**, visits as columns.
Marks reconstructed with the rotation-aware bbox parser (activity = page-x strip; 25 visit
y-centres from the week numbers) and **validated visually against renders of all six pages**.

## Column model — NOTE: differs from the sister trial

25 visit columns; grid col = visit index + 1.

| grid cols | epoch | visits (weeks) |
|----|----|----|
| 2 | Screening | V1 (−1) |
| 3 | Randomisation | V2 (0) |
| 4–11 | Dose escalation period (merged) | V3(2) V4(4) P5(6) V6(8) P7(10) V8(12) P9(14) V10(16) |
| 12–24 | Maintenance period (merged) | P11(18) V12(20) P13(24) V14(28) P15(32) V16(36) P17(40) V18(44) P19(48) V20(52) P21(56) V22(60) P23(64) |
| 25 | End of treatment | V24 (68) |
| 26 | End of trial | V25 (75) |

Differences vs NN9536-4376 (worth flagging): **Randomisation is V2 (week 0)** here, followed by a
"Dose escalation period" (not "Run-in"); **V3 is a clinic visit** and **P13 a phone contact**
(the sister trial had P3 and V13). Windows: V1 "−7 to 0", **V2 "±0"**, V3–V24 "±3", V25 "0 to +5".

## Merged-cell decisions

- "Dose escalation period" distributed across grid cols 4–11 (`"4:11"`).
- "Maintenance period" distributed across grid cols 12–24 (`"12:24"`).
- Screening / Randomisation / End of treatment / End of trial each a single column.

## Hierarchy / indentation

- **indent 0** — 6 grey section headers: SUBJECT RELATED INFORMATION AND ASSESSMENTS, EFFICACY,
  SAFETY, OTHER ASSESSMENTS, TRIAL MATERIAL, REMINDERS.
- **indent 1** — direct activities and white sub-headers.
- **indent 2** — leaf rows under a sub-header.
- Sub-headers (indent 1): Body measurements (9.1.1); Vital Signs (9.4.3) ×2 (under EFFICACY and
  SAFETY); Clinical Outcome Assessments ×2 (9.1.2 efficacy, 9.4.1 safety); Administration of trial
  product (7.1, 7.5). Retained as printed.

## Split / merge corrections

- **IWQoL-Lite and PGI-S split**: the source rows "Impact of weight on quality of life – Lite
  Clinical Trials version (IWQoL-Lite for CT)" and "Patient Global Impression of Status (PGI-S)"
  render as adjacent x-strips that the rotated parser merged; split into two rows (identical marks
  V2,V6,V10,V12,V16,V20,V24), confirmed on the page-3 render.

## Footnotes — activity-level and CELL-level

Seven footnotes a–g. Five are activity-name footnotes; **two are cell-level**:

- **a** (Demography definition) → Informed consent and Demography.
- **b** ("For all female subjects.") → Childbearing potential, History of Breast Neoplasm,
  ICIQ-UI-SF, Breast neoplasms follow-up.
- **c** ("Smoking is defined as…") → Tobacco Use.
- **d** ("DEXA scan is performed in a sub-population.") → DEXA scan (activity name).
- **e** ("DEXA scan should be performed in fasting state prior to randomisation…") → **cell-level**
  on the DEXA scan **V1** cell (source "Xᵉ"); `location_type = schedule_cell`.
- **f** ("Only for subjects where the separate informed consent for future research has been
  signed.") → Biosamples for future analysis.
- **g** ("Fasting at V25 is defined as at least 2 hours without food and drink…") → **cell-level**
  on the Attend-visit-fasting **V25** cell (source "Xᵍ"); `location_type = schedule_cell`.

## Low-confidence calls / flags for review

1. **DEXA scan indentation** set to indent 1 (standalone efficacy item after the ICIQ-UI-SF /
   COA block). It sits flush-left in the render; a case could be made for it belonging to the
   Clinical Outcome Assessments sub-group — flagged.
2. **Verbatim source spellings kept**: "PK dairy" (for "PK diary"), "Systolic blood Pressure"
   (lowercase "blood"), "Anti-Semaglutide Antibody".
3. Every visit-column assignment on the narrow left columns (Screening / Randomisation / V3)
   was visually confirmed, e.g. "First date on trial product" at **V3**, Randomisation-criteria
   at **V2**, and the "Evaluation of…" rows at **P13** (week 24) + V24.

## Orphan risk

None. All 7 footnotes are referenced; every activity/cell marker resolves to a defined annotation.
