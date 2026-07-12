# NCT03637764 — single-pass v3.1.0 re-extraction — uncertainty report

**Checkpoint. Nothing committed / pipeline not run yet.** Preview raw in scratch, awaiting approval.

Protocol: Sanofi ACT15377 (isatuximab + atezolizumab), Amended Protocol 05, 23-Nov-2020.
Supersedes the prior v2.8/v2.4 PDF→Excel→JSON raw.

## Method
Text-layer table (not image-based). The full mark matrix was **re-derived from the PDF**, not copied
from the prior raw:
- Column x-centres and boundaries fixed from `pdftotext -bbox` header day-labels (D-28…Every-90) and
  confirmed by detected vertical rule-line geometry (11 column boundaries, cols 2–11 + excluded Notes col).
- Every `X` glyph read from bbox word coordinates and binned to a column.
- Every merged cell span read from per-row vertical rule-line detection (a missing internal boundary = a
  merge), cross-checked against direct visual reads. Two "As clinically indicated" rows and the AE/SAE /
  Prior right-hand cells were additionally verified on high-zoom crops.

## Table
- **1 table**, `main_soa`. Section 1.3, document pp.18–21 (soa PDF pp.1–4).
- Header: 3 property rows — Phase (`epoch`), Sub-phase (`period`), Timing (`study_day`).
- **10 schedule columns** (col2–col11): Screening D-28→D-15, Screening D-14→D-1, Cycle 1 D1/D8/D15,
  Cycle 2+ D1, EOT, Safety At-60, Safety At-90, Survival Every-90. Rightmost **Notes column excluded**
  (captured as annotations).
- **29 activities**. Two organisational headers (Laboratory Assessments r8, Disease Assessment r17) carry
  no marks (confirmed).
- Out of scope: the **PK/Immunogenicity Flow Chart (p.22)** and **Biomarker Flow Chart** are separate
  tables; the main SoA references them via merged "See … Flow Chart" text (see below). soa_pages 18-21 unchanged.

## Corrections applied vs the prior Excel-first raw (all PDF-confirmed)
1. **r7 12-Lead ECG** — "As clinically indicated" spans **c7:c8** (Cycle 2 + EOT). Prior had c7 only.
2. **r13 Coagulation (HCC/SCCHN/EOC)** — "As clinically indicated" spans **c7:c8**; Cycle 1 (c4–c6) is grey
   (no marks). Prior had it as [4:7].
3. **r23 AE/SAE Assessment** — "X (ongoing related AEs…)" spans **c9:c11** (incl. Survival). Prior had [9:10].
4. **r24 Prior/Concomitant Medication** — "X (related to AE/SAEs listed above)" spans **c9:c11**. Prior [9:10].

All other 25 activities' marks reproduced the prior raw exactly (independently re-derived → identical).

## Merged marks / text distributed (row → span)
- Merged screening `X` [2:3]: Informed consent r1, Demography r2, Height r4, Vital Signs r5, Resting O2 r6,
  12-Lead ECG r7, Serology r15, CT/MRI r18, AE/SAE r23, Prior/Concom r24. (Physical exam r3, Blood chem r10,
  Hematology r11, Coag-GBM r12, Coag-HCC r13, Urinalysis r16, Pregnancy r9, Brain MRI r19 carry a **single**
  col3 mark — bbox-distinguished at x≈198 vs merged-boundary x≈184.)
- "As clinically indicated": r7 [7:8], r13 [7:8], r16 Urinalysis [5:7].
- "Continuously throughout period" [4:8]: AE/SAE r23, Prior/Concom r24.
- "X (Weeks …)" c7, "X (if necessary)" c8, "X (until PD is confirmed …)" **[10:11]**: CT/MRI r18, Brain MRI
  r19, AFP/CA125 r20 (c9 At-60 empty — confirmed).
- "Cycle 2 Day 1 only" in c7 (single wide Cycle-2 column) — Blood Typing r14.
- "See Pharmacokinetics and immunogenicity Flow Chart" **[2:11]**: PK r25, ADA r26.
- "See Biomarker Flow Chart" **[2:11]**: Tumor Biopsy r27.

## Judgement call — flow-chart pointers (please review)
PK/ADA/Tumor-biopsy rows contain a single merged text cell spanning all schedule columns. Per prompt **§5**
(merged text cells like "See instructions"/"See Section x.y" → one distributed cell entry per column) these
are modelled as **distributed `cell_value`s** [2:11], and the prior raw's `n9`/`n10` "See … Flow Chart"
`source_note` annotations were removed. Alternative reading (§6) would keep them as `source_note`
cross-references on the activity label. Flagging for your call.

## Annotations (11) — all resolve, no orphans
- `a` "A cycle is 21 days" (footnote) → Treatment-Phase header cells c4–c7.
- `b` WOCBP negative pregnancy test (footnote) → Pregnancy test label.
- `c` "evaluation not applicable for Cohort E" (footnote) → 16 `Xc` cells (r3/r4/r5/r6/r10/r11/r12/r21 × c5,c6).
- `n1`–`n7` Notes-column section cross-references (`source_note`): 8.2.1, 8.2.2, 8.2.3, 10.3 Table 12,
  10.3, "10.3 Before each transfusion", 8.1.
- `n8` "Informed consent may be signed prior to D-28" (footnote).
- `n1`–`n8` markers are **synthesised** (the Notes column prints no marker letters); recorded here.

## Low-confidence / for human review against resolved HTML
- The `a`/`b`/`c` footnote letters ARE printed in source; `n1`–`n8` are synthesised from the Notes column.
- "As clinically indicated" spans (r7, r13 = [7:8]) — corrected from prior; verified on zoom but worth a
  glance in the resolved grid.
- Flow-chart-pointer modelling decision above.

## Validation
schema OK · every annotation ≥1 marker_location (no orphans) · all 11 used markers defined · no scheduling
marks on organisational headers (r8, r17).
