# CDISC_Pilot — SoA extraction uncertainty report (prompt v3.7.2)

Source: `/root/ph3/blind/CDISC_Pilot/CDISC_Pilot_soa.pdf` (2 PDF pages = document pages 53–54 per PAGEMAP.md).
Deliverable: `CDISC_Pilot_Table_01_extraction.json` (one table).

---

## 1. How many tables, and why one

The excerpt prints **one logical SoA table as two horizontally tiled column blocks** (§5), not two tables.

- Page 53 is headed `Protocol Attachment LZZT.1` / `Schedule of Events for Protocol H2Q-MC-LZZT(c)`
  and carries VISIT columns 1–8. Page 54 is headed
  `Schedule of Events for Protocol H2Q-MC-LZZT(c) (concluded)` and carries VISIT 9, 10, 11, 12, 13, ET, RT.
- **The body rows repeat exactly.** All 28 printed body row bands appear on both pages, in the same order,
  with the same labels — `Informed consent`, `Patient number assigned`, … `Adverse events`. Page 54 does not
  continue the row list; it re-prints it under a different column block. That is the tiled-wide-table pattern,
  so the tiles were merged by activity name and the **union** of marks taken.
- It is therefore *not* `continuation` (the taxonomy requires identical column headers with rows continuing —
  here the columns differ and the rows repeat), *not* `domain` (no second activity category), *not* `track`
  (same participants, one continuous timeline), *not* `subsidiary`, *not* `reference`.
- `table_type: main_soa` — the rows are procedures performed on subjects and this is the anchor grid.

**Page coverage (§4).** Declared range 53–54. Every activity carries `source_page: 53` — 30 rows from page 53,
**0 rows from page 54**. This is the declared tiled-table exception, not a skipped page. Page 54 supplied no
rows but did supply all marks in columns 11–17, specifically for these activity rows:

| activity (row_position) | marks page 54 supplied (column_position) |
|---|---|
| Physical examination (7) | 15, 16 |
| Vital signs/Temperature (13) | 11–17 |
| ECG (16) | 11–16 |
| Concomitant Medications (19) | 11–17 |
| Laboratory (Chem/Hemat): (20) | 11–16 |
| Laboratory (Urinalysis) (21) | 11, 14, 16 |
| Plasma Specimen (Xanomeline) (22) | 11, 13, 16 |
| Study drug record (24) | 11–16 |
| TTS Acceptability Survey (27) | 15, 16 — **this row's only marks in the whole table** |
| ADAS-Cog (28), CIBIC+ (29), DAD (30) | 12, 14, 16, 17 each |
| NPI-X (31) | 11, 12, 13 (each `X` + marker `b`), 14, 15, 16, 17 |
| Adverse events (32) | 11–17 |

`TTS Acceptability Survey` is the row that proves the union rule matters: on page 53 its band is completely
empty, so a tile-A-only read would have delivered an activity with no schedule at all.

## 2. Column model — L = 2, first data column = 3

Raster rule-line recovery (§1d, 200 dpi, ink < 50% grey) gives, on page 53, eleven vertical rules at
x = 121.1, 238.1, 274.5, 305.8, 337.5, 368.8, 400.5, 431.8, 463.5, 494.8, 526.5 pt → ten columns; on page 54,
ten rules at 129.4, 237.1, 270.2, 305.5, 340.7, 376.4, 411.7, 447.3, 482.6, 518.2 pt → nine columns.

- **Label column 1** = the activity-name column (`ACTIVITY` is printed in it, on the second header row).
- **Label column 2** = the narrow column carrying the header rows' own labels, `VISIT` and `WEEK`.
- Both are excluded from `schedule_grid` / `activity_schedule`; **the first data column is position 3** and
  nothing was renumbered. Tile A = positions 3–10, tile B = positions 11–17. Fifteen data columns total.

