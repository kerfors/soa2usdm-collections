# NCT04677179 — SoA extraction uncertainty report

Source: NCT04677179_soa.pdf (Protocol J1P-MC-KFAH(b)), 30 PDF pages = document pages 17–46 (PAGEMAP.md). Printed footers run one lower than the document page and were not used. No protocol markdown was available.
Prompt 3.8.1, single pass. Four tables extracted.

## Decisions needed (13)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p17–23, Table 1 row 5 (same in Tables 2–4) | The shaded "Fasting visit" band (X at the fasting visits, reprinted on every page) is a header schedule_property, not an activity | Record "Fasting visit" as a level-0 activity row with X marks | T1.3 |
| D2 | p17, Table 1 Visit number row, marker t1 | Title-cell note "V1 and V9 procedures may be conducted over more than 1 day…" bound to the V1 and V9 header cells | Treat as a table-wide note on the Visit number row | T1.5 |
| D3 | p17, Table 1 rows 6–17 (and top rows of Tables 2–4) | Rows above the first section header (Informed consent … Alcohol and caffeine use; Concomitant medications … Tobacco use in Tables 2–4) recorded at indentation level 0 as stand-alone activities | Level 1 under an implicit unnamed group | T1.4 |
| D4 | p24, Table 2 | Typed `track`, track_label "Responders" (schedule applies only to responders at Week 12; exclusive with Table 3) | `main_soa` as an independent maintenance schedule | T2.1 |
| D5 | p28, Table 2 (continued) | "Table 2 (continued)" (V20–V29, p28–32) treated as the second column tile of one tiled Table 2; rows unioned by name, marks merged; redacted "CCI" rows matched across tiles by position between the same neighbours | Separate continuation table, or a different matching of the redacted rows | T2.2 |
| D6 | p24/p28, Table 2 Visit number row, markers t5/t6 (and t2/t3) | Remote-visit note bound to the visit cells it names (V10–V19 tile 1 as t5; V20–V28 tile 2 as t6, V29 excluded). Tile variants that differ in wording/punctuation kept as separate notes (remote note: one comma; ETV cross-reference: "an ETV" vs "an early termination visit") | Merge the variant pairs, and/or bind to the whole Visit number row | T2.5 |
| D7 | p27, Table 2 "Endoscopic Procedure" header, marker c30 | Comment "Not applicable for responders during the time period of V10 through V19" bound to the section-header row (governs the group) | Bind to the Endoscopy and Colon biopsy rows, or to the V10–V19 columns | T2.5 |
| D8 | p33, Table 3 | Typed `track`, track_label "Nonresponders" (applies only to nonresponders at Week 12) despite identical V10–V29 numbering to Table 2 | `domain` of Table 2 or `main_soa` | T3.1 |
| D9 | p38, Table 3 (continued) | "Table 3 (continued)" (V20–V29, p38–42) treated as tile 2 of one tiled Table 3; rows unioned; "CCI" rows matched by position | Separate continuation table / different matching | T3.2 |
| D10 | p33/p38, Table 3 Visit number row, markers t5/t6/t7 | "Visits 17 through 28…" note bound to V17–V19 (t5) and V20–V28 (t6, tile-2 text lacks final period); "V16 procedures…" note bound to the V16 cell (t7) | Table-wide notes on the Visit number row; merge the two remote-visit variants | T3.5 |
| D11 | p43, Table 4 | Typed `main_soa` (independent schedule, own columns ETV/V997/V801/V802, applies to all participants) | `track` (e.g. "Follow-Up") as a separate study-phase timeline | T4.1 |
| D12 | p43, Table 4 Visit number row, markers t1/t2 | "V802 is only for randomized participants who were positive for anti-HBc…" bound to the V802 cell; "V997: Additional study procedures…" bound to the V997 cell | Table-wide notes on the Visit number row | T4.5 |
| D13 | p43, Table 4 Vital signs, marker c4 | Vital-signs comment (ending "V997: Vital signs collection is optional.") kept as one footnote on the row | Split the V997 sentence into a note on the Vital signs × V997 cell | T4.5 |

## Recorded, not open (8)

