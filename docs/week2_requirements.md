# Requirements Specification — ResearchLens

**Project Title:** ResearchLens — AI Research Paper Discovery and Summarisation Assistant  
**Version:** 1.0

---

# Section 0 — Project Framing & Application Scope Checkpoint

| Framing Element | ResearchLens |
|---|---|
| **Problem Evidence** | University students and early-stage researchers may spend considerable time finding relevant academic papers and reviewing abstracts to decide which papers are useful.<br><br>- Shuhidan et al. (2019) discusses Malaysian university students' difficulties in locating and evaluating academic information. [Source](https://ibimapublishing.com/articles/JELHE/2019/822970/)<br><br>- Gusenbauer and Haddaway (2020) evaluates 28 academic search systems. [Source](https://pmc.ncbi.nlm.nih.gov/articles/PMC7079055/)<br><br>- Arnold et al. (2023) reviews information overload. [Source](https://pmc.ncbi.nlm.nih.gov/articles/PMC10322198/)<br><br>These sources support the broader problem of academic information searching and information overload.|
| **Existing Approaches** | - **OpenAlex** — An open scholarly catalogue and API for searching academic works and retrieving metadata. [Source](https://openalex.org/) <br><br> - **Semantic Scholar** — An academic search platform providing paper details, abstracts, and citation information when available. [Source](https://www.semanticscholar.org/) <br><br> - **arXiv** — A public repository of research papers with an API for searching records and retrieving metadata. [Source](https://arxiv.org/) |
| **Project / Engineering Gap** | - **OpenAlex** — Abstracts and full-text documents are not available for every record, so some papers cannot be summarised. <br><br> - **Semantic Scholar** — Some papers have missing abstracts or unavailable full-text documents. <br><br> - **arXiv** — Its coverage focuses on specific research fields, and preprints are not necessarily peer-reviewed. |
| **Objective O1** | To develop a Streamlit application that retrieves academic papers from OpenAlex based on research topics and selected filters. |
| **Objective O2** | To implement abstract summarisation using a locally run pretrained NLP model, with a Python-based extractive summarisation fallback. |
| **Intended Users** | - University students conducting assignments or final-year projects.<br><br>- Early-stage researchers exploring research topics.<br><br>- Users who want to identify relevant academic papers more efficiently. |
| **Supported Tasks** | - Search for academic papers using topics or keywords.<br><br>- Filter results by publication year or year range.<br><br>- View available titles, authors, publication years, citation counts, DOIs, and source links.<br><br>- View available abstracts and generate concise summaries.<br><br>- Handle missing abstracts, empty search results, API failures, and summarisation errors. |
| **Information / Knowledge Domain** | - **Domain:** Academic Research and Scholarly Information Retrieval.<br><br>- **AI techniques:** Information retrieval and natural language processing (text summarisation).<br><br>- **Course discipline:** AI Engineering. |
| **Operating Context** | - A web application developed using Python and Streamlit.<br><br>- OpenAlex is the primary source of academic records.<br><br>- A local pretrained summarisation model, `sshleifer/distilbart-cnn-12-6`, is planned, with an extractive Python fallback if needed.<br><br>- An internet connection is required to retrieve records from OpenAlex.<br><br>- Summarisation is intended to run locally rather than through the OpenAI API. |
| **Important Constraints** | - Abstracts may not be available for every paper.<br><br>- The system summarises abstracts, not complete papers.<br><br>- Search results depend on available OpenAlex records and metadata.<br><br>- Local model performance depends on computing resources.<br><br>- Summaries may omit details or contain errors and should be checked against the source.<br><br>- The system must not assume every paper is peer-reviewed. |
| **Explicit Out-of-Scope Tasks** | - Summarising complete full-text papers.<br><br>- Integrating Semantic Scholar or arXiv as additional runtime data sources in V1.<br><br>- Replacing professional literature reviews or researchers' critical evaluation.<br><br>- Guaranteeing that all generated summaries are error-free. |
| **Evaluation Focus** | The evaluation will determine whether ResearchLens can retrieve relevant academic papers, display available metadata and source links correctly, produce useful and faithful abstract summaries, respond within the proposed search-time target, and handle missing data and errors appropriately. |

---

# Section 1 — Functional Requirements

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT (where relevant) |
|---|---|---|
| **FR1** | Allow users to search academic papers by entering a research topic or keywords and retrieve matching records from OpenAlex. | - Present search results as guaranteed to be exhaustive or perfectly relevant.<br><br>- Invent academic records that are not returned by the source. |
| **FR2** | Allow users to filter academic paper results by publication year or year range. | - Claim that a filter has been applied correctly when the required metadata is unavailable. |
| **FR3** | Display available paper metadata, including title, authors, publication year, citation count, DOI, and source link. | - Invent missing authors, dates, citation counts, DOIs, or other metadata. |
| **FR4** | Generate a concise summary from an available academic abstract using the planned local summarisation capability. | - Generate an abstract-based summary when the abstract is missing.<br><br>- Claim that the summary covers the complete paper. |
| **FR5** | Display the generated summary alongside the original abstract and source link when available. | - Present the summary as a replacement for the original source or as guaranteed to be completely accurate. |
| **FR6** | Handle missing abstracts, empty search results, API failures, and summarisation errors with appropriate messages or fallback behaviour. | - Allow expected error conditions to cause an unhandled application crash.<br><br>- Fabricate missing abstracts or summaries. |

---

# Section 2 — Non-Functional Requirements

*Note: The thresholds below are proposed starting targets for testing, not completed test results or confirmed lecturer requirements.*

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **NFR1** | Performance | The system shall return search results within the proposed response-time target under normal conditions. | Measure response time for at least 20 search queries, excluding unusual external API outages. | 90th-percentile (p90) search response time. | ≤ 5 seconds |
| **NFR2** | Reliability | The system shall handle expected errors without an unhandled application crash. | Test empty results, API errors, missing abstracts, and model failures. | Percentage of tested error cases handled without an unhandled crash. | ≥ 95% |
| **NFR3** | Correctness | Generated summaries shall not introduce unsupported factual claims from the original abstract. | Review at least 20 summaries against their original abstracts using a factual-support checklist. | Percentage of reviewed summaries without unsupported factual claims. | ≥ 90% |
| **NFR4** | Maintainability | The system shall organise search, abstract processing, and summarisation into identifiable, testable modules. | Inspect the code structure and test components individually. | Presence of separately identifiable and testable modules for the main functions. | Search, abstract processing, and summarisation are separated into identifiable modules. |

---

# Section 3 — Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| **RAI1** | Grounding | The system shall display available source links and the original abstract when available, allowing users to verify the information. | Inspect a sample of retrieved records and their displayed details. | Percentage of tested records with available source links correctly displayed and original abstracts shown when available. | 100% of tested records with available source links display those links; original abstracts are shown when available. |
| **RAI2** | Transparency | The system shall label generated summaries as being based on abstracts. | Inspect all tested summary outputs. | Percentage of tested summaries labelled as abstract-based. | 100% |
| **RAI3** | Factual Accuracy | The system shall clearly handle missing abstracts and shall not fabricate an abstract or generate an abstract-based summary from unavailable text. | Test records with missing abstracts. | Percentage of tested missing-abstract cases handled clearly without fabricated abstracts or summaries. | 100% |

---

## Reflection — Which Responsible AI Requirement Will Be the Most Difficult to Measure or Enforce Technically? Why?

The most difficult requirement to measure is summary faithfulness because a summary may sound clear while omitting important information or introducing claims that are not supported by the original abstract.

To evaluate this, I will compare generated summaries against their original abstracts using a checklist. I will check whether the main claims are supported by the source and whether unsupported factual statements have been introduced. The proposed target is that at least 90% of reviewed summaries contain no unsupported factual claims.

---
