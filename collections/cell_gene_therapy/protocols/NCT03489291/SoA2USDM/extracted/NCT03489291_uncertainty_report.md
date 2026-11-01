# NCT03489291 — uncertainty report (prompt 3.9.4)

CSL Behring CT-AMT-061-01, Version 4.0 (Amendment 3.0), 08 Feb 2022. SoA excerpt = doc pp.19-26 (PAGEMAP authoritative). Four SoA tables, all with a text layer and vector rule lines on every table page.

## Decisions needed (6)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | Table 1, doc p.19, header row 2 "Weeks/Day" | Typed study days (study_day), same as the identically labelled row in Table 2 | Type the row as weeks (week), following the first word of the label "Weeks/Day" (5 of 10 values are week labels) | Table 1 - Low-confidence calls |
| D2 | Table 1, doc p.20, footnote g; rows 26-31 (redacted CCI rows) | Footnote g has no visible marker anywhere in the table; kept with `unresolved` locations on the six redacted rows, so it binds to nothing | Bind g to the six redacted CCI rows as activity notes (word match on "CCI" / the questionnaires), so it governs them | Table 1 - Orphan risk |
| D3 | Table 2, doc p.21, whole table | Typed subsidiary: columns differ from Table 1 (no D0 0h; D0 post IMP split into 1h/2h/3h; 24h under D1) and it details Table 1's "Blood & semen sampling (see Table 2)" | Type as domain: lab-category companion of Table 1 for the same participants on essentially the same timeline | Table 2 - Classification |
| D4 | Table 3, doc p.24, footnote e; rows 8-13 (redacted CCI rows) | Footnote e has no visible marker; kept with `unresolved` locations on the six redacted rows | Bind e to the six redacted CCI rows as activity notes (their marks fall exactly at Months 24/36/48/60, the visits the note names) | Table 3 - Orphan risk |
| D5 | Table 4, doc p.25, rows 16-18 | One ruled label cell "Sampling for vector genome detection: / - Blood / - Semen" split into a mark-free heading (level 1) + Blood and Semen (level 2), each carrying only the marks printed on its own line | One activity per line with the whole cell's marks on each (heading gets marks too), or a single activity for the whole cell | Table 4 - Low-confidence calls |
| D6 | Table 4, doc p.25, header row 2 col 7 (Month 48) | Printed markers "bb, c" recorded as b,c (doubled b read as a typo) | Record literally as "bb,c": no footnote "bb" exists, so footnote b would not govern Month 48 | Table 4 - Low-confidence calls |

## Recorded, not open (16)

- §2 **track vs main_soa** — Table 3 (Long-Term Follow-up, Months 18-60) typed `main_soa`: the next sequential phase after Week 52 for participants, not a branch; sequence alone never makes a track.
- Taxonomy **domain** / decision tree — Table 4 typed `domain`: identical columns to Table 3, same participants, laboratory category.
- Taxonomy **continuation** ("A page break inside ONE printed table is not a continuation") — Tables 1 and 2 each run over two pages under a reprinted header; extracted as one table each.
- §1b **De-duplicate repeated rows** / §4 **row_position** — reprinted headers left out with numbers kept: Table 1 rows 20-22 (p.20), Table 2 rows 19-22 (p.22).
- §5 **Leading label columns** — every table has L=1; first data position 2. T1 data 2-12, T2 2-13, T3/T4 2-10; the last data column in each is "Additional Visits".
- §5 **Merged marks — distribute** / **Spanned values** — T1 row 8 "24-hr stay at clinic": ruled cell 3:6 holding "x----x" distributed over 3:6 (cell_value literal, 41 dashes as in the text layer). "Continuous": T1 rows 11, 18, 19, 25, 32 over 2:11; T3 rows 4, 15 over 2:9.
- §5 **Qualified marks** — "x (w26)", "x: W6, 12", "x: W26", "x: W1, 2, 3, 4, 5, 6, 12", "x:W6, 12", "x:W16, 26", "x (only CRP)", "x: W12" kept literally in the one ruled cell each is printed in.
- §5 **Empty cells** — schedule_grid complete (blank header cells as ""); activity_schedule non-empty cells only.
- §3 **Header cells / merged** — T2 "D0 post IMP" and "±15 minutes" recorded on 4:6 with merged_cell_range; Trial Period bands recorded with their ranges.
- §3 **hierarchical_level** — Visit Window rows are window rows: null.
- §3 **property_type** — T3/T4 "Months" typed `other` (no month type in the list).
- §3 **Header bands — typing** — T2 "Weeks/Day" typed study_day because the only multi-column value (D0 post IMP) is a day label.
- §4 **Hierarchy — printed signals only** — T2/T4 bold full-width bands "Local Laboratory" / "Central Laboratory" are level 0 mark-free group headers; T2 Central Laboratory group stays open across the page break onto p.22 (no band reprinted), so all p.22 rows are level 1; Blood/Semen (dash-indented) level 2. T1/T3 flat.
- §6 **source_note** (inline reference) — "(see Table 2)" (T1 row 16) and "(see Table 4)" (T3 row 7) stripped from activity_name into source_note pr1 in each table.
- §6 **Abbreviations are not annotations** — abbreviation lines under every table not emitted.
- §6 **Redacted / illegible content** — redacted activity labels transcribed as the visible stamp "CCI"; redacted spans inside footnotes T1 g and T3 e kept as the visible "CCI" stamps, never filled.

