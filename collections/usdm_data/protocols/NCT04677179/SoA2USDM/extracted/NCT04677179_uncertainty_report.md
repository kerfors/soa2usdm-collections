# NCT04677179 (J1P-MC-KFAH(b), INSTRUCT-UC) — Extraction Uncertainty Report

Ulcerative colitis. Source: `NCT04677179_soa.pdf` (30 pages = doc pp.17–46). Four tables.
Extractor: Claude Opus 4.8. Status: `ready_for_resolution`. **This protocol needed more reconstruction than the others — review more closely.**

## Tables & classification

- **Table 1 — `main_soa`.** Screening & Induction, V1–V9 (doc pp.17–23). Extracted cleanly (borders + clean text layer). Highest confidence.
- **Table 2 — `track`, "Maintenance (responders)".** V10–V29 (doc pp.24–32).
- **Table 3 — `track`, "Extension (nonresponders)".** V10–V29 (doc pp.33–42).
- **Table 4 — `track`, "Early Termination / Unscheduled / Post-Treatment".** ETV, V997, V801, V802 (doc pp.43–46).

Classification note: Tables 2/3 share the V10–V29 numbering but split the population (responders vs nonresponders at Week 12) into different treatment phases → treated as separate `track` timelines rather than `domain`. Documented judgment call.

## Method — and why this one is lower-confidence

The single-pass path was requested, but this SoA fought it on two fronts:

1. **~Two-thirds of pages are image-based** (table drawn as an image, text layer stored one glyph at a time — "Vital signs" → "V i t a l  s i g n s"). **X marks are single glyphs and were read reliably by coordinate.** Activity **labels were reconstructed** from the glyph stream with a gap threshold + cleanup dictionary; minor artifacts may remain (a few section-header fragments bled into adjacent activity names — e.g. one row reads "QIDS-SR16 Clinician-Administered").
2. **Tables 2 & 3 are tiled** into two column blocks — V10–V19 and V20–V29 ("(continued)") — that **list different activity subsets**. This is real, not an extraction gap: the source states "Endoscopic Procedure: Not applicable for responders during V10 through V19," and the universal rows (Concomitant medications, Adverse events, Weight, Vital signs, ECG) print only in the V20–V29 continued tile. The two tiles were **merged by activity name** with the V20–V29 tile as the canonical order. Verify the merge — particularly that rows carrying marks in only one column range reflect the source, and check the appended `CCI`/duplicate rows at the bottom of Tables 2/3 (unmatched V10–19-tile rows).

Given these two factors, this extraction was produced as a reviewed draft (you chose the Excel-checkpoint flow and approved proceeding). The draft Excel with both tiles laid out separately is in this folder (`NCT04677179_SoA_draft.xlsx`) for side-by-side checking.

## Specific things to verify against the PDF

- **Tile merge (T2/T3):** the V10–19 vs V20–29 mark ranges per activity, and the trailing appended CCI/QIDS rows.
- **Image-page labels:** spot-check activity names on the maintenance/extension/ETV pages (a handful retain section-fragment bleed or a missing space).
- **CCI rows:** every "CCI (redacted)" activity is a black-bar redaction in the source (confidential commercial information); marks preserved, name unknown. Multiple CCI rows per table are not name-mergeable.
- **Property values** (Weeks/Study day/tolerance) were read from the clean header pages and hardcoded per table; low risk but confirm.

## Consolidation result

4 tables → 67 unified activities, 57% compression; **review_queue = 1** item (one fuzzy activity-name match to check). Four timelines render in the consolidated HTML (Screening/Induction main + Maintenance, Extension, ETV tracks).
