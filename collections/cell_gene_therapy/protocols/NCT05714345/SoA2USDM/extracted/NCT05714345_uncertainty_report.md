# NCT05714345 — Layer 1 extraction uncertainty report

Allogene ALLO-647-201 Amendment 1 (EXPAND), Appendix 12 Schedule of Activities, doc pp.121-130. Prompt 3.9.4, schema soa-table-extraction 1.1.

## Decisions needed (5)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | T1, doc p.122, row 30 (CMV Antigen by PCR), columns 15-16 | The two redacted cells are recorded as scheduled cells with the value `[redacted in source]`. The data shows that something is printed there without saying what. | Leave the two cells out of the schedule (treat them as empty), so that no redacted cell counts as a scheduled occurrence. | 3.4 |
| D2 | T1, doc pp.121-124, header rows 2 (Visit Identifier) and 3 (Visit Window) | Redacted header cells are recorded as `[redacted in source]`. Fragments that show on only one of the four reprinted headers are kept: `Day -34` above the redaction in column 3 (doc p.123), a bold `9` on line 2 of column 21 (doc p.122), and `)` at the edge of the column-5 window redaction (doc p.123). | Record redacted header cells as empty, or drop the one-page fragments because the other three reprints show nothing there. | 3.4 |
| D3 | T1, doc p.121, row 1, marker 2 on "Screening and Post Randomization" (merged 3:4) | The marker is placed on both columns the merged header covers (3 and 4), so footnote 2 governs both screening columns. | Place the marker on column 3 only, the column the superscript sits over. | 3.5 |
| D4 | T1/T2 footnote text, doc pp.124-130 | Each redaction gap inside a footnote is written `[redacted in source]`. Two boxes with no visible text between them, including boxes split across a line break, are written as one marker. | Write one marker per printed box, or close the gap with no marker. | 3.6 |
| D5 | T2, doc p.129, rows 8, 9, 10, 23, 24, 25 (column 2) and row 23 column 3 | The seven redacted body cells are recorded as scheduled cells with the value `[redacted in source]`. | Leave them out of the schedule (treat them as empty). The Pregnancy test row (row 10) would then have no scheduled cell. | 4.4 |

## Recorded, not open (14)

- §2 **track vs main_soa**: T2 (Follow-up and End of Study) is typed `main_soa`. It is a sequential phase that every participant passes through, printed as a separate table. Sequence alone does not make it a track.
- Taxonomy **continuation** ("a page break inside ONE printed table is not a continuation"): T1 runs over doc pp.121-124 under a reprinted header with no title or number of its own, so it is extracted as one table.
- §4 **row_position**: the reprinted header rows of T1 keep their numbers (24-26, 49-51, 71-73) and are not emitted.
- §4 **Hierarchy — printed signals only**: T1 rows 4-8 come before the first shaded band and stay at level 0. Groups stay open across page breaks: rows 27-32 on doc p.122 sit under "Local Laboratory" from p.121, and rows 52-53 on doc p.123 sit under "Other Clinical assessments" from p.122. T2 rows 4-13 come before "Tumor assessments" and stay at level 0.
- §4 **Hierarchy**: the T1 group row "Lymphodepletion" (row 33) carries marker 19 but no marks, so it stays a mark-free group header.
- §5 **Leading label columns**: L = 1 in both tables, so the first data column is position 2. T1 has data columns 2-21 and T2 has 2-4.
- §5 **Merged marks / Spanned values**: every text cell spanning columns is spread over the columns its rule lines enclose, with `source_range` set (spans in 3.2 / 4.2).
- §5 **Spanned values, exception a** (a cross-reference over the whole schedule): T2 rows 20 "See Section 8" and 21 "See Section 5.11" span every data column. They are emitted as `source_note` pr1/pr2 with no schedule entries.
- §5 **Spanned values** (a pointer covering only some columns keeps its span): T1 row 39 "Based on Section 5.12.3; NCCN guidelines" covers columns 5-18 only, so it stays as cell values over 5:18.
- §5 **Qualified marks**: T1 row 56 column 8 prints `X` over "EOI only". This stays in one cell as `X EOI only`.
- §5 **Glyph case** / §1 transcribe: T1 row 17 `(X)` in columns 2 and 3 is kept literally. No legend defines it.
- §5 **Empty cells**: `schedule_grid` is complete (T1 3x20 = 60, T2 3x3 = 9). Blank header cells are `""`.
- §6 **Abbreviations are not annotations**: the abbreviation block under T1 on doc p.124 gives zero annotations. Its `+` and `→` symbols do not appear in the grid, so no legend entry was made either.
- §6 **Redacted / illegible content**: footnote 1 of both tables is redacted in full. It is emitted with the text `[entire footnote text redacted in source]` and bound to its printed marker, so nothing is made up and nothing is orphaned.