## Per table

### Table 1 — main_soa — doc pp.19-20
"Flow Chart for Efficacy and Safety Evaluation, for Screening, IMP Administration + Post-Treatment Follow-up (Week -6 to Week 52)". 11 data columns (W-6, D0 pre IMP, D0 0h, D0 post IMP, D1, D3, W1-12, W14-26, W31-48, W52, Additional Visits). 3 header rows. 26 activities, 111 marks, 9 annotations (a-h footnotes, pr1 source_note).
Rows per page: p.19 = 16 (rows 4-19); p.20 = 10 (rows 23-32).

**Low-confidence calls.** D1: the "Weeks/Day" row mixes day and week labels five-to-five with no multi-column value; study_day chosen for consistency with Table 2.

**Orphan risk.** D2: footnote g (questionnaires completed before other assessments) is printed in the footnote list but its marker does not appear on any visible cell, label or header. The questionnaire rows are almost certainly the six redacted "CCI" rows (26-31) and the marker was most likely inside the redacted labels. Recorded as `location_type: unresolved` on rows 26-31; marker g is NOT put on those rows' annotation_markers (no printed marker, no scope claim).

**Redaction.** Rows 26-31: labels fully covered by black boxes with a red "CCI" stamp; all marks visible (x at D0 pre IMP, "x (w26)" in W14-26 for rows 26-30, x at W52). The six rows share the name "CCI" — downstream name-based matching must not merge them. A black bar directly below the frame on p.20 begins the abbreviation line ("CCI … IMP: investigational medicinal product; [redacted]; W: week.") — it is below the frame's bottom rule and does not conceal body rows.

### Table 2 — subsidiary — doc pp.21-23
"Flow Chart for Laboratory Parameters, for Screening, IMP Administration + Post-Treatment Follow-up (Week -6 to Week 52)". 12 data columns (W-6, D0 pre IMP, D0 post IMP 1h/2h/3h, D1 24h, D3, W1-12, W14-26, W31-48, W52, Additional Visits). 4 header rows. 25 activities (2 group bands + 1 sub-heading), 114 marks, 12 annotations (a-l).
Rows per page: p.21 = 14 (rows 5-18); p.22 = 11 (rows 23-33); **p.23 = 0** — a footnote-only page (footnotes e-l), no table frame.

**Classification.** D3. By the taxonomy decision tree: columns are not the same as Table 1 → not the anchor → finer timing (D0 split into hours) for the Table 1 activity "Blood & semen sampling (see Table 2)" → subsidiary. The alternative (domain) is plausible because the table is a full lab schedule on almost the same timeline for the same participants; note that as domain its columns would not join Table 1's one-for-one at D0.

**Text layer.** Two 14pt Arial "I" glyphs in the Hours row (x≈305 and 326 pt on pp.21 and 22) sit exactly on the vertical rules between 1h/2h/3h; they are rule fragments, not content, and are not transcribed.

### Table 3 — main_soa — doc p.24
"Flow Chart for Efficacy and Safety Evaluation, for Long-Term Follow-up (up to Month 60)". 9 data columns (Months 18-60, Additional Visits). 3 header rows. 12 activities, 75 marks, 8 annotations (a-g, pr1).
Rows per page: p.24 = 12 (rows 4-15).

**Orphan risk.** D4: footnote e (questionnaires; COVID-19 window for Months 24/36/48/60) has no visible marker; the six redacted rows 8-13 carry marks exactly at Months 24/36/48/60. Recorded `unresolved` on rows 8-13, marker not placed on any element.

**Redaction.** Rows 8-13 labels redacted ("CCI" stamp), marks visible. The black bar below the frame starts the abbreviation line; it does not conceal body rows. Footnote b/c wording is swapped relative to Table 1 (here b = visit/time-point timing, c = prophylaxis); transcribed per table.

