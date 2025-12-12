# Enhanced Agentic RAG Pipeline: Three Strategic Notebooks

This repository is dedicated to exploring advanced techniques in Retrieval-Augmented Generation (RAG) by applying sophisticated **Prompt Engineering**, **Semantic Routing**, and **Agentic Reasoning** on top of the established SELF-RAG (Self-Refine Retrieval-Augmented Generation) architecture.

The repository contains **three distinct Jupyter notebooks**, each focused on a critical optimization area for building safe, fast, and highly grounded language models.

---

## The Three Notebooks at a Glance

| Notebook | Focus Area | Core Contribution | Key Result |
| :--- | :--- | :--- | :--- |
| **Notebook 1** | Prompt Engineering | Enhanced grounding templates, self-critique, and evaluation logic. | Significant increase in Grounding Scores via prompt design. |
| **Notebook 2** | Semantic Routing | Decoupling query intent from execution (Simple, RAG, Tool paths). | approx 45% Latency Reduction and Quality incrase |
| **Notebook 3** | Agentic Reasoning | Adding a multi-step planning and self-correction layer. | Prevents hallucinations in long reasoning chains; verifiable steps. |

---

## 1. Notebook 1: Enhanced SELF-RAG Pipeline: Prompt Engineering Improvements

This notebook focuses on maximizing answer quality through **advanced prompt engineering** *without* modifying model weights.

### Core Enhancements

This project demonstrates how prompt engineering can significantly improve RAG answer quality across diverse tasks by introducing:

* **Structured Grounding Templates:** A strict generation format that enforces: **Evidence Summary**, **Final Answer**, **Title Citations**, and an "I don't know" fallback.
* **Retrieval Controller:** A reasoning-based classifier to decide when retrieval is necessary, reducing irrelevant lookups.
* **Refined Self-Critique:** The critique logic now detects hallucinations/missing citations and **corrects only the erroneous portions** for stability.
* **Grounding-First Judge:** An LLM judge that prioritizes **Grounding** when evaluating answers, using a strict JSON schema for reproducible scores.

### 🏃 How to Run Notebook 1

| Step | Action | Description |
| :--- | :--- | :--- |
| **Step 1** | **Environment Setup (Cells 0-4)** | Verify GPU, install dependencies, clone the base SELF-RAG repository, and load the base model + retriever. |
| **Step 2** | **Prompt Engineering (Cells 5-12)** | Run sequentially to define prompt helpers (Cell 5), the **Enhanced Prompt Engineering (Core Contribution)** (Cell 9), the Updated Judge (Cell 10), run the evaluation (Cell 11), and finally, visualize the scores (Cell 12). |

### Output

Running the notebook produces:
* A summary table comparing baseline vs. enhanced answers.
* A chart showing **improved grounding scores**.
* CSV and JSON logs containing all evaluation metadata.

---

## 2. Notebook 2: Semantic Router Experiment: RAG vs. Optimized Routing

This notebook validates the use of a **Semantic Router** to improve efficiency and fidelity by matching query intent to the optimal execution strategy.

### 💡 Architecture and Optimizations

The system implements a cognitive **Router layer** that classifies user queries into three execution paths:

1.  **SIMPLE:** Direct LLM generation for conversational/creative queries. **Bypasses retrieval** for low latency.
2.  **RAG:** Standard Self-RAG pipeline (Retrieve $\rightarrow$ Generate $\rightarrow$ Critique) for factual queries.
3.  **TOOL:** Deterministic Python execution for math and counting tasks.

**Key Optimizations:**
* **Robust Math Tools:** Handles natural language inputs (e.g., "squared").
* **Negative Constraints:** Router prompt engineering prevents misclassifying numbered facts (e.g., "GPT-3") as math problems.
* **Output Cleaning:** Removes internal Self-RAG tags (e.g., `[Utility:5]`) for cleaner user output.

### 🏃 How to Run Notebook 2 (`Router_Experiment_Notebook.ipynb`)

1.  **Preparation:** Open the notebook in a **GPU-enabled environment** (e.g., Colab A100).
2.  **Setup:** Run **Cells 0-12** to set up the environment and load the base Self-RAG model.
3.  **Experiment:** Run **Cells 13-20** to define the robust Python Tools (Cell 13), the Intent Classifier/Router (Cell 14), the Smart Controller/Dispatcher (Cell 15), execute the A/B Testing Loop (Cell 17), and generate the final report (Cell 19).

### Key Results

Based on the latest A/B test (N=30):
* **Latency:** The Router architecture achieved a **~45% reduction** in average latency (2.27s $\rightarrow$ 1.26s).
* **Quality:** Average quality score improved by **+0.27 points** (2.30 $\rightarrow$ 2.57), primarily by eliminating hallucinations in computational tasks.

---

## 3. Notebook 3: Agentic Self-RAG: Enhanced Retrieval-Augmented Reasoning

This notebook introduces a full **Agentic Layer** on top of the baseline SELF-RAG system, enabling multi-step planning, verification, and self-correction.

### 🧠 Agentic Layer (Core Contribution)

The Agentic Layer transforms SELF-RAG from a simple retrieval-checker into a **safe autonomous reasoning agent** capable of:

| Agentic Step | Description |
| :--- | :--- |
| **Multi-step Planning** | Breaks down complex queries into sequential steps. |
| **Step-wise Retrieval** | A separate retrieval decision is made **per step**. |
| **Evidence-based Verification** | The agent repeatedly asks: `IsRelevant?`, `IsSupported?`, `IsUseful?` |
| **Self-Correction Loops** | Revises plans/actions based on verification results. |

This process forms a **safety loop** that prevents hallucinations in long reasoning chains.

### 🏃 How to Run Notebook 3

1.  **Install Dependencies:** Run the following command:
    ```bash
    pip install transformers sentence-transformers faiss-cpu accelerate
    ```
2.  **Run Code Block:** Copy the full code block from the repository/notebook into a Google Colab cell.
3.  **Test Harness:** The system automatically executes the benchmark:
    * Evaluates **7 reasoning scenarios**.
    * Compares the **SELF-RAG baseline** vs. the **AGENTIC SELF-RAG extension**.

### Output and Conclusion

* The notebook generates **bar charts** and **heatmaps** detailing reliability comparisons.
* All raw results are saved in `results/selfrag_agentic_scores.json`.

**Conclusion:** The Agentic Verification Layer prevents hallucinations, produces verifiable intermediate actions, and ensures evidence-backed answers, transforming the system into a **safe autonomous reasoning agent**.