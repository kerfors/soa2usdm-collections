# NCT05051579 (J2A-MC-GZGI(a)) — SoA extraction uncertainty report

Layer 1, single pass, PDF_TO_JSON_PROMPT v3.7.3, schema `soa-table-extraction` 1.0.
Source: `NCT05051579_soa.pdf`, 6 PDF pages = document pages 13–18 per `PAGEMAP.md`.
The printed footers read 12–17, i.e. exactly one lower than the document page, as PAGEMAP warned; no
page number was taken from a footer.

Output: one file, `NCT05051579_Table_01_extraction.json`. Schema validation passed with 0 errors
(`jsonschema.Draft7Validator`, run programmatically, not eyeballed).

---

## Decisions needed (4)

Open calls a reviewer should confirm or overturn. Each was made deliberately and is defensible; each
could reasonably go the other way. Nothing below is a suspected error — errors would be in §3 or §8.

| # | where | call made | alternative | detail |
|---|---|---|---|---|
| D1 | doc p.17, rows 67–68 — PK sample | `"Predose"` / `"Postdose"` read as row labels; names composed as `"Pharmacokinetic (PK) sample - Predose/Postdose"`; **no column-2 mark emitted** | treat the two cells as a merged text `cell_value` over columns 2:3 with `source_range` | §5.2 |
| D2 | doc p.16, `n12`, rows 49–51 | bound to **three** rows: FSH (geometry) + LH and Estradiol (`text_match`, the note names all three analytes) | strict geometric reading — FSH only; drop the two `text_match` locations and the `n12` marker from rows 50–51 | §6 |
| D3 | doc p.13, `n1` | intro paragraph printed **above** the table's top rule kept as a table-scope `footnote` on schedule_property row 1 | drop it — it is section prose, not a table cell | §5.4 |
| D4 | header band, property row 6 | `Fasting Visit` typed `modality`, `hierarchical_level: null` | `condition` | §5.6 |

Also medium-confidence, lower stakes: property row 2 typed `period` and property row 5 given
`hierarchical_level: 5` (§5.6).

On D2, a smaller point independent of which way it goes: the FSH location is labelled
`method: "proximity"`, but the binding comes from the note cell sitting inside the FSH row's own
rule-line box, which is what `"synthesized"` describes. It is the only `proximity` in the file, so it
is the only location the validator flags for page verification. Relabelling it is a one-value
correction.

## Recorded, not open (4)

Calls made under an explicit rule, with the rule named in the section — listed so a reviewer knows
they were considered, not overlooked.

- The 12 merged instruction blocks became `footnote` annotations, not activity rows and not merged
  text cells (§5.1, §4 + §6).
- The 22-term abbreviations block yielded **zero** annotations, `ET` included (§5.3, §6 + §8).
- `n2` bound to the 16 covered header cells rather than the `schedule_property` row; the grey
  dose-escalation shading deliberately **not** modelled as a property (§5.5).
- Indentation read from the shaded grouping bands, `indentation_method: "visual_estimate"` on all 70
  activities (§5.7).

---

## 1. Per-table summary

**Table 1 — `main_soa`** — "Schedule of Activities (SoA)" (Section 1.3), document pages 13–18.

| | |
|---|---|
| table_type | `main_soa` |
| physical columns | 21 |
| label columns `L` | **1** |
| first data column position | **2**; data columns run **2–21** |
| schedule_property rows | 6 |
| schedule_grid cells | 98 |
| activities | 70 (9 section-header rows at indentation 0 + 61 activity rows at indentation 1) |
| activity_schedule marks | 372 (all literal `X`) |
| annotations | 14 (all `footnote`) |

### Why `main_soa`, and why one table and not six

Rows are procedures performed on participants, so the `reference` test fails immediately. This is
the protocol's only schedule grid and its primary anchor, so `main_soa`.

