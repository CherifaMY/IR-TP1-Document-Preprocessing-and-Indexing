# IR-TP1: Document Preprocessing and Indexing

## How to Run
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Run the Jupyter Notebook `tp1_preprocessing_indexing.ipynb` to execute the preprocessing pipeline, build the matrices and positional inverted index, run all verification assertions, and generate all output files inside the `results/` directory.

## Implementation Choices
- **Preprocessing:** Lowercasing and regex punctuation stripping (`[^\w\s]`) were implemented manually, followed by NLTK's English stop-word list and `PorterStemmer`.
- **Data Structures:** Vocabulary, binary/TF matrices, and the positional inverted index (`term -> doc_id -> [positions]`) were built from scratch using Python dictionaries and `defaultdict`, with `pandas` used solely for tabular display and CSV export.

## Main Results
| Version | Vocabulary Size | Avg Tokens/Doc | Total Postings | Total Stored Positions |
| :--- | :---: | :---: | :---: | :---: |
| Version A | 38 | 6.8 | 64 | 68 |
| Version B | 31 | 5.8 | 55 | 58 |
| Version C | 21 | 5.8 | 53 | 58 |

## Interpretation
Preprocessing progressively compacts the index while shifting the balance between precision and recall. Lowercasing and punctuation removal unify surface-level variations without losing semantic content. Stop-word removal eliminates non-informative function words (`the`, `and`, `to`), reducing average document length from 6.8 to 5.8 tokens and lowering total stored positions from 68 to 58, though it alters relative token offsets and prevents exact phrase matching on removed terms. Porter stemming produces the largest vocabulary reduction (31 to 21 terms) by conflating morphological variants such as `index`, `indexed`, and `indexing` into a single stem (`index`). While stemming improves recall by matching related word forms, it does not always improve retrieval: aggressive suffix stripping can merge distinct concepts (over-stemming) and degrade precision when queries require exact grammatical forms.
