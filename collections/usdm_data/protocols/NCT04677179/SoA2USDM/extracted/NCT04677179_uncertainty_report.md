# NCT04677179 (J1P-MC-KFAH(b), INSTRUCT-UC) — Extraction Uncertainty Report

Ulcerative colitis. Source: `NCT04677179_soa.pdf` (30 pages = doc pp.17–46). Four tables.
Extractor: Claude Opus 4.8. Status: `ready_for_resolution`. **This protocol needed more reconstruction than the others — review more closely.**

> **Update — 2026-07-21 (post-correction).** The Tables 2/3 tile merge flagged below was reviewed against the PDF and found to have dropped the V10–V19 marks for the recurring/physical rows (root cause: the extractor wrongly assumed those rows print only in the V20–V29 "(continued)" tile — the source shows them in *both* tiles). Fixed via `op=add` cells in the corrections sidecars (`Table_02` t2-c5..c38, +34 cells; `Table_03` t3-c3..c40, +38 cells), source-linked per visit; the raw extraction is unchanged. Resolved output now carries the correct per-visit union. Items corrected here are marked **[RESOLVED]** inline; the consolidation figures below are refreshed to the post-correction run.

## Tables & classification

- **Table 1 — `main_soa`.** Screening & Induction, V1–V9 (doc pp.17–23). Extracted cleanly (borders + clean text layer). Highest confidence.
- **Table 2 — `track`, "Maintenance (responders)".** V10–V29 (doc pp.24–32).
- **Table 3 — `track`, "Extension (nonresponders)".** V10–V29 (doc pp.33–42).
- **Table 4 — `track`, "Early Termination / Unscheduled / Post-Treatment".** ETV, V997, V801, V802 (doc pp.43–46).

Classification note: Tables 2/3 share the V10–V29 numbering but split the population (responders vs nonresponders at Week 12) into different treatment phases → treated as separate `track` timelines rather than `domain`. Documented judgment call.

## Method — and why this one is lower-confidence

The single-pass path was requested, but this SoA fought it on two fronts:

1. **~Two-thirds of pages are image-based** (table drawn as an image, text layer stored one glyph at a time — "Vital signs" → "V i t a l  s i g n s"). **X marks are single glyphs and were read reliably by coordinate.** Activity **labels were reconstructed** from the glyph stream with a gap threshold + cleanup dictionary; minor artifacts may remain (a few section-header fragments bled into adjacent activity names — e.g. one row reads "QIDS-SR16 Clinician-Administered").
2. **Tables 2 & 3 are tiled** into two column blocks — V10–V19 and V20–V29 ("(continued)") — that **overlap in their row sets**. Most body rows (including the universal ones: Concomitant medications, Adverse events, Vital signs, and the block-marked Weight, Tobacco use, Symptom-directed PE, ECG — plus, in T3, Review MMS) appear in **both** tiles and must be **union-merged** across them. Only a few rows are genuinely tile-specific — e.g. the source states "Endoscopic Procedure: Not applicable for responders during V10 through V19." **[RESOLVED 2026-07-21]** The original extraction merged by activity name with the V20–V29 tile as canonical order but wrongly treated the recurring rows as V20–V29-only, dropping their V10–V19 marks; this has been corrected in the sidecars (see the update note at top). Still open by design: the appended `CCI`/duplicate rows at the bottom of Tables 2/3 (unmatched V10–19-tile rows) were **not** merged — CCI names are redacted, so they are not safely name-mergeable (see CCI note below).

Given these two factors, this extraction was produced as a reviewed draft (you chose the Excel-checkpoint flow and approved proceeding). *(The interim draft Excel `NCT04677179_SoA_draft.xlsx` used for side-by-side tile checking was a working checkpoint and is no longer retained in this folder; the corrected resolved/consolidated HTML supersedes it.)*

## Specific things to verify against the PDF

- **Tile merge (T2/T3): [RESOLVED 2026-07-21]** the V10–V19 vs V20–V29 mark ranges per activity were verified against the PDF and the dropped V10–V19 marks restored via corrections (t2-c5..c38, t3-c3..c40). Still to check by hand: the trailing appended CCI/QIDS-SR16 duplicate rows (deliberately left unmerged — see CCI note).
- **Image-page labels:** spot-check activity names on the maintenance/extension/ETV pages (a handful retain section-fragment bleed or a missing space).
- **CCI rows:** every "CCI (redacted)" activity is a black-bar redaction in the source (confidential commercial information); marks preserved, name unknown. Multiple CCI rows per table are not name-mergeable.
- **Property values** (Weeks/Study day/tolerance) were read from the clean header pages and hardcoded per table; low risk but confirm.

## Consolidation result

Post-correction run (2026-07-21): 4 tables → **65 unified activities, 59% compression; review_queue = 0**. Four timelines render in the consolidated HTML (Screening/Induction main + Maintenance, Extension, ETV tracks). *(The pre-correction draft reported 67 / 57% / review_queue = 1; the shift reflects the applied name + mark corrections.)*
