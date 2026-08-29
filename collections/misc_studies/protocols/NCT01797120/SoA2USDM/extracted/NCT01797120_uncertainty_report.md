# NCT01797120 — SoA extraction uncertainty report

Source: `NCT01797120_soa.pdf` (PrECOG PrE0102, protocol section "7 Study Parameters", doc pages 69–71 per PAGEMAP). One table extracted → `NCT01797120_Table_01_extraction.json`. Prompt v3.8.0, schema soa-table-extraction v1.0.

## Decisions needed (5)

| # | where | call made | alternative | detail |
|---|-------|-----------|-------------|--------|
| D1 | p.69, header rows 1–2, cols 7–9 | The header texts of the last three columns ("End of Induction Phase \<OR\> End of Treatment Section 5.6", "Continuation Phase (until progression or unacceptable toxicity)", "Follow-Up Off Therapy (every 3 months for total of 3 yrs from date of randomization)") are each ONE tall cell spanning both header rows; each is recorded once, on header row 1 (the phase row), leaving row 2 empty in those columns. | Record them on header row 2 instead, as visit-level labels parallel to "Pre-Study", or on both rows. The column each text (and its ^/£/o marker) binds to is unchanged either way. | 2 |
| D2 | p.69, above the table frame | The bold banner "TREATMENT MUST BEGIN ≤ 7 WORKING DAYS FROM RANDOMIZATION", printed between the section heading and the table, sits outside the table's ruled frame and is recorded only in `table_metadata.notes`, not as an annotation. | Treat it as a table-scope note: emit a footnote annotation with a synthesized marker anchored to header row 1. | 2 |
| D3 | p.69, header row 1, col 7 | "Section 5.6" is kept inside the column-7 header value, as printed. | Strip "Section 5.6" out of the header value and emit it as a separate source_note (cross-reference) bound to that header cell. | 7 |
| D4 | p.69, header row 2 | Header row 2 (Pre-Study / Cycle 1 Day 1 / Cycle 1 Day 15 / Day 1 Each Subsequent Cycle / Every 12 weeks) is typed `visit` — the values name the occasions on which procedures are performed. | Type it `timepoint` or `other`: the values mix a pre-study period, cycle-day timepoints and a frequency ("Every 12 weeks") rather than named visits. | 7 |
| D5 | p.69, header row 2 | The row's `property_name` is its printed column-1 label "Procedure", although that word labels the activity column rather than describing the row's schedule values. | Synthesize a descriptive name (e.g. "Visit / Occasion") and treat "Procedure" purely as the label-column heading. | 7 |

## Recorded, not open (9)

- One table spanning doc pages 69–71, not a `continuation`: rows run onto p.70 under a reprinted header with no separate table number/caption (taxonomy v6, continuation rule).
- Reprinted header rows on p.70 deduplicated; each activity and mark counted once (§1b).
- "(see Appendix C)" stripped from the "Concomitant Meds" label into a `source_note` with synthesized marker `pr1` on the citing activity (§6 inline-reference rule); raw label preserved in `cell_text`.
- "(includes Ht)" kept literally in the Vital Signs Pre-Study cell (`cell_value` "X (includes Ht)") — a qualifier not expressible as columns (§5 qualified marks).
- Header-cell markers placed on that column's `schedule_grid` cell, cleaned out of `cell_value` (§6): `*` on cols 3 and 5 (row 2), `^` on col 7, `£` on col 8, `o` on col 9 (row 1).
- Superscript on "Follow-Up Off Therapy" read as footnote letter `o`, not a degree symbol: the bbox token is the letter "o", footnote o is defined on p.71, and no "°" definition is printed anywhere.
- Marker `n` printed on the activity label "PFTs with DLCO as medically indicatedⁿ" goes on that activity's `annotation_markers` (§6); the row carries no marks, which is data (§1).
- No abbreviation list and no X-legend are printed with the table, so no `abbreviation`/`legend` annotations exist (§6).
- Doc page 71 is inside the declared page range but contributes no activity rows — it carries only footnotes k–o and *, ^, £ (§4).

## 1. Table summary

- **Table 1** — `table_type: main_soa` (the only schedule grid in the excerpt; primary anchor). Title from the section heading "Study Parameters"; the table itself has no printed number or caption.
- Columns: 9 physical columns; **L = 1** label column ("Procedure"), so data columns are positions **2–9** (first data position 2): 2 Pre-Study, 3 Cycle 1 Day 1, 4 Cycle 1 Day 15, 5 Day 1 Each Subsequent Cycle, 6 Every 12 weeks, 7 End of Induction Phase \<OR\> End of Treatment, 8 Continuation Phase, 9 Follow-Up Off Therapy.
- Header rows: 2 (row 1 epoch band, row 2 visit/occasion labels). Activities: 20 (rows 3–22), all flat (indentation_level 0, uniform text-layer left edge; no section-header rows). Marks: 71. Annotations: 19 (18 footnotes + 1 source_note).
- Activity rows per page: p.69 → 15 rows (rows 3–17), p.70 → 5 rows (rows 18–22), p.71 → 0 rows (footnotes k–o, *, ^, £ only — declared exception, see "Recorded, not open").

## 2. Header structure and merged cells