### Table 4 — domain — doc pp.25-26
"Flow Chart for Laboratory Parameters, for Long-Term Follow-up (up to Month 60)". 9 data columns (same as Table 3). 3 header rows. 18 activities (2 group bands + 1 sub-heading), 107 marks, 10 annotations (a-j).
Rows per page: p.25 = 18 (rows 4-21); **p.26 = 0** — footnote-only page (end of footnote c, footnotes d-j).

**Low-confidence calls.** D5: on p.25 the vector-genome entry is ONE ruled label cell with three printed lines; each data cell holds two stacked x, y-aligned with the Blood (y≈355) and Semen (y≈366.5) lines, none with the heading line (y≈344). Split into heading (mark-free) + Blood + Semen with line-aligned marks, matching how Table 2 rules the same entry as three rows. Rows numbered 16/17/18 (one per printed line). D6: Month 48 header prints "48 bb, c" (also in the markdown); recorded as b,c.

## Merged-mark decisions
- T1 row 8 "24-hr stay at clinic": "x-----x" in one ruled cell over 3:6 (D0 pre IMP → D1).
- T1 "Continuous" over 2:11: rows 11 (Concomitant medication), 18 (Recording of e-diary data), 19 (Review of e-diary data), 25 (Adjudication of bleeding episodes), 32 (Adverse events).
- T3 "Continuous" over 2:9: rows 4 (Concomitant medication), 15 (Adverse events).
- No merged marks in Tables 2 and 4 (merged cells there are header-only: D0 post IMP and ±15 minutes over 4:6; Trial Period bands).

## Synthesised
- property names: none synthesised (every header row has a printed label).
- annotation markers: pr1 in Table 1 ("see Table 2", row 16) and pr1 in Table 3 ("see Table 4", row 7). No n/g markers needed.

## Mechanical mark-check
Method: §1b bbox column-binning. `pdftotext -bbox` tokens; column x-centres fixed from the finest header row of each page (Weeks/Day, Hours After IMP, Months); mark tokens (`^\(?[Xx]…`, plus "Continuous") binned to the nearest centre; spans expanded from per-row vector rule geometry (pymupdf drawings, no pdfplumber); body row bands taken from the horizontal rules of the right-hand (Additional Visits) column. Result: **0 disagreements** — mechanical matrix equals the JSON cell-for-cell in all four tables (111 / 114 / 75 / 107 marks). Note: on doc p.20 the label column loses the frame's bottom rule under the redaction bar below the table, which made the Adverse events row vanish from a label-column band list; using the Additional Visits column's rules (§1d **Take the row boundaries from a column that holds text**) recovered it. T4 rows 16-18 resolved per printed line by token y.

## Annotation text integrity
- Text layer is normal (not glyph-spread); no reconstruction. No private-use code points.
- Line-break hyphenation joined: "on-demand" (text layer "on-"/"demand"; markdown has "ondemand" in T1/T2/T4 — PDF hyphen kept); T3 c text layer "on -demand" and T3 e "influenc ed" are text-layer spacing artefacts, written "on-demand" / "influenced" as the page and markdown show.
- Source wording kept as printed, including "re-test should be performed with two weeks" (T2 d / T4 f) and the doubled ")" at the end of T2 i.
- No annotation text is contained in another within any table. Footnotes reprinted across tables with small wording differences (e.g. T1 c vs T2 c; T2 h vs T4 h which adds "Semen samples are only to be collected as applicable."; T2 i "Hemoglobin" vs T4 i "hemoglobin") are emitted per table verbatim.
- PDF/markdown agreement: all marks and labels agree; markdown drops spaces in places ("24-hr stayat clinic") — PDF text used.

## Orphan risk
- T1 g and T3 e: marker not printed on any visible element (D2, D4); `unresolved` locations, marker not on annotation_markers.
- No marker referenced without a printed definition, except the "bb" in T4 Month 48 (D6).

## Method provenance
- `activity_name_source.indentation_method: font_signal` — T2 and T4 group bands and their level-1 children (bold band = group signal).
- `activity_name_source.indentation_method: visual_estimate` — T2 rows 24-25 and T4 rows 17-18 (dash-indented Blood / Semen, x-offset).
- `marker_locations[].method: synthesized` — T1 pr1 (row 16), T3 pr1 (row 7).
- `unresolved` locations — T1 g (rows 26-31), T3 e (rows 8-13).
- No raster methods, no proximity bounding, no visual-read cells.
