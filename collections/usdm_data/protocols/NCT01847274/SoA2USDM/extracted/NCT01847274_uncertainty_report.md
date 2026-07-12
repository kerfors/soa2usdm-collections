# NCT01847274 — SoA extraction uncertainty report

Protocol: Niraparib, PR-30-5011-C Version 8 (ovarian cancer). Three SoA tables.

## `_soa.pdf` regenerated ✅
The original excerpt (doc pp 62–71) was wrong: it held Table 7 + Section 7.2 narrative but was missing
Tables 8 and 9. Per your go-ahead I regenerated `NCT01847274_soa.pdf` from the full protocol to the correct
7-page slice — Table 7 (doc pp 63–66), Table 8 (doc pp 72–73), Table 9 (doc p 78) — dropping the 7.2
narrative. Three extraction files follow. `soa_pages` will be recorded as `63-78`.

- **Table 01 — Table 7: Main Study** (`main_soa`, pp 63–66). 28 activities, 90 cells, 28 footnotes (1–28).
- **Table 02 — Table 8: Food Effect Sub-Study** (`track`, "Food Effect Sub-Study", pp 72–73). 17 activities,
  79 cells, 13 footnotes.
- **Table 03 — Table 9: Extended Visit Cycle** (`track`, "Extended Visit Cycle", p 78). 15 activities, 40
  cells, 14 footnotes.

All three schema-valid; no orphan annotations; every element marker resolves; no dup cells. Marks placed from
`pdftotext -bbox` coordinates (authoritative), cross-checked against every rendered page.

## ⚠️ Most important review item — Table 9 undefined footnotes (source defect)
Table 9 has its **own** footnote numbering (its printed footnotes 1–3 are new — the "Location" column
definitions — and are unrelated to Table 7's 1–3). The body references markers **4–15**, but **only 1–3 are
printed on the page** — footnotes 4–15 are simply missing from the source. Final handling (your approved
recommendation):

- Markers **4–15 are kept on the cells/names** (faithful — the superscripts really are on the page).
- Each undefined annotation's **text states the gap honestly** and gives the probable Table 7 cross-reference
  as a *labelled hypothesis*, not asserted source content — e.g. *"Footnote not printed in the source
  (Table 9 prints only footnotes 1–3). Table 9 is the Extended-Visit-Cycle variant of the Main Study, so this
  most likely corresponds to Table 7 footnote 12: '…'."*
  Probable mappings: 5~T7 fn10 (SAEs), 6~T7 fn12 (CBC), 7~fn13 + 8~fn14 (CA-125), 9~fn16 (ECG), 10~fn22
  (RECIST), 11~fn24 (PROs), 12~fn25 (no new capsules), 13~fn26 (every 90 days), 14~fn27 (survival), 15~fn28
  (bone marrow).
- Marker **4** (Vital signs cell in the local-clinic-or-in-home column) is kept but noted as having **no
  confident Table 7 equivalent**.

No footnote text is asserted as printed-source content; the resolved definitions are hypotheses for a human
to confirm/replace.

## Table 01 (Table 7) judgement calls
1. **Row-1 `property_type` = `cycle`** — labeled "Cycle" but mixes cycles with Screening / Discontinuation /
   Post-Treatment phases. `epoch` defensible.
2. **Bone marrow aspirate and biopsy** — single `X` (marker 28) merged across the on-study columns 3–10
   (`source_range` 3:10); as-needed MDS/AML assessment.
3. **Header markers:** 1 on the Cycle property; 2 on Subsequent Cycles (col 8); 3 on Cycle 1 Day 1 (col 3).
4. **Cell-level markers:** 6, 7 (×2 cells), 8, 10, 11, 17, 19+20, 21, 25, 26 — footnote rides a specific `X`.
5. **Split markers:** Serum CA-125 name `13,14`; PK Cycle-1 Day-1 cell `19,20` (each → two footnotes).

## Table 02 (Table 8) judgement calls
1. **`track`** (separate Food-Effect sub-study population), `track_label` "Food Effect Sub-Study".
2. **Three-level header:** Period (Screening / 14-Day Food Effect / Cycle / Subsequent Cycles /
   Discontinuation) → Cycle (C1 = Cycle 1 cols 5–6, C2 = Cycle 2 col 7) → Day (-28 to -1, FE 1, FE 8, 1, 15,
   1, Cycle n Day 1). The "Cycle" group cell (cols 5–7) carries marker 1.
3. **Continuation re-numbering:** the "Continued" page (doc 73) re-labels the header footnotes **12, 13**
   (identical text to 1, 2). Since the header is encoded once (markers 1, 2), **markers 12, 13 are not
   emitted**. Page-73 body markers 14 (AE monitoring / discontinuation) and 15 (bone marrow) are kept.
4. **Bone marrow** — single `X` merged across cols 3–9 (`source_range` 3:9, marker 15).
5. **Cell-level markers:** 3 (pregnancy cols 2 & 5), 4 (randomization), 5 (study treatment FE cols 3–4),
   6 (study treatment discontinuation), 10 (ECG FE cols 3–4), 14 (AE discontinuation).

## Table 03 (Table 9) judgement calls
1. **`track`** (extended-visit-cycle variant), `track_label` "Extended Visit Cycle".
2. **Three-level header:** Cycle (three "Subsequent Cycles" columns, markers 1/2/3) → Day (Extended Cycle
   Day 1 / Cycle n Day 1) → Location (In Clinic / Site Staff Telephone / Local Clinic or In-Home / In Clinic).
   The three cycle columns are distinguished only by the Location row (`modality`).
3. **Bone marrow** — single `X` merged across all columns 2–6 (`source_range` 2:6, marker 15).
4. See the Table 9 undefined-footnote section above (the key review item).

## Not captured
Abbreviation blocks (each table footer) not emitted as `abbreviation` annotations — no in-grid markers.

**STOP — please confirm (especially the Table 9 footnote cross-references) before I run the pipeline.**