The grid runs over six pages under a header band that reprints in full at the top of each page. The
source gives the overflow **no table number, caption or title of its own** — it is one printed table
whose rows simply run onto the next page. Per the taxonomy ("Ask what the source numbered, not where
the paper ran out") that is ONE table spanning pages 13–18, not a chain of `continuation` tables. The
reprinted header band was de-duplicated to a single set of six `schedule_property` rows, and the
reprinted `Fasting Visit` row was counted once (it is a header row, not an activity — see §5 below).
This is not a horizontally tiled table: every page carries the *same* columns and *different* rows.

### Activity rows per page (all six declared pages contribute)

| doc page | activity rows | of which section headers | marks contributed |
|---:|---:|---:|---:|
| 13 | 12 | 2 (Administrative, Physical Evaluation) | 79 |
| 14 | 10 | 1 (Participant Education) | 61 |
| 15 | 11 | 3 (Participant Administration Log, Patient-Reported Outcomes, Clinician-Administered Assessments (Paper)) | 57 |
| 16 | 16 | 1 (Laboratory Tests and Sample Collections) | 71 |
| 17 | 13 | 0 | 38 |
| 18 | 8 | 2 (Stored Samples, Randomization and Dosing) | 66 |
| **total** | **70** | **9** | **372** |

No page in the declared range contributed zero rows, so the §4 page-coverage check passes with no
exception to declare.

---

## 2. Column model (§5) — how `L` was fixed

The vector layer carries real rule lines. 22 distinct vertical rules were recovered at
x = 63.1, 163.9, 217.4, 248.6, 274.3, 300.0, 325.7, 351.6, 377.3, 403.0, 428.6, 454.3, 480.0, 505.7,
531.4, 557.0, 582.7, 608.4, 634.3, 660.2, 685.9, 728.9 pt — identical on all six pages — giving
**21 columns**.

Column 1 (63.1–163.9) is the activity-name column and also carries the header rows' own labels
("Visit Number", "Week Relative to Randomization", …). There is no Protocol-Reference column and no
Procedure-Category column, so **`L` = 1** and the data columns are **2–21**, not renumbered:

| position | 2 | 3 | 4 | 5 | … | 19 | 20 | 21 |
|---|---|---|---|---|---|---|---|---|
| visit | 1 (Screening) | 2 (Lead-In) | 3 | 4 | … | 18 | ET | 801 |

Marks were binned to these rule-line bands, **not** to header glyph centres, so a mark can never be
attributed to a neighbouring visit by a rounding accident.

---

## 3. Mechanical mark-check (§1b) and the diff against the visual read

Method: **bbox column-binning against rule-line geometry** (text-layer table, §1b — pdfplumber not
used).

1. `pdftotext -bbox` plus PyMuPDF word boxes for every token on all six pages.
2. Mark tokens matched with `^[Xx][*a-zA-Z0-9]?$` (the footnoted-mark-safe pattern). **No token in
   this document matched anything other than a bare `X`** — there are no superscripted marks
   anywhere in the grid, so nothing was at risk of being dropped by a naive `== 'X'` filter.
3. Each mark box binned by x-centre to one of the 21 rule-line column bands.
4. Mark tokens clustered into rows by y (4 pt tolerance), in printed order per page.
5. Merged spans resolved from the per-row vertical-rule segments (see §4 below), not from glyph
   position.
6. The resulting page → row → column-set sequence was diffed cell-for-cell against my visual read of
   the 150 dpi renders and against the delivered JSON.

**Result: 67 mark rows compared (61 activity rows + the 6 reprints of the Fasting Visit header row),
0 disagreements.** Every column set in `activity_schedule` reproduces the mechanically derived set
exactly, in the same page order.

Two observations from the check, neither an error:

- On page 13 the **Weight** row has the mark pattern `{2, 4…21}` — byte-identical to the reprinted
  **Fasting Visit** header row. A de-duplicator keyed on the mark pattern alone would silently delete
  one of them. De-duplication here was keyed on row identity (label text + row band), not on the
  pattern, so both survive.
- The header band is textually identical on all six pages (verified token-for-token); nothing in it
  varies page to page, so collapsing it to one set of six property rows loses nothing.

---

## 4. Merged cells and distributed values

**No body mark is merged.** For every activity row the vertical rules are present at all 20 internal
column boundaries, confirmed from the rule-line segment lists. So there is no mark to distribute, no
`source_range` is set anywhere in `activity_schedule`, and no arrow or vertically-merged mark exists
in this table.

Merges occur only in the header band and in the instruction bands:

| what | rule-line span | encoding |
|---|---|---|
| "Study Period I Screening/Lead-in" | 63.1–248.6 pt = label column + columns 2–3 | 2 grid cells, `merged_cell_range` `"2:3"` (the label column is excluded from the range per §5) |
| "Study Period II Treatment Period" (+ its two sentences) | 248.6–660.5 pt = columns 4–19 | 16 grid cells, `merged_cell_range` `"4:19"` |
| unlabelled period cell over ET | column 20 | empty, not emitted |
| "Study Period III Safety Follow-Up" | column 21 | 1 grid cell, not merged |
| row 2 (Screening / Lead-In), empty cell over columns 4–19 | merged, empty | not emitted (empty) |
| instruction bands under Vital signs, Symptom-directed PE, 12-Lead ECG, Return ABPM device, Record ABPM measurements, Explain diet…, Discuss diet…, Dispense study drug administration log, Participant Survey, FSH/LH/Estradiol, PK sample | 163.9–728.9 pt = columns **2–21** | footnote annotations, see §6 |
| instruction band under Dispense study intervention capsules | 248.6–634.3 pt = columns **4–18** only | footnote annotation `n14` |

The capsules block is the one instruction band that does **not** span the whole grid: its cell stops
at the column 18 / 19 boundary, with a separate merged empty cell over columns 19–21. That was read
off the rule lines (the horizontal rule at y=338.4 exists only over x 634.6–728.6), not guessed from
where the text stops.

---

## 5. The judgement calls, stated plainly

### 5.1 The merged instruction blocks — annotation, not activity row, not merged text cell

*Example: "One bottle of capsules is to be dispensed at Visit 3 and at Visit 4. …" under
**Dispense study intervention capsules** on page 18.*

Twelve such blocks exist. Each sits in its own sub-band directly beneath its activity's mark row,
inside the same row group; the activity's label cell in column 1 spans both sub-bands, and the
instruction cell is merged across the data columns.

They are **not activity rows**: §4 says explicitly not to create activity rows for
"instruction-overflow rows that only carry footnote text", and none of these bands names a procedure
performed on a subject — they explain the row above.

They are **not merged text cells in the grid** (§5's "See instructions" case): a merged text cell
carries a *scheduling* value that says the activity happens at those columns. These blocks schedule
nothing — "May be performed based on participant needs", "The site pharmacy should clearly label
which bottle to take first" — and encoding them as 18 or 20 identical `activity_schedule` values
would assert an occurrence at every visit that the source does not assert.

They are **annotations** under §6's Notes/Instructions rule: each is one rule-line-bounded note cell
→ exactly one `footnote` annotation, its text bounded by the cell's own rules (never by proximity),
with a synthesised marker bound to the activity it sits under. `annotation_text_source.method` is
absent on all of them, because all were read from a rule-line-bounded text-layer cell — the default.

### 5.2 The PK "Predose" / "Postdose" sub-labels (lowest-confidence call)

On page 17 the label cell **"Pharmacokinetic (PK) sample"** is vertically merged over two grid rows.
The sub-labels **Predose** and **Postdose** are printed not in the label column but in
**column 2 — the Screening column** (rule-bounded 163.9–217.4 pt, verified on a 260 dpi crop).

This is a source quirk, not a second label column: column 2 schedules every other row in the table
(it carries Visit Number "1", Week "-2", a Fasting Visit `X` and 34 activity marks), so `L` stays 1.

Decision: two activities, `row_position` 67 and 68, named
`"Pharmacokinetic (PK) sample - Predose"` and `"Pharmacokinetic (PK) sample - Postdose"`, with
`activity_name_source.cell_text` recording both source cells as
`"Pharmacokinetic (PK) sample | Predose"` / `"… | Postdose"`. **No `activity_schedule` entry was
emitted at column 2 for either row**, because those two cells hold a row label, not a mark —
transcribing "Predose" as a column-2 cell value would assert a PK sample at the Screening visit
(Week −2), which the protocol does not schedule.

These are the only two `activity_name` values in the file that are not verbatim source strings.
Alternatives considered and rejected: naming them "Predose"/"Postdose" (drops "PK", useless
downstream); emitting a third, unprinted parent row (fabricates a row position). Flagged as the
single call most worth a human look.

The mark sets are internally corroborated: postdose marks land on columns 7, 9, 13, 16 = Weeks 4, 8,
16, 26, exactly the four windows named in note `n13`, plus ET.

### 5.3 The abbreviations block yields zero annotations

The block under the table defines 22 terms (ABPM, AEs, BP, C-SSRS, CKD-EPI, ECG, eGFR, ET, FSH,
HbA1c, HBC, HCV, IWQOL-Lite CT, IWRS, PGIS for Physical Activity, PHQ-9, PK, SF-36v2 acute form, PR,
TXP, UACR, Wks).

Per §6, an abbreviation's marker is the term **printed as a marker** in a grid cell, header cell or
activity label — not the term occurring inside running text. Every one of these terms occurs only as
running text inside an activity label ("Adverse events (AEs)", "eGFR (CKD-EPI)") or inside a header
string ("2 Wks Post End of TXP"). Binding any of them would be a `text_match` or `synthesized`
location, which §6/§8 forbid as the standalone list in disguise. **No `abbreviation` annotation was
emitted.**

Closest edge case: **ET**, which is printed alone as the Visit Number value of column 20. It is a
visit code occupying a data cell, not a marker attached to a cell, so it was dropped with the rest.
This is a deliberate call; if a downstream reviewer wants the ET expansion it should come from the
protocol glossary, not from this table.

Note the source itself has a typo the extraction preserves by not touching it: the block reads
**"HBC = hepatitis B virus"** while the activity row reads "Hepatitis B Virus (HBV) screening tests".
Since no abbreviation annotation was emitted, nothing in the JSON propagates the typo.

### 5.4 The intro paragraph above the table (`n1`)

"The Schedule of Activities described below should be followed for all participants enrolled in Study
GZGI. However, … please refer to Section 10.11 (Appendix 11) for additional guidance."

This is section prose printed **above** the table's top rule, not inside a table cell. It was
captured anyway, as a table-scope `footnote` with one `schedule_property` location on row 1
(`method: "synthesized"`), following §6's convention for a note with no modelled element to attach
to. It is typed `footnote` and not `source_note` because it both explains (who the schedule applies
to) and points; §8's bare-pointer rule only reclassifies notes that do nothing but point.
Reasonable reviewers could drop this one; it is the second-most debatable call in the file.

### 5.5 The Study Period II header-cell note (`n2`)

The "Study Period II / Treatment Period" header cell contains two extra sentences: "For early
terminations that occur before the last visit in treatment period, see the activities listed for ET
in this table. Shaded columns represent the dose escalation period Weeks 0-16."

`cell_value` for those 16 grid cells was set to the heading part only, `"Study Period II Treatment
Period"`, and the two sentences were emitted as **one** annotation (`n2`) — one cell, one annotation,
per §6 — bound to all **16 covered header cells** (row 1, columns 4–19, `location_type:
"schedule_cell"`, `method: "synthesized"`), with `annotation_markers: "n2"` written onto each of
those 16 `schedule_grid` cells. Binding it to the `schedule_property` row instead would have scoped
it to the whole header row including Screening/Lead-in and Visit 801, which the printed cell does not
cover.

The second sentence is effectively a shading legend. **The grey shading itself was NOT modelled** as
a schedule property: nothing in the source labels a "dose escalation" band as a header row, and
inventing one would be inference. For the record, the shaded columns are 4–13 (Visits 3–12, Weeks
0–16), consistent with the sentence.

### 5.6 `property_type` and `hierarchical_level` calls

| row | property_name | type | level | confidence |
|---|---|---|---|---|
| 1 | Study Period | `epoch` | 1 | high — Screening/Treatment/Follow-Up is the textbook epoch row |
| 2 | Screening/Lead-In Period | `period` | 2 | medium — sub-divides Study Period I into two named sub-phases |
| 3 | Visit Number | `visit` | 3 | high |
| 4 | Week Relative to Randomization | `week` | 4 | high |
| 5 | Allowable Interval Tolerance (days) | `window` | 5 | high on type; level 5 is a judgement — it qualifies each column's timing rather than distinguishing columns |
| 6 | Fasting Visit | `modality` | `null` | **lowest confidence in the header band** |

**Fasting Visit** was typed `modality` (visit type/attribute — fasting vs non-fasting) rather than
`condition` (which the schema defines as *conditional timing*, and §3 illustrates with a population/
eligibility qualifier band; fasting is neither). `hierarchical_level` is `null` because removing the
row leaves no two columns indistinguishable — Visit Number already separates all 20. A reviewer who
prefers `condition` here would not be unreasonable.

It is treated as a **schedule_property, not an activity**: it sits inside the blue shaded, bold
header band above the first section-header row, its label is bold like the other header labels, and
it reprints with the header on every page. It is *not* counted among the 70 activities and its 19
marks live in `schedule_grid`, not `activity_schedule`.

### 5.7 Indentation

The label column carries **no leading whitespace at all** — hierarchy is expressed purely by
full-width, bold, centred, shaded grouping bands. Every activity therefore has
`activity_name_source.indentation_method: "visual_estimate"` (§1e): 0 for the nine grouping bands, 1
for everything else. No grandchild level exists. All nine level-0 rows carry zero marks, as §4
requires; the table is not flat, so the flat-table exception does not apply.

---

## 6. Synthesised values

**Synthesised `property_name` (2):**

- Row 1 → `"Study Period"`. The row's leftmost cell is part of the merged "Study Period I
  Screening/Lead-in" cell; there is no separate label cell. `synthesized: true`,
  `structure_method: "inferred_from_layout"`.
- Row 2 → `"Screening/Lead-In Period"`. Label cell (column 1) is genuinely empty; the name was built
  from the row's own two values, "Screening" and "Lead-In". `synthesized: true`,
  `structure_method: "inferred_from_layout"`.

Rows 3–6 take their names verbatim from printed label cells and carry no `structure_method`.

**Synthesised annotation markers (14).** The source prints **no footnote markers anywhere** — not on
a cell, not on a label, not on a header. Every note is an unmarked merged text block. Markers `n1`
… `n14` were synthesised in printed document order:

| marker | bound to | doc page | location method |
|---|---|---|---|
| n1 | schedule_property row 1 (table scope) | 13 | synthesized |
| n2 | schedule_grid row 1, columns 4–19 | 13 (reprints on 14–18) | synthesized |
| n3 | Vital signs (2 sitting BP and PR measurements) | 14 | synthesized |
| n4 | Symptom-directed physical examination | 14 | synthesized |
| n5 | 12-Lead ECG | 14 | synthesized |
| n6 | Return ABPM device | 14 | synthesized |
| n7 | Record ABPM measurements | 14 | synthesized |
| n8 | Explain diet and physical activity plan | 14 | synthesized |
| n9 | Discuss diet and physical activity progress | 15 | synthesized |
| n10 | Dispense study drug administration log, instruct in use | 15 | synthesized |
| n11 | Participant Survey | 15 | synthesized |
| n12 | Follicle-stimulating hormone (FSH) / Luteinizing hormone (LH) / Estradiol | 16 | **proximity** (FSH) + **text_match** (LH, Estradiol) |
| n13 | PK sample – Predose and PK sample – Postdose | 17 | synthesized |
| n14 | Dispense study intervention capsules | 18 | synthesized |

Every marker in every `marker_locations` entry was also written onto that row's / cell's
`annotation_markers`, verified programmatically over all 14 annotations and all 32 locations — 0
failures. This matters because `resolve` binds through the row's `annotation_markers`, not through
`marker_locations`.

**`n12` is the only note whose scope is a judgement.** Its cell sits in its own band **below** the
FSH row with an empty label cell, and its text names three analytes: "Collect serum estradiol, FSH,
and LH in women whose menopausal status needs to be determined…". Geometrically it touches only the
FSH row (→ `method: "proximity"`). Textually it plainly governs the LH and Estradiol rows two and
three rows below (→ `method: "text_match"` on both). Both bindings are recorded honestly rather than
collapsed to one. If a reviewer prefers a strict geometric reading, drop the two `text_match`
locations and the `n12` marker from rows 50 and 51.

**No `unresolved` locations were needed.** Every marker's target was determinable.

---

## 7. Annotation text integrity

- **The text layer is NOT glyph-spread.** Tokens are whole words with normal inter-word gaps; no
  `deglyph_reconstruction` was needed and no field was reconstructed. All 14 `annotation_text`
  values, all 70 `activity_name` values and all 98 `schedule_grid` values were read directly from the
  text layer.
- **Verbatim check:** each of the 14 `annotation_text` strings was matched programmatically as a
  whitespace-normalised substring of the concatenated six-page text layer. **14/14 matched.** Each
  starts at its cell's first word and ends at its last.
- **Containment pairs: none.** No annotation's text is contained in another's, checked
  programmatically over all 182 ordered pairs. So the §8 re-verification for split note cells has
  nothing to adjudicate — there is no split-vs-source-faithful call to make in this document.
- **Notes bounded by rule lines, never by proximity.** Every note's extent came from its cell's own
  vertical/horizontal rule segments. `annotation_text_source` is absent on all 14 annotations, i.e.
  the default method (rule-line-bounded text-layer cell). **No note is `proximity_bounded`.**
- Two values were cleaned of line-wrap artifacts and so are not verbatim substrings of the raw text
  layer, deliberately: `"Study Period III Safety Follow-Up"` (prints as "Follow-⏎Up") and
  `"Lead-In"` (prints as "Lead⏎-In"). Both are hyphenation across a line break inside one cell.

---

## 8. Orphan risk

None. All 14 annotations carry ≥1 `marker_locations` entry (32 locations total), every one of which
is mirrored in the target element's `annotation_markers`. No marker appears in the source whose
definition is missing (the source prints no markers at all), so §6's referenced-but-undefined case
does not arise. No redaction, no illegible region, no page that might conceal rows: all six pages
render cleanly and their rule-line grids are complete and identical in column geometry.

---

## 9. Method provenance — every non-default method recorded

| field | value | where | why |
|---|---|---|---|
| `activity_name_source.indentation_method` | `visual_estimate` | all 70 activities | the label column has no leading whitespace; hierarchy comes from full-width shaded grouping bands on the rendered page |
| `schedule_property.structure_method` | `inferred_from_layout` | rows 1 and 2 | their `property_type`/`hierarchical_level` come from merge geometry and row order, not from a printed label |
| `marker_locations[].method` | `synthesized` | 29 of 32 locations | the source prints no footnote markers; every note binding is by convention |
| `marker_locations[].method` | `proximity` | `n12` → FSH (row 49) | note cell sits directly beneath the FSH row band, no printed marker |
| `marker_locations[].method` | `text_match` | `n12` → LH (row 50), Estradiol (row 51) | bound by the note naming those analytes |
| `activity_schedule[].method` / `schedule_grid[].method` | *absent* | all 372 + 98 cells | default: pdftotext/bbox coordinate read of the text layer |
| `annotation_text_source` | *absent* | all 14 | default: rule-line-bounded text-layer cell |
| `activity_name_source.method` | *absent* | all 70 | default: direct text-layer read |
| `location_type: "unresolved"` | *none* | — | every target was determinable |

---

## 10. §8 delivery checklist, run literally

| check | result |
|---|---|
| `schema_name` = `soa-table-extraction` | pass |
| `schema_version` = `1.0` | pass |
| `extraction_status` = `ready_for_resolution` | pass |
| every `property_comment` meaningful | pass — all 6 state row content and type reasoning |
| every `cell_value` clean of markers | pass — all 372 marks are bare `X`; source prints no markers |
| every annotation has ≥1 `marker_locations` | pass — 14/14, 32 locations |
| containment pairs re-verified against the page | pass — none exist |
| annotation text complete against its source cell | pass — 14/14 verbatim substring match, no letter-spacing |
| `by_type` not degenerate | pass — 14 annotations (< 20), all `footnote`; §8: all-`footnote` is normal, and none is a notes-column artefact |
| no `abbreviation` bound only by `text_match`/`synthesized` | pass — zero `abbreviation` annotations |
| bare pointers typed `source_note` | pass — no annotation is a bare pointer; `n1`, `n3`, `n5` point *and* explain, so they stay `footnote` |
| every page in `page_start`..`page_end` contributed activity rows | pass — 13:12, 14:10, 15:11, 16:16, 17:13, 18:8 |
| every `marker_locations` marker also in the row's `annotation_markers` | pass — verified programmatically, 0 failures |
| merged marks distributed with `source_range` | n/a — no merged body marks exist; header merges carry `merged_cell_range` `"2:3"` / `"4:19"`, numeric, never A1 |
| `track_label` set for `track` tables only | pass — not a track, field absent |
| `method` provenance recorded, no guessed targets | pass — see §9 |
| schema validation | pass — 0 errors, `jsonschema.Draft7Validator` |
