# NCT04557384 — single-pass v3.1.0 re-extraction — uncertainty report

**Checkpoint. Pipeline not run / nothing committed.** Preview raws in scratch.

Protocol: Lilly I4T-MC-JVDU(e) (SC ramucirumab). **Image-only SoA** (no text layer on any of the 9 pages).
Supersedes the prior PDF→Excel→JSON raws (which you had Excel-verified — "KL").

## Method (§1a image-based)
No text layer, so marks were read from the rendered images (170 dpi). Two independent passes:
1. **Visual re-read** of all 9 pages, comparing every activity's marks against the prior raw.
2. **Rule-line + dark-pixel detector** (vertical/horizontal grid lines → cell boundaries; near-black pixel
   count per cell → mark present/absent), **validated cell-for-cell against the visual read on page 1**
   (e.g. Physical examination c2,c4,c5,c6,c7,c12,c16 — exact match) before trusting it.

## Result — all 3 tables confirmed, 0 corrections
- **Table 01** `main_soa` (§1.3, pp.16–22, 7 pages): 25 activities × 15 visit columns (Screening ≤28/≤7,
  Cycle 1–3 D1/D8/D15/D22, DX, V801). Every mark and every merged "See instructions" / "See Section 1.3.1"
  span matched the prior raw. Grey shading = Not Applicable → empty (confirmed). Instruction-only overflow
  rows (injection-site timepoint detail on p.20; other wrapped instruction cells) correctly **not** modelled
  as activities.
- **Table 02** `track`, `track_label` = "Continued Access" (p.23): 3 activities × 2 columns (Study Treatment
  501-5XX, Follow-Up 901). Matched exactly.
- **Table 03** `reference` (p.24): PK Sampling Schedule — 15 rows (Sample 1–14 + End of treatment) × 2
  collection columns (Ramucirumab PK, Immunogenicity). Rows are samples, not subject activities → correctly
  typed `reference`. All PK/IG X marks matched exactly.

No content changed. This re-extraction updates provenance to single-pass v3.1.0 and records the independent
image re-read. The prior extraction was Excel-verified by you; my check corroborates it.

## Merged marks / text (Table 01, carried over — all confirmed)
- Merged single "X" distributed across On-Treatment span [4:15]: Concomitant medication, AE collection.
- "See instructions" spans: Vital signs [4:6]; ECG / Pregnancy test / Thyroid panel / Radiologic imaging /
  Injection-spontaneous / Participant diary / Administer ramucirumab [4:15]; Injection-solicited [4:6];
  Administer combination medications split **[4:10] + [12:15]** (DX/col11 grey between).
- "See Section 1.3.1" [4:16]: PK, IG.

## Inline references (v3.1.0 §6)
**None to rework.** Parentheticals in activity/sample labels ("(solicited)", "(spontaneous)", "(Cohorts B
and C only)", "Sample n (Cycle x, Day y, …)") are qualifiers/descriptors, not section/appendix/attachment
references. Section/appendix cross-references (Appendix 2, Section 10.3, 8.2.5, 1.3.1, 6.1) live in the
right-hand **Instructions column** and are modelled as `source_note`/`footnote` annotations (i1–i23),
per §6 — not inline in activity names.

## Annotations
- Table 01: 26 (footnote `a` = Short-term follow-up definition; i1–i23 Instructions-column notes; +others).
- Table 02: 4 (footnote `a` continued-access follow-up; i1–i3 Instructions notes).
- Table 03: 0 (no markers; sample attributes fold into row identity).
All markers defined and resolve; no orphan annotations.

## Low-confidence / for review
None from the mark matrix — full agreement between the visual re-read, the pixel detector, and the prior
user-verified raw across all 9 pages. Annotation text carried over unchanged from the prior raw.

## Validation
All 3 tables: schema OK. `track_label`="Continued Access" on Table 02 only. soa_pages unchanged
(16-22 / 23 / 24).
