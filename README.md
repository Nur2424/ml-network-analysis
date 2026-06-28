# ML Repo Network Analysis

A network science analysis of the machine learning / AI open-source ecosystem on GitHub. This project maps how ~500 of the most popular ML/AI repositories are connected through their shared contributors, and asks what structure emerges from those collaboration patterns.

**[View the live presentation →](https://nur2424.github.io/ml-network-analysis/presentation.html)**

---

## Key findings

- **The network is highly non-random.** Its clustering coefficient is **5.3× higher** than an Erdős–Rényi random graph of the same size and density contributors work in overlapping circles, not random connections.
- **The ecosystem has a core-periphery structure.** The degree distribution is bimodal: a dense core of foundational repos (TensorFlow, PyTorch, HuggingFace Transformers, OpenCV) whose contributors circulate freely, surrounded by a larger periphery of specialised projects.
- **Communities exist but are porous.** Louvain detection finds 11 communities with modularity Q = 0.21 below the 0.3 "strong structure" threshold, indicating that contributors cross sub-field boundaries rather than staying siloed.
- **Network position does not predict popularity.** A two-sample t-test (p = 0.24) shows core and periphery repos have statistically indistinguishable star counts being a contributor hub and being famous are independent.

---

## Dataset & methodology

Data was collected from the **GitHub REST API** (Search + Contributors endpoints) across four topics: `machine learning`, `deep learning`, `neural-network`, and `artificial intelligence`, filtered to repositories with more than 100 stars.

The analysis builds a **bipartite graph** of repositories and contributors, then projects it onto the repository side: two repos are connected if they share at least one contributor, weighted by the number shared. Analysis is restricted to the **largest connected component** (412 of 500 nodes), since most network metrics require a connected graph.

**Techniques applied:** degree distribution analysis (log-log), Erdős Rényi null model comparison, degree / betweenness / eigenvector centrality, Louvain community detection, attribute assortativity, and hypothesis testing (two-sample t-test).

| Metric | Value |
|---|---|
| Repositories analyzed | 500 |
| Network nodes (largest component) | 412 |
| Collaboration edges | 8,960 |
| Average degree | 43.5 |
| Network density | 0.072 |
| Clustering coefficient | 0.564 (vs 0.107 random) |
| Communities (Louvain) | 11 |
| Modularity Q | 0.21 |

---

## Repository structure

```
ml-network-analysis/
├── notebooks/
│   ├── 01_fetch_repos.ipynb          # Pull repo metadata via Search API
│   ├── 02_fetch_contributors.ipynb   # Pull top 30 contributors per repo
│   ├── 03_clean_and_explore.ipynb    # Clean data, build edge list
│   ├── 04_build_network.ipynb        # Bipartite graph, projection, ER null model
│   └── 05_centrality_communities.ipynb  # Centrality, communities, hypothesis tests
├── data/
│   └── processed/                    # Cleaned CSVs + saved graph
├── plots/                            # Generated figures
├── presentation.html                # Slide deck
├── requirements.txt
└── WORKFLOW.md                       # Development workflow reference
```

---

## Reproducing the analysis

```bash
# Clone and set up environment
git clone https://github.com/Nur2424/ml-network-analysis.git
cd ml-network-analysis
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Add a GitHub personal access token (scope: public_repo) to a .env file
echo "GITHUB_TOKEN=your_token_here" > .env

# Run notebooks 01 → 05 in order
```

Notebooks 01 and 02 pull live data from the GitHub API and require a token. Notebooks 03–05 run from the saved data in `data/processed/`.

---

## Tech stack

`Python` · `pandas` · `networkx` · `scipy` · `matplotlib` · `GitHub REST API`

---

## Context

Built for the **Data Management and Analysis** course (Prof. Walter Quattrociocchi) in the Computer Science and AI bachelor program at **Sapienza University of Rome**.

A predictive extension training a multilayer perceptron to forecast repository star growth from network and content features is in progress as a portfolio continuation beyond the course.

**Author:** [Nur2424](https://github.com/Nur2424)
