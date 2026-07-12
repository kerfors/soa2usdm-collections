# NCT04730349 — SoA extraction uncertainty report

Protocol: BMS-986321 / CA045020 (19-214-27), Bempegaldesleukin (NKTR-214) + nivolumab, pediatric oncology. Amendment 01, 28-Nov.
Section 2 "Schedule of Activities" = three tables. All three schema-valid; every annotation has ≥1 marker_location; every element marker resolves; no scheduling marks on organizational headers.

## Completeness check
- `_soa.pdf` = 16 physical pages, printed document pages 27–42, continuous. Physical = doc − 26.
- Doc p27 (physical p1) is the SoA section-intro page ("An overview … Table 2-1, 2-2, 2-3"); excluded from page_start.
- Table 2-1 = doc 28–30; Table 2-2 = doc 31–38; Table 2-3 = doc 39–42.
- `.md` confirms Section 2 → Tables 2-1/2-2/2-3 → Section 3 INTRODUCTION. Nothing missing, nothing extraneous. Excerpt is correct — no regeneration needed.
- page_start/page_end use printed document page numbers (matches prior extractions).

## Tables
| # | title | type | schedule cols | activities | annotations |
|---|-------|------|--------------|-----------|------------|
| 1 | Screening Procedural Outline | main_soa | 1 (Screening Visit) | 21 (17 proc + 4 headers) | 18 |
| 2 | On-treatment Procedural Outline | main_soa | 6 (C1 D1/3/5/8; C2+ D1/D3-5) | 27 (21 proc + 6 headers) | 23 |
| 3 | Long-term Follow-up Period | main_soa | 4 (Safety FU V1/2/3; Survival FU) | 19 (14 proc + 5 headers) | 15 |

All three classified **main_soa**: each is an independent schedule with its own column structure (screening single-visit; on-treatment cycle/day grid; follow-up visits). Not domain/continuation (different columns), not track (same population/phase progression of one study). The document also references a "Table 2-1" cross-reference elsewhere — that is this Table 2-1, no separate table.

## Header modelling
- **T2-2** two-level header: cycle band (row 1, `cycle`) over study-day row (row 2, `study_day`). Both label cells hold the spanning "Procedure" activity-column header, so both property_names are **synthesised** ("Cycle", "Study Day"). The "Cycle = 3 wks" / "Each cycle = 3 wks" caption (cycle length, uniform for both bands) is kept in the Cycle property_comment rather than a separate row — it does not distinguish columns. Visit windows "(± 1 day)" / "(- 1 day)" are kept inline in each Day cell_value (faithful transcription), not split into a window row.
- **T2-3** single visit header row, property_name **synthesised** ("Follow-up Visit"). Col 5 "Survival Follow-up Every 3 Months (± 14 Days)" is a different phase but presented as one more column in the same header row — modelled as a column value of the visit property (noted in comment).
- **T2-1** single "Screening Visit" column; property_name taken directly.

## Merged marks / merged text (distributed, source_range set)
- **T2-2**: Day 1 (col 2) is a **separate** cell; ten rows carry one merged text cell spanning **cols 3-7** (source_range "3:7"): "Continuously" (Adverse Events, Concomitant Med Use); the Body/Brain imaging instruction blocks; "Performed at time of imaging assessments…"/"Performed as clinically indicated" (CSF/Bone Marrow ×4); "See Section 9.5.1 for further details." (PK Plasma, Nivolumab PK, Immunogenicity); "See Notes" (PRO). The 3-7 span (not 2-7) was confirmed two ways: a cell border at the Day1|Day3 boundary (zoomed image) and the markdown showing Day 1 empty.
- **T2-2 Oral Hydration Follow-up**: source prints a single "X (Day 3-5)" centred over the Cycle-1 Day3/Day5 region → distributed to **cols 3-4** (source_range "3:4"), cell_value "X". The "(Day 3-5)" label is not separately stored (it equals the distributed span). Flagging for review.
- **T2-2 12-lead ECG col 7**: source prints "X (Cycle 5 only)" → kept literally as cell_value "X (Cycle 5 only)" (single column; the Cycle-5 condition is not a column span, so preserved in the cell text).
- **T2-3**: merged text cells span all 4 schedule cols (source_range "2:5"): "As clinically indicated." (CSF, Bone marrow), "See Section 9.5.1 for further details." (PK, Immunogenicity). "If toxicities are present." / "If toxicities are present" (Lab Tests, Visits 2 & 3) are **individual** cells (not merged) — transcribed verbatim incl. the period difference. AE Survival cell = "See Notes" (single col 5).

