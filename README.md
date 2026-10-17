# ResearchLens — AI Research Paper Discovery & Summarisation Assistant

> ResearchLens AI helps university students and early-stage researchers discover relevant academic papers and understand their abstracts through concise summaries.

## Problem Statement

Students and early-stage researchers often need to search through many academic papers before finding information relevant to their research topics. Although academic search platforms provide paper details, users may still need to read multiple abstracts individually to understand each paper's main ideas. ResearchLens AI combines academic paper discovery with AI-based summarisation to make initial literature exploration more convenient.

## Evidence of the Problem

- **Difficulty finding academic information:** Shuhidan et al. (2019) identified difficulties among Malaysian university students in locating and evaluating information for academic purposes. [Source](https://ibimapublishing.com/articles/JELHE/2019/822970/)

- **Challenges in retrieving relevant research:** Gusenbauer and Haddaway (2020) evaluated 28 academic search systems and found differences in their ability to retrieve research effectively. [Source](https://pmc.ncbi.nlm.nih.gov/articles/PMC7079055/)

- **Information overload:** Arnold et al. (2023) reviewed 87 studies and reports on information overload, highlighting the broader challenges of managing large amounts of information. [Source](https://pmc.ncbi.nlm.nih.gov/articles/PMC10322198/)

These studies provide initial evidence for the challenges ResearchLens AI aims to address.

## Initial Related Work / Existing Approaches

- **OpenAlex** — An open scholarly catalogue and API for searching academic works and retrieving metadata. **Limitation:** Abstracts and full-text access are not available for every record. [Source](https://openalex.org/).
- **Semantic Scholar** — An academic search platform providing paper details and abstracts when available. **Limitation:** Its relevance search returns a maximum of 1,000 results, and some papers have missing abstracts or unavailable full-text PDFs. [Source](https://www.semanticscholar.org/).
- **arXiv** — A public repository of research papers with an API for searching records and retrieving metadata. **Limitation:** Its coverage focuses on particular research fields, and preprints should not automatically be treated as peer-reviewed publications. [Source](https://arxiv.org/).

## Project Objectives

- **O1:** To develop an application that retrieves academic papers from OpenAlex based on research topics and selected filters.
- **O2:** To implement abstract summarisation using a locally run pretrained NLP model, with a Python-based extractive summarisation fallback.

## Target Users

University students preparing assignments, research proposals or final-year projects; postgraduate students beginning literature reviews; and early-stage researchers exploring new research topics.

## Data Sources

- **OpenAlex Works API:** Live academic paper metadata, available abstracts, publication years, authors and source links. [API URL](https://help.openalex.org/api/)
- **arXiv:** Public research papers and metadata for selected test cases and evaluation. [API Documentation](https://info.arxiv.org/help/api/index.html)
- **Semantic Scholar API:** Academic paper metadata for optional comparison and cross-checking during evaluation. [API Documentation](https://api.semanticscholar.org/api-docs/)


## Weekly Progress

| Week | Focus | Deliverable | Status |
|---|---|---|---|
| W1 | System map and project scope | README and project scope | ✅ |