## 1. Inputs and method

- PDF pages 1-4 (doc 121-124) and 9 (doc 129) are each one full-page raster image (1584x1224 px = 144 dpi). They have no text layer and no vector rules. PDF pages 5-8 and 10 (doc 125-128, 130) carry a real text layer and hold footnotes only. Redaction on those pages is drawn as filled black vector rectangles, and the text under them is absent from the text layer.
- I read the grid from the embedded images at native resolution, zooming into crops where needed. I used the text layer for footnotes 7-41 (T1) and 4-16 (T2). Footnotes 1-6 (T1, doc p.124) and 1-3 (T2, doc p.129) are on raster pages and were transcribed by eye.

## 2. Tables found

| Table | type | title | doc pages | data columns | activities (incl. group rows) | marks (X / (X) / qualified X) | text-span cells | redacted body cells | annotations |
|---|---|---|---|---|---|---|---|---|---|
| 1 | main_soa | Schedule of Activities: Screening and Treatment | 121-124 | 20 (pos 2-21) | 66 (10 group rows) | 314 (311 / 2 / 1) | 170 entries from 11 spans | 2 | 41 (all footnote) |
| 2 | main_soa | SCHEDULE OF ACTIVITIES: Follow-up and End of Study | 129-130 | 3 (pos 2-4) | 22 (2 group rows) | 33 | 12 entries from 4 spans | 7 | 18 (16 footnote, 2 source_note) |

## 3. Table 1

### 3.1 Structure and page coverage
- Header rows: 1 Epoch (synthesised; Pre-Screening | Screening and Post Randomization 3:4 | Lymphodepletion 5:7 | Treatment and Follow-up 8:21), 2 Visit Identifier (visit, level 2), 3 Visit Window (window, level null).
- Activity rows per page: doc p.121 = 20 (rows 4-23), p.122 = 22 (rows 27-48), p.123 = 19 (rows 52-70), p.124 = 5 (rows 74-78). Every page in the range contributes rows. Doc pp.125-128 are footnote pages, outside the declared range 121-124.
- The column identities are almost entirely redacted, so the only thing that tells the 14 Treatment and Follow-up columns apart is their position.

### 3.2 Merged spans (text spread over columns, confirmed from the per-row raster rule lines)
- Row 13 ICE assessment/ ICANS: "As clinically indicated" 8:21
- Row 15 ECHO or MUGA: "As clinically indicated" 5:21
- Row 20 Coagulation: "As clinically indicated (e.g. CRS)" 8:20 (column 21 is a separate X)
- Row 21 Reflex autoantibodies: "In case of suspected ALLO-647 treatment emergent autoimmunity" 5:21
- Row 31 EBV and adenovirus monitoring: "Per institutional practices" 5:21
- Row 39 Anti-infection prophylaxis: "Based on Section 5.12.3; NCCN guidelines" 5:18 (columns 19-21 are separately ruled and empty)
- Row 42 Brain MRI: "As clinically indicated" 8:21
- Row 43 Bone marrow aspirate and biopsy: "If positive at [redacted in source], or as clinically indicated" 8:21
- Row 44 Lymphoma biopsy: "As clinically indicated" 11:21 (columns 8-10 are separately ruled and empty)
- Rows 57 and 59 (ALLO-501A in BM aspirate / in CSF): "As clinically indicated" 8:21
- Row 58 ALLO-501A in lymph node biopsy: X in column 10, then "As clinically indicated" 11:21
- No X mark sits in a merged cell. All X marks are in single ruled cells.

