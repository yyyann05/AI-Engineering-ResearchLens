# Week 3 Data Inventory — ResearchLens AI

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| S01 | OpenAlex Works API, query `artificial intelligence`; saved as `data/raw/s01_openalex.json` | JSON/API; academic works for keyword search and metadata display | Captured during Colab run; OpenAlex | OpenAlex API; use a free API key and keep it private; check current terms | Actual record count, JSON keys, sample title/year, abstract-index availability |
| S02 | OpenAlex Works API, query `machine learning`; saved as `data/raw/s02_openalex.json` | JSON/API; second representative query subset | Captured during Colab run; OpenAlex | Same as S01 | Actual record count, JSON keys, sample title/year, abstract-index availability |

## Requirement mapping
- **FR1:** OpenAlex work records support keyword-based paper retrieval.
- **FR2:** Publication year and open-access fields support filtering when present.
- **FR3 / RAI1:** Title, authors, year, DOI, work ID and source data support metadata display and traceability.
- **FR4 / FR5 / RAI2:** Available abstracts provide input to the separate local summarisation model; summaries must be labelled abstract-based.
- **FR6 / RAI3:** Missing abstracts and invalid API responses must be handled without fabricating records or summaries.

## Data type and preparation
Semi-structured JSON/API. Selected Topic 3 pathway: **D — JSON/API**. Raw API responses are retained. Selected fields are extracted into JSONL; available abstracts are reconstructed from `abstract_inverted_index`. Missing abstracts remain empty and are flagged.

## Storage
- Raw: `data/raw/`
- Part B prepared output: `data/processed/prepared_json.jsonl`
- Part B lineage: `data/week3_manifest.json`
- Part C output and manifest: `data/pipeline_v1/json/`

Keep raw and prepared data separate. Do not commit API keys or secrets.
