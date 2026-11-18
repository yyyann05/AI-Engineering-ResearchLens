# Week 3 Pipeline Decisions — ResearchLens AI

## 1. Topic 2 requirements and data needs

| Requirement | Data needed | Pipeline use |
|---|---|---|
| FR1 | OpenAlex work records matching keywords | Retrieve paper records |
| FR2 | Publication year and open-access status, when available | Preserve fields for filtering |
| FR3 / RAI1 | Title, authors, year, DOI, work ID and source information | Display metadata and preserve traceability |
| FR4 / FR5 / RAI2 | Abstract text, when available | Input to the separate local summarisation model; label output abstract-based |
| FR6 / RAI3 | Missing fields, missing abstracts and API error states | Handle unavailable data without fabrication |

## 2. Source and pathway
- Source: OpenAlex Works API (`https://api.openalex.org/works`).
- Data type: semi-structured JSON/API.
- Selected pathway: Topic 3 Pathway D.
- Inspection: two query subsets, `artificial intelligence` and `machine learning`.
- OpenAlex currently provides a free API key option; store the key in Colab Secrets or an environment variable, never in the notebook or GitHub.

## 3. Preparation comparison and evidence
- Option A: retain the original nested JSON. It preserves the response but is less convenient for the application.
- Option B: extract selected paper fields into flat JSON records and reconstruct available abstracts from `abstract_inverted_index`.
- Selected: Option B, while retaining raw JSON.
- Record actual evidence after running: input/prepared counts, excluded records, available/missing abstract counts, sample record and output size. Do not invent these values.

Benefit: prepared records are easier to display and use as summarisation input.
Accepted limitation: only selected fields are flattened; raw JSON is retained to preserve other fields.

## 4. Preparation configuration
Extract work ID, DOI, title, authors, publication year/date, citation count, open-access status, source name and abstract. Reconstruct abstract text only when the inverted index exists. Preserve missing values and set `abstract_available` accurately. Exclude malformed records without a stable work ID. Export JSONL.

## 5. ETL / ELT decision
Use an **ETL-style** process: extract from OpenAlex, transform fields and abstract representation, validate, then load prepared JSONL. This creates a predictable data structure for ResearchLens AI.

## 6. Batch / streaming decision
Use **batch retrieval** for V1. Users submit a search query and receive a result set; continuous event streaming is not required.

## 7. Output locations
- Raw: `data/raw/s01_openalex.json`, `data/raw/s02_openalex.json`
- Part B: `data/processed/prepared_json.jsonl`
- Part B lineage: `data/week3_manifest.json`
- Part C: `data/pipeline_v1/json/prepared_json.jsonl`
- Part C manifest: `data/pipeline_v1/json/pipeline_manifest.json`

## 8. Failure points
1. API authentication, timeout or HTTP error: stop ingestion, report the error and check connectivity/API access before retrying.
2. Invalid JSON structure or missing work ID: report and exclude invalid records; do not fabricate records.
3. Missing abstract: retain metadata and flag abstract unavailable; do not generate a summary for that record.

## 9. Provenance and access caveats
Each raw response records its query, endpoint, request URL and retrieval timestamp. SHA-256 checksums are recorded for raw and prepared files. Manifests record source-to-output mappings. A checksum supports integrity checking but does not prove accuracy or permission. Keep API keys private. Colab runtime files are temporary; download authorised artefacts before disconnecting.