## Annotations
- **Notes / Comments column** (right-hand, all three tables) → footnote/source_note annotations with **synthesised markers** (n1…, s1…) linked to the activity row (or property row). Explanatory notes → `footnote`; pure cross-references → `source_note` (T2-2: "See Figure 5.1-1…" s1, "See Section 7.1.1.1." s2; T2-3: "See Section 9.4.6 for the list of laboratory tests." s1, Body Imaging "See Section 9.1.1 for further details." s2).
- **Header-cell footnotes** (column scope): T2-1 `a` on Screening Visit; T2-2 `a,b` on Cycle 1 band, `a,b,c` on Cycle 2+ band (marker on first cell of each merged band), `d` on Cycle-1 Day 3 cell; T2-3 `a` on Visits 1/2/3, `b` on Survival Follow-up.
- **T2-2 `e`** rides the "Pharmacokinetic (PK)/Immunogenicity Assessments" section header → activity-scope on that header row (annotation marker on an organizational header is allowed; it is not a scheduling mark).
- **T2-2 `*`** ("At Day 8 visits collect vital signs only.") on the Targeted Physical Day-8 cell (X*) → cell scope.
- **T2-3 `c`** is printed on the **Notes-column header** ("Notes^c"). Because the Notes column is not a modelled schedule element, `c` is modelled **table-scope**: one schedule_property marker_location for traceability, marker NOT placed on any element (so it resolves unlinked/table-wide). Please confirm this treatment.
- Abbreviation lists (all three tables) NOT captured — terms carry no in-grid marker (would be orphans).

## Source defects — redactions (CCI) — REVIEW
Redaction boxes appear in this published protocol; visible content transcribed, hidden content not recoverable (markdown is redacted the same way):
- **T2-1**: four screening notes (CSF-Solid, Bone Marrow-Solid, CSF-Leukemia, Bone Marrow-Leukemia) end at "See Section 9.1[.2.5]." followed by a black box — trailing text redacted (marked "[Remainder of note redacted in source.]"). A **larger redaction box below the last activity** (doc p30) may hide additional rows — not recoverable.
- **T2-2**: footnote `e` — PDF has a redaction between "…will likely be for" and "pharmacokinetic assessments"; markdown shows continuous text with no gap, so the markdown wording was used (flag: one or more words may be redacted). Four Efficacy notes end with redaction boxes (marked). A **large redaction box below Immunogenicity Samples** (doc pp35-36) may hide additional rows.
- **T2-3**: a **redaction box below Pregnancy Test** (doc p40) may hide additional rows.

## PDF / markdown disagreements
- **T2-3 "Pregnancy Test"** row (X at Visits 1-3, plus its note) is clearly present in the PDF but was **dropped by the markdown** extraction. Taken from the PDF.
- Markdown lost several "±" symbols (rendered as spaces) and merged "See"+"Section"; PDF symbols/spellings used.

## Low-confidence calls for your review
1. T2-2 Oral Hydration "X (Day 3-5)" distributed to Cycle-1 cols 3-4 (Day 3, Day 5).
2. T2-2 12-lead ECG col 7 kept as literal "X (Cycle 5 only)" rather than "X" + separate condition.
3. T2-3 footnote `c` modelled table-scope (Notes-header marker).
4. T2-2 "Cycle = 3 wks" caption folded into Cycle property_comment (not a separate property).
5. Redaction boxes 1/2/3 above — whether any hide activity rows cannot be determined from the source.
