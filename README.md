# CITADREX Legal Information Retrieval

CITADREX is an experimental legal information retrieval system designed for citation-aware document ranking, legal reference discovery, and structured retrieval workflow analysis.

The project focuses on identifying relevant legal references from structured legal datasets by combining text preprocessing, citation handling, lexical similarity, candidate generation, ranking heuristics, and review-oriented output generat
CITADREX is built as a lightweight retrieval engine for working with legal texts such as laws, court considerations, case-related records, and reference d
#
Legal information retrieval is different  ordinary keyword search. Legal documents often contain citations, references, formal language, repeated terminology, and context-dependent relevance. A useful retrievastem must not only match words, but also understand how legal references, document structure, and citation patterns influence relevance.

CITADREX approaches this problem through a structured retrieval pipeline. It processes legal input files, extracts useful retrieval signals, generates candidate matches, ranks those candidates, and exports multiple result files for evaluation, manual review, and refineme
The system is designed to support experimentation. Multiple output variants are generated so different ranking configurations can be compared and improved.


## Key Features

- Citation-aware legal document retrieval
- Legal text preprocessing and normalization
- Candidate reference generation
- Similarity-based ranking using lightweight NLP methods
- Heuristic score boosting and ranking refinement
- Multiple output variants for comparison
- Review pack generation for manual inspection
- Override template support for human-in-the-loop correction
- Runtime logging for reproducibility and debugging
- Organized input, output, and log folder structure

---

## Project Structure

```text
CITADREX/
├── INPU
│   ├── court_considerations.7z
│   ├── court_considerations.csv
│   ├── val.csv
│   ├── train.csv
│   ├── test.csv
│   ├── sample_submission.csv
│   └── laws_de.csv
│
├── OUTPUT/
│   ├── output.png
│   ├── submission_k8.csv
│   ├── submission_k5.csv
│   ├── submission_k12.csv
│   ├── submission_k10.csv
│   ├── submission_adaptive.csv
│   ├── submission.csv
│   ├── review_pack.csv
│   ├── manual_overrides_template.csv
│   └── candidate_long.csv
│
└── LOG/
    └── citadrex-t1.log
