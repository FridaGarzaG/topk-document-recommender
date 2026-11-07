# Top-K Document Recommender with Cosine Similarity (TF-IDF)

## Objective
Generate the top-5 most similar documents to a seed document (by ID) or to a free-text query, using TF-IDF and cosine similarity (Path A), and validate the results with three tests on a simulated organizational corpus.

## File structure
```
recomendador_topk/
├── recommender.ipynb
├── corpus.csv
├── requirements.txt
├── README.md
├── report.pdf
└── evidencias/
    ├── Test1_Policy_DOC001.txt
    ├── Test2_Incident_DOC014.txt
    ├── Test3_Query_FreeText.txt
    └── validation_checks.txt
```

## Corpus
31 simulated English documents: 11 *Policy/Process*, 10 *Support/Incidents*, 5 *FAQ*, and 5 *Meeting* minutes. The fields are `id`, `title`, `text`, and `category`, and the IDs are unique (DOC001 to DOC031).

## Dependencies
Python 3.9 or higher and the libraries listed in `requirements.txt`: `pandas`, `scikit-learn`, and `notebook` (or `ipykernel` if the notebook is opened in VS Code).

It is recommended to use a virtual environment:
```bash
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## Execution steps
1. Open `recommender.ipynb` in Jupyter (`jupyter notebook recommender.ipynb`) or in VS Code.
2. Select the kernel of the virtual environment (`.venv`).
3. Run all cells (*Run All*).

The notebook must be executed in the same folder as `corpus.csv`. It contains the executed outputs of the three tests (Policy/Process, Support/Incidents, and free text) and of the input validation.

## Usage example
The function `recommend` accepts a document ID, a free-text query, or both:
```python
recommend(seed_id="DOC014")
recommend(query="I can't log in because I forgot my password")
recommend(seed_id="DOC014", k=3)    # change the number of results
```

Output of the first call:
```
INPUT (ID): DOC014 - Incident: VPN Connection Failure for Remote Workers
================================================================================
Top-5 recommendations
--------------------------------------------------------------------------------
#  ID      Score    Category           Title
1  DOC009  0.3236   Policy/Process     Remote Work and VPN Access Policy
2  DOC026  0.1445   FAQ                FAQ: How do I connect to the network from home?
3  DOC013  0.1214   Support/Incidents  Ticket: User Locked Out Due to Expired Password
4  DOC020  0.1171   Support/Incidents  Incident: Network Connectivity Loss at the North Branch
5  DOC016  0.0964   Support/Incidents  Incident: Phishing Attempt Reported by Employees
================================================================================
```

An optional interactive cell (commented out) at the end of the notebook allows typing a document ID or free text.

## How it works
The text of each document (title plus content) is cleaned by converting to lowercase, removing accents and special characters, removing English stopwords, and applying light stemming (the first six letters). The cleaned text is vectorized with `TfidfVectorizer`, and `cosine_similarity` compares the seed or query vector with the corpus matrix. The scores are sorted to return the Top-K results, and the seed document is excluded when an ID is used.

## Input validation
The notebook handles an invalid ID, an empty query, a query with no known terms, missing columns, duplicated IDs, and a missing corpus file (see the section *Input Validation* of the notebook and `evidencias/validation_checks.txt`).

## Evidence
Three tests are executed inside `recommender.ipynb` and also saved as text files in `evidencias/`. Each one shows the input, the Top-5 with similarity scores, and a comment on the coherence of the recommendations:
- **Test 1 (Policy/Process):** seed document DOC001.
- **Test 2 (Support/Incidents):** seed document DOC014.
- **Test 3 (free text):** "I can't log in because I forgot my password".