# NCT02291289 — SoA extraction uncertainty report

Protocol: Bevacizumab MO29112 (metastatic colorectal cancer, mCRC), Version 9. The SoA is **five appendices** (11.1–11.5), extracted as **five tables**. All schema-valid; no orphan annotations; every element marker resolves; every annotation is referenced.

| Table | Appendix | type | track_label | pages | cols | activities | annotations |
|-------|----------|------|-------------|-------|------|-----------|------------|
| 1 | App 1 Screening/Baseline + Induction (All Patients) | main_soa | — | 169–171 | 6 | 21 | 20 (a–t) |
| 2 | App 2 Maintenance | track | Cohort 1 | 174–176 | 4 | 25 | 24 (a–x) |
| 3 | App 3 Maintenance | track | Cohort 2 | 179–181 | 4 | 24 | 21 (a–u) |
| 4 | App 4 Maintenance | track | Cohort 3 | 184–185 | 4 | 22 | 21 (a–u) |
| 5 | App 5 Maintenance | track | Cohort 4 | 189–191 | 4 | 26 | 23 (a–w) |

Classification per your decision: Appendix 1 = **main_soa** (shared anchor); Appendices 2–5 = **track** (per-cohort Maintenance timelines, `track_label` Cohort 1–4).

## Image-based scanned tables
All grid pages are **scanned images** (no text layer). Marks and text cells were read **visually** from the rendered pages. The **footnotes are text** — the ones on the dedicated footnote pages were extracted programmatically (`pdftotext`) to capture their full wording verbatim; the header/early footnotes printed on the grid images (App1 a–e, App2 a–j, App3 a–e, App5 a–g) were read visually. App5 footnote g spans a grid page and continues on the following text page (assembled). Recommend a spot-check of the resolved grids.

## Completeness
- `_soa.pdf` = 26 pages = doc pp169–194. Appendices 1–5 occupy doc pp169–193. The tail of the last page (doc p194) begins **Appendix 6 (FOLFOX Regimens)** — a dosing-regimen table, **not** a Schedule of Assessments — so it is out of scope and not extracted. Excerpt is complete for the five SoA appendices.
- Footers give exact doc pages; page_start/page_end set accordingly per appendix.

## Structure & modelling
- **Marks are lowercase "x"** throughout (Roche house style); transcribed literally. A few cells render as uppercase "X" in the scan (e.g., App3 HIV serology, TSH/Pulse-oximetry discontinuation cells) — treated as the same scheduling mark and normalised to "x"; flagged.
- **Header**: App1 has 3 header rows — a top **condition band** ("Patients who have PD … not eligible for any study cohort", cols 6–7) modelled as a level-null `condition` property, over a `epoch` phase row and a `timepoint` timing row. Apps 2–5 have 2 header rows (`epoch` phase + `timepoint` timing). Phase labels carry footnotes a/b/c (Maintenance/Discontinuation/Follow-up).
- **Text cells transcribed verbatim** as `cell_value`: "As required", "If clinically indicated", "Mandatory at end of Induction Treatment Phase", "According to local standard of care", "Every cycle", "At 6 months", "Prior to Cycles 1, 4, 7…", "Experimental arm only", "No sample collection / Supplemental Biomarker Program CLOSED", multi-line tumour-assessment and pregnancy-test schedules, etc. Multi-line cells are kept as a single space-joined string.
- **Merged mark**: App1 "Study medication administration" shows "x Administered every 2 weeks" spanning the two Induction columns (4–5) → distributed with source_range "4:5".
- **Footnote c** in every table links the Post-Treatment Follow-Up header **plus** the "Subsequent anti-cancer therapies (see [c])" and "Patient survival (see [c])" activities (the "(see [c])" cross-references). Footnote **r** in App1 dedups over Whole blood + Plasma samples.
- **Un-marked activities** (no bracketed footnote): Cohort-specific informed consent, Stool sample (all maintenance tables), and TSH/T3/T4 + Pulse oximetry (Cohorts 2 & 4) — modelled without annotation_markers, as printed.
- Inline section/appendix references inside footnote text (e.g., "See Appendix 8", "Section 4.4") left in the footnote wording, not split out.

## Cohort-specific content (read faithfully, not copied between cohorts)
- Cohort 1 (App2): Head & neck / Chest CT / Dermatology / Anal-pelvic exams for SCC (experimental arm).
- Cohort 2 (App3): TSH/T3/T4, Pulse oximetry, Tuberculosis test, HIV/HBV/HCV serology (atezolizumab arm).
- Cohort 3 (App4): LVEF cardiac monitoring; arm-specific maintenance cadence (control every 2 two-week cycles / experimental every 3-week cycle) and pregnancy-test schedule; HER2 (trastuzumab/pertuzumab).
- Cohort 4 (App5): LVEF, Ophthalmology exam, TSH, Pulse oximetry, TB test, serology; the per-cohort cycle lists differ (e.g., Pulse oximetry Cohort 2 "Prior to Cycle 1 then every 2 cycles" vs Cohort 4 "Prior to Cycles 1, 3, 5, 7…") — transcribed as printed for each.

## Low-confidence calls
1. Marks/text read visually from scans (no text layer); footnote wording pulled from the text layer where available.
2. Lowercase "x" used for all marks; a few scan-rendered uppercase "X" normalised.
3. App1 top band modelled as a level-null `condition` property (vs folding into the phase cells).
4. Multi-line grid text cells joined into one string (line breaks not preserved).
5. Appendix 6 (FOLFOX regimens) on doc p194 excluded as non-SoA.
