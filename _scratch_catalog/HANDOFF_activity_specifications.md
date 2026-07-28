# Cross-Protocol Activity Catalog — handoff to the *Activity Specifications* project

**Purpose.** Input artifact for the Activity Specifications work. It aggregates the
Schedule-of-Activities rows from all 22 processed protocols in the `usdm_data`
collection into one searchable catalog, so the Activities ↔ Biomedical Concepts
question can be examined against real, fully traceable data rather than in the abstract.

## What it is / how it is built
- **Source:** each protocol's `*_consolidated.json` `unified_activities` — the per-protocol
  consolidation output of the soa2usdm pipeline (activities already consolidated *across
  tables within* a protocol, with source-row traceability).
- **Cross-protocol clustering** reuses the pipeline's own similarity verbatim
  (`soa2usdm.consolidate`: `normalize_text` → `word_overlap_score` → `find_best_match`,
  exact / 0.85 auto / 0.60 review). Scope is **per-collection**; no cross-collection reach.
- **Volume:** 899 source activities → 351 catalog activities (128 multi-protocol,
  201 singletons). Cross-protocol tiers: 253 exact, 12 fuzzy_auto, 86 fuzzy_review.
- **Traceability per occurrence:** catalog activity → sponsor (from manifest `d4k_folder`)
  → protocol → source table → row. Self-contained HTML viewer + backing JSON.

## The tension it surfaces (why it belongs in this project)
The catalog groups by **surface activity wording**, which is *not* the same identity as a
Biomedical Concept. Word-overlap fails in both directions at once:
- **Under-merges the concept** — semantically identical activities stay in separate
  clusters when sponsor wording diverges.
- **Over-merges qualifiers** — distinct specimen / method / analyte variants collapse onto
  one identity because the distinguishing tokens are numeric or ≤2 chars and get stripped
  (e.g. dose levels, `PGI-C` vs `PGI-S`, systolic vs diastolic).

## Worked example — pregnancy testing (EliLilly, real data)
Splits into **two** catalog clusters within a single sponsor:
- *Pregnancy test* (7 occ): `Serum pregnancy test`, `Urine pregnancy test`,
  `Local serum pregnancy test`, `Pregnancy Test (Female patients of childbearing potential only)`, `Pregnancy Test`.
- *Serum or Urine Pregnancy (females only)* (3 occ): `Serum pregnancy`,
  `Urine pregnancy (local)`, `Serum or urine pregnancy test (women of childbearing potential only)`.

Same concept, same sponsor, split only by wording — and specimen (serum vs urine) is treated
as a naming variant rather than a qualifier. A BC-anchored view would resolve **one** concept
(pregnancy test) with specimen / method / population as attributes. This is precisely the
modelling the Activity Specifications work addresses.

## Open question to resolve in this project
Should cross-protocol activity **identity** be anchored to CDISC controlled terminology /
Biomedical Concepts — with specimen, method, fasting state, timing, population as qualifiers —
rather than to consolidated surface wording? If so, the catalog's spine becomes:

    BC identity  ←  activity-spec mapping  ←  consolidated activity  ←  protocol SoA row

The word-overlap catalog stays useful as the **recall net + review queue** that feeds the
mapping; the BC layer supplies the normalized identity and the qualifier structure.

## Files to bring over
- `catalog.html` — self-contained searchable viewer (data embedded; opens in a browser).
- `catalog.json` — backing data, schema `soa-activity-catalog-prototype`, for programmatic reasoning.
- Origin: `soa2usdm-collections/_scratch_catalog/` (untracked prototype).
