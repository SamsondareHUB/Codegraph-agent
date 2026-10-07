# CodeGraph-Agent

**CodeGraph-Agent** is a lightweight framework for graph-based bug localization in Python repositories, designed for agentic software engineering with small, local language models (e.g., Gemma 4).

## What it does

- Builds a **typed code graph** from a Python repository:
  - Nodes: files and functions.
  - Edges: CALLS (function → function) and IMPORTS (file → file).
- Ranks candidate functions for a given **natural-language bug description** by combining:
  - Text similarity between the bug and function documentation (sentence embeddings).
  - Graph-based importance (PageRank on the call graph).
- Returns **top-k functions** to condition an LLM agent (e.g., Gemma 4) for patch generation.

## Repository contents

- `codegraph_agent.ipynb` – Kaggle notebook demonstrating the full pipeline:
  - Graph construction.
  - Function extraction.
  - Bug-to-function ranking.
  - Demo experiments on synthetic repositories.
- `codegraph.py` – Core implementation:
  - `build_repo_code_graph(repo_dir)`
  - `extract_function_texts(repo_dir)`
  - `rank_functions_for_bug(G, func_info, bug_description, top_k=5)`

## Requirements

- Python 3.9+
- `networkx`
- `sentence-transformers`
- `numpy`

Install with:

```bash
pip install networkx sentence-transformers numpy
```

## Basic usage

```python
from codegraph import build_repo_code_graph, extract_function_texts, rank_functions_for_bug

repo_dir = "path/to/your/python/repo"

# Build graph and extract function metadata
G = build_repo_code_graph(repo_dir)
func_info = extract_function_texts(repo_dir)

# Rank functions for a bug query
bug = "Preprocessing does not normalize the data properly."
top_funcs = rank_functions_for_bug(G, func_info, bug, top_k=5)

print("Top functions:")
for fid in top_funcs:
    print(fid, func_info[fid])
```

## Relation to Kaggle competition

This project was developed for the **Google – The Gemma 4 Developer Agent Paper Track** on Kaggle.  
- Kaggle Writeup: https://www.kaggle.com/competitions/gemma-4-developer-agent-paper/writeups/codegraph-agent-graph-structured-code-representation 
- Public notebook: https://www.kaggle.com/code/samsonoluwadare/codegraph-agent/edit

## License

MIT
