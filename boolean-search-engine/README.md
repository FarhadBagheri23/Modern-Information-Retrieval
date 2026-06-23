# Boolean Search Engine

A compressed, positional **inverted-index** search engine built from scratch over a dataset
of ~6,200 academic papers, supporting strict boolean **AND** queries on separate document
fields (title and abstract).

> Practical project for the *Modern Information Retrieval* course at
> Sharif University of Technology – Kish International Campus.

## What it does

1. **Text preprocessing** — lowercase, strip punctuation, tokenize, and detect stopwords
   by collection frequency (reporting each stopword with its count).
2. **Inverted positional index** — two independent indexes
   (`term → {doc_id: [positions]}`) for titles and abstracts, storing the exact position
   of every term occurrence.
3. **Compression & persistence** — posting lists are gap-encoded and **Variable-Byte**
   compressed, then saved to / loaded from disk (lossless round-trip verified; the abstract
   index compresses to ~69% of the raw pickle).
4. **Search** — fielded strict-AND queries (`title`, `abstract`, or `both`) implemented as
   posting-list intersection.

## Results

Run against a held-out validation set of 6 multi-term queries, the combined
(title ∪ abstract) search recovers **58 / 60** expected papers. The two misses are the
*correct* output of strict boolean AND — those papers don't contain every query term — not
errors.

## Files

| File | Description |
|------|-------------|
| `boolean_search_engine.ipynb` | The full implementation and walk-through (run top to bottom) |
| `data.csv` | The paper dataset (`paperId`, `title`, `abstract`) |
| `validation.json` | Validation queries and their expected paper IDs |

The compressed index files (`*.vb`) and `stopwords.csv` are **generated** by running the
notebook, so they are not committed.

## Run it

```bash
pip install pandas jupyter
jupyter lab boolean_search_engine.ipynb   # then Run All
```
