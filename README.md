# Polarized Communities via Label Propagation (ASNM Project)

This repository explores the **2-Polarized Communities (2-PC)** problem on signed social networks by proposing an optimized variant of the Label Propagation Algorithm (LPA). The implementation utilizes a greedy approach driven by a **CELF-like priority queue** and an **orchestrator** for multi-run consensus, significantly outperforming traditional spectral algorithms (such as Eigensign) in terms of polarization scores.

---

## 🎯 Project Overview & Objectives

In social network analysis, identifying highly polarized, opposing communities is a computationally expensive (NP-hard) task. Most state-of-the-art approaches (like Eigensign or Random Eigensign) rely on spectral methods applied to the signed adjacency matrix, which scale well but lack flexibility and local optimization capabilities.

This project introduces **LPA 2-PC**, a semi-supervised variant of Label Propagation tailored for signed networks:
1. **Local Greedy Maximization**: Instead of a "majority vote", nodes dynamically update their labels (communities) based on the maximum expected increase in global polarity $\Delta f(x)$.
2. **CELF-like Priority Queue**: Recomputations are minimized using lazy evaluations, where only the neighboring nodes affected by an update are re-evaluated.
3. **Multi-run Orchestrator**: Because LPA is stochastic and heavily dependent on the quality of initial seed nodes, an orchestrator automates multiple executions to achieve consensus, expanding coverage and providing robust polarization outcomes.

---

## 📊 Dataset: POLITISKY24

The methods were primarily tested on **POLITISKY24**, a large-scale, multi-layer social network extracted from the BlueSky microblogging platform focusing on U.S. political discussions. 

- **Original Scope**: Over 18 million posts across ~8,000 users.
- **Data Layers**: Evaluated user interactions including *Likes*, *Reposts*, *Replies*, and *Quotes*.
- **Natural Language Inference (NLI)**: To assign positive (+1) or negative (-1) signs to text-based interactions (replies and quotes), a pre-trained DeBERTa NLI model was utilized to classify texts as `entailment`, `contradiction`, or `neutral`.
- **Final Network**: The aggregated signed graph resulted in **81,951 nodes** and **1,566,730 edges** (~64% positive, ~36% negative).

*Note: The raw datasets are excluded from this repository due to size constraints. The original POLITISKY24 dataset proposal can be referenced via [arXiv:2506.07606](https://arxiv.org/abs/2506.07606).*

---

## 📁 Repository Structure

### 1. `cumulative_to_signed_graph_format.ipynb` & `cumulative_to_signed_graph_with_index.ipynb`
These notebooks handle the data ingestion pipeline, filtering out self-loops and neutral edges, deduplicating node relationships, and standardizing the network into a clean undirected signed graph format required by the algorithms.

### 2. `layers_cumulative_sign.ipynb`
This notebook merges the independent interaction layers (Likes, Reposts, Replies, Quotes), applying heuristically-derived importance weights to produce the final comprehensive multi-layer signed graph.

### 3. `polarized_communities-master/`
Contains the implementation of the core algorithms:
- The baseline spectral models (Eigensign, Random Eigensign) based on [Bonchi et al., 2019].
- The novel **LPA 2-PC** approach featuring the CELF-like queue and the Multi-run Orchestrator.
- Evaluation scripts handling execution, patience checks, and metric computation.

---

## 🚀 Results

Across various standard datasets (e.g., *bitcoin*, *epinions*, *wikiconflict*), the orchestrated **LPA 2-PC** algorithm showcased robust adaptability. On the custom `POLITISKY24` dataset, utilizing known stance labels (e.g., Pro-Trump/Pro-Harris) as fixed initial seeds permitted the orchestrator to achieve a polarization score $f(x) = 403.32$ across 1,861 labeled nodes, surpassing both isolated LPA runs and spectral baselines while maintaining viable execution times.

---

## 🛠️ Main Requirements
- Python 3.x
- NumPy, Pandas
- NetworkX
- NLI Models (`MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli`)
- Jupyter Notebooks

---

## 👤 Author
**Jacopo Alfonso Le Pera**
