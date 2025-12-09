# Semantic Router Experiment: RAG vs. Optimized Routing

## Overview
This repository contains the code and experimental data for validating a **Semantic Router** architecture. The goal is to prove that decoupling query intent from execution strategy can significantly reduce latency and improve answer fidelity compared to a monolithic RAG (Retrieval-Augmented Generation) pipeline.

## Architecture
The system implements a cognitive "Router" layer that classifies user queries into three execution paths:
1.  **SIMPLE:** Direct LLM generation for conversational/creative queries. Bypasses retrieval.
2.  **RAG:** Standard Self-RAG pipeline (Retrieve -> Generate -> Critique) for factual queries.
3.  **TOOL:** Deterministic Python execution for math and counting tasks.

## Key Optimizations
- **Robust Math Tools:** Handles natural language inputs like "squared" or "times".
- **Negative Constraints:** Router prompt engineering prevents misclassifying numbered facts (e.g., "GPT-3") as math problems.
- **Output Cleaning:** Removes internal Self-RAG tags (e.g., `[Utility:5]`) for cleaner user output.

## Key Files
- `Router_Experiment_Notebook.ipynb`: The main notebook containing the full implementation:
    - **Cell 13:** Robust Python Tool definitions (Calculator, Word Counter).
    - **Cell 14:** Intent Classifier (Router) with prompt engineering.
    - **Cell 15:** Smart Controller (Dispatcher) with output cleaning.
    - **Cell 17:** A/B Testing Loop (Baseline RAG vs. Router).
    - **Cell 19:** Visualization and Reporting.
- `router_experiments/`: Directory containing CSV logs of the A/B tests.

## Results Summary
Based on the latest A/B test (N=30):
- **Latency:** The Router architecture achieved a **~45% reduction** in average latency (2.27s -> 1.26s).
- **Quality:** Average quality score improved by **+0.27 points** (2.30 -> 2.57), driven largely by the elimination of hallucinations in computational tasks.

## How to Run
1.  Open the notebook in a GPU-enabled environment (e.g., Colab A100).
2.  Run Cells 0-12 to set up the environment and load the base Self-RAG model.
3.  Run Cells 13-20 to define the Router, run the A/B test, and generate the report.