# NCT03548935 — Uncertainty report (PDF_TO_JSON_PROMPT v3.8.1)

Source: NCT03548935_soa.pdf (6 PDF pages = document pages 8–13 per PAGEMAP.md), Novo Nordisk NN9536-4373, Protocol v2.0, 21 December 2017, Section 2 "Flowchart". No protocol markdown was available, so all text comes from the PDF text layer.

## Decisions needed (3)

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | p.11, rows 60–61 (PHQ-9, C-SSRS) | Recorded as children (level 2) of the "Clinical Outcome Assessments (9.4.1)" group row. They have the light child-row shading and sit directly under that group, but their text indent is the same as the level-1 rows. | Record them at level 1, as siblings of the group row, going by text indent alone. | 3.3 |
| D2 | p.8 (and p.11), row 4 (Visit Window), column 3 (V2) | Recorded as "±0", which is what pages 8, 9, 10, 12 and 13 print. Page 11 prints "0" in the same cell. | Record "0", or treat the two readings as the same value. | 3.2 |
| D3 | p.8, row 1 (unlabelled top header band) | Typed as epoch and named "Study period" (a synthesised name). "Randomisation", "End of treatment" and "End of trial" are kept as values of that band, alongside Screening, Dose escalation period and Maintenance period. | Type the band as period, or treat Randomisation / End of treatment / End of trial as visit-level labels rather than epochs. | 3.2 |

## Recorded, not open (6)

- §2 / type definitions: one printed table running over pages 8–13 under a reprinted header is extracted as ONE table, not as continuations (the source gives the overflow no number or caption of its own).
- §1b: header rows reprint on all six pages. They were de-duplicated and each is counted once.
- §5: L = 1 label column, so the data columns are positions 2–26 (25 visits V1…V25).
- §6: inline section and appendix references were taken out of activity names and stored as `source_note` annotations pr1–pr30, one per distinct reference. References printed together on one label, e.g. "(7.1, 7.6)", were split into separate notes.
- §6: no legend or abbreviation annotations were emitted. The source prints no legend for "X" and has no abbreviation block in the table.
- §3: property name "Study period" is synthesised (label cell empty), with `synthesized: true`.

## 1. Tables

| Table | type | title | pages | columns | activity rows |
|---|---|---|---|---|---|
| 1 | main_soa | Flowchart | 8–13 | 25 data columns (2–26) | 78 (row positions 5–82) |

Activity rows per page: p.8 = 13, p.9 = 15, p.10 = 13, p.11 = 16, p.12 = 15, p.13 = 6. Every page in the declared range contributed rows. Page 13 also carries footnotes a–g.

Classification: the only SoA table in the excerpt, and the anchor grid for the whole trial, so it is `main_soa`. There are no other tables, so the domain, track and subsidiary types do not apply.

## 2. Mechanical mark-check

- The pages are portrait with the table rotated 90°. The text layer is present and not glyph-spread. The vector rule lines were not inspected with pdfplumber (the prompt forbids it). Rule lines were recovered from the 200 dpi raster (§1d): 27 vertical rules, the same on pages 8–12. Page 13's shorter table matches at the rules it detects. Horizontal row bands were taken from the label (text) column.
- Every `pdftotext -bbox` token was binned into (row band × column) cells. All body cell tokens are `X`, plus `X e` (DEXA scan, V1) and `X g` (Attend visit fasting, V25), whose superscripts were split out as markers.
- Merged cells were checked per row: no internal vertical rule is missing in any body row, so no body cell is merged and nothing is distributed. The only merges are in header row 1: Dose escalation period = 4:11 and Maintenance period = 12:24.
- The bbox matrix was diffed against a visual read of every page render (pages 8–13). **No disagreements.** Some marks print at a slightly higher baseline inside two-line rows (e.g. "Evaluation of diet and physical activity" at V20/V22; "Diet and physical activity counselling" at V20–P23; "Training in trial product" at V20/V22). `-layout` text splits these across lines, but row-band binning places them correctly in the same row, and the renders confirm it.
- Visit-label token: "V20P21V22" is a single text-layer token. It was split into V20 / P21 / V22 (columns 21–23) using the rule-line column boundaries and the week row values 52 / 56 / 60.
- Tallies: 428 marked cells.

## 3. Low-confidence calls

### 3.1 Annotation text integrity
The footnotes a–g on p.13 are one-line text-layer notes, and each was read complete. No two annotations overlap or contain one another, and there is no notes column.

### 3.2 Header properties
- Row 1 (synthesised "Study period", epoch, level 1): see D3.
- Row 2 "Visit (V), Phone (P)": visit, level 2.
- Row 3 "Timing of Visit (Weeks)": week, level 3.
- Row 4 "Visit Window (Days)": window, level null, because its values repeat across columns and do not tell them apart. V2 is "±0" (see D2).

### 3.3 Hierarchy
Indentation was judged from token x-offsets on the rendered geometry plus row shading (`indentation_method: visual_estimate` on every activity):
- The six dark-grey section rows (SUBJECT RELATED INFORMATION AND ASSESSMENTS, EFFICACY, SAFETY, OTHER ASSESSMENTS, TRIAL MATERIAL, REMINDERS) are level 0 and carry no marks.
- The group rows Body measurements, Vital Signs (×2), Clinical Outcome Assessments (×2) and Administration of trial product are level 1 and carry no marks.
- Their light-shaded children are level 2.
- Wrapped labels hang to the left margin on their second line; the first-line indent was used.
- Page 13 rows are taken to continue under REMINDERS, because page 13 has no new section header.
- PHQ-9 / C-SSRS: see D1.

### 3.4 Text as printed
- "Hand out and instruct in PK dairy" ("dairy" is kept as printed; it is probably meant to be "diary").
- "Systolic blood Pressure" keeps its mixed casing as printed.

## 4. Orphan risk / markers
- Footnotes a–g are all printed and all bound:
  - a: Informed consent label
  - b: Childbearing potential, History of Breast Neoplasm, ICIQ-UI-SF and Breast neoplasms follow-up labels (4 locations)
  - c: Tobacco Use
  - d: DEXA scan label
  - e: DEXA scan cell at V1 (col 2)
  - f: Biosamples label
  - g: Attend visit fasting cell at V25 (col 26)
- The source_note texts pr1–pr30 are given as "Section x.y" / "Appendix n". The source prints only the bare number, which is a hyperlinked cross-reference, so the word "Section" was added for readability.
- No marker is undefined, and no annotation lacks a location.

## 5. Synthesised markers and method provenance
- Synthesised markers: pr1–pr30. These are inline-reference source notes, bound by the printed reference inside each label, so the locations carry no `method`.
- Synthesised property name: "Study period" (row 1).
- `indentation_method: visual_estimate` is set on all 78 activities.
- No `unresolved` locations, and no proximity- or text_match-bound locations.
