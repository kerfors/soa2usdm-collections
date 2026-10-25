# NCT04529096 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT04529096_soa.pdf (4 PDF pages = document pages 9-12 per PAGEMAP.md; printed footers read 8-11, one lower, and were ignored). Document: ISA H0P-MC-BP01, original protocol, 11-May-2020 (CT.gov Prot_000.pdf). No protocol markdown; all text from the PDF text layer, checked against the rendered pages.

## Decisions needed (2)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | pp.10-11, rows 12-15, note n8 | "If the participant discontinues after V801, this assessment is not needed for ED per the CPMP SoA." is bound to the activity rows (Rescue medication usage reporting, NRS, PGI, RMDQ). It is a condition on the ED mark of the master CPMP SoA; these rows have no ED mark in this table. | Bind it to the empty ED cell (column 10) of each row, or record the location as `unresolved` because its target is in another document. | Cross-document notes |
| D2 | p.12, rows 27-28, note n16 | "V3-V6: collect sample before IV infusion starts." (ADA and Epiregulin Notes cells, identical text, one annotation) is bound to the marked cells V3, V4, V5 of both rows. Neither row has a mark at V6, although the note names V3-V6. | Bind the note to the whole row of each activity, or to V3-V6 including the unmarked V6 cell. | Notes naming visits |

## Recorded, not open (9)

- One table (taxonomy): title band and header reprint on pp.10-12, no new number or caption.
- §4 row_position: the title band 'H0P-MC-BP01 ISA' printed inside the frame is table_title, not a row; row 1 is the epoch band. (Pilot OA01 printed the same text in the epoch band's label cell; there the section heading was the title.)
- De-duplication (§1b): title band, header rows and the Notes cell of the window row counted once.
- Leading label columns (§5): L = 1; V3-V7 -> 2-6, V801-V803 -> 7-9, ED -> 10.
- §3 Header bands — typing: row 1 typed `epoch` by its spanning values (Double-Blind Treatment, Follow-up); single-column "Randomization" (V3) and "Early Discontinuation" (ED) stay values of the band. Name synthesised: the label cell is blank.
- §3 window row: "Visit Window (days)" typed `window`, level null.
- §2 table type: `main_soa` — every participant randomised into this ISA passes through it in sequence. See section 6 for what the type does not say.
- §6 Binding by what a note names: n3 (V803, ED), n5 (V3-V6), n12 (V3), n13 (V3-V6), n14 (V7-V803), n17 (V7-V803) split from their cells and bound to the named cells (`text_match`); the rest of each cell stays on the row (n2, n4, n11, n15). n16 is the whole text of the ADA cell and the first sentence of the Epiregulin cell; deduplicated by text (§6) into one note with locations on both rows.
- §6 Text above the frame: the intro paragraph on p.9 is the table-wide note g1 (line-end hyphen in 'H0P-' + 'MC-BP01' kept, joined without a space).

## 1. Table summary

- Table 01: `main_soa`, "H0P-MC-BP01 ISA" (ISA Section 1.3 Schedule of Activities (SoA)), document pages 9-12.
- 9 data columns (positions 2-10), 4 header rows (levels 1-3, window null).
- 24 activity rows (row positions 5-28), flat (`assumed_flat`, no group bands).
- 96 X marks, 18 annotations (g1, n1-n17).
- Rows per page: p.9 = 0 (section heading and intro text only, g1), p.10 = 10, p.11 = 11, p.12 = 3 (plus redacted rows, section 4).

## 2. Merged marks

- No body mark spans columns. Header spans: row 1 "Double-Blind Treatment" 3:6, "Follow-up" 7:9.

## 3. Mechanical mark-check

- bbox binning, column centres from the Visit Number row on each page. X count per page equals the extraction (p.10 34, p.11 38, p.12 24); per row the columns match the visual read. Cell boundaries from raster rule lines (§1d) agree.

## 4. Redaction

- p.12: a CCI bar covers the label and schedule cells (V3-V803 and the left part of ED) below "Epiregulin sample". The Notes column shows two empty ruled cells there, so two rows are probably hidden; the visible part of their ED cells is blank. Not transcribed; row positions after 28 are not assigned.

## 5. Notes that are similar on purpose

- n13 ("V3-V6: collect samples ... and within 15 minutes after ...", PK) and n16 ("V3-V6: collect sample before IV infusion starts.", ADA/Epiregulin) differ in wording; separate notes.
- n14 ("... at any time.") and n17 ("... at any time to match PK.") differ in wording; separate notes.
- Containment: none of the annotation texts is contained in another.

## 6. Delta schedule: what this table is, and what the schema cannot say

- The intro text (g1) says the SoA shows only what is unique to the ISA and points to the master and the CLBP DSA H0P-MC-CPMP(2) SoAs. The participant's full schedule is the union of three SoAs; the DSA is not public.
- Visits V3-V7 and ED reuse the master's visit numbers and weeks; V801-V803 are added by the ISA.
- n8 overrides the master's ED mark for four assessments (D1); RMDQ (the CLBP instrument) takes the place OA01 gives WOMAC. The extraction has no way to point at a cell in another document, so the override is held as text on the row.
- None of this is captured as data by `table_type` or by any extraction field; it is recorded in `table_metadata.notes` and in g1.

## 7. Method provenance

- Default methods throughout (text layer, bbox). Indentation `assumed_flat` on every row. Synthesised markers g1, n1-n17 (`synthesized` / `text_match` as listed in Recorded, not open). No `unresolved` locations.
