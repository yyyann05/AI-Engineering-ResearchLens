# ResearchLens AI — Topic 2 Requirements Specification

**Version:** 1.0  
**Domain:** Academic Research and Scholarly Information Retrieval  
**Status:** Draft. Numerical thresholds are proposed starting targets, not measured results.

## Section 0 — Project Framing & Application Scope

### Problem and evidence
University students and early-stage researchers need to locate relevant academic papers for assignments, literature reviews, and research projects. Finding suitable papers and reviewing many abstracts can require effort.

- **Difficulty finding academic information:** Shuhidan et al. (2019) examined difficulties experienced by Malaysian university students in locating and evaluating information for academic purposes. [Research paper](https://ibimapublishing.com/articles/JELHE/2019/822970/)
- **Differences in academic search performance:** Gusenbauer and Haddaway (2020) evaluated 28 academic search systems and identified differences in their ability to retrieve research. [Research paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC7079055/)
- **Information overload:** Arnold et al. (2023) reviewed literature on information overload and challenges associated with managing large amounts of information. [Research paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC10322198/)

These sources support the broader problem of finding and managing information. They do not by themselves prove that students specifically need AI-generated abstract summaries. Target-user feedback remains to be collected.

### Existing approaches
- **OpenAlex:** Open scholarly catalogue and API for searching academic works and retrieving metadata. **Limitation:** abstracts and full text are not available for every record. [Source](https://help.openalex.org/api/)
- **Semantic Scholar:** Academic search platform providing paper details, abstracts, and citation information when available. **Limitation:** some records have missing abstracts or unavailable full-text documents. [Source](https://api.semanticscholar.org/api-docs/)
- **arXiv:** Public repository of research papers with an API for searching records and retrieving metadata. **Limitation:** coverage focuses on specific research fields, and preprints are not necessarily peer-reviewed. [Source](https://info.arxiv.org/help/api/index.html)

### Project / engineering gap
The practical engineering goal is to bring paper discovery, basic filtering, paper metadata, available abstracts, and concise abstract summaries together in one interface. This is a practical integration goal, not a claim of scientific novelty. V1 will use OpenAlex as its primary search source rather than integrating multiple academic search APIs.

### Objectives
- **O1:** To develop a web-based academic paper discovery system using Python, Streamlit, and the OpenAlex API.
- **O2:** To implement paper filtering and display relevant metadata, including titles, authors, publication years, citation counts, and available links.
- **O3:** To implement and evaluate concise AI-generated summaries of available paper abstracts without relying on the OpenAI API.

### Intended users
Undergraduate students conducting assignments and literature reviews; postgraduate students exploring research topics; and early-stage researchers searching for academic literature.

### Supported tasks
- Search academic works by topic or keywords.
- Filter by publication year or year range and optionally open-access status.
- Inspect available metadata, abstracts, DOI, citation count, and source links.
- Generate a concise summary of an available abstract.
- Compare the summary with the original abstract and open the source record.

### Information / knowledge domain
Academic research and scholarly information retrieval, using NLP-based text summarisation. Initial content is academic paper metadata and abstracts available through OpenAlex.

### Operating context
A browser-based Streamlit prototype. The application requests records from OpenAlex and runs the chosen summarisation approach locally where supported. This is a semester prototype, not a production-scale research platform.

### Important constraints
- OpenAlex records may not contain an abstract or full text.
- OpenAlex is the primary search source in V1.
- Summarisation input is the abstract, not the entire paper.
- A pretrained model may require an initial download and suitable local compute; an extractive Python fallback is planned.
- No OpenAI API is required.
- Target-user feedback and evaluation results have not yet been collected.
- API availability and response time may affect search performance.

### Explicit out-of-scope tasks
The application will not claim to summarise a full paper when only an abstract was processed, guarantee access to paywalled/full text, judge scientific correctness or paper quality, treat every preprint as peer-reviewed, train a large model from scratch, integrate multiple academic APIs in the initial MVP, or replace critical reading.

### Evaluation focus
Determine whether the system retrieves topic-relevant records, shows traceable source information, generates summaries faithful to available abstracts, responds within practical time limits, and provides useful support to target users. Report observed results rather than assuming success.

---

## Section 1 — Functional Requirements

| ID | Requirement — The system SHALL | Scope boundary — The system SHALL NOT |
|---|---|---|
| FR1 | Allow users to enter a topic or keywords and retrieve matching academic records from OpenAlex. | Claim results represent every relevant paper online. |
| FR2 | Allow filtering by publication year or year range and optionally open-access status. | Apply unsupported filters or imply unavailable information is known. |
| FR3 | Display available title, authors, publication year, citation count, DOI when available, and source link. | Invent missing metadata or present unavailable values as facts. |
| FR4 | Display an available abstract and generate a concise summary from it. | Claim full-text analysis when only the abstract was processed. |
| FR5 | Show the original abstract alongside the summary and provide a source link. | Present the summary as a substitute for critical reading or a guarantee of correctness. |
| FR6 | Handle missing abstracts, empty results, API errors, and summarisation errors with clear messages. | Generate an abstract summary when no abstract text is available. |

## Section 2 — Non-Functional Requirements

**Thresholds below are proposed initial targets, not measured results.**

| ID | Quality attribute | Requirement | Measurement method | Metric | Proposed threshold |
|---|---|---|---|---|---|
| NFR1 | Performance | Search should respond within a practical time under the defined test workload. | Record end-to-end time for at least 20 search queries under the same conditions. | p90 search response time | ≤ 5 seconds, excluding unusually slow external service outages |
| NFR2 | Reliability | Common missing-data and service-error cases should not cause an unhandled crash. | Test empty results, missing abstracts, API errors, and summariser errors. | Test cases handled without unhandled crash | ≥ 95% |
| NFR3 | Summary faithfulness | Summaries should avoid claims unsupported by the input abstract. | Manually review at least 20 summaries with a checklist. | Reviewed summaries with no identified unsupported factual claim | ≥ 90% |
| NFR4 | Cost | Summarisation shall not require OpenAI API calls. | Inspect code/configuration and record external services used during summarisation tests. | OpenAI API calls by the application | 0 |
| NFR5 | Maintainability | Search, abstract processing, and summarisation should be separately testable responsibilities. | Inspect repository structure and run component-level tests. | Responsibilities represented by separately testable modules | Search, abstract processing, and summarisation separated |

## Section 3 — Responsible AI Requirements

**Thresholds below are proposed initial targets.**

| ID | Risk | Requirement | Measurement method | Metric | Proposed threshold |
|---|---|---|---|---|---|
| RAI1 | Transparency / traceability | Show a source link for each paper record and the original abstract when available. | Inspect 30 returned records where source-link information is available. | Records with source link and correctly handled available abstract | 100% |
| RAI2 | Misleading summary | Label generated text as an abstract summary and do not imply full-text analysis. | Inspect interface and outputs for accurate labels and input-scope statements. | Outputs correctly labelled as abstract-based summaries | 100% |
| RAI3 | Missing data | State that summarisation is unavailable when a record has no abstract. | Test at least 10 missing-abstract records or controlled cases. | Missing-abstract cases correctly handled | 100% |
| RAI4 | Privacy / external processing | Do not send abstract text to the OpenAI API. | Inspect code and API configuration during summarisation tests. | OpenAI API calls containing abstract text | 0 |

## Reflection
Summary faithfulness may be the hardest Responsible AI requirement to measure. A fluent summary can still omit a qualification or introduce an unsupported detail, so it needs manual comparison against the original abstract using a consistent checklist.

## GitHub checkpoint
Save as `docs/week2_requirements.md`.

```bash
git add docs/week2_requirements.md
git commit -m "Week 2: Requirements specification (FR, NFR, RAI)"
git push
```
