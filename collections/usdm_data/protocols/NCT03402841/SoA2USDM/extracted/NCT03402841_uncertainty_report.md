# NCT03402841 — SoA Extraction Uncertainty Report

Protocol D0816C00020 v3.0 (olaparib, platinum-sensitive high-grade serous ovarian cancer, AstraZeneca).
Extractor: Claude Opus 4.8 (Cowork), single-pass v3.0. Marks and cell footnote markers read from PDF
text-layer word coordinates (poppler `pdftotext -bbox`); markdown not used for the grid. Both tables
were visually confirmed against rendered pages.

## SOURCE ISSUE (RESOLVED) — read this first

> **Resolved 2026-07-12:** `NCT03402841_soa.pdf` has been regenerated to the complete 5-page
> SoA source (protocol printed pages 40–44), so it now includes Table 1 (Screening, Visit 1)
> ahead of Table 2. The description below reflects the original 3-page excerpt at extraction time.

**`NCT03402841_soa.pdf` contains only Table 2.** The protocol's SoA is split into **Table 1 "Study
Schedule – Screening (Visit 1)"** (protocol pp.40–41) and **Table 2 "Study Plan Detailing the
Procedures"** (pp.42–44). The `_soa.pdf` excerpt holds only Table 2 (Visits 2+); Table 1 (the screening
schedule, Visit 1) is **missing** and was sourced from the full protocol PDF `NCT03402841.pdf` pp.40–41.
Same pattern as NCT04004988. If `_soa.pdf` is meant to be the complete SoA, it should be regenerated to
include Table 1.

## Tables (both `main_soa`)

- **Table 01 — `main_soa`** (Screening, Visit 1), pp.40–41. 1 header row, 2 columns
  ("Before screening period", "-28 to -1"), 18 activities, 19 marks, 8 annotations.
- **Table 02 — `main_soa`** (Procedures, Visits 2+), pp.42–44. 3 header rows, 7 timepoint columns,
  15 activities, 45 marks, 11 annotations.

Two independent schedules with different column structures → each `main_soa` per the taxonomy (screening
vs on-study).

## Table 02 column model (7 timepoint columns)

`2` Visit 2 (Day 1, window 0) · `3` Visit 3 (Day 29, ±3d) · `4` Visit No. 4 and subsequent tumour
assessment visits (±7d) · `5` Visit No. 5 and subsequent safety visits (±3d) · `6` Study treatment
discontinued (±7d) · `7` Follow up 30 days after last dose (+7d) · `8` Long-term follow up (no window).
Header rows: `visit` / `study_day` / `window`.

## Cell-level vs activity-level markers (Table 02)

- **`b`** (repeat-not-needed-if-assessed-within-7-days) is a cell marker on the **Visit 2 / Day 1**
  column for Physical examination, Vital signs, Haematology, Urinalysis.
- **`j`** (dispensing ±2-day window) is a cell marker on Olaparib dispensed/returned at Visit 3 and Visit 4.
- **`a`** marks two header cells: the Visit-No.-5 header (row 1, col 5) and the col-4 Day cell
  ("…every 12 weeks thereafter^a"), placed via `schedule_cell` locations to keep column scope.
- **`c`** appears on three activity names (Physical examination, Vital signs, Urinalysis) — one annotation
  with three `activity_name` locations.

## Low-confidence / judgement calls

1. **Table 01 header `property_type`.** The single "Day" row mixes a period reference
   ("Before screening period") and a day range ("-28 to -1"); classified `study_day`.
2. **Table 02 rows 17–18 hierarchy.** "Subsequent cancer therapy following discontinuation…" (row 17)
   and "Time to subsequent therapy and Survival" (row 18) — the second reads as a sub-item, but both
   share the same left margin in the PDF (x=78), so both are extracted at `indentation_level = 1`
   (siblings). Flag for review if a parent/child relationship is wanted.
3. **Table 01 `note1`.** The unlettered "Note: MRI/ CT scan more than 28 days prior to Day 1 may be
   acceptable…" printed under footnote e was captured as a footnote with synthesized marker `note1`,
   anchored to Tumour assessment (row 16) since it concerns the scan timing.
4. **Verbatim source spelling.** Table 02 row 12 activity name is kept as "Blood sample for
   restrospective gBRCA test" (source misspelling of "retrospective") to transcribe literally.
5. **Footnote letter set.** Table 02 footnotes run a–h then **j, k, l** (no "i" in the source); all
   present markers resolve.

## Orphan risk

None. Every annotation (8 in Table 01, 11 in Table 02) carries ≥1 `marker_location`; all lettered
markers appearing in cells or on activity names resolve to a defined annotation (verified programmatically).
