
# Topic 4: Data Quality Audit Report
## ResearchLens AI – AI Research Paper Discovery & Summarisation Assistant

## 1. Project and Data Overview

ResearchLens AI is an AI Research Paper Discovery and Summarisation Assistant that uses the OpenAlex Works API to retrieve research paper information. The project uses Pathway D – JSON/API for data collection and preparation.

This audit evaluates the prepared dataset used by the project before it is used in the application. The audit focuses on data completeness, consistency, accuracy, timeliness, uniqueness, validation rules, coverage, bias and responsible data practices.

**Audited dataset information:**

- **Data source:** OpenAlex Works API
- **Data collection pathway:** Pathway D – JSON/API
- **Prepared dataset:** `data/pipeline_v1/json/prepared_json.jsonl`
- **Matching manifest:** `data/pipeline_v1/json/pipeline_manifest.json`
- **Total records audited:** 20
- **Audit evidence folder:** `data/week4/`

The prepared dataset contains 13 fields: ID, DOI, title, authors, publication year, publication date, citation count, open access, source name, abstract, abstract availability, provider and source file.

## 2. Five-Dimension Quality Scorecard

The five dimensions were assessed using the available Week 4 audit evidence.

| Dimension | Finding | Evidence and Impact | Decision |
|---|---|---|---|
| Completeness | All 13 required fields were checked across 20 records. Eight records have missing abstract values. The other 12 fields have no missing values. | `week4_completeness.csv`. Missing abstracts prevent the system from producing abstract-based summaries for those records. | Review |
| Consistency | All required columns exist, and there were no failures in the check between abstract text and the `abstract_available` flag. | `week4_consistency.csv`. Incorrect abstract availability flags could cause the system to process records without usable abstracts. | Pass |
| Accuracy | Prepared titles, publication years, DOIs and reconstructed abstracts were compared against their corresponding raw OpenAlex records. All recorded comparisons passed. | `week4_accuracy.csv`. Incorrect metadata or abstract content could affect search results and summary reliability. | Pass |
| Timeliness | Publication-year and manifest information were inspected, but no project freshness threshold was defined. | `week4_five_dimension_scorecard.csv`. Without a defined freshness requirement, the dataset's suitability in terms of timeliness cannot be confirmed. | N.V. |
| Uniqueness | No repeated OpenAlex work IDs were detected. | `week4_duplicates.csv` and `week4_audit_summary.json`. No duplicate-ID issue was identified in the audited dataset. | Pass |

Overall, the dataset has satisfactory results for consistency, accuracy and uniqueness. However, missing abstracts and the absence of a defined freshness threshold require further attention.

## 3. Validation Findings

Two project-specific validation rules were applied to the 20 prepared records.

### V1: Non-empty ID and title (RAI1- source traceability)

This rule checks that every research paper has an ID and title. These fields are important for identifying and displaying research papers correctly.

- **Records tested:** 20
- **Passed:** 20
- **Failed:** 0
- **Result:** Pass

### V2: Abstract availability flag agrees with abstract text (RAI3- missing-abstract handling)

This rule checks whether the `abstract_available` field correctly represents the availability of abstract text. This prevents the application from attempting to summarise records without an available abstract.

- **Records tested:** 20
- **Passed:** 20
- **Failed:** 0
- **Result:** Pass

The validation results are recorded in `week4_validation_results.csv`.

Both rules passed for all audited records. No records required corrective action under these two validation rules.

## 4. Duplicate, Anomaly and Coverage Findings

### Duplicate Findings

The audit checked repeated OpenAlex work IDs to identify duplicate research paper records.

The result was zero duplicate IDs among the 20 audited records. Therefore, no duplicate-ID issue was identified in the prepared dataset.

### Missing Abstract Findings

The completeness check identified eight records with missing abstract values out of 20 records. This represents 40% of the audited dataset.

These records may still be useful for displaying paper metadata, but they cannot support abstract-based summarisation when the abstract is unavailable.

### Coverage Findings

The dataset was collected from two raw source files:

| Source File | Number of Records |
|---|---:|
| `s01_openalex.json` | 10 |
| `s02_openalex.json` | 10 |
| **Total** | **20** |

The results show an equal number of records from both source files.

However, this only demonstrates the distribution of records by source file. It does not establish that the dataset represents all relevant research topics, disciplines, publication periods or research communities.

The coverage evidence is recorded in `week4_coverage.csv`.

## 5. Bias Assessment

One possible risk is **query-dependent coverage bias**.

The dataset was collected through specific OpenAlex search queries. Therefore, the retrieved papers depend on the search terms and collection scope used. Research that does not match the selected queries may not appear in the prepared dataset.

This could affect ResearchLens AI because users may receive search results that do not cover all relevant research areas or perspectives.

Another observed limitation is the missing abstracts in eight records. This may affect the availability of summarisation results across the dataset.

**Suggested mitigation:**

- Review whether the selected search queries cover the intended research scope.
- Monitor the distribution of retrieved records across relevant research areas.
- Preserve missing-abstract indicators and do not generate summaries when abstract text is unavailable.

The current evidence does not establish the actual level of representation bias. Further coverage analysis is required before making conclusions about dataset representativeness.

## 6. Privacy, Rights and Governance

### B3 – Privacy, Rights and Governance

**[SPACE RESERVED – INSERT B3 RESULTS HERE]**

This section will record:

- Provenance and version information.
- Rights and permissions for intended use and data sharing.
- Privacy and sensitive data assessment.
- Consent or collection basis.
- Ongoing maintenance responsibilities and revalidation triggers.
- At least one concrete safeguard or required action.

## 7. Mitigation and Impact on Requirements

Based on the available audit findings, the following improvements are recommended.

### Improvement 1: Review Missing Abstracts

Eight out of 20 records have missing abstract values. The project should preserve these records and their missing-abstract indicators. The application should display an appropriate message instead of generating a summary without abstract content.

- **Priority:** Address before relying on summaries across the full dataset.
- **Responsible person:** Project owner / student developer (suggested).
- **Affected requirement:** Abstract availability handling and abstract-based summarisation.
- **Evidence:** `week4_completeness.csv` and `week4_validation_results.csv`.

### Improvement 2: Define a Data Freshness Requirement

The timeliness assessment could not be completed because the project has not defined a freshness threshold. A suitable requirement should be established to determine how recent the collected research information needs to be.

- **Priority:** Before making claims about dataset freshness.
- **Responsible person:** Project owner / student developer (suggested).
- **Affected requirement:** Search result relevance and data timeliness.
- **Evidence:** `week4_five_dimension_scorecard.csv`.

### Residual Risks

- The current 20-record dataset may not represent all relevant research topics.
- Missing abstracts limit the number of records available for summarisation.
- Dataset freshness cannot be confirmed without a defined threshold.
- Rights and privacy matters require the governance review in Section 6.

The actual Topic 2 requirement IDs should be added after checking `week2_requirements.md`. They are not available in the supplied evidence ZIP.

## 8. Quality-Gate Decision

### B4 – Quality-Gate Decision

**[SPACE RESERVED – INSERT B4 RESULTS HERE]**

This section will record:

- The selected decision: GO / GO WITH CONDITIONS / HOLD.
- The exact dataset and development stage covered by the decision.
- At least two supporting evidence points.
- At least two priority improvements, including responsibility and timing.
- Residual risks.
- Conditions or evidence that will trigger re-review.

The quality-gate decision should only be finalised after reviewing the B3 governance evidence and completing the B4 assessment.

---

## Evidence Appendix

The following evidence files support this report:

1. `week4_audit_scope.json`
2. `week4_audit_summary.json`
3. `week4_completeness.csv`
4. `week4_consistency.csv`
5. `week4_accuracy.csv`
6. `week4_duplicates.csv`
7. `week4_validation_results.csv`
8. `week4_five_dimension_scorecard.csv`
9. `week4_coverage.csv`
10. `week4_evidence_index.json`
