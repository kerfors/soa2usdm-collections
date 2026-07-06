# soa2usdm-collections

Protocol collections for [**soa2usdm**](https://github.com/kerfors/soa2usdm) — the derived Schedule of Activities (SoA) extraction outputs and their HTML visualizations. This is the **data** repository; the code, schemas, and prompts live in `soa2usdm`.

Rendered visualizations are published via **GitHub Pages**: [kerfors.github.io/soa2usdm-collections](https://kerfors.github.io/soa2usdm-collections) (GitHub does not render HTML files in the repository browser, so the Pages site is where the consolidated SoA views are meant to be read).

## What's here — and what isn't

This repo holds the **derived** artifacts of the pipeline: extraction JSON (raw + verified), resolved JSON, consolidated JSON, and the generated HTML views, one folder per protocol.

It does **not** hold the source protocol PDFs. Those are fetched on demand from ClinicalTrials.gov using the manifest, and are `.gitignore`d so they are never committed. There is a single source of truth for the protocol documents — ClinicalTrials.gov — and this repo points at it rather than redistributing it.

> **Provenance note.** The manifest points at the current `ProvidedDocs` PDF on ClinicalTrials.gov for each study. The d4k `usdm_data` set may have been built from an earlier posted version of the same protocol, so a rebuild here could pick up a newer document than the one d4k modelled. This is an accepted trade-off: ClinicalTrials.gov is the canonical, non-redistributed source. Each extraction records the SoA page range it used, so drift is detectable per protocol.

## Collection: `usdm_data`

The initial collection mirrors the protocol set published in [data4knowledge/usdm_data](https://github.com/data4knowledge/usdm_data/tree/main/source_data/protocols). The manifest `collections/usdm_data/studies_protocols.xlsx` has two sheets:

- **`studies`** — provenance metadata per protocol: d4k folder name, lead sponsor, therapeutic area, acronym, phase, status, brief title.
- **`protocols`** — the download manifest consumed by `00_download_extract.ipynb`: `nct_id`, `study_code`, `soa_pages`, `protocol_url` (a ClinicalTrials.gov `ProvidedDocs` URL).

Protocol folders are named by NCT ID (e.g. `NCT03637764`); the d4k folder name and sponsor/therapeutic-area are carried as manifest metadata and surfaced as labels on the Pages index.

`soa_pages` (the page range of the SoA table within each protocol PDF) is populated only where it has been determined by review. Blank entries are protocols not yet processed — they are filled in as each protocol is worked through, not guessed.

> **CDISC_Pilot** (the synthetic LZZT pilot study) appears in the d4k set but has no ClinicalTrials.gov source, so it is listed in the `studies` sheet for completeness but excluded from the download `protocols` sheet.

## Building the collection

This repo starts fresh: the manifest and folder scaffold are here; the derived outputs are produced protocol by protocol using the `soa2usdm` pipeline.

1. Clone `soa2usdm` and this repo side by side:

   ```
   parent/
   ├── soa2usdm/
   └── soa2usdm-collections/
   ```

2. In `soa2usdm`, run `notebooks/00_download_extract.ipynb` with `COLLECTION = "usdm_data"` to fetch a protocol PDF, slice out its SoA pages, and scaffold the protocol folder here.
3. Run the Layer 1 extraction conversations (see the `soa2usdm` prompts), then `01_batch.ipynb` for resolution, consolidation, and visualization.
4. Regenerate the Pages index and commit the derived outputs (not the PDFs).

The pipeline discovers this collection automatically at `../soa2usdm-collections/collections/`, or wherever `SOA2USDM_COLLECTIONS` points.

## License

Derived content is shared under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/). Any code in this repo is under the [MIT License](LICENSE). Source protocol documents remain the property of their respective sponsors and are not redistributed here.

## Author

Kerstin Forsberg — information architect specializing in clinical data standards. Built iteratively with Claude (Anthropic).
