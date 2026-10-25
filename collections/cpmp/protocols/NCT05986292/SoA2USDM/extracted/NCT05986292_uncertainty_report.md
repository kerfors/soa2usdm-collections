# NCT05986292 — SoA extraction uncertainty report

Prompt 3.9.4, single pass. Source: NCT05986292_soa.pdf (7 PDF pages = document pages 17-23 per PAGEMAP.md; printed footers read 16-22, one lower, and were ignored). Document: master protocol H0P-MC-CPMP amendment (a), 11-Dec-2020 (CT.gov Prot_005.pdf). No protocol markdown was made (pymupdf4llm is not installed on this machine), so all text comes from the PDF text layer, checked against the rendered pages.

## Decisions needed (2)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.18, row 5 (Procedure/Assessments) | The bold grey band "Procedure/Assessments", printed once under the header, is a group header (level 0) over rows 6-21, like the later bands "Scales, Questionnaires, and Outcome Measures" and "Clinical Laboratory Tests". | Treat it as the heading of the activity column (row 5 left out), and put rows 6-21 at level 0. | Hierarchy |
| D2 | p.18, header row 2 | The band "Screening Visit for Master / Pre-Randomization Screening Visit / Randomization to ISA / (blank V4-V7) / Early Discontinuation" is a visit-name row: type `visit`, level 2. | Type it `other` (visit description) with level null, since Visit Number alone tells the columns apart. | Header rows |

## Recorded, not open (9)

- One table (taxonomy, "a page break inside ONE printed table is not a continuation"): the header reprints on pp.18-22 with no new number or caption.
- De-duplication (§1b): header rows and the Notes cell of the Visit Number row reprint on every page and are counted once.
- Leading label columns (§5): L = 1; V1-V7 map to positions 2-8, ED to 9. The Notes column is not a data column.
- §3 Header cells / synthesised names: row 1 has a blank label cell (name "Epoch"); row 2's label cell prints the table heading "H0P-MC-CPMP Master Protocol" (name "Visit").
- §6 Notes column: each note cell is one `footnote` with a synthesised marker (n1-n19), bound to the row it sits beside.
- §6 Binding by what a note names: n10 (VAS: "V2 procedure should be repeated if >60 days from V1") bound to the VAS V2 cell; n13 ("At V1, use the C-SSRS Baseline version.") split from the C-SSRS note and bound to the V1 cell, rest (n14) on the row; n18 ("V2 test is optional ...") split from the serum pregnancy note and bound to the V2 cell, rest (n17) on the row. All `text_match`. V1 in n2, n9 and n19 is a reference point, not a condition, so those stay on the row.
- §6 Deduplicate by text: "Post randomization collections will be defined in each respective ISA." is one annotation (n16) with 7 locations (Hematology, Chemistry, Lipid panel, Urinalysis, TSH, HbA1c, Exploratory biomarker samples).
- §6 Text above the frame: the sentence on p.17 is the table-wide note g1.
- §4 Hierarchy, printed signals only: "Alcohol consumption" (p.21) and "Recreational drug use" (p.22) carry no own band, so they stay in the open "Clinical Laboratory Tests" group (level 1).

## 1. Table summary

- Table 01: `main_soa`, "Schedule of Activities (SoA)" (Section 1.3), document pages 17-23.
- 8 data columns (positions 2-9), 4 header rows (levels 1-4).
- 50 activity rows (row positions 5-54): 5 group bands at level 0, 45 activities at level 1.
- 151 X marks, 24 annotations (4 printed markers a-d, g1, n1-n19).
- Rows per page: p.17 = 0 (intro sentence only, captured as g1), p.18 = 8, p.19 = 12, p.20 = 10, p.21 = 12, p.22 = 8, p.23 = 0 (abbreviations and footnotes a-d only).

## 2. Merged marks

- No body mark spans columns. Header spans: row 1 "Screening" 2:3, "Double-Blind Treatment Period" 4:8.

## 3. Mechanical mark-check

- Text-layer bbox binning: column centres from the Visit Number row, each X token binned to the nearest centre and assigned to its label row. Per page the X count equals the extraction (p.18 9, p.19 49, p.20 53, p.21 32, p.22 8); per row the columns match the visual read.
- Footnote d: the superscript `d` tokens next to V2 marks were counted per page (4, 4, 7, 2) and match the 17 cells carrying marker d. Serum pregnancy V2 has no d.

## 4. Annotation text integrity

- p.22 has a glyph-spread text layer. Labels on that page (rows 47-54) were rebuilt from glyph tokens (`activity_name_source.method: glyph_reconstruction`) and checked against the rendered page. The two n16 occurrences on p.22 read the same as the five on pp.20-21 on the rendered page.
- No containment pairs: n13/n14 and n17/n18 are deliberate splits of one cell each (§6), together the full printed text.

## 5. Redaction

- p.22: a CCI bar below the "Participant Device" band covers the full table width. It may hide one or more activity rows (device-related, by the band name). Nothing was transcribed for them; "Participant Device" is kept as a mark-free group row with no visible children.

## 6. Method provenance

- Activity names rows 47-54: `glyph_reconstruction`.
- Marker locations: n-notes and g1 `synthesized` (by position); n10, n13, n18 `text_match`.
- Header rows 1-2: `structure_method: inferred_from_layout` (synthesised names).
- No `unresolved` locations.

## 7. Notes that refer to other layers (for the CPMP study)

These are transcribed literally; they are listed because they state how the master SoA relates to the DSA/ISA layers.

- g1 (above the table): the SoA holds the activities common to all disease states and interventions.
- n1 (Visit Number row): visits after V7 may be specified in the ISA.
- n7 (ECG): post-randomization ECGs are in each ISA.
- n16 (7 lab/sample rows): post-randomization collections are defined in each ISA.
- n2 (Informed consent for ISA): ISA consent timing depends on ISA availability.
- No note in this table names the disease-state addendum (DSA).
