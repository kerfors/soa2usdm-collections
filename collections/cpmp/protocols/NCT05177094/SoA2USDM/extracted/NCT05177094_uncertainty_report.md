# NCT05177094 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT05177094_soa.pdf (2 PDF pages = document pages 11-12 per PAGEMAP.md; printed footers read 10-11, one lower, and were ignored). Document: ISA H0P-MC-NP02 amendment (a), 30-Sep-2021 (CT.gov Prot_000.pdf). No protocol markdown. Text from the PDF text layer, **glyph-spread on both pages** (rebuilt, §1c), checked against the rendered pages.

## Decisions needed (2)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.12, row 17 (Urine pregnancy), note n7 | "For all WOCBP at V3: collect sample before dosing." is one sentence naming V3, bound to the V3 cell only; the WOCBP restriction then reads as applying to the V3 sample only. | Split it: "For all WOCBP" on the whole row, "at V3: collect sample before dosing" on the V3 cell. | Notes naming visits |
| D2 | p.12, row 20 (PK sample), note n11 | "The date and time of LY3526318 administration prior to the PK sampling will be recorded, i.e., the dose administered in the clinic at V3 and the last at-home dose prior to each of V4, V5, V6, and V7." is split off and bound to the PK cells V3-V7 (text_match). | Keep it on the PK row: the visits are reference points for the dose times to be recorded, not a condition on the visits. | Notes naming visits |

## Recorded, not open (10)

- One table (taxonomy): header reprints on p.12, no new number or caption.
- De-duplication (§1b): header rows counted once.
- Leading label columns (§5): L = 1; V3-V7 -> 2-6, ED -> 7.
- §3 Header bands — typing: row 1 typed `epoch` by its spanning value (Double-Blind Treatment); single-column "Randomization to ISA" (V3) and "Early Discontinuation" (ED) stay values of the band. Rows 1 and 2 have blank label cells: names synthesised ("Epoch", "Visit").
- §3 window row: "Visit Window (days)" typed `window`, level null; its '±3' is one cell merged across V4-V7 (3:6).
- §2 table type: `main_soa` — every participant randomised into this ISA passes through it in sequence. See section 6.
- §4 Hierarchy — printed signals only: rows 5-9 precede the first section band and stay at level 0; "Scales, Questionnaires, and Outcome Measures" (row 10), "Clinical Laboratory Tests and Sample Collection" (row 12) and "Participant Device" (row 22) are full-width shaded bands, mark-free group rows at level 0; their visible rows at level 1 (`font_signal`). The laboratory group stays open across the page break (p.12).
- §4 row_position: each redacted region keeps one number (11, 21), unassigned; rows after them continue from it.
- §6 Binding by what a note names: n1 and n5 name V5, V7 and ED in every sentence and are bound to those cells; n2 (V3), n3 (V4-V7), n9 (V3), n10 (V4-V7) split from their cells and bound to the named cells (`text_match`); n4 stays on the row. "See Section 6.1." (n6) and "See Section 5.3.2." (n8) are bare pointers -> `source_note`.
- §6 Redacted content: the Notes cell of "Participant returns device" is wholly covered by a CCI box; n12 says so and carries no source text.

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (ISA Section 1.3), document pages 11-12.
- 6 data columns (positions 2-7), 4 header rows (levels 1-3, window null).
- 17 activity rows (row positions 5-10, 12-20, 22-23): 14 activities and 3 group rows.
- 57 X marks, 13 annotations (g1, n1-n12).
- Rows per page: p.11 = 9, p.12 = 8 (plus redacted rows, section 4).

## 2. Merged marks

- No body mark spans columns. Header spans: row 1 "Double-Blind Treatment" 3:6; row 4 "±3" 3:6.

## 3. Mechanical mark-check

- bbox binning, column centres from the visit row on each page (glyph-spread 'E D' merged before binning). X count per page equals the extraction (p.11 31, p.12 26); per row the columns match the visual read. Cell boundaries from raster rule lines (§1d) agree.

## 4. Redaction

- p.11: a tall CCI bar (about four single-line row heights) under the "Scales, Questionnaires, and Outcome Measures" band covers label, V3-ED and Notes: the whole content of that group is hidden (position 11). The abbreviation line lists NRS, which is not in any visible row.
- p.12: a CCI bar covers one full row below "PK sample" (position 21), and the Notes cell of "Participant returns device" (n12).
- The abbreviation line carries one CCI box.

## 5. Text-layer reconstruction (§1c)

- pp.11-12 are glyph-spread ('Physicalexa mination', 'Sy mpto m directed physicalexa m(see Section ...'). Rebuilt fields: every activity name (`glyph_reconstruction`), every Notes-cell annotation and g1 (`deglyph_reconstruction`), and the header cell values. Re-read as prose against the page; 'V4 -V7' in the stream is printed 'V4-V7'.

## 6. Delta schedule: what this table is, and what the schema cannot say

- The intro text (g1) points to the master and the DPNP DSA CPMP(3) SoAs. The participant's full schedule is the union of three SoAs; the DSA is not public.
- Compared with OA02 and BP02 (same compound, LY3526318): same activity rows and marks; NP02 adds the "Scales, Questionnaires, and Outcome Measures" band, whose rows are redacted. No follow-up visits and no cross-document ED override note.
- None of this is captured as data by `table_type` or by any extraction field; it is recorded in `table_metadata.notes` and in g1.

## 7. Method provenance

- `glyph_reconstruction` on every activity name; `deglyph_reconstruction` on g1 and n1-n11. Indentation `font_signal` (section bands). Synthesised markers g1, n1-n12. No `unresolved` locations.
