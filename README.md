# Modern Information Retrieval

Hands-on implementations I built while studying **Modern Information Retrieval** at
**Sharif University of Technology – Kish International Campus**. Each project implements
a core piece of a search engine from scratch (standard library only, no search frameworks),
with an emphasis on understanding the internals rather than calling a black box.

## Projects

### [Boolean Search Engine](./boolean-search-engine)
A complete, compressed, **positional inverted-index** search engine over ~6,200 academic
papers, with separate indexes for titles and abstracts and strict boolean **AND** queries.

Built end to end:

| Stage | What I implemented |
|-------|--------------------|
| **Text preprocessing** | Tokenizer (case folding + punctuation removal), frequency-based stopword detection and reporting |
| **Positional index** | Two separate `term → {doc_id: [positions]}` indexes for titles and abstracts |
| **Compression** | Variable-Byte coding over gap-encoded posting lists, with lossless save/load to disk |
| **Search** | Fielded strict-AND queries (`title` / `abstract` / `both`) via posting-list intersection |

**Result:** validated against a held-out query set, the engine recovers **58/60** expected
results — the two misses are correct behaviour of strict boolean AND, not bugs.

See the [project README](./boolean-search-engine) and the
[notebook](./boolean-search-engine/boolean_search_engine.ipynb) for the full walk-through.

### [Ranked Retrieval Engine](./ranked-retrieval-engine)
Three classic **ranked-retrieval** models built from scratch over the same ~6,185 academic
papers (title + abstract), each returning the Top-20 documents, then evaluated and compared
against a validation set of relevance judgments.

| Stage | What I implemented |
|-------|--------------------|
| **Text preprocessing** | Tokenizer (case folding + punctuation removal) and stopword removal (collection frequency > 1000) |
| **Vector Space Model** | `tf-idf` with **lnc.ltc** weighting, *fast cosine* scoring, and a **hand-written Top-K min-heap** (no full sort, no `heapq`) |
| **Cluster Pruning** | Inexact Top-K via **leader–follower** clustering — query only the nearest cluster (~√N docs) |
| **Okapi BM25** | Probabilistic relevance model with `k1=1.2, k3=1.2, b=0.75` |
| **Evaluation** | Mean Average Precision (MAP) and an overlaid 11-point interpolated Precision–Recall curve |

**Result:** on this corpus the ranking is **BM25 (MAP ≈ 0.62) > VSM (≈ 0.36) > Cluster
Pruning (≈ 0.06)** — BM25's tunable term-frequency saturation and length normalization fit
the academic abstracts best, while cluster pruning trades retrieval quality for a ~10× smaller
search space.

See the [notebook](./ranked-retrieval-engine/ranked_retrieval.ipynb) for the full walk-through.

## Running

```bash
pip install -r requirements.txt
jupyter lab   # then open the notebook and Run All
```