**Position 8 is a real but entirely empty column — flagged.** On page 53 there is a rule-bounded column
between the VISIT 5 column (centre 416.2 pt) and the VISIT 7 column (centre 479.2 pt), spanning
431.8–463.5 pt. It carries **no VISIT value, no WEEK value and no mark in any of the 28 body rows**. The
printed VISIT sequence on page 53 is consequently 1, 2, 3, 4, 5, blank, 7, 8 and the WEEK sequence
-2, -.3, 0, 2, 4, blank, 6, 8 — i.e. **Visit 6 is absent from the source**. I have transcribed the blank column
as a data column with empty `cell_value` in both header rows rather than deleting it, so that VISIT 7 and
VISIT 8 keep their printed positions (9 and 10). I have **not** inferred a Visit 6, a week value for it, or
any mark in it. A reviewer should confirm this is the intended reading of position 8; the alternative
(drop the column and renumber 7→8, 8→9) would shift every downstream position by one.

## 3. Mechanical mark-check (§1b) — bbox column-binning, no disagreements

Method: `pdftotext -bbox` on both pages; column x-centres fixed from the **header** VISIT/WEEK labels and
cross-checked against the raster vertical rules above; row bands taken from raster horizontal rules read
inside the **activity-name column** (a text column, per §1d); every token assigned to the (band, column)
its centre falls in; mark tokens matched as `X`, `Xa`, `Xb`, `P` (the trailing superscript tokenises
separately here, so an `== 'X'` filter would not have dropped anything, but the binning does not rely on that).

The resulting matrix was diffed cell-for-cell against a direct visual read of both rendered pages.
**139 marks, zero disagreements** — the mechanical matrix and the visual read agree on every cell, including
the sparse rows (`Apo E genotyping` → column 6 only; `Patient randomized` → column 5 only;
`Laboratory (Urinalysis)` → 3, 6, 11, 14, 16) and the dense rows (`Vital signs/Temperature`,
`Adverse events` → all 7 columns in each tile).

- No cell values other than `X` and `P` occur in the grid.
- `P` is kept as a `cell_value` (legend-defined in-grid scheduling mark, §5), not converted to an annotation.
- Glyph case: every mark is upper-case `X`; no `x`, `✓`, `•` or arrow occurs. Nothing was normalised.

## 4. Merged marks, spans, arrows

**None.** No mark, text cell or arrow in this table spans more than one column, and no mark is centred across
two activity rows. No `source_range` and no `merged_cell_range` is set anywhere in the extraction, and no
`is_merged_cell` is true. The two header rows are one value per column throughout.

## 5. The one structural judgement call — `Study drug record` / `Medications dispensed` / `Medications returned`

These three lines sit inside **one rule-bounded row band** (page 53: 500.2–539.8 pt; page 54: 446.8–486.5 pt)
with **no internal horizontal rule** — verified on a magnified crop of the rendered page as well as by the
raster line detector. The `X` glyphs in that band are printed on the **first** line only, aligned with
`Study drug record`.

I transcribed them as **three activities** (`row_position` 24, 25, 26), with the marks recorded on row 24 only,
so rows 25 and 26 carry no marks anywhere in the table.

Reasoning, and the counter-argument, both stated so a reviewer can overturn it cheaply:
- *For three rows:* they are three grammatically complete, distinct procedure names, not a wrapped phrase.
  Merging them would produce a single compound `activity_name` no downstream consumer could map, and would
  silently assert that `Medications returned` occurs at Visit 3 — which the source does not mark.
- *Against:* this table does normally draw a rule between distinct activities (`Ambulatory ECG placed` and
  `Ambulatory ECG removed` each get their own band), and multi-line cells elsewhere in this table *are*
  single wrapped activities — `CT Scan (if not within` / `last year and patient passes` / `all other screens)`
  occupies one 3-line band, and `Plasma Specimen` / `(Xanomeline)` one 2-line band. Both of those I treated
  as one activity each. So the band-per-activity convention is not applied consistently by the source.

If a reviewer prefers the literal band reading, rows 24–26 collapse to one activity and the activity count
drops from 30 to 28; no mark changes.

## 6. Activities — counts and hierarchy

- 28 printed row bands → **30 activities** (the drug-accountability band expanded to 3, above).
- **Flat table:** every activity label in both tiles begins at the same x (125.94 pt on page 53), so every
  `indentation_level` is 0 and there are no organizational/section-header rows. This is the §4 flat-table
  exception where every level-0 row carries its own marks. `indentation_method` is left absent because the
  level came from the text layer's own left-edge geometry, not from a font signal or an assumption.
