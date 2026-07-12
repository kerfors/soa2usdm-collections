# NCT04004988 — SoA Extraction Uncertainty Report

Protocol I8F-MC-GPGS (tirzepatide / LY3298176), clinical-pharmacology 2-period crossover.
Extractor: Claude Opus 4.8 (Cowork), single-pass v3.0 path. Marks are PDF-authoritative
(read from the PDF text-layer word coordinates, poppler `pdftotext -bbox`), text from markdown.

## Tables

- **Table 01 — `main_soa`**, protocol page 10. 15 grid columns (14 timepoint columns + Comments
  excluded), 17 activities, 70 schedule marks, 12 annotations.
- **Table 02 — `continuation` (continuation_of = 1)**, protocol page 11. Same header repeated,
  rows continue (Immunogenicity, Blood glucose monitoring, PK Sampling). 3 activities, 20 marks,
  9 annotations. Classified `continuation` because the column structure is byte-identical to
  Table 01 and the header row is physically repeated — rows simply carry on.

## SOURCE ISSUE (RESOLVED) — read this first

> **Resolved 2026-07-12:** `NCT04004988_soa.pdf` has been regenerated to the complete 3-page
> SoA source (protocol printed pages 9–11), so it now includes the page-11 continuation. The
> description below reflects the original 2-page excerpt as it was at extraction time.

**`NCT04004988_soa.pdf` is incomplete.** It is a 2-page excerpt (page 9 section title + page 10
table) and **omits protocol page 11**, which holds the continuation rows (Immunogenicity, Blood
glucose, PK Sampling) plus all footnote definitions (a, b, c), the abbreviation list and the two
general Notes. Page 11 was therefore sourced from the **full protocol PDF `NCT04004988.pdf`
(PDF page 12)**. If the `_soa.pdf` excerpt is meant to be self-contained, it should be
regenerated to include page 11.

## PDF vs markdown disagreement (important)

The protocol markdown pipe-table grid is **column-shifted for some rows** and cannot be trusted
for mark placement. Clearest case: **Outpatient Visit** — markdown places the 8 X's at D4–D36
(ED blank); the PDF coordinates place them at **D5, D6, D7, D8, D15, D21, D36, ED**. Other rows
(e.g. Safety 12-lead ECG) matched between the two. All marks in the JSON were taken from PDF
word x-coordinates, mapped to columns using the day-header word positions as column centers.
Markdown was used only for activity-name spelling, footnote/abbreviation text, and the Comments
column.

## Column model (both tables)

`1` Procedure(label, excluded) · `2` Screening=D-28 to D-2 · `3` D-1 · `4` D1 · `5` D2 · `6` D3 ·
`7` D4 · `8` D5 · `9` D6 · `10` D7 · `11` D8 · `12` D15 · `13` D21 · `14` D36 (±1) ·
`15` ED (separate column, **no day label**; header "ED" sits in the epoch row) · `16` Comments (excluded).

## Merged-cell decisions

- **Epoch header "Periods 1 and 2 Study Days – …"** is a merged header spanning **cols 3–14**
  (D-1 … D36). Distributed to 12 `schedule_grid` cells, each `is_merged_cell=true`,
  `merged_cell_range="3:14"`. ED (col 15) is its own column, outside this span.
- No merged marks in the table body (each activity mark sits in a single column).

## Synthesized values

- **`property_name` row 1 = "Study Phase"** (label cell blank; epoch phases Screening / Periods 1&2 / ED).
- **`property_name` row 2 = "Study Day"** (`synthesized=true`). The physical col-1 cell holds
  **"Procedure"** (the activity-column header, recorded in `property_name_source.cell_value`),
  not a day-row label, so the day-row name was synthesized.
- **Comments column → synthesized footnote markers `n1…n9` (Table 01) and `n1…n3` (Table 02).**
  Each non-empty Comments cell became a `footnote` annotation linked to its activity row, and the
  synthesized marker was added to that activity's `annotation_markers`.
- **Table-wide annotations on page 11 with no in-cell marker** — the abbreviation list (`abbr`,
  type `abbreviation`) and the two general Notes (`note1`, `note2`, type `footnote`) are anchored
  nominally to `schedule_property` row 1 (there is no specific cell they mark). Flagged for review.

## Low-confidence / judgement calls

1. **Epoch row `property_type`.** Row 1 mixes an epoch (Screening), a period-level label
   ("Periods 1 and 2 Study Days …") and an epoch (ED) on one row. Classified **`epoch`** as the
   coarsest phase level; a case could be made for `period`.
2. **Header-cell footnote locations (b, c).** Per-timepoint header markers were placed on the
   specific `schedule_grid` header cell (b → row 2 cols 3, 4, 14; c → row 1 col 15) and the
   annotation `marker_locations` use `location_type="schedule_cell"` with the header row/column to
   preserve which column each governs. Note `schedule_cell` is nominally an activity×timepoint
   type; used here for header grid cells as the only location type that carries a column.
3. **Continuation duplicate headers.** Table 02 repeats the full header (schedule_properties +
   grid) since it is physically present on page 11. If consolidation dedupes headers by
   `continuation_of`, this is expected; if not, headers may double-count.
4. **a/b/c texts in Table 01.** The footnote *definitions* live on page 11, but their markers
   appear in Table 01 (ECG^a, Pharmacogenetic^a, D-1^b, D1^b, D36^b, ED^c). Full texts were
   included in Table 01's annotations to keep the file self-contained.
5. **Indentation.** No section headers or sub-grouping exist; all activities are a flat list,
   assigned `indentation_level = 1` (procedure level), consistent with prior verified extractions.

## Orphan risk

None. All 12 (Table 01) / 9 (Table 02) annotations carry ≥1 `marker_locations`, and every marker
appearing in a cell (`a, b, c, n1…n9`) resolves to a defined annotation (verified programmatically).
