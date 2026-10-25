# NCT04476108 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT04476108_soa.pdf (4 PDF pages = document pages 10-13 per PAGEMAP.md; printed footers read 9-12, one lower, and were ignored). Document: ISA H0P-MC-NP01 amendment (a), 27-Mar-2020 (CT.gov Prot_000.pdf). No protocol markdown; all text from the PDF text layer, checked against the rendered pages.

## Decisions needed (2)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | pp.11-12, rows 12-15, note n8 | "If the participant discontinues after V801, this assessment is not needed for ED per the CPMP SoA." is bound to the activity rows (Rescue medication usage reporting, NRS, PGI, BPI). It is a condition on the ED mark of the master CPMP SoA; these rows have no ED mark in this table. | Bind it to the empty ED cell (column 10) of each row, or record the location as `unresolved` because its target is in another document. | Cross-document notes |
| D2 | p.13, rows 27-28, notes n16, n17 | "V3-6: collect sample before IV infusion starts" (ADA) and "V3-6: collect samples before IV infusion starts." (Epiregulin) are bound to the marked cells V3, V4, V5. Neither row has a mark at V6, although the notes name V3-6. | Bind each note to the whole row, or to V3-V6 including the unmarked V6 cell. | Notes naming visits |

## Recorded, not open (8)

- One table (taxonomy): header reprints on pp.11-13, no new number or caption.
- De-duplication (§1b): header rows and the Notes cell of the window row counted once.
- Leading label columns (§5): L = 1; V3-V7 -> 2-6, V801-V803 -> 7-9, ED -> 10.
- §3 Header bands — typing: row 1 typed `epoch` by its spanning values (Double-Blind Treatment, Follow-up); single-column "Randomization" (V3) and "Early Discontinuation" (ED) stay values of the band. Name synthesised: the label cell prints the table heading "H0P-MC-NP01 ISA".
- §3 window row: "Visit Window (days)" typed `window`, level null.
- §2 table type: `main_soa` — every participant randomised into this ISA passes through it in sequence. See section 6 for what the type does not say.
- §6 Binding by what a note names: n3 (V803, ED), n5 (V3-6), n12 (V3), n13 (V3-6), n14 (V7-V803), n18 (V7-V803) split from their cells and bound to the named cells (`text_match`); the rest of each cell stays on the row (n2, n4, n11, n15).
- §6 Text above the frame: the intro paragraph under the Section 1.3 heading on p.10 is the table-wide note g1.

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (ISA Section 1.3), document pages 10-13.
- 9 data columns (positions 2-10), 4 header rows (levels 1-3, window null).
- 24 activity rows (row positions 5-28), flat (`assumed_flat`, no group bands).
- 96 X marks, 19 annotations (g1, n1-n18).
- Rows per page: p.10 = 0 (Section 1.2 Schema figure, then the Section 1.3 heading and intro text, g1), p.11 = 10, p.12 = 11, p.13 = 3 (plus redacted rows, section 4).

## 2. Merged marks

- No body mark spans columns. Header spans: row 1 "Double-Blind Treatment" 3:6, "Follow-up" 7:9. The Urine Chemistries V803 mark sits low in its cell; it is in the V803 cell.

## 3. Mechanical mark-check

- bbox binning, column centres from the Visit Number row on each page. X count per page equals the extraction (p.11 34, p.12 38, p.13 24); per row the columns match the visual read. Cell boundaries from raster rule lines (§1d) agree.

## 4. Redaction

- p.13: a CCI bar covers the label and all schedule cells (V3-ED) below "Epiregulin sample". The Notes column shows two empty ruled cells there, so two rows are probably hidden. Not transcribed; row positions after 28 are not assigned.

## 5. Notes that are similar on purpose

- n16 ("V3-6: collect sample before IV infusion starts", no final period) and n17 ("V3-6: collect samples before IV infusion starts.") differ in wording and punctuation; per §6 they are two notes.
- n13 (PK, "... and within 15 minutes after ...") differs from n16/n17; separate notes. n14 ("V7 – V803: ... at any time.") and n18 ("V7-V803: ... at any time to match PK.") differ; separate notes.
- Containment: none of the annotation texts is contained in another.

## 6. Delta schedule: what this table is, and what the schema cannot say

- The intro text (g1) says the SoA shows only what is unique to the ISA and points to the master and the DPNP DSA H0P-MC-CPMP(3) SoAs. The participant's full schedule is the union of three SoAs; the DSA is not public.
- Visits V3-V7 and ED reuse the master's visit numbers and weeks; V801-V803 are added by the ISA.
- n8 overrides the master's ED mark for four assessments (D1); BPI (the DPNP instrument) takes the place OA01 gives WOMAC.
- None of this is captured as data by `table_type` or by any extraction field; it is recorded in `table_metadata.notes` and in g1.

## 7. Method provenance

- Default methods throughout (text layer, bbox). Indentation `assumed_flat` on every row. Synthesised markers g1, n1-n18 (`synthesized` / `text_match` as listed in Recorded, not open). No `unresolved` locations.
