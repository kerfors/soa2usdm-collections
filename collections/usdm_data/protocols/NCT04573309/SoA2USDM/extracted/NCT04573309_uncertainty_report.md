# NCT04573309 — SoA extraction uncertainty report

Protocol: ALXN1840 (bis-choline tetrathiomolybdate), Wilson disease, ALXN1840-WD-204, Protocol
Amendment 3.1 (US), 18 Mar 2022. Section 1.3, document pages 14–17.

Two tables:

- **Table 01 — Table 1: Schedule of Activities** (`main_soa`, landscape, pp 14–16). 8 section headers +
  35 activities = 43 rows, 212 `X` cells, 24 footnotes (a–x).
- **Table 02 — Table 2: PK/PD Assessments on Days 1, 25, 29, 39** (`subsidiary`, portrait, p 17). 1 combined
  PK/PD activity, 8 cells, 3 annotations.

Both schema-valid; no orphan annotations; every element marker resolves; no marks on section-header rows.

## Marks — method
Wide 24-column landscape table; marks placed from `pdftotext -bbox` word coordinates (authoritative), with
column x-centres taken from the Days header row and each `X` assigned by nearest centre. Every rendered page
was read and cross-checked. The protocol markdown is heavily transposed for this table and was used only for
footnote wording.

## Structure
Table 1 header repeats across pages 14–15 (rows continue) → merged into one `main_soa` file. 24 data columns
(positions 2–25): Screening (2–3), C-I check-in (4), Inpatient Period 1 (5–12), OP outpatient (13), Day 23
(14, no epoch), Inpatient Period 2 (15–23), UNS unscheduled (24), EOS/ET (25).

## Judgement calls (please review)

1. **Two epoch header bands collapsed.** The source stacks two partial epoch rows (top band:
   Screening^a / C-I^b / UNS^d / EOS or ET^e; second band: Screening / C-I / Inpatient Period 1 / OP^c /
   Inpatient Period 2). These are complementary halves of one epoch dimension (not a clean 2-level
   hierarchy), so modeled as **one** `Epoch` property (level 1) plus `Days` (level 2). Documented.

2. **Column 14 (Day 23) has no epoch label.** It sits between OP (col 13) and Inpatient Period 2 (cols
   15–23) — the readmission/transition day (footnote c). Left with an empty epoch value. Flag.

3. **PK/PD activity name wraps across the page break.** "Blood sampling for PK: Plasma total Mo and PUF-Mo"
   (bottom of p 14) and "PD: Plasma total and PUF-Cu, LBC, ceruloplasmin, ceruloplasmin-bound Cu" (top of
   p 15) are one table row split by pagination (the PD line carries no marks of its own). Modeled as a
   **single** activity with the page-14 marks. Confirm.

4. **"Discontinue …" trailing arrows.** "Discontinue chelation therapy" (X at Day -4to-1, col 7) and
   "Discontinue zinc therapy" (X at Day -21, col 3) each show a right-arrow running to the study end,
   indicating the discontinuation persists. Captured as the **single starting X only** (no fabricated marks
   in the spanned columns). Flag — let me know if you want the arrow modeled as a spanned cell value.

5. **Cu/Mo-controlled meals — grid vs footnote.** The grid marks meals at cols 5–11 (Day -7 to 8) and 14–22
   (Day 23 to 39); footnote v describes "inpatient Period 1 (Day -8 to Day 9)" and "Period 2 (Day 23 to
   Day 40)". Transcribed the **grid as shown** (per transcribe-not-infer); the footnote text is captured
   verbatim as annotation v. Flag for awareness.

6. **Table 2 = `subsidiary`.** Finer within-visit PK/PD timing (hours -0.5 to 24) for the Table 1 PK/PD
   activity on Days 1/25/29/39 — classic subsidiary. Its general Note is captured as synthesized `note1`
   (table-scoped). Marker `a` sits on the timepoint header (property scope); `b` on the 24 h cell.

## Annotation scoping (Table 1)
- **Column-scoped:** epoch markers a (Screening), b (C-I), c (OP), d (UNS), e (EOS/ET) on grid cols 2/4/13/24/25.
- **Cell-scoped:** g on the Day 10–22 (col 13) cells of Outpatient visit, Chemistry, Urinalysis (3 cells);
  r on Chemistry Day -4to-1 (col 7) and 26–28 (col 17) cells; p on PK/PD cells (cols 8, 16, 18, 22);
  n on Study intervention compliance Day 10–22 (col 13) cell.
- **Activity-scoped:** f, h, i, k, l, m, o, q, t, u, v, w, x on their activities; **j** dedup across WD history
  + Prior WD treatment; **s** dedup across pregnancy test + menstruation-check row.

## Not captured
Abbreviation lists (p 16, p 17) not emitted as `abbreviation` annotations — no in-grid markers; would be
orphans. Consistent with prior extractions.

**[Pre-pipeline sign-off gate — superseded 2026-07-21.]** The pipeline has since been run (resolved/consolidated outputs exist), so this gate no longer applies. The items above are documented extraction decisions, consistent with prior extractions — no unresolved blocker.