Rule lines recovered from the 150 dpi raster (§1d; the method also reads vector rules) on both p.69 and p.70 give identical geometry: full-height column boundaries at x ≈ 17.8 / 208.3 / 279.4 / 349.9 / 421.0 / 491.5 / 562.6 / 620.6 / 688.3 / 760.3 pt.

- **Header row 1**: within the row-1 band the only internal boundaries are at 208.3, 562.6, 620.6 and 688.3 pt — so one cell spans x 208.3→562.6, i.e. data columns **2–6 inclusive**: the "Induction Phase (fulvestrant + everolimus or placebo for a maximum of 12 cycles)" band **includes the Pre-Study column**. This is mechanically confirmed from the rule lines, not an interpretation; the merge is recorded on each of columns 2–6 with `merged_cell_range` "2:6".
- **Columns 7–9 header cells** have no internal horizontal rule between header rows 1 and 2 — each is one cell vertically merged across both header rows. The schema cannot express vertical merges, so each text is recorded once on row 1 (decision D1) and the vertical merge is stated in `table_metadata.notes`.
- The banner "TREATMENT MUST BEGIN ≤ 7 WORKING DAYS FROM RANDOMIZATION" sits at y ≈ 98–115 pt, above the table's top rule at y ≈ 120 pt — outside the frame (decision D2).

## 3. Merged-mark decisions

None in the body: every one of the 71 marks sits in a single-column cell (all 9 column boundaries are present through the body bands on both pages; no `source_range` used anywhere). The only merged cell in the table is the header "Induction Phase" band (columns 2:6, §2 above). No arrows, no vertically-merged marks.

## 4. Synthesised names and markers

- `property_name` "Study Phase" for header row 1 (label cell empty; `property_name_source.synthesized: true`).
- Synthesized marker `pr1` (source_note "see Appendix C") on activity row 7, Concomitant Meds — the only synthesized annotation marker.

## 5. Mechanical mark-check

Method: §1b bbox column-binning. `pdftotext -bbox`; column x-centres fixed from the header labels of the 8 data columns; mark tokens matched with `^[Xx][*a-zA-Z0-9]?$`. In this PDF every superscript footnote letter tokenizes as a **separate** small-height token (h ≈ 8.2 pt vs 12.3 pt for X) printed above/right of its X (e.g. "a,i", "c,d", "i,m,o", "f,g"); each was bound to its mark by coordinates. Two stray prose tokens "a" from the p.70 footnote paragraphs fall below the table body band and were excluded by the y-range.

Result: mechanical matrix = 71 marks (60 on p.69, 11 on p.70); diffed cell-for-cell against the visual read of the rendered pages: **zero disagreements** — including the empty cells (e.g. Adverse Events has no Pre-Study mark; Fasting Glucose/Lipid have no Cycle 1 Day 15 mark; PFTs row has no marks at all). Lowercase `x` never occurs; all marks are uppercase X as recorded.

## 6. Annotation text integrity

- The text layer is clean (whole-word tokens, no glyph-spread); no fields were reconstructed (§1c not applicable). Footnote wording taken from the text layer with line-wraps joined; the p.70 line-break inside "HCV RNA-\nPCR" (footnote h) is a hyphenated compound and was joined as "HCV RNA-PCR", matching its spelling elsewhere. Curly quotes in footnote h ("'cured'") preserved.
- Containment/overlap pairs among annotations: none. Footnote n opens with the full text of activity row 22's label and footnote l with the text of row 21's label — annotation-vs-activity-label overlap, verified source-faithful against the rendered pages (the footnotes genuinely restate the activity names), not a split note cell.
- No notes column exists; all footnotes are bounded paragraphs below the table, each starting at its printed marker — none were hard to bound.

## 7. Low-confidence calls

- **D3** — "Section 5.6" left inside the col-7 header value. The §6 inline-reference rule is written for activity labels; extending it to header cells would emit a source_note instead.
- **D4** — property_type of header row 2: `visit` chosen over `timepoint`/`other` (mixed value kinds).
- **D5** — property_name "Procedure" for header row 2, taken from the printed label though it names the activity column.
- table_type `main_soa` is not considered low-confidence: it is the only grid, and rows are clearly subject procedures.
- No PDF/markdown disagreements (no markdown supplied).

## 8. Orphan risk

None. All 19 annotations have ≥ 1 `marker_locations` entry; every marker printed in the table (a–o, *, ^, £) has its definition printed in the source (a–j on p.70, k–o and *, ^, £ on p.71); no marker is referenced-but-undefined and no location is `unresolved`. `annotation_markers` and `marker_locations` were cross-checked mechanically in both directions: exact agreement.

## 9. Method provenance

- `schedule_grid` row 1, columns 2–6: `method: "raster_pixel_detection"` — the cell text comes from the text layer, but the merged span 2:6 was established from rule lines recovered from the rendered page (§1d), since `pdftotext -bbox` carries no vector data. This is the only non-default method recorded in the JSON.
- The vertical merge of the col 7–9 header cells (D1) was likewise established from raster rule lines; the schema has no field for vertical merges, so it lives in `table_metadata.notes`.
- No `unresolved` marker locations; no `proximity`/`text_match` bindings; the two synthesized elements are listed in §4.
