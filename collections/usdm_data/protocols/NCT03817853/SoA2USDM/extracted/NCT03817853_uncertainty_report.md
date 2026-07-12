# NCT03817853 — SoA Extraction Uncertainty Report

Protocol MO40597 v5 (obinutuzumab + G-chemo, follicular lymphoma). Appendix 1 Schedule of Activities.
Extractor: Claude Opus 4.8 (Cowork), single-pass v3.0. Marks and cell-level footnote markers read
from the PDF text-layer word coordinates (poppler `pdftotext -bbox`); markdown not used for the grid.

## Table

- **Table 01 — `main_soa`**, one table. 4 header rows, 27 activities (1 section header), 89 marks,
  29 annotations. `soa.pdf` is 3 pages: the grid is on page 1 (protocol p.100); pages 2–3
  (protocol pp.101–102) hold the footnotes, general Notes and abbreviation list — all captured.
  No additional tables; this protocol has a single SoA.

## Column model (11 timepoint columns)

`1` Day (label, excluded) · `2` Screening D–28 to D–1 · `3` Screening D–7 to D–1 · `4` Induction C1 D1 ·
`5` C1 D8 · `6` C1 D15 · `7` C2 D1 · `8` Cycles 3–6/8 D1 · `9` EOI · `10` Maintenance (every 8 weeks ±10 days) ·
`11` EOM · `12` Follow-up (3 months). All 11 confirmed by matching body-mark x-coordinates to the
day-header word centers.

## Header hierarchy (4 levels, merged)

- **L1 `epoch`** (row 1): Screening (merged cols 2–3) · **Treatment** (merged cols 4–11) · Follow-up (col 12).
- **L2 `period`** (row 2): Induction (6–8 cycles) (merged cols 4–8) · EOI · Maintenance · EOM.
- **L3 `cycle`** (row 3): Cycle 1 (merged cols 4–6) · Cycle 2 (col 7) · Cycles 3–6/8 (col 8).
- **L4 `study_day`** (row 4): the day labels; label cell literally reads "Day".

## Merged-cell decisions (header)

Each merged header cell was distributed to every covered column with `is_merged_cell=true` and the
span in `merged_cell_range`: Screening `2:3`, Treatment `4:11`, Induction `4:8`, Cycle 1 `4:6`.
Header footnote markers were placed on **every** covered cell (a on cols 2–3; b on cols 4–8), and
each such cell is a `marker_location`. No merged marks in the table body.

## Synthesized values

- **`property_name`** for the three upper header rows synthesized: "Phase" (L1), "Treatment sub-phase"
  (L2), "Cycle" (L3) — their label cells (col 1) are blank. L4 name "Day" is taken from the source cell.
- No synthesized annotation markers were needed for the body — every note in this table carries a real
  lettered footnote marker (a–z, aa). Only the table-wide general Notes block (`note1`) and the
  abbreviation list (`abbr`) were given synthesized markers, anchored nominally to `schedule_property`
  row 1 (they have no in-cell anchor).

## Low-confidence / judgement calls

1. **L2 `property_type = period`.** Row 2 mixes sub-phases (Induction, Maintenance) with end-of-phase
   milestone visits (EOI, EOM). Classified `period`; EOI/EOM could alternatively be modeled as `visit`.
2. **"Study drug administration" as a section header.** It is a merged row label spanning the
   obinutuzumab and chemotherapy sub-rows; extracted as an `indentation_level=0` parent with those two
   as `indentation_level=1` children. All other activities are a flat list at level 1.
3. **Cell-level footnote markers vs activity-level.** Markers that sit on a specific `x` were kept as
   cell markers: f (Informed consent, col 2); c/d (obinutuzumab — c on C1 D1/D8/D15, d on C2 D1 /
   Cycles 3–6/8 D1 / Maintenance); u and w (first three columns of Concomitant medications / Adverse
   events); x (Provider-reported, col 8 = Cycles 3–6/8 D1, i.e. Cycle 4). u/w also appear on their
   activity names, so those annotations carry both an `activity_name` and `schedule_cell` locations.
4. **Header footnote `location_type`.** Per-column header markers (a, b, e, aa) use
   `location_type="schedule_cell"` pointing at the header grid cell to preserve which column each governs.
5. **`chemotherapy` (footnote s)** row has marks only at C1 D1, C2 D1, Cycles 3–6/8 D1 (cols 4, 7, 8),
   matching "bendamustine Days 1–2 / CHOP Days 1–5 / CVP Days 1–5" being represented by the Day-1 column
   of each induction cycle group.

## Orphan risk

None. All 29 annotations carry ≥1 `marker_location`; all 27 lettered markers appearing in cells or on
activity names resolve to a defined annotation (verified programmatically).