- §6 abbreviations: the abbreviation blocks under Tables 1, 2, 3 and 4 carry no in-grid markers → zero abbreviation annotations emitted.
- §6 Notes/Comment column: every non-empty Comment cell → one `footnote` (or `source_note` for the bare "See Section 8.2.2."), deduplicated by text within each table, with synthesised markers `c1…` (activity_name / schedule_property locations, method `synthesized`).
- §6 title-cell notes: the instruction lines inside each table's title cell are emitted as annotations with synthesised markers `t1…`; pure cross-references ("For procedures at an ETV, see ETV in Table 4." etc.) typed `source_note`; population statements and cross-references with no single visit bound to the Visit number schedule_property (method `synthesized`).
- §6 redacted content: label cells covered by a "CCI" redaction box are transcribed with activity_name "CCI"; their marks are read normally. Row count under each redaction box taken from the horizontal rules in the mark columns (e.g. p23 and the Stool Samples groups: one box spans two ruled rows → two activities). No redaction appears in a Comment column.
- §5 label columns: L = 1 in all tables (activity/property label column); first data column position 2. The right-hand Comment column is a notes column, excluded from the grid.
- §5 merged cells: none. No mark, arrow or text cell spans more than one visit column in any table; no header cell is merged.
- §1c glyph-spread text: see "Annotation text integrity"; activity names and notes on raster pages rebuilt and flagged with `glyph_reconstruction` / `deglyph_reconstruction`.
- §2 tiling vs continuation: "Table 2 (continued)" and "Table 3 (continued)" reprint the same body rows under different visit columns, so per the type definitions they are tiles of Tables 2 and 3, not `continuation` tables (the call itself is open as D5/D9).

## Method (all tables)

- PDF pages 1, 3, 4, 9, 13, 16, 18, 23, 26, 30 (doc 17, 19, 20, 25, 29, 32, 34, 39, 42, 46) are vector text pages. All other pages are a full-page 150-ppi raster image with a glyph-spread text layer on top (no vector rule lines).
- Mechanical mark check (§1b): `pdftotext -bbox`, X tokens (x > 250 pt, excluding the Fasting-visit band) binned to header column centres. Totals: T1 158, T2 180, T3 211, T4 54, equal to the `activity_schedule` counts. Lowercase "x" tokens in the bbox stream were fragments of words (e.g. "exam") in label/comment columns and were excluded by position.
- Visual diff: every raster page and most vector pages rendered at 110 dpi and compared cell by cell with the bbox matrix. **No disagreement found.** Row and cell bounds on raster pages were confirmed visually against the rendered grid rather than with an automated §1d rule-line detector; recommend a spot-check of the resolved grids for the multi-row comment cells.
- Indentation: shaded bold section headers = level 0 (`font_signal`); rows under them = level 1; stand-alone rows above the first header = level 0 (`visual_estimate`, D3).

## Table 1 — Screening and Induction (doc 17–23)

- T1.1 `main_soa`. 9 data columns (V1–V9, positions 2–10). 5 schedule properties, 63 activity rows (8 section headers), 158 marks, 32 annotations.
- T1.2 Activity rows per page: 17: 12 · 18: 8 · 19: 6 · 20: 13 · 21: 7 · 22: 9 · 23: 8. Every page contributes.
- T1.3 Properties: Visit number (visit, 1), Weeks from randomization (week, 2), Study day (study_day, 3), Visit interval tolerance (window, 4), Fasting visit (other, null; D1). No synthesised property names.
- T1.4 Hierarchy: 12 top rows at level 0 (D3); six redacted "CCI" rows (p18 ×1, p21 ×1, p22 ×2, p23 ×2), all level 1 under their section header.
- T1.5 Annotations: t1 (V1/V9 cells, `text_match`; D2), t2/t3 source notes on the Visit number row; c1–c29 Comment-column notes. c23 (PK collection timing) binds two rows (PK samples and the following CCI row) — two separate cells with identical text, deduplicated.
- Containment pair: c16 (Serum pregnancy) is the opening sentence of c17 (Urine pregnancy). Re-verified on p20/p21: two separate rule-bounded cells on different rows — **source-faithful**, not a split note.

## Table 2 — Maintenance, responders (doc 24–32)

- T2.1 `track`, track_label "Responders" (D4). 20 data columns (V10–V29, positions 2–21). 40 activity rows, 180 marks, 19 annotations.
- T2.2 Tiled (D5): tile 1 = doc 24–27 (V10–V19), tile 2 = doc 28–32 (V20–V29). Rows present in both tiles carry the tile-1 source_page. Rows printed only in tile 2: Review modified Mayo score (MMS) (p28); Diary return, Clinician-Administered Questionnaires (Paper), Physician's Global Assessment (PGA) (p29); Endoscopy, Colon biopsy sample collection (p31). Rows printed only in tile 1: none (the Endoscopic Procedure header's comment is tile-1 only).
- T2.3 Activity rows per page: 24: 10 · 25: 8 · 26: 6 · 27: 10 · 28: 1 · 29: 3 · 30: 0 · 31: 2 · 32: 0. Pages 30 and 32 are the declared tile exception, not skipped pages: p30 supplied the V20–V29 marks of Hematology, Clinical chemistry, Lipid panel, Urinalysis, Urine pregnancy, CCI and HBV DNA; p32 supplied Dosing V20–V28 (and the "No dosing at V29." note). Pages 28, 29, 31 additionally supplied the V20–V29 marks of all their other rows.
- T2.4 Genetics sample has no mark in either tile (comment only) — transcribed as unmarked.
- T2.5 Annotations: t1 population note, t2/t3 (two wordings of the ETV cross-reference, tile 1 / tile 2) and t4 on the Visit number row; t5/t6 remote-visit variants bound to cells (D6); c30 on the Endoscopic Procedure header (D7); c29 "No dosing at V29." on Dosing.
- Near-duplicate pair t5/t6: identical except for a comma after "thereof)"; source-faithful (each tile prints its own title cell).

