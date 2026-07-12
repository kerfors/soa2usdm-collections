# NCT04557384 (I4T-MC-JVDU(e)) — Layer 1 Extraction Uncertainty Report

Oncology, SC ramucirumab. Source: `NCT04557384_soa.pdf` (9 pages = doc pages 16–24).
Extractor: Claude Opus 4.8. Status: `ready_for_resolution`.

## Table classification

- **Table 01 — `main_soa`.** "Screening, On-Study, and Post-Treatment Schedule of Activities", doc pp.16–22. Cycle-based grid, 15 schedule columns, 5-level stacked header (Phase → Cycle-length → Cycle → Window → Day).
- **Table 02 — `track`, track_label = "Continued Access".** "Continued Access Schedule of Activities", doc p.23. Distinct visit structure (501-5XX, 901) → track per the repo's Continued-Access definition.
- **Table 03 — `reference`.** "Pharmacokinetic Sampling Schedule" (§1.3.1), doc p.24. Classification confirmed with reviewer (rows are sample-collection specifications, not procedures). Excluded from the consolidated timeline; retained as a source table and referenced by the main-SoA PK/IG rows.

## Method — important difference from NCT04184622

This SoA has **no text layer** (image-only pages) and the protocol markdown does **not** contain the grid. So, unlike the coordinate-based extraction used for NCT04184622, all marks here were **read visually** from 300-dpi page renders (the wide Instructions column was cropped away to read the 15-column grid cleanly). The two merged rows on page 1 (Concomitant medication, Physical examination) were re-cropped and double-checked. Grey shading = Not Applicable → empty, read directly.

Because placement is visual rather than coordinate-derived, cell-level confidence is a notch lower than for NCT04184622 — the draft Excel was reviewed against the PDF before JSON.

## Modelling decisions (documented)

- **Instructions column** (rightmost) is a per-row notes column, not a schedule column → captured as per-row `source_note` annotations (markers `i1…iN`), per convention.
- **Merged marks/text distributed with `source_range`:** Concomitant medication and AE collection each show a single X spanning the whole On-Treatment block (cols c3–c14); "See instructions" spans cycles (Vital signs & Injection-site-solicited over Cycle 1 only; combination-meds split Cycle 1–2 / Cycle 3-n around the grey DX column). Each was distributed across every covered column with `source_range`.
- **"See Section 1.3.1"** in the PK and IG rows is transcribed literally and carries a `source_note` pointing to Table 03.
- **Synthesized markers:** the above-table DX note (`dx`) attached to the Cycle 2-n column; "D22 for 28-day cycles only." (`d22`) attached to the two D22 day-cells; footnote `a` = Short-term follow-up definition (Table 01) / Continued-access follow-up definition (Table 02). No printed symbol existed for the DX / D22 notes, so markers were synthesized and documented.
- **Reference table (Table 03):** each sample's Study Cycle / Day within Cycle / Collection Time Point was folded into the `activity_name` (e.g. "Sample 3 (Cycle 1, Day 4 ± 1, 48-96 hrs postdose window)") since the extraction schema has no per-row descriptor field; PK/IG collection modelled as two X columns.

## Points worth a glance vs the PDF

1. Sparse cycle-D1 rows (ECOG PS at c1/c3/c6/c11/c15; Physical examination adds c4/c5) — visually read, so confirm the exact D-columns.
2. Hematology / Clinical chemistry pattern (c2–c8, c10, c11, c15 with the Cycle-2 D22 column grey) — identical pattern for both; confirm.
3. Combination-medications split around the grey DX column (c10) on page 22.

## Consolidation result

3 tables → 26 unified activities (main + Continued-Access track; reference excluded from timeline), review_queue empty. Both timelines render in the consolidated HTML.
