# NCT05324124 — SoA Extraction Uncertainty Report

Protocol J2G-MC-JZPA (Eli Lilly). Phase 1, open-label, randomized, 2-period, 2-sequence
crossover food-effect study of selpercatinib (160 mg) in healthy participants.
Source: `NCT05324124_soa.pdf` (4 pages = printed Document pages 9–12).

## Table(s)

One table extracted — `NCT05324124_Table_01_extraction.json`, type **`main_soa`**.

- Single logical SoA table spanning Document pages 9–12. The header (Study Phase → Days →
  Study Day) repeats at each page break; it is encoded **once** (not `continuation`), per the
  table-type convention.
- Counts: 20 activities, 41 schedule-grid (header) cells, 77 activity×timepoint cells,
  9 annotations. Schema-valid; all used markers resolve.

## SOURCE ISSUE — none blocking

- Completeness verified: `NCT05324124.md` places the SoA at Section 1.3, Document pages 9–12;
  `_soa.pdf` contains exactly those 4 pages. No missing continuation page or screening table.
  **No regeneration of `_soa.pdf` needed.**

## Column model

Established from the day-header row x-centres (bbox), consistent across all 4 pages:

| col | meaning | header value |
|----|----|----|
| 1 | activity label | Procedure |
| 2 | Screening | -28 to -2 days prior to Day 1 |
| 3–14 | Treatment Period | Days -1, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11 |
| 15 | ED | ED |
| 16 | Follow-Up phone call | within 7 to 10 days after last dose |
| 17 | Comments | (not a schedule column — excluded from grid) |

Dosing is Day 1 (col 4) and Day 8 (col 11); 24h reads fall on Day 2 (col 5) and Day 9 (col 12),
consistent with the crossover design (7-day washout between Day 1 and Day 8 doses).

Note: the markdown day-row rendered a doubled `ED|ED`; the PDF has a **single** ED column
(x≈571) between Day 11 and the Follow-Up column. bbox is authoritative.

## Header model / synthesized property names

Three header bands (all `property_name` synthesized because col-1 is blank or holds "Procedure"):

- Row 1 `Study Phase` (epoch): Screening; "Treatment Period" merged over cols 3–14; Follow-Up
  phone call. ED sits **outside** the Treatment Period band (white cell), labelled only in row 3.
- Row 2 `Days` (other): the single word "Days" merged over cols 3–14, labelling the numeric day
  columns. Does not cover Screening or ED. Classified `other` (unit/label band, not day values).
- Row 3 `Study Day` (study_day): Screening window, Days -1..11, ED, and the Follow-Up window.
  Mixed days + windows; classified `study_day` for the dominant numeric values.

## Merged-cell decisions

- "Treatment Period" (row 1) and "Days" (row 2) each distributed across cols 3–14 with
  `merged_cell_range "3:14"` / `is_merged_cell true` on every covered position.
- **Continuous arrow** (AE/Serious AE review, Concomitant medication review): a double-headed
  arrow (←===→) spans Days 1–11. Distributed one `activity_schedule` cell per column 4–14,
  `cell_value "←→"`, `source_range "4:14"`. Left head confirmed at Day 1 (col 4), right head at
  Day 11 (col 14) by 300 dpi render. These two activities also carry discrete X at Screening
  (col 2), Day -1 (col 3), and ED (col 15).

## Column-shift corrections (markdown wrong, bbox right)

- **Genetic sample**: markdown grid placed X under Screening; bbox + render show X at **Day 1**
  (col 4, x≈253). Extracted as Day 1.
- All other marks read from bbox coordinates; page 12 additionally confirmed by render because
  its text layer is character-spaced/mangled.

## Cell- vs activity-level markers

- Comments column → per-row `footnote` annotations with synthesized markers `n1`–`n8`, each
  linked to its activity row (`activity_name`): n1 discharge, n2 medical assessment, n3 height/
  weight, n4 pregnancy test, n5 12-lead ECG, n6 vital signs, n8 selpercatinib administration.
- `n7` (Clinical laboratory tests → "See Appendix 10.2, …") typed `source_note` (pure
  cross-reference), still linked to the activity row.
- In-grid `P` (predose) and hour timepoints (`2h`, `24h`, `1`) transcribed literally as
  `cell_value` — no explicit "P = predose" legend appears in the excerpt, so P is not made an
  annotation.

## Redactions (CCI) — nothing fabricated

- One **fully redacted activity row** on page 12 (large black block between "Genetic sample"
  and "Selpercatinib administration"): captured as `activity_name "CCI"`, no marks.
  ⚠ The redaction block is tall (~3 row-heights); it **may conceal more than one activity row** —
  unknowable from the source. Recorded as a single CCI row.
- The abbreviations line is truncated by a CCI redaction after "h = hour;". Captured the visible
  abbreviations (CRU, ECG, ED, h) as annotation `ab1` (type `abbreviation`, anchored to
  schedule_property row 1) with an explicit "[remainder … redacted — CCI]" note.

## Low-confidence calls / flags for review

1. **`Days` header row as `property_type "other"`** — it is a label band, not day values; the
   numeric days live in row 3. Reasonable alternatives exist but `other` avoids duplicating
   `study_day` semantics.
2. **CCI activity block may hide >1 row** (see Redactions). Flagged.
3. **Arrow glyph token** `"←→"` chosen to represent the continuous double-headed arrow across
   Days 1–11. Faithful to the merged-mark distribution rule; downstream may prefer a different
   canonical continuous token.
4. **`ab1` is intentionally "unused"** as a cell marker (orphan by design) — abbreviation lists
   anchor to the header row per convention, they are not placed in a cell.

## Orphan risk

None among footnotes: every synthesized marker n1–n8 appears on exactly one activity and each
resolves to a defined annotation. `ab1` orphan is by design (abbreviation list).
