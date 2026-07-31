<div align="center">

⚖️ CITADREX

Citation-Aware Legal Information Retrieval

An experimental retrieval pipeline for mapping complex legal questions to relevant Swiss statutes, court decisions, and legal references.

<p>
  <img src="https://img.shields.io/badge/Domain-Legal%20Information%20Retrieval-1F2937?style=for-the-badge" alt="Legal Information Retrieval" />
  <img src="https://img.shields.io/badge/Language-Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Approach-TF--IDF%20%2B%20Co--citation-7C3AED?style=for-the-badge" alt="TF-IDF and Co-citation" />
  <img src="https://img.shields.io/badge/Status-Experimental-D97706?style=for-the-badge" alt="Experimental" />
</p>

Overview •Retrieval Task •Pipeline •Results •Repository •Limitations

</div>

📌 Overview

CITADREX is an experimental legal information-retrieval project designed to predict relevant legal citations for natural-language legal questions.

The repository focuses on Swiss legal material and includes references such as:

Federal statutory provisions, for example Art. 221 Abs. 1 StPO

Civil Code provisions, for example Art. 8 ZGB

Code of Obligations provisions, for example Art. 41 Abs. 1 OR

Federal Supreme Court decisions, for example BGE 137 IV 122 E. 6.2

Individual decision references, for example 1B_210/2023 E. 4.1

CITADREX approaches citation prediction as a ranking problem. Given a detailed legal question, the system generates an ordered list of legal authorities that may be relevant to that question.

Legal question → Candidate generation → Citation ranking → Predicted legal references

The project is intended for retrieval research, ranking experiments, and manual result analysis. It is not a legal-advice system and should not be used as a substitute for professional legal review.

🎯 Project Objective

Legal research is more complex than ordinary keyword search.

A single legal question may involve:

Multiple statutes

Different procedural and substantive rules

Abbreviated legal references

Citations to previous judgments

Repeated legal terminology

Context-dependent relevance

Relationships between authorities that are not obvious from lexical similarity alone

The goal of CITADREX is to explore whether lightweight retrieval techniques can identify useful citations by combining:

Query-text similarity

Law-text similarity

Citation-frequency signals

Co-citation relationships

Nearest-neighbor transfer

Ranking heuristics

Adaptive output-length selection

Human review and manual correction

🔎 Retrieval Task

Input

A legal question represented by a unique query identifier:

query_id,query
test_001,"A detailed legal question..."

Output

An ordered, semicolon-separated citation list:

query_id,predicted_citations
test_001,Art. 10 Abs. 2 URG;Art. 2 UWG;Art. 17 IPRG

Expected Behaviour

For each query, the retrieval system should:

Understand the legal issue expressed in the query

Generate a broad set of candidate citations

Score candidates using available retrieval signals

Rank the candidates

Select an appropriate number of citations

Export the final predictions in submission format

🧠 Retrieval Pipeline

The execution log documents the following experimental pipeline.

flowchart TD
    A["Training, validation and test queries"] --> B["Text preprocessing"]
    C["German law corpus"] --> D["Law-text index"]
    E["Court citation corpus"] --> F["Court citation index"]

    B --> G["Query TF-IDF matrices"]
    D --> H["Law-text TF-IDF matrices"]
    F --> I["Citation candidates"]

    E --> J["Co-citation graph"]
    A --> K["Nearest-neighbor query matching"]

    G --> L["Candidate scoring"]
    H --> L
    I --> L
    J --> L
    K --> L

    L --> M["Ranked citation candidates"]
    M --> N["Fixed-k evaluation"]
    M --> O["Adaptive-k evaluation"]
    N --> P["Strategy selection"]
    O --> P

    P --> Q["Test-query predictions"]
    Q --> R["Submission CSV files"]
    Q --> S["Review and override artifacts"]

1. Data Loading

The experiment loads:

Training queries

Validation queries with gold citations

Test queries

A German-language law corpus

Court citation identifiers and citation text

2. Corpus Construction

The recorded run processed multiple court-data chunks and reported a final corpus size of:

2,161,111 records

3. Co-Citation Graph

A co-citation graph is built to model relationships between legal authorities that appear together.

This allows the system to use citation structure in addition to direct text similarity.

4. Law Indices

The pipeline builds searchable indices for legal provisions and court references.

5. TF-IDF Representation

The experiment creates:

Query TF-IDF matrices

Law-text TF-IDF matrices

These representations provide lightweight lexical matching between questions and legal material.

6. Nearest-Neighbor Transfer

The system evaluates whether citations associated with similar validation or training queries can be transferred to a new query.

The run tested threshold pairs for high- and medium-confidence nearest-neighbor copying.

7. Candidate Ranking

Candidate citations are combined and ranked using the available retrieval signals.

The repository does not currently include the source notebook, so the exact score formula and feature weights cannot be verified from the committed files.

8. Output-Length Selection

The pipeline compares two approaches:

Fixed-k: return the same number of citations for every query

Adaptive-k: vary the number of citations according to query-level ranking behaviour

📊 Experiment Results

The committed execution log records validation over 10 validation queries.

Nearest-Neighbor Threshold Sweep

High threshold

Medium threshold

Validation F1@8

0.80

0.60

0.1667

0.75

0.55

0.1667

0.70

0.50

0.1667

0.65

0.50

0.1667

0.60

0.45

0.1667

Selected thresholds:

high = 0.80
medium = 0.60

Fixed-k Validation

k

Validation F1

2

0.0938

3

0.1167

5

0.1411

6

0.1533

8

0.1667

10

0.1793

12

0.1802

15

0.1804

20

0.1712

25

0.1821

30

0.1739

Selected Strategy

Strategy

Validation F1

Best fixed-k (k=25)

0.1821