- No non-activity rows were created: the repeated `ACTIVITY` / `VISIT` / `WEEK` label band on page 54 is
  header, not an activity, and there is no notes/instruction column and no instruction-overflow row.

**Source inconsistency, transcribed not repaired:** page 53 prints `Hemoglobin A1C` (subscript `1C`,
upper-case C) and page 54 prints `Hemoglobin A1c` (lower-case c) for the same row. I took the tile-A
spelling `Hemoglobin A1C` for `activity_name` and did not silently harmonise the two; the difference is
noted here as a source defect.

## 7. Annotations — 8, all with printed locations

Footnote blocks are ordinary text-layer text under each grid; `annotation_text` was read from that text layer
(default method — no `annotation_text_source` recorded anywhere). Both pages print their own block and the
overlapping notes were **deduplicated by text** into one annotation each.

| marker | type | locations |
|---|---|---|
| `X` | legend | 130 `schedule_cell` locations — every cell whose value is a plain `X` |
| `a` | footnote | 1 `schedule_cell` — `Hemoglobin A1C` × column 3 |
| `b` | footnote | 4 `schedule_cell` — `NPI-X` × columns 10, 11, 12, 13 |
| `P` | legend | 4 `schedule_cell` — column 3 of `ADAS-Cog`, `CIBIC+`, `DAD`, `NPI-X` |
| `CT` | abbreviation | 1 `activity_name` — the `CT Scan …` row |
| `ECG` | abbreviation | 3 `activity_name` — `Ambulatory ECG placed`, `Ambulatory ECG removed`, `ECG` |
| `ET` | abbreviation | header cell, row 1 column 16 |
| `RT` | abbreviation | header cell, row 1 column 17 |

- **No synthesised markers.** Every marker is printed in the source: `X` and `P` as in-grid marks, `a` and `b`
  as the superscripts on `Xa` / `Xb`, and `CT` / `ECG` / `ET` / `RT` as the literal terms printed in an
  activity label or a header cell.
- **No synthesised `property_name` values** — `VISIT` and `WEEK` are printed in label column 2.
- **No orphans**: every annotation has ≥ 1 `marker_location`, and every marker in a `marker_locations` entry
  also appears in that element's `annotation_markers` (checked mechanically — 0 failures).
- **No `unresolved` locations** and **no non-default `method`** on any location, cell, activity or property.
  The raster was used only to recover rule-line geometry (column and row boundaries); all text and all cell
  contents came from the text layer, so the default provenance applies throughout (§1e).

### 7a. Abbreviation calls — the borderline one

§6 forbids a standalone abbreviation list bound by `text_match` or `synthesized`. I emitted four abbreviation
annotations, and each is bound by a **printed** occurrence, not by word overlap:

- `ET` and `RT` are unambiguous — they are printed as the VISIT-row header cells of columns 16 and 17
  (`ET = Early` … `Termination; RT = Retrieval`), and are the only expansion of those two column labels.
- `CT` and `ECG` are the borderline pair: they are printed inside activity labels (`CT Scan (if not within`,
  `Ambulatory ECG placed`, `ECG`). I read that as §6's "the term printed as a marker in … an activity label"
  rather than the forbidden "term merely occurring inside running text", and recorded no `method` on the
  locations because the term genuinely is printed there. A reviewer who disagrees should drop these two
  annotations; nothing else in the extraction depends on them.

### 7b. `ET` / `RT` location typing — schema tension, flagged

The `ET` and `RT` markers sit in **header grid cells**, and §6 is explicit that a per-column header marker must
attach to that column's `schedule_grid` cell rather than to the `schedule_property` row, so the footnote keeps
which column it governs. `location_type` has no `schedule_grid` member, so I used
`location_type: "schedule_property"` **with** `row_position: 1` and `column_position: 16` / `17`, and put the
marker on the `schedule_grid` cell's `annotation_markers` (not on the `schedule_property`'s). The schema's own
prose says to omit `column_position` for `schedule_property`; I kept it because dropping it would collapse the
abbreviation onto the whole VISIT row and lose the column. Flagged so the choice is visible.

### 7c. `X` legend fan-out

