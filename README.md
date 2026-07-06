# Evolve Agent — Evolutionary Algorithm Discovery System

An evolutionary optimization system that improves algorithms through LLM-guided mutations, fitness-based selection, and iterative refinement — inspired by AlphaEvolve.

---

## Overview

Evolve Agent takes an initial code solution or algorithm description and iteratively improves it across configurable generations using three mutation strategies:

- **LLM-Guided Mutation** — Gemini API generates improved candidates using high-fitness snippets as context
- **Random / AST-Based Mutation** — Structural code mutations via abstract syntax tree operations
- **Template-Based Generation** — Predefined solution patterns as candidate seeds

Evaluated on two domains: **Pacman Agent Optimization** and **3×3 Matrix Multiplication**.

---

## System Architecture

```
+-------------------------------------------------------------+
|                       Evolve Agent                          |
|                                                             |
|  +--------------+    +--------------+    +--------------+  |
|  |  Candidate   |--->|  Evaluator   |--->|  Selector    |  |
|  |  Generator   |    | (Fitness fn) |    |  (Top-k)     |  |
|  +--------------+    +--------------+    +--------------+  |
|         ^                                        |          |
|         |           Evolution Loop               |          |
|         +----------------------------------------+          |
|                                                             |
|  Strategies: No-Evolution | Random (AST) | LLM-Guided      |
|  Acceleration: Hash cache + FAISS vector DB                 |
+-------------------------------------------------------------+
```

| Component | Description |
|---|---|
| Candidate Generator | Produces mutated candidates via LLM, AST ops, or templates |
| Evaluator | Runs candidate in sandboxed environment, computes fitness |
| Selector | Retains top-k candidates for next generation |
| Cache | Hash-based memoization — avoids re-evaluating identical code |
| Vector DB | FAISS or list-based store of top-fitness snippets for LLM context |
| UI | Streamlit dashboard — fitness visualization and result export |

---

## Fitness Functions

**Pacman**
```
Fitness = w1 × score + w2 × survival_time − w3 × steps
```
Default weights: `w1=0.5, w2=0.3, w3=0.2`

**Matrix Multiplication**
```
Fitness = correctness_score (0–60) + efficiency_score (0–40)
efficiency_score = max(0, 40 × (1 − ops / 54))
```
Naive = 54 ops · Strassen ≈ 51 · Target ≤ 40 for maximum score.

---

## Setup

```bash
git clone https://github.com/siddharthaBollam/evolve-aoa.git
cd evolve-aoa
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

**Run:**
```bash
streamlit run Evolve.py
```

Paste your Gemini API key in the sidebar when the app loads.

---

## Requirements

```
streamlit>=1.35.0
matplotlib>=3.8.0
numpy>=1.26.0
google-generativeai>=0.7.0
faiss-cpu>=1.8.0          # optional
sentence-transformers>=3.0.0  # optional
```

---

## Demo

[Watch walkthrough](https://drive.google.com/file/d/1kJJ-5m57xDBkG3opdGViKpUv8GwnWC3A/view?usp=sharing)