### 3.3 Mechanical mark-check (§1a / §1d image method)
- Rules were taken from the raster. Vertical rules are image columns with more than 50% ink over the table height: 22 rules, giving 21 columns, the same on all four pages. Horizontal rules are image rows with more than 85% ink inside the label column, which is a text column and is never redacted, so no redacted row was lost. Bands: p.121 23, p.122 25, p.123 22, p.124 8, each = 3 header + body rows, matching the visual row count.
- Each cell rectangle was inset 2-3 px and the near-black pixels (<90) counted. The threshold is an absolute count: ≥40 = ink, <8 = empty.
- **Redaction masking:** solid black regions were found by eroding the <60 mask with a 7x7 box, then dilated 11x11 and removed from the near-black count. X strokes (about 2 px wide) do not survive the erosion. A cell with more than 150 masked pixels is classed as redacted, not marked.
- Merged spans were read per row from whether each internal vertical rule is present inside that row's band.
- I compared the result cell by cell with my visual read on all 66 body rows (1,320 cells), with all marks and spans checked against the image crops. No mark disagreed. The only flagged cells are row 54 columns 2-4, where the full-width group band's label text runs into the data columns. That row is a mark-free group header. Every recorded span matched the rule geometry (internal rules absent, outer rules present).
- A spot-check of the resolved grid is recommended, as for every image-based table. Rows to check first: 30 (CMV, with redacted cells), 56 (EOI only), 39 (5:18 span) and 44/58 (11:21 spans).

### 3.4 Redaction handling
- Header: Visit Identifier columns 3-21 and Visit Window columns 5 and 10-21 are `[redacted in source]`. Visit Identifier column 2 and Visit Window columns 2-4 and 6-9 are white and recorded as `""`. The redaction boxes differ a little between the four reprints of the header. Kept fragments (D2): column 3 `Day -34 [remainder redacted in source]` (from doc p.123), column 21 `[redacted in source] 9` (from doc p.122; the `9` is regular-size bold text, not a superscript footnote marker), and window column 5 `[redacted in source] )` (doc p.123).
- Body: row 30 columns 15-16 are T-shaped redaction boxes and recorded as `[redacted in source]` (D1). In row 43 the text cell is partly redacted and the gap is kept inside the value.
- No redaction box is large enough to hide a whole activity row. The row bands come from the label column, and every band has a readable label.

### 3.5 Annotations and markers
- Footnotes 1-41 each appear exactly once, bound through printed superscripts. Marker 1 is on the Visit Identifier label (schedule_property row 2). Marker 2 is on the merged epoch cell, columns 3 and 4 (D3). Markers 3-41 are on activity labels.
- Marker 19 is on the group row 33 and on its three children (rows 34-36): one note, 4 locations. Marker 25 is on rows 52-53, and marker 28 on rows 57-59.
- Rows with no marker: Tumor history, Eligibility criteria, ECOG, ECHO or MUGA, Blood culture, TSH, CMV, EBV, ALLO-501A, Anti-infection prophylaxis, AE, Concomitant treatments, and the group rows apart from Lymphodepletion.

