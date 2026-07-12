# NCT04320615 — SoA extraction uncertainty report

Protocol: Tocilizumab (RoActemra/Actemra), COVID-19, Roche WA42380, Version 3. The SoA is split into three
appendices (landscape), document pages 77–85:

- **Table 01 — Appendix 1: Days 1 and 2** (`main_soa`, pp 77–80). 28 rows (1 section header + 27 activities),
  56 cells, 21 annotations. 5 data columns (Screening, Baseline/Day 1 pre-dose + post-infusion timepoints, Day 2).
- **Table 02 — Appendix 2: Days 3–28** (`main_soa`, pp 81–83). 17 rows, 220 cells, 12 annotations. 27 data
  columns (Days 3–28 + Study Completion/Discontinuation).
- **Table 03 — Appendix 3: After Day 28** (`main_soa`, pp 84–85). 14 rows, 32 cells, 8 annotations. 3 data
  columns (Day 35, Day 45, Day 60/Study Completion).

All three schema-valid; no orphan annotations; every element marker resolves; no marks on section-header rows.

## Marks — method
Wide landscape tables (up to 27 columns); marks placed from `pdftotext -bbox` word coordinates
(authoritative), column x-centres taken from the Study Day header row, each `x` assigned by nearest centre.
Every rendered page was read and cross-checked. Markdown used only for footnote wording.

## Judgement calls (please review)

1. **Three appendices modeled as three sequential `main_soa` tables.** They cover different, non-overlapping
   day ranges (Days 1–2 intensive → Days 3–28 → After Day 28) with different column structures — independent
   schedules per the taxonomy, so each is `main_soa` (not domain/continuation). They represent consecutive
   phases of one participant journey. Flag if you'd prefer a different grouping.

2. **"Optional" merged cells (PaO2/FiO2).** Source shows "← Optional →" spanning the post-baseline columns.
   Distributed per the merged-mark rule: App 1 across cols 3–6 (`source_range` 3:6); App 2 across cols 2–27
   (`source_range` 2:27) **plus** a standalone "Optional" in the Study Completion column (col 28). App 3 has no
   PaO2/FiO2 row.

3. **Header property modeling.** Each appendix's stacked header bands modeled as separate properties:
   App 1 → Epoch (Screening/Baseline) + Study Day (−2 to 0 / Day 1 / Day 2, Days 1–2 merged over their
   timepoint columns) + Timepoint (0 Pre-dose, 15 min post-infusion, 24 hrs, 36 hrs). App 2 → Epoch
   ("Days 3-28" merged 2:27 + Study Completion/Discontinuation) + Study Day. App 3 → Epoch (Study Completion
   over col 4) + Study Day (Assessment Window). Columns with no epoch label (App 1 timepoint cols 4–6; App 3
   Days 35/45) left with empty epoch. Documented.

4. **Header footnote markers.**
   - App 1: `a` + `b` on the Screening header (grid col 2) → column-scoped.
   - App 2: `a` on the merged "Days 3-28" phase — placed on the representative col 2 cell (resolves
     column-scoped to col 2); the footnote governs the whole Days 3–28 phase. Flag.
   - App 3: `a` on the Day 35 and Day 45 day cells (cols 2–3) → resolves column-scoped to those two columns.

5. **Cell-level markers (App 1).** `o` on the Serum PD Day-1 cells (cols 3–4); `q` on the Serum PK Day-1
   cells (cols 3–4). Footnote `p` is on the Serum PK activity name (extra-sample instruction).

6. **Footnote dedup within a table.** App 1 `h` (SpO2 + Vital signs); App 2 `b` (Vital signs + SpO2); App 3
   `c` (Vital signs + SpO2) — one annotation, two activity locations each.

7. **General Notes.** App 1 ("all assessments prior to dosing") and App 2 ("discharged patients within
   ±3 days") captured as synthesized `note1` (table-scoped). App 3 has no general note.

8. **Repeated footnote wording is intentionally NOT cross-deduplicated across appendices** (each appendix is a
   separate extraction file). Note that App 2/App 3 vitals, hematology, and chemistry footnotes add a
   "…not recorded/performed if follow-up visits are conducted by telephone" clause absent from App 1 — kept
   as distinct text per table.

## Not captured
Abbreviation lists (per appendix) not emitted as `abbreviation` annotations — no in-grid markers; would be
orphans. Consistent with prior extractions.

**STOP — awaiting your verification before running the pipeline.**