Adaptive selection

0.2012

The recorded run selected the adaptive strategy and generated predictions for 40 test queries.

These scores come from a very small validation set and should be treated as experimental indicators rather than production-level performance estimates.

📂 Repository Structure

citadrex-legal-ir/
│
├── INPUT/
│   ├── court_consideration example.txt
│   ├── laws_de.7z
│   ├── sample_submission.csv
│   ├── train.csv
│   ├── val.csv
│   └── test.csv
│
├── OUTPUT/
│   ├── candidate_long.csv
│   ├── manual_overrides_template.csv
│   ├── output.png
│   ├── review_pack.csv
│   ├── submission.csv
│   ├── submission_adaptive.csv
│   ├── submission_k5.csv
│   ├── submission_k8.csv
│   ├── submission_k10.csv
│   └── submission_k12.csv
│
├── LOG/
│   └── citadrex-t1.log
│
└── README.md

🗃️ Repository Artifacts

Input Files

File

Purpose

court_consideration example.txt

Example citation-and-text representation from court material

laws_de.7z

Compressed German-language legal corpus

train.csv

Training data used by the retrieval experiment

val.csv

Validation queries with gold citation lists

test.csv

Test queries requiring citation predictions

sample_submission.csv

Required output-column example

Output Files

File

Purpose

submission.csv

Selected final prediction file

submission_adaptive.csv

Predictions produced by adaptive output selection

submission_k5.csv

Fixed top-5 citation predictions

submission_k8.csv

Fixed top-8 citation predictions

submission_k10.csv

Fixed top-10 citation predictions

submission_k12.csv

Fixed top-12 citation predictions

manual_overrides_template.csv

Template for manually replacing predictions

candidate_long.csv

Candidate-level analysis artifact

review_pack.csv

Review-oriented inspection artifact

output.png

Saved visual output from the experiment

Log File

LOG/citadrex-t1.log records:

Corpus loading

Court-data chunk processing

Index construction

Threshold sweeping

Fixed-k validation

Adaptive strategy evaluation

Test-query scoring

Exported file paths

Total execution timing

🔁 Human-in-the-Loop Review

CITADREX includes a manual override template with the following structure:

query_id,query,manual_predicted_citations
test_001,"A detailed legal question...",

This supports a review process where a legal researcher can:

Inspect the original question

Review the machine-generated predictions

Compare alternative top-k outputs

Replace weak or incorrect citations

Export a corrected final prediction set

flowchart LR
    A["Machine predictions"] --> B["Review candidate citations"]
    B --> C{"Prediction acceptable?"}
    C -- Yes --> D["Keep prediction"]
    C -- No --> E["Add manual override"]
    D --> F["Final citation list"]
    E --> F

🛠️ Methods Represented in the Experiment

Based on the committed run log and generated artifacts, the project uses or evaluates:

Legal text normalization

TF-IDF vectorization

Lexical similarity

Query nearest-neighbor retrieval

Co-citation graph construction

Law and court-citation indexing

Candidate generation

Heuristic ranking

Threshold calibration

Fixed top-k selection

Adaptive top-k selection

F1-based validation

Manual override workflow

⚠️ Current Repository Status

This repository currently stores the datasets, generated outputs, execution log, and experiment documentation.

It does not currently include:

The executable Python script

The original notebook

A requirements.txt file

An environment specification

Automated tests

A command-line interface

A web interface

A software license

Because the source pipeline is not committed, the experiment cannot currently be reproduced directly from this repository.

To Make the Project Reproducible

Add:

src/
├── preprocess.py
├── build_indices.py
├── retrieve.py
├── evaluate.py
└── export.py

notebooks/
└── citadrex_experiment.ipynb

requirements.txt
LICENSE

Then document a runnable command such as:

python -m src.retrieve

Only add that command after the corresponding executable code exists.

⚠️ Current Limitations

The executable ranking implementation is missing from the repository.

Exact ranking weights and candidate-merging rules cannot be verified.

Validation contains only 10 labelled queries.

The reported metric may vary considerably on a larger evaluation set.

Query text and legal corpus language may differ, reducing lexical matching quality.

TF-IDF cannot fully capture legal reasoning or semantic equivalence.

High-frequency citations can dominate heuristic ranking.

Nearest-neighbor transfer can repeat irrelevant citations from superficially similar questions.

Large top-k values may improve recall while reducing precision.

candidate_long.csv and review_pack.csv are currently empty in the committed repository.

Predictions may contain legally related but incorrect or incomplete authorities.

Generated citations require expert legal verification.

🔮 Development Roadmap

Commit the original experiment notebook

Convert notebook logic into reusable Python modules

Add requirements.txt

Add deterministic configuration and random seeds

Document the exact ranking formula

Add preprocessing tests

Add citation-format validation

Merge duplicate citation variants

Add BM25 retrieval

Add multilingual sentence embeddings

Add cross-encoder reranking

Add graph-based citation propagation

Evaluate precision, recall, MAP, MRR, and nDCG

Expand the validation dataset

Add error-analysis notebooks

Build an interactive citation-review interface

Add model and dataset cards

Add a project license

⚖️ Responsible Use

CITADREX is a research and experimentation project.

Its outputs:

May contain incorrect citations

May omit controlling legal authority

May rank outdated or contextually irrelevant provisions

Must be reviewed by a qualified legal professional

Must not be treated as legal advice

Users are responsible for checking the licensing and usage terms of every included dataset and legal corpus.

👨‍💻 Author

<div align="center">

Nithish Sarwin

Artificial Intelligence & Machine Learning StudentJava and Backend Developer

GitHub ·LinkedIn ·Email

</div>

<div align="center">

Exploring transparent and reviewable retrieval methods for legal citation discovery.

</div>
