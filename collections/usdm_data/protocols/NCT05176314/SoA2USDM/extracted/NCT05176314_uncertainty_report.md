# NCT05176314 — SoA Extraction Uncertainty Report

Protocol J2N-MC-JZNW (a) — rosuvastatin / pirtobrutinib drug–drug-interaction study (Loxo/Lilly).
Extractor: Claude Opus 4.8 (Cowork), single-pass v3.0. Marks and cell values read from PDF text-layer
word coordinates (poppler `pdftotext -bbox`); page 1 confirmed visually. Source `_soa.pdf` is complete
(3 pages: grid pp.10–11, footnotes/abbreviations p.12).

## Table

- **Table 01 — `main_soa`**, one table, protocol pp.10–11. 2 header rows, 21 timepoint columns,
  24 activities, 122 cell values, 8 annotations. The page-2 header repeat is a page-break artifact and
  is encoded once (single table, not a continuation) — consistent with the multi-page single-table
  convention in the repo's verified extractions.

## Column model (21 timepoint columns)

`2` Screening (D-42 to -2) · `3` Day -1 · `4`–`21` Days 1–18 · `22` FU/ED (Day 24 ± 2 days).
Header rows: L1 `epoch` (Screening · **Treatment Period (Study Days)** merged cols 3–21 · FU/ED),
L2 `study_day` (the day numbers). Day 18 header carries footnote `a`; the FU/ED header carries `b`.

## CCI redactions (important — confidential content missing in source)

The source redacts confidential commercial information as **"CCI"** in four places:

- **Two activity rows print only as "CCI"** (extraction rows 22 and 24, sitting immediately before and
  after "Rosuvastatin PK samples"). Their real names and their entire mark rows are redacted — almost
  certainly the pirtobrutinib PK-sampling schedule. They are captured as activities named `CCI` with
  **no schedule marks** (marks are not recoverable from the source).
- **Footnotes `d`, `f`, `g` are redacted**; their `annotation_text` is recorded as
  "Redacted in source (CCI — confidential commercial information)." The markers themselves are intact in
  the grid: `d` on Supine vital signs, `g` on Rosuvastatin PK samples, `f` on the Day-6 PK cell.

Nothing was fabricated for the redacted content — names/marks/footnote text left empty or flagged as CCI.

## Legend value "P"

The legend defines **P = predose**. Per the extraction rules, in-grid `P` (e.g. 12-lead ECG, Clinical
laboratory tests) and `P, 2 h` / `P, 0.5, 1, … 12 h` are kept as **cell values**, not annotations.
Cells also carry postdose-hour timepoints ("24 h", "48 h", "120 h", etc.) as literal cell text.

## Synthesized values

- Both header-row `property_name`s synthesized ("Study Phase" L1, "Study Day" L2) — the col-1 label
  cell holds the activity-column header "Study Procedure", not a header-row name.
- Abbreviation list captured as one `abbreviation` annotation anchored nominally to `schedule_property`
  row 1 (no in-cell marker).

## Low-confidence / judgement calls

1. **"Treatment Period (Study Days)" span.** Modeled as merged cols 3–21 (Day -1 … Day 18), matching the
   drawn header; Day -1 (admission) is thus grouped under the treatment period as the source presents it.
2. **Header footnote locations.** `a` (Day 18) and `b` (FU/ED) placed on their specific header grid cells
   via `schedule_cell` locations; `f` placed on the Day-6 PK-sample cell (row 23, col 9).
3. **PK-sample cell text.** The Day-1/6/13 rosuvastatin PK cells hold the full serial-sampling list
   "P, 0.5, 1, 1.5, 2, 2.5, 3, 4, 5, 6, 8, 12 h" transcribed verbatim as one cell value; the follow-on
   day columns hold single postdose hours (24 h … 120 h).
4. **CCI activity rows** kept in sequence (rows 22, 24) to preserve row order and downstream
   traceability, even though their content is redacted.
5. **Indentation.** Flat activity list; all `indentation_level = 1` (no section headers present).

## Orphan risk

None. All 8 annotations carry ≥1 `marker_location`; every marker used in a cell or on an activity name
(`a, b, c, d, e, f, g`) resolves to a defined annotation (verified programmatically). The two CCI activity
rows carry no markers by design.