### 3.6 Annotation text integrity
- The text layer is not glyph-spread, so no word reconstruction was needed. The superscript `2` in `mg/m2` (fn 19) is written `m²` and the subscript in `O2 Sat` (row 11 label) is written `O₂`, as the page shows them. Bullets in fn 19 are kept as `•`.
- Redaction gaps inside notes are written `[redacted in source]` (D4). Notes with gaps: 2, 6, 7, 8, 9, 10, 11, 12, 17, 18, 20, 22, 23, 26, 27, 29, 30, 31, 33, 34, 35, 38. Fn 1 is redacted in full.
- Source typos are kept verbatim: fn 23 "FFPE cell pelletthe material", fn 5 "HbsAG", "Cocciomycosis", "Histoplamosis".
- Containment pairs: none within T1. T1 fn 16 and T2 fn 7 have the same text, but they belong to different tables, as printed.

### 3.7 Low-confidence calls
- Indentation comes from the shaded full-width group bands, because labels are not indented (`indentation_method: visual_estimate`).
- Every legible character was transcribed. I found no illegible character outside the redaction boxes.

## 4. Table 2

### 4.1 Structure and page coverage
- Header rows: 1 Epoch (synthesised; Follow-up 2:3, EOS col 4), 2 Visit Identifier (visit, level 2), 3 Visit Window (window, level null; marker 1 on its label).
- Activity rows per page: doc p.129 = 22 (rows 4-25). Doc p.130 holds footnotes 4-16 only and contributes no rows. This is expected.

### 4.2 Merged spans
- Rows 11 (Reflex autoantibodies), 16 (Brain MRI), 17 (Bone marrow aspirate and biopsy), 18 (Lymph node biopsy): "As clinically indicated" 2:4.
- Rows 20 and 21: whole-schedule pointers, emitted as source_notes with no schedule entries (Recorded, §5 exception a).

### 4.3 Mechanical mark-check
- I used the same method as T1. There are 5 vertical rules (4 columns) and 25 bands (3 header + 22 body), matching the visual count. The detector agrees with the visual read on all 22 body rows (66 cells). The only flagged cells are column 3 of rows 20 and 21, which hold the "See Section ..." pointer text and are not emitted, by rule.

### 4.4 Redaction handling
- The header Visit Identifier and Visit Window rows are each covered by one bar running from inside column 2 to inside column 4. All six cells are `[redacted in source]`. Whether these header cells are merged cannot be seen.
- Body cells recorded as `[redacted in source]` (D5): column 2 of rows 8 TSH, 9 Urinalysis, 10 Pregnancy test, 23 RCL, 24 Genomic safety biospecimen and 25 PBMCs, plus row 23 column 3. In row 10, columns 3-4 are blank and ruled.

### 4.5 Annotations
- Footnotes 1-16 are bound through printed superscripts. Marker 1 is on the Visit Window label. Footnotes 1-3 were transcribed from the raster (doc p.129) and 4-16 come from the text layer (doc p.130). Fn 1 is redacted in full, and fn 14 and fn 15 contain redaction gaps.
- Source wording kept verbatim: fn 4 "C- reactive protein", and fn 13 has no final period.

## 5. Synthesised values
- property_name `Epoch` for row 1 of both tables (empty label cell).
- Annotation markers `pr1` ("See Section 8", T2 row 20) and `pr2` ("See Section 5.11", T2 row 21). No `n` or `g` markers were needed.

## 6. Method provenance (non-default methods recorded)
- `activity_name_source.method: visual_transcription` and `indentation_method: visual_estimate` on every activity in both tables, because there is no text layer.
- `schedule_grid[].method: visual_read` on every header cell in both tables.
- `activity_schedule[].method: raster_pixel_detection` on every body cell in both tables, including span extents and redacted cells.
- `annotation_text_source.method: visual_transcription` on T1 fn 1-6, T2 fn 1-3 and T2 pr1/pr2.
- No `unresolved` locations, no `proximity` or `text_match` bindings.

## 7. Orphan risk
- None. Every annotation has at least one location, and every location's marker is in that element's `annotation_markers`. The fully redacted footnote 1 in both tables is bound to its printed marker. It carries no recoverable content, so a reviewer may choose to drop it downstream.
