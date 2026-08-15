# NCT04677179 (J1P-MC-KFAH(b), INSTRUCT-UC) — Extraction Uncertainty Report

Ulcerative colitis. Source: `NCT04677179_soa.pdf` (30 pages = doc pp.17–46). Four tables.
Extractor: Claude Opus 4.8 (single-pass), then reworked in the sessions listed under History.
**Page numbers in this report are doc/PDF pages; the printed footer runs one lower** (doc p.43 = printed 42).

## Current state (as of 2026-08-15)

All four tables resolve with `resolution_status: resolved`, 0 errors. Corrections sidecars are **empty**:
after the 2026-07-28 re-extraction the fresh raw extraction is the ground truth, with all later fixes
baked in; provenance for every rework lives in git history.

Consolidated: **60 unified activities** (165 source activities, 63% compression), **`schedule_matrix`
603 cells**, review_queue 0. **Annotations: 41 unified footnotes** from 74 raw (T1 31 / T2 14 / T3 14 /
T4 15), `by_type = {footnote: 41}`, longest duplicated sentence block 0 chars, none over 400 chars,
0 validation warnings. `xannot-035` (T4 c1 "No fasting in this period") carries a `referenced_props`
reference to the *Fasting visit* property (2026-08-15 consolidation; it previously reported as a
benign orphan warning because property scope had no target in the consolidated model).

A full row audit (extracted labels vs page labels, all 30 pages) found no missing activity rows. The only
labels on the pages absent from the extraction are section headers — *Patient-Reported Outcomes
(Electronic)*, *Clinician-Administered Questionnaires (Paper)*, *Laboratory Tests and Sample Collections*,
*Stool Samples* — omitted consistently in all four tables, a deliberate convention.

## Tables & classification

- **Table 1 — `main_soa`.** Screening & Induction, V1–V9 (doc pp.17–23). Vector page + clean text layer; highest confidence.
- **Table 2 — `track`, "Maintenance (responders)".** V10–V29 (doc pp.24–32), two-tiled (V10–V19 / V20–V29 "(continued)").
- **Table 3 — `track`, "Extension (nonresponders)".** V10–V29 (doc pp.33–42), two-tiled likewise.
- **Table 4 — `track`, "Early Termination / Unscheduled / Post-Treatment".** ETV, V997, V801, V802 (doc pp.43–46).

Judgment call: Tables 2/3 share the V10–V29 numbering but split the population (responders vs
nonresponders at Week 12) into different treatment phases → separate `track` timelines, not `domain`.

## Why this protocol is lower-confidence than the others

**20 of the 30 SoA pages are full-page raster images** with a one-glyph-per-token text layer
("V i t a l s i g n s") and no vector rule lines; only doc p.17 has a vector table. X marks read reliably
by coordinate; activity labels and Comment-column notes had to be reconstructed (glyph re-segmentation,
raster rule-line cell geometry). Several body rows are **CCI black-bar redactions** — marks preserved,
names unknowable, multiple CCI rows per table not name-mergeable. Schedule property values (Weeks/Study
day/tolerance) were read from the clean header pages and hardcoded per table.

## Open items (all low severity)

- **`T4 c11`** ("No dosing at ETV, V997, or post-treatment follow-up visits.") sits on *Urine pregnancy
  (local)*; almost certainly belongs on *Randomization and Dosing* (cf. `T1 c22` "No dosing at V3."). A
  footnote-block note outside the Comment-column geometry, so it was left as found rather than moved on a guess.
- **Four corrupt abbreviation-key fragments** (`T1 c23`, `T2 c13`, `T3 c13`, `T4 c12`) from the
  multi-column abbreviation block under each table (doc pp.23/32/42/46) — column clip interleaved two
  text columns. No schedule logic; decision was to leave them.
- **`T1 c21_2`** reads "Addition alc. Difficile testing …" where doc p.23 prints "Additional *C. difficile*
  testing …" — the italic run broke word re-segmentation. Preserved verbatim (rework never regenerates text).
- **One Comment cell deliberately not extracted**: the ETV↔V801/V802 interval rule on Table 4's *Weeks from
  randomization* row (doc pp.43–45) — property-scope content; add deliberately if it matters downstream.
- **Spot-checks worth one pass**: image-page activity labels (minor section-fragment bleed possible), and
  Table 4's restored first page (14 rows, 26 marks — new Layer-1 content from the 2026-07-30 rework).
- **Not defects**: the PK-samples note legitimately prints twice per table (on *PK samples* and the CCI
  row below — confirmed on doc p.22).

## History (details in git log and the per-commit messages)

- **2026-07-21** — The T2/T3 two-tile layout had cost the recurring rows their V10–V19 marks (extractor
  kept only the "(continued)" tile). Restored via corrections sidecars (+34 / +38 cells); same day the
  appended unmatched CCI/QIDS rows from the V10–V19 tile were folded into their canonical rows by row
  position and the 9 artifact rows removed (65 → 60 unified activities).
- **2026-07-28** — Annotation layer regenerated from the PDF ("Option B"): the raw text-layer fragments
  were lossy and glyph-scrambled, so notes were re-read via `pdftotext -bbox` + de-glyph word
  re-segmentation (source characters only; spaces and casing restored). Marks and structure preserved
  byte-identically. Fresh raw became ground truth; sidecars emptied.
- **2026-07-30 (rev2)** — Note boundaries and row bindings made geometric via 200 dpi raster rule-line
  detection (one code path for raster and vector pages). 12 over-merged notes split, 6 bindings corrected;
  longest duplicated block 145 → 0 chars. Same day (rev3): **Table 4's never-extracted first page
  (doc p.43) restored** — 14 rows, 26 marks, `schedule_matrix` 577 → 603 — and per-row
  `annotation_markers` regenerated from `marker_locations` (24 stale rows; resolve reads the former).
- **2026-07-30, committed 2026-08-15** — *Genetics sample* row restored in T2/T3/T4 (printed on every
  tile, absent from those extractions; verified on doc p.31; carries no marks in these tables, so
  `schedule_matrix` unchanged). The DNA-pharmacogenetics note (`xannot-024`) re-bound to it — it had
  falsely spanned three activities. Source activities 162 → 165.
