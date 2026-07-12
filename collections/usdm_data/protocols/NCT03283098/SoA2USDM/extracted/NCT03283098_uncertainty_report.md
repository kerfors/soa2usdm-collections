# NCT03283098 — SoA extraction uncertainty report

Protocol: Amgen AMG 416 (etelcalcetide), Protocol 20140197, 25 May 2018. Hemodialysis population.
Section 7.1 "Table 1. Schedule of Assessments", document pages 32–36. Portrait tables (only the
"Screening" column header is rotated vertical). All grid cells are `X` (no timing-text).

Table 1 is split into three sub-tables sharing one 20-visit timeline → modeled as three files:

- **Table 01 — 1a. Schedule of Non-laboratory Assessments** (`main_soa`, pp 32–33). 14 activities, 94 `X` cells, 9 annotations.
- **Table 02 — 1b. Schedule of Laboratory Assessments** (`domain`, pp 34–35). 1 section header + 13 lab activities, 65 cells, 8 annotations.
- **Table 03 — 1c. Schedule of Pharmacokinetic Assessments** (`domain`, p 36). 1 section header + 6 PK activities, 23 cells, 1 annotation.

All three schema-valid; no orphan annotations; every element marker resolves; no marks on section-header rows.

## Marks — method
20-column tables; marks placed from `pdftotext -bbox` word coordinates (authoritative), cross-checked
against every rendered page and the markdown grid. Column x-centres taken from the day-number header row;
each `X` assigned by nearest centre.

## Shared timeline (all three tables, data cols 2–21)
Screening, -2, 1 (HD), 2, 3 (HD), 6 (HD), 8 (HD), 10 (HD), 13 (HD), 15 (HD), 17 (HD), 20 (HD), 22 (HD),
24 (HD), 27 (HD), 28, 29/ET (HD), 34 (HD), 41 (HD), 55 (HD). `(HD)` = hemodialysis day (legend).

## Judgement calls (please review)

1. **markdown/PDF disagreement — Table 1b column count.** The protocol markdown renders Table 1b with a
   **duplicated Day-1 column** ("1 (HD) | 1 (HD)"). The **PDF has only one Day-1 column** (bbox confirms
   20 day columns, identical to 1a). Extracted per the PDF (authoritative); the markdown doubling is a
   transposition artifact. Flag.

2. **"Study Visit (Day)" caption collapsed.** Each table has a "Study Visit (Day)" caption row above the
   day-number row. Modeled as **one** `study_day` property (level 1) carrying the day values, with the
   caption as the property name — rather than two header rows. Non-distinguishing caption; documented.

3. **Left-hand "Time" qualifier column (1b, 1c).** 1b and 1c have two left label columns (Time, Assessment).
   Resolve treats only physical col 1 as the label, so the day columns are indexed 2–21 (aligning with 1a)
   and the Time qualifier is handled per-table:
   - **1b:** the PDF text layer shows "Pre HD" **once** at the top of the Laboratory block (the markdown
     replicated it per-row; the PDF does not). Captured as a single synthesized annotation **t1** ("Pre HD
     … applying to all laboratory assessments") on the "Laboratory Assessments" section header — **not**
     folded per-row.
   - **1c:** the Time qualifier **varies per row** (Pre HD, SDA + 10/30/60/90 min, SDA + 18 to 30 hr) and is
     the only thing distinguishing six otherwise-identical "PK" rows, so it is **folded into activity_name**
     ("PK (Pre HD)", "PK (SDA + 10 min)", …). `cell_text` kept as raw "PK". The asymmetry (1b annotation vs
     1c name-fold) is driven by uniform-vs-varying Time. Confirm you're happy with this, or you'd prefer
     Time modeled as level-0 header rows (runbook §5 two-column-label-block style).

4. **1c `table_type` = `domain`.** 1c shares 1a's exact 20 day columns (finer timing is within-visit via the
   Time qualifier, not finer day granularity), so classified `domain` per the taxonomy's Amgen 1a/1b/1c
   example. `subsidiary` is arguable. Flag.

5. **1c footnote 'a' not restated on p 36.** The 29/ET header in 1c carries marker `a`, but page 36 shows
   only the abbreviation legend — the `a` text is not repeated. Reused the shared Day-29-withdrawal footnote
   text (as defined for 1a/1b on pp 33/35) so the marker resolves. Flag.

6. **Table 1a is a flat activity list.** No section headers in 1a; all 14 rows are activities at
   `indentation_level` 0 (legitimately carrying marks). 1b/1c do have level-0 section headers
   ("Laboratory Assessments", "Central Laboratory") with no marks.

## Annotation scoping
- **1a:** `a` + `h` on the 29/ET header (grid col 18) → resolve **column-scoped** to col 18. `b` → activities
  Body Height/Body Weight/Physical exam; `c,d,e,f,g,i` → their single activities.
- **1b:** `a` on 29/ET (col 18, column scope). `b` is **cell-level** on the Day -2 (col 3) cells of Albumin,
  Phosphorus, Calcium, Pregnancy, iPTH (5 cells). `c,d,e,f,g` → activity names (`d` on Albumin + Calcium).
  `t1` synthesized (Pre HD).
- **1c:** `a` on 29/ET (col 18, column scope) only.

## Not captured
Abbreviation legends (ET, HD, cCa, SDA, Kt/V, URR) not emitted as `abbreviation` annotations — no in-grid
markers; would be orphans. Consistent with prior extractions.

**STOP — awaiting your verification before running the pipeline.**
