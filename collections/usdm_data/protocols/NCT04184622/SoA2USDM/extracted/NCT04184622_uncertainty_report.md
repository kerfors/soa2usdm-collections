# NCT04184622 (SURMOUNT-1) — Layer 1 Extraction Uncertainty Report

Protocol I8F-MC-GPHK(b). Source: `NCT04184622_soa.pdf` (11 pages = document pages 18–28).
Extractor: Claude Opus 4.8 (PDF→Excel→JSON two-conversation path). Status: `ready_for_resolution`.

## Table classification (and why)

- **Table 01 — `main_soa`.** §1.3.1 "Schedule of Activities covering visits to primary study endpoint", doc pp.18–21. Primary anchor grid, 24 visit columns (1–21, 99, ED, 801).
- **Table 02 — `track`, track_label = `Prediabetes`.** §1.3.2 "…additional 2-year treatment period for participants with prediabetes at randomization", doc pp.22–24. Different population, own visit numbering (101–116, 199, ED, 802) and duration → `track` by the repo's own definition (this protocol is the canonical `track` example in `soa_table_type_definitions.md`). Confidence: high.
- **"Additional Information…" table (doc pp.25–28) — `reference`.** Rows are notes, not procedures → not extracted as a timeline; captured as footnote annotations.

## Method / accuracy

- X placement was taken from the PDF's **own text coordinates**, each mark snapped to its visit-header column centre (tolerance = 0.6 × column pitch; every mark snapped within tolerance). The column model was validated by overlaying computed centres on the rendered pages — lines threaded cleanly through every X and header number. This is more reliable than eyeballing a 24-column grid, so cell placement confidence is high.
- Grey (RGB 0.65) cells = Not Applicable → transcribed as empty (metadata, not content).

## Points to check against the PDF

1. **Table 02 column reconstruction (199* / ED*).** In the PDF text layer these two headers are a single merged token `199*ED*`. The two columns were reconstructed assuming even column spacing (fit from the clean 101–116 headers + the 802 anchor). Overlay confirms alignment, but this is the one place columns were inferred rather than read directly. — Table 02, columns 17–18.
2. **Table 02 "Self-Harm Follow-up Form".** The X in the 199* column is printed vertically raised and clustered as its own line during coordinate extraction; it was merged back so the row reads X across all 19 columns. Confirm against doc p.24.
3. **Sparse lab rows (Table 01).** Rows with few marks (e.g. Fasting insulin, C-peptide, Calcitonin, Immunogenicity, TZP PK) place onto specific visits from coordinates only — worth a glance. TZP PK = V5, V7, V12 (Weeks 8, 16, 36).

## Synthesized markers (documented)

- The source uses a single `*` symbol for all cross-references; the "Additional Information" notes are keyed by **activity/visit name**, not by symbol. Distinct markers **n1–n27** were synthesized, one per note, each with explicit `marker_locations`.
- Attachment: 22 notes via activity-name stars; 2 via cell-level `X*` (n15 = Dispense study drug @ V21 "X\*"; n21 = screening lab eligibility on the `X*` V1 cells of Urinary albumin/creatinine, Cystatin-c, Calcitonin, Hematology, TSH); 6 header-level (Visit/Fasting, Visit 3, 99/199, ED, 801/802, Allowable Deviation).
- No orphan markers (every `annotation_markers` string resolves to an annotation) and every annotation has ≥1 `marker_location`. Schema-validated against `soa-table-extraction v1.0`.

## Not captured as annotations

- Abbreviation lists (doc pp.24, 28) are recorded in the draft Excel `Metadata_Annotations` sheet for reference but not emitted as annotations (they have no discrete marker_locations). No `X = performed` legend is printed in this protocol, so none was fabricated.

## Consolidation result

2 tables → 55 unified activities, 41% compression, **review_queue empty** (clean cross-table activity matching).
