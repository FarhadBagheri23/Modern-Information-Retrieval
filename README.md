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

## Running

```bash
pip install -r requirements.txt
jupyter lab   # then open the notebook and Run All
```
