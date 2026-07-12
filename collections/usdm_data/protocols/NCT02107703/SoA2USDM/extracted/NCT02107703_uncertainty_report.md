# NCT02107703 — SoA Extraction Uncertainty Report

Protocol I3Y-MC-JPBL (Eli Lilly), MONARCH 2 — Phase 3 abemaciclib (LY2835219) + fulvestrant in
HR+/HER2− advanced/metastatic breast cancer. Source: `NCT02107703_soa.pdf` (7 pages), Attachment 1
"Study Schedule" (Document pages 72–78).

## Table(s)

Two tables extracted.

- `NCT02107703_Table_01_extraction.json` — **`main_soa`**, the main Study Schedule. 38 activity
  rows (7 category headers + 3 category-level standalone activities + 28 procedures), 8 leaf data
  columns, 25 header-grid cells + marks, 108 activity×column marks, 17 footnotes (a–q).
- `NCT02107703_Table_02_extraction.json` — **`track`** (track_label "Extension period"), the
  "Study Schedule for the extension period only." 4 activity rows, 3 leaf columns, 3 footnotes (a–c).

Both schema-valid; all markers resolve; no orphan annotations.

## SOURCE ISSUE — none

- `_soa.pdf` page 1 (Document p72) is the Attachment cover (excluded from `page_start`). The main
  grid is on Document pages 73–75 with footnote overflow on 76–77; the extension schedule is p78.
  All SoA content is present. **No `_soa.pdf` regeneration needed.**

## Presentation

Landscape but **non-rotated** — standard row-clustering bbox method applied. Marks reconstructed
from `pdftotext -bbox` and validated against 130–200 dpi renders of every page.

## Table 1 — column model (8 leaf columns; grid cols 4–11)

| grid col | letter | phase | cycle/visit | rel. day |
|----|----|----|----|----|
| 4 | A | Baseline | BL / Visit 0 | ≤28 |
| 5 | B | Baseline | BL / Visit 0 | ≤14 |
| 6 | C | Patients on Study Treatment | Cycle 1 / Visit 1 | Day 1 |
| 7 | D | Patients on Study Treatment | Cycle 1 / Visit 1 | Day 15 ±3 |
| 8 | E | Patients on Study Treatment | Cycle 2–3 / Visit 2–3 | Day 1 (ᵖ) |
| 9 | F | Patients on Study Treatment | Cycle 4 and Beyond | Day 1 (ᵖ) |
| 10 | G | Postdiscontinuation Follow-Up | Short-Term Follow-Up / 801 | — |
| 11 | H | Postdiscontinuation Follow-Up | Long-Term Follow-Up / 802-8XX | — |

Five header rows: Study Phase (epoch, synthesized), Cycle, Visit, Approximate Duration (days),
Relative day within a cycle. Baseline merged 4:5; Patients on Study Treatment merged 6:9;
Postdiscontinuation Follow-Up merged 10:11; Cycle "1" and Visit "1" merged 6:7.

## Hierarchy / how the Procedure-Category column is encoded

The source has separate "Procedure Category" and "Procedure" columns. Encoded as:

- **indent 0** — category header rows (Study Entry/Enrollment, Medical History, Physical
  Examination, Tumor Assessment, Lab/Diagnostic Tests, Study Drug, Health Outcomes).
- **indent 1** — the procedures within each category.
- **indent 0 (with marks)** — three category-level standalone activities that span both columns:
  Survival Information, Adverse Event Collection/CTCAE Grading, Concomitant Medications.

Note: "Medical History" appears as both a category header and a procedure, as printed.

## Merged-cell / merged-mark decisions

- **Merged Cycle-1 marks**: Adverse Event Collection and Concomitant Medications show a *single*
  X centered over Day 1 + Day 15±3 (no internal divider — confirmed by zoom). Distributed across
  cols 6–7 with `source_range "6:7"`. Rows with two separate X's (Weight, Vital Signs, ECOG,
  Central hematology, Central serum chemistry) are kept as distinct C and D marks.
- **Merged treatment-text cells**: Fulvestrant Therapy ("Days 1 and 15 of Cycle 1, then Day 1 of
  Cycle 2 and beyond") and LY2835219 Therapy ("Every 12 hours on Days 1 through 28 of every
  cycle") distributed across cols 6–9 with `source_range "6:9"`.

## Footnotes

17 footnotes (a–q) captured verbatim from the text layer. Anchoring:

- Most are activity-name footnotes. Where the source repeats the marker on each cell (Xᶜ, Xᵇ, Xⁱ,
  Xʲ, Xᵈ, Xᵉ, Xᶠ, Xᵏ, Xˡ, Xᵐ, Xⁿ, Xᵒ), the footnote is anchored **once to the activity name** (its
  meaning is per-activity), not duplicated on every cell — flagged.
- **Header-cell footnotes**: **a** (Short-/Long-Term Follow-Up) → Cycle row, cols 10–11
  (`schedule_cell`); **p** (cycle-start delay) → Relative-day row, cols 8–9 (`schedule_cell`).
- **m** ("See Pharmacokinetic Sampling Schedule (Attachment 7)") typed `source_note`.
- **q** (Informed Consent) is printed on both the activity name and the baseline X; anchored to the
  activity name.

## Table 2 (extension) — model

3 leaf columns (grid 4–6): extension cycle Day 1 (4) and Day 15 (5), Extension Period Follow-Up /
Visit 901 (6). Adverse Events Collection X at cols 4 and 6; Fulvestrant/LY therapy merged-text
cells across cols 4–5. Footnotes a (header Follow-Up cell), b, c.

## Low-confidence calls / flags for review

1. **Table 2 typed `track`** (track_label "Extension period") rather than a second `main_soa` —
   it is a genuinely separate timeline (own cycle X-Y and visit 501-5XX/901 numbering). Judgment
   call; could alternatively be a second `main_soa`.
2. **Baseline sub-column A vs B**: assignments made from bbox x-centres (A≈283 "≤28", B≈310
   "≤14"); baseline-only marks (e.g. Informed Consent → A/≤28; Inclusion-Exclusion, Medical
   History, Weight, lab tests → B/≤14) follow the footnote timing where stated.
3. **PK sampling at C, D, E** (Cycle 1 Day 1, Day 15, Cycle 2–3) — the detailed PK timing lives in
   Attachment 7 (footnote m / not in this excerpt).
4. Footnote-on-cell duplication collapsed to the activity name (see Footnotes).
5. Category headers were synthesized as their own rows to represent the two-column
   Category/Procedure hierarchy.

## Orphan risk

None. All footnotes a–q (Table 1) and a–c (Table 2) are referenced; every activity/header marker
resolves to a defined annotation.
