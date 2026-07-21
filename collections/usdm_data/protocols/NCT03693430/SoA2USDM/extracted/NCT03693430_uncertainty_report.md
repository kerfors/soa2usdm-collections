# NCT03693430 — SoA extraction uncertainty report

Protocol: NN9536-4378 (semaglutide 2.4 mg, obesity), Novo Nordisk, Protocol v3.0 Final, 29 June 2018. One SoA table (Section 2 "Flowchart"). Schema-valid; no orphan annotations; every element marker resolves; no scheduling marks on organizational headers.

## Image-based — marks by validated image-detection
This `_soa.pdf` is **genuinely image-based** (4-char text layer; each page is one full-page image). There is no text layer to parse, so marks were reconstructed by **calibrated image mark-detection**: grid geometry from the vertical/horizontal rule lines (34 visit columns + label column), then a per-cell dark-pixel count with a fixed threshold. The method was **validated against direct visual reads** on representative full-width rows — Concomitant medication (V1–V33), Trial product compliance (P3–V33), Body weight/Waist/SBP/DBP (clinic-visit pattern), Adverse event (V2–V34), the quarterly labs (HbA1c/FPG/Lipids/Biomarkers at V12/V20/V28/V33), the sparse Fasting serum insulin (V2, V33 only), the Evaluation rows (phone visits P13/P21/P29), and Diet counselling — all matched exactly. **Recommend a spot-check of the resolved grid** given ~470 marks were read by detection rather than by eye on every cell.

## Completeness
- `_soa.pdf` = 4 pages = doc pages 9–12 (footers "9/10/11/12 of 94"). page_start/page_end = 9–12.
- Section 2 Flowchart is the only SoA; complete across the 4 pages (footnotes a–d on doc p12). The `.md` has no usable text version of the flowchart (image PDF).

## Structure
- **main_soa**; same family/conventions as sister trials NCT03548987 / NCT03548935 (verified against their extractions). Header (4 rows) repeated across doc pp 9–12 → merged into one file, encoded once.
- **34 visit columns** (schema cols 2–35): V=in-clinic, P=phone. Epochs: Screening (col 2), Randomisation (col 3), Dose escalation period (cols 4–11, merged), Maintenance period (cols 12–33, merged), End of treatment (col 34), End of trial (col 35).
- Header properties: row 1 Study Phase (`epoch`, synthesised name), row 2 Visit (`visit`), row 3 Timing of Visit Weeks (`week`), row 4 Visit Window Days (`window`).
- **63 rows**: 5 grey ALL-CAPS section headers (indent 0, mark-free) + 4 sub-parents (Body measurements, Vital signs ×2, Administration of trial product — indent 1, mark-free) with children at indent 2 (Height/Body weight/Waist, SBP/DBP, Pulse, Dispensing visit/Drug accountability). All other rows indent 1.
- **Inline section references kept in `activity_name`** (e.g., "Inclusion criteria (6.1)", "HbA1c (Appendix 2)") — matching sister-trial practice; they are NOT modelled as source_note annotations.

## Marks — points to review
1. **Lowercase "x"** — Control of Eating Questionnaire (CoEQ) uses a lowercase "x" mark (vs "X" everywhere else); transcribed literally as `cell_value` "x" (V2, V12, V20, V33). No legend distinguishes x/X — likely stylistic.
2. **Labs vs Evaluations on different columns** — quarterly labs (HbA1c/FPG/Lipids/Biomarkers/Haematology/Biochemistry/ECG) mark clinic visits V12/V20/V28 (weeks 20/52/84), while "Evaluation of lipid-lowering/antihypertensive/glycaemic" mark the next phone visits P13/P21/P29 (weeks 24/56/88). Confirmed by zoom — labs drawn at clinic, reviewed by phone ~4 weeks later.
3. **Fasting serum insulin** has only 2 marks (V2 Randomisation, V33 End of treatment) — verified, genuinely sparser than the other labs.
4. **Two follow-up rows** (Breast neoplasms, Colon neoplasms) mark only End of treatment (V33) + End of trial (V34). On page 3, the "Breast neoplasms follow-up" row is laid out between the repeated epoch band and the Visit row; treated as a normal SAFETY activity in reading order.

## Source defect
- **V33 (End of treatment) visit window** prints **±3 on doc pp9–10** but **±5 on pp11–12** (repeated-header disagreement). **[VERIFIED 2026-07-21]** Both values are legibly printed on the repeated header — checked against the rendered SoA pages, neither is a scan artifact (V34 End-of-trial reads "0 to +5" on both), and the protocol body never restates the EOT window. This is therefore an unresolvable **source contradiction**, not an open extraction question. Encoded **±3** (first occurrence); treat ±3/±5 as source-ambiguous downstream.
- Top-right of every page has a redaction box over the running-head document title (cosmetic; not part of the table).

## Low-confidence calls
1. Marks read by image-detection, not per-cell visual transcription (method validated on ~12 representative rows; column geometry exact).
2. CoEQ lowercase "x" preserved as "x".
3. V33 window encoded ±3 (source contradicts itself — ±5 on later pages; confirmed source defect 2026-07-21, see above).
4. Sub-parent indentation (Body measurements/Vital signs/Administration of trial product at indent 1, children indent 2) inferred from grey shading + indentation, consistent with sister trials.