Per §6's worked example ("a legend `X`/`P` used as an in-grid mark → `legend` with `marker_locations` on the
cells that use it") the `X` legend carries 130 cell locations, and the marker-agreement rule then requires
`annotation_markers: "X"` on each of those 130 cells whose `cell_value` is already `"X"`. This is
self-referential but is what the two rules jointly demand; recorded here in case a reviewer wants the legend
scoped to the table instead. Cells that print `Xa` / `Xb` were bound to `a` / `b` only, not additionally to
`X`, so each qualified mark resolves to its own footnote.

### 7d. Annotation text overlap — re-verified, source-faithful

The §7/§8 containment check flags these; all were re-checked against the printed pages and **none is a split
note cell**:

- `X = Performed at this visit.` shares the long run `= Performed at this visit` with
  `Xa = Performed at this visit if patient is an insulin-dependent diabetic.` and with
  `Xb = Performed at this visit and via telephone interview 2 weeks following this visit.` These are **three
  separate printed legend lines** on page 53 defining three distinct symbols — source-faithful, not one cell
  split across rows. Nothing was merged, truncated or dropped.
- Page 54 re-prints `X = Performed at this visit.` and the `Xb` line verbatim; deduplicated to one annotation
  each, per §6.
- Page 54's abbreviation line contains page 53's as a prefix (`Abbreviations: CT = computed tomography; ECG =
  electrocardiogram` … `; ET = Early` / `Termination; RT = Retrieval`). Source-faithful — the concluded tile
  simply adds the two extra column labels. Because I split the block per term, no containment pair survives in
  the delivered JSON.
- One wording note: the page-53 `P` legend runs across three printed lines
  (`P = Practice only - It is recommended that a sampling of the CIBIC+, ADAS-Cog, DAD,` /
  `and NPI-X be administered at Visit 1. Data from this sampling would not be` /
  `considered as study data and would not be collected.`). I joined them with single spaces. The rendered page
  shows a wider gap after `Visit 1.` than the text layer records; I used the text layer's single space rather
  than inventing a double space. The text layer is **not** glyph-spread (§1c) — words are whole tokens — so no
  field was reconstructed.

## 8. Schedule properties

Two header rows, both with printed labels in column 2:

- row 1 `VISIT`, `property_type: visit`, `hierarchical_level: 1` — values are visit identifiers
  (`1`…`13`, `ET`, `RT`), not days or weeks.
- row 2 `WEEK`, `property_type: week`, `hierarchical_level: 2` — values are week numbers
  (`-2`, `-.3`, `0`, `2`, `4`, `6`, `8`, `12`, `16`, `20`, `24`, `26`).

Both levels are needed: without the WEEK row, columns 16 and 17 (`ET`, `RT`) would still be distinguishable,
but without the VISIT row several week values could not be told apart from one another across tiles.
The week value for VISIT 2 is transcribed exactly as printed, `-.3` — the source does not write it with a leading zero. The `ET` and `RT` columns carry
**no** WEEK value, and position 8 carries neither; those cells are emitted with empty `cell_value` rather than
being filled in. `structure_method` is not set — both `property_type` and `hierarchical_level` come from the
printed labels and the printed row order.

Note the second header row shares its printed line with the activity-column heading `ACTIVITY`; `ACTIVITY` is
the label column's own heading and was not modelled as a schedule property or an activity.

## 9. Summary of low-confidence calls, in priority order

1. `Study drug record` / `Medications dispensed` / `Medications returned` split into three activities from one
   rule-bounded band (§5 above). Highest-impact call; 30 vs 28 activities.
2. Empty rule-bounded column retained as data position 8, with Visit 6 absent from the source (§2).
3. `CT` and `ECG` emitted as abbreviation annotations bound to activity labels (§7a).
4. `ET` / `RT` marker locations typed `schedule_property` with a `column_position` (§7b).
5. `X` legend carrying 130 cell locations and a self-referential `annotation_markers` on each (§7c).
6. `Hemoglobin A1C` (page 53) vs `Hemoglobin A1c` (page 54) — transcribed as printed, not harmonised (§6).

No page was skipped, no mark was inferred, no footnote text was fabricated, and no marker was left orphaned.