## Table 3 — Extension Induction / Extension Maintenance, nonresponders (doc 33–42)

- T3.1 `track`, track_label "Nonresponders" (D8). 20 data columns (V10–V29, positions 2–21). 40 activity rows, 211 marks, 18 annotations. No epoch row separates Extension Induction from Extension Maintenance, so none is recorded.
- T3.2 Tiled (D9): tile 1 = doc 33–37 (V10–V19), tile 2 = doc 38–42 (V20–V29). Only "Diary return" (p39) is printed in tile 2 alone.
- T3.3 Activity rows per page: 33: 11 · 34: 7 · 35: 7 · 36: 8 · 37: 6 · 38: 0 · 39: 1 · 40: 0 · 41: 0 · 42: 0. Pages 38, 40, 41, 42 are the tile exception: p38 supplied V20–V29 marks for Concomitant medications through 12-lead ECG; p40 for the laboratory rows through HBV DNA; p41 for PK samples, CCI, Flow cytometry, CCI, Endoscopy, Colon biopsy and the two Stool-sample CCI rows; p42 for Dosing V20–V28.
- T3.4 Hematology/Clinical chemistry: no mark at V18 (verified on p35 render). Genetics sample unmarked in both tiles.
- T3.5 Annotations: t1 population note, t2 (ETV) and t4 (V997) on Visit number row (both tiles use the same ETV wording, so no t3); t5/t6 remote-visit variants (D10; differ only by the final period); t7 V16 multi-day note on the V16 cell.

## Table 4 — ETV, unscheduled, post-treatment follow-up (doc 43–46)

- T4.1 `main_soa` (D11). 4 data columns (ETV, V997, V801, V802; positions 2–5). 39 activity rows, 54 marks, 16 annotations.
- T4.2 Activity rows per page: 43: 15 · 44: 12 · 45: 10 · 46: 2 (Randomization and Dosing header + unmarked Dosing row).
- T4.3 Weeks-from-randomization cells transcribed with their line breaks joined ("58 or ETV + 8 weeks after last dose").
- T4.4 Header-row comments: c1 (visit-interval note) bound to the Weeks from randomization property row; c2 "No fasting in this period" to the Fasting visit row (no fasting marks in this table).
- T4.5 t1/t2 bound to V802/V997 cells (D12); vital-signs note kept whole (D13).
- Shared-run pair c12 (Endoscopy) / c13 (Colon biopsy): both open "Recommended at ETV based on judgment of the investigator and after discussion with the sponsor's medical monitor." and end with the same "If not performed at ETV…" sentence; re-verified on p45 — two separate rule-bounded cells on consecutive rows, **source-faithful**.

## Annotation text integrity

- Glyph-spread text layer on all raster pages (§1c). Reconstructed fields: activity names (`activity_name_source.method: glyph_reconstruction`) and Comment-column notes (`annotation_text_source.method: deglyph_reconstruction`) where every occurrence of the note is on a raster page. Where a note also prints on a vector page, text was taken from the vector page and no method is recorded. Words were re-segmented and checked against the rendered pages and against the same wording on the vector pages (e.g. "peripheral lymph nodes", which the text layer runs together as "peripherally mph").
- Title-cell notes were taken from the vector pages (the title cell reprints on every page).
- Diary-compliance note transcribed with its source line breaks (list of diary items).
- No note could not be bounded; every Comment cell is closed by a visible rule on the render.

## Orphan risk

- None: every annotation has ≥ 1 marker_location and every location's marker is present in the row's/cell's `annotation_markers`. No marker referenced without definition (the tables print no superscript markers at all; all markers are synthesised).
- Redacted "CCI" rows: the redaction could hide label text only; no evidence of hidden rows (ruled rows under each box are counted).

## Method provenance

- `activity_name_source.method: glyph_reconstruction` — every activity whose source_page is a raster page (doc 18, 21, 22, 23, 24, 26, 27, 28, 30, 31, 33, 35, 36, 37, 38, 40, 41, 43, 44, 45).
- `activity_name_source.indentation_method: font_signal` for headers and grouped rows; `visual_estimate` for the ungrouped level-0 top rows (D3).
- `annotation_text_source.method: deglyph_reconstruction` — raster-only Comment notes (listed per annotation in the JSON).
- `marker_locations.method: synthesized` — all Comment-column and table-wide title notes; `text_match` — title notes bound to the visit cells they name (T1 t1; T2 t5, t6; T3 t5, t6, t7; T4 t1, t2).
- No `proximity`, no `proximity_bounded`, no `unresolved` locations. No schedule_property `structure_method` recorded (all header rows carry printed labels).
