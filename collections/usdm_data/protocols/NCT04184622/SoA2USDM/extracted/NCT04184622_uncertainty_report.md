# NCT04184622 — single-pass v3.1.0 re-extraction — uncertainty report

**Checkpoint. Pipeline not run / nothing committed.** Preview raws in scratch.

Protocol: Lilly I8F-MC-GPHK(b) (tirzepatide, Study GPHK). Text-layer tables.
Supersedes the prior PDF→Excel→JSON raws (which you had Excel-verified — "KL").

## Method & result
Both tables' mark matrices were **independently re-derived from the PDF** (`pdftotext -bbox` word
coordinates binned to visit-header column centres; rule-line horizontal bands per row; X* footnoted marks
counted) and diffed **cell-for-cell** against the prior raws.

- **Table 01 (main_soa, 54 activities, 24 visit columns): full match, 0 discrepancies.**
- **Table 02 (track = Prediabetes, 39 activities, 19 visit columns): full match, 0 discrepancies.**

No content changed. This re-extraction only updates provenance to single-pass v3.1.0 and records the
independent bbox re-confirmation. The prior extraction was already a text-coordinate method and
Excel-verified by you; my check corroborates it.

Verification subtleties handled during the diff (all confirmed correct in the prior raw):
- Fasting Visit / Telephone Visit are `schedule_properties` (condition / modality), repeated as header rows
  on every continuation page — de-duplicated so they aren't double-counted.
- 5 lab rows (Urinary albumin/creatinine, Cystatin-c, Calcitonin, Hematology, TSH) carry an **X\*** at
  Visit 1 (Screening) — footnoted marks, correctly present in the prior raw.

## Tables
- **Table 01** `main_soa` — §1.3.1, pp.18–21. 5 header properties (Visit, Week of Treatment, Allowable
  Deviation window, Fasting Visit condition, Telephone Visit modality). 24 columns V1–V21, V99, ED, V801.
  27 annotations (n1–n27).
- **Table 02** `track`, `track_label` = "Prediabetes" — §1.3.2, pp.22–24. 19 columns V101–V116, V199, ED,
  V802. 39 activities, 18 annotations.

## Inline references (v3.1.0 §6)
**None to rework.** Parentheticals in activity labels ("(3 sitting BP and HR)", "(includes PK sample)",
"(Baseline/Screening Version)", "(include Cr for eGFR …)") are assay/version qualifiers, not
section/appendix/attachment references — correctly retained in `activity_name`. The intro's "Section 10.10
Appendix 10" is a general SoA note, not tied to an activity label.

## Low-confidence / for review
None from the mark matrix (full cell-for-cell match). Annotation semantics (n1–n27) are carried over
unchanged from the prior user-verified raw and were not re-adjudicated.

## Validation
Both tables: schema OK. soa_pages unchanged (18-21 / 22-24). `track_label` set on Table 02 only.
