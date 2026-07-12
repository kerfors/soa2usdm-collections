# NCT05259917 — SoA extraction uncertainty report

Protocol: KVD900-301 (KONFIDENT), sebetralstat/KVD900 for hereditary angioedema (HAE), Version 4.1 (US Only), 04 May 2023. One SoA table (Table 1: Schedule of Events). Schema-valid; no orphan annotations; every element marker resolves; every annotation is referenced by an element; marker_locations valid.

## Not image-based
Despite the runbook flag, this `_soa.pdf` has a **clean text layer** (the only embedded image is the sponsor logo). Marks were reconstructed from PDF text-layer coordinates (`pdftotext -bbox`) and confirmed visually — same method as the text-based protocols, not the visual-only path.

## Completeness check
- `_soa.pdf` = 2 pages, printed document pages 17–18 (footer "Page 17/18 of 67"). page_start/page_end = 17–18.
- `.md`/full-PDF confirm Table 1 is the only Schedule-of-Events table. Table 3 = "Sample Randomization Schedule" (not a SoA). Excerpt is complete for Table 1 (all rows + footnotes a–s present).
- **Table S1 "Frequency of Patient Assessment"** (also printed as "Table 4") is referenced by footnote r — a finer-timing spec (0–4h → every 0.5h; 5–12h → every 1h; 14–24h → every 2h; 25–48h → every 12h, with windows) for the PGI-S/PGI-C/VAS/GA-NRS assessments. It is **not** in the `_soa.pdf` excerpt (it sits in the trial-procedures narrative). **Per your decision, Table S1 is out of scope** — captured here as a note only; footnote r carries the cross-reference. Its rows are time-periods/frequencies, not an activity×timepoint grid.

## Table classification
- **main_soa**, **flat table**: there are no organizational section headers — every one of the 24 rows is a level-0 activity that carries marks. (The "no marks on header rows" rule is satisfied vacuously; this is the documented flat-table exception.)

## Header modelling
Two header rows:
- Row 1 (level 1, `epoch`): Screening (col 2), Randomization (col 3), **Treatment Period** (epoch band merged over cols 4-6), Final Visit/ET (col 7). Mixed epoch/visit — typed **epoch** as the dominant phase framing (Treatment Period is the spanning band); Randomization and Final Visit/ET are point visits sitting on this row. Col-1 label "Visit" used as property_name. Markers a (Randomization), c (Final Visit/ET).
- Row 2 (level 2, `visit`, synthesised name): 1st/2nd/3rd eligible HAE attack (cols 4-6); marker b on each. Cols 2,3,7 have no second-row label.

## Marks — judgement calls for review
1. **Randomization vertically-merged X.** The Randomization column (col 3) has a single X drawn vertically merged across the **In-clinic** and **TeleVisit** rows (footnote a: "The Randomization Visit may be a televisit or an in-clinic visit"). Transcribed as X on **both** In-clinic (col 3) and TeleVisit (col 3). (The schema has no vertical-merge concept, so the shared mark is duplicated to both activities.)
2. **Horizontal arrows = continuous activities.** Three rows use a horizontal double-headed arrow instead of X marks, captured as `cell_value` "↔" distributed across the covered columns with `source_range`:
   - Conventional on-demand treatment washout → cols **4-6** (Treatment Period), source_range "4:6".
   - Concomitant Medication Review → cols **2-7** (Screening→Final), source_range "2:7".
   - Adverse Event Review → cols **2-7**, source_range "2:7". (The drawn arrow spans the full width; footnote s clarifies AEs are recorded "from the first dose … to Final Visit/ET" — the arrow, not the footnote's narrower window, is transcribed.)
   Arrow extents were confirmed by zoomed image crops + pixel analysis. The "↔" glyph is my chosen representation of the double-headed arrow.

## Annotations
- Footnotes a–s (all `footnote`, defined on doc page 18). Header-cell (column-scope): a (Randomization), b (three attack columns), c (Final Visit/ET). Activity-scope: d–s.
- **Footnote r deduplicated** over 4 activities (PGI-S, PGI-C, VAS, GA-NRS) — one annotation, four `activity_name` marker_locations. It references Table S1 (out of scope).
- Abbreviation list (C1-INH, ECG, eDiary, ET, GA-NRS, HAE, IMP, PGI-C, PGI-S, RTSM, VAS) **not captured** — no in-grid markers.

## Low-confidence calls
1. Row-1 property_type = `epoch` for a mixed phase/visit row (Screening/Randomization/Treatment Period/Final Visit/ET). `visit` would also be defensible.
2. Arrow representation as `cell_value` "↔" distributed with source_range (vs. a single cell or a synthesized annotation).
3. Adverse Event Review arrow transcribed full-width (cols 2-7) per the drawing, though footnote s narrows the actual AE-recording start to first dose.
