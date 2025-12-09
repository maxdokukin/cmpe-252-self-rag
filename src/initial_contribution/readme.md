# Phase 1: Self-RAG Baseline (Monolithic Architecture)

## Overview
This section of the codebase implements the **foundational Self-RAG pipeline**. It sets up the retrieval environment, loads the knowledge base, and defines the standard generation loop.

**CRITICAL NOTE:** This implementation represents the **unoptimized baseline**. It contains **NO SEMANTIC ROUTER**. Every user query is forced through the full retrieval and reflection pipeline, regardless of intent. This provides the control group for comparing latency and quality against the routed architecture.

## Architecture (Monolithic)
The system follows a linear execution path for all inputs:
1.  **Retrieve:** Query -> Search Vector DB (Wikipedia) -> Fetch Top-5 Passages.
2.  **Generate:** Prompt LLM with context -> Generate Answer.
3.  **Reflect:** (Optional) Critique answer with Self-RAG reflection tokens.

## Code Structure (Cells 0-12)

### **Infrastructure & Setup**
* **Cell 0-1:** Runtime verification (A100 GPU required) and dependency installation (`vllm`, `faiss`).
* **Cell 2-3:** Knowledge Base setup. Clones the Self-RAG repo and initializes the FAISS index with a Wikipedia subset (~9GB).
* **Cell 4:** Loads the `selfrag_llama2_7b` model using vLLM for high-performance inference.

### **The RAG Pipeline**
* **Cell 6 (`ask`):** The standard RAG function. It blindly retrieves documents for every query and generates an answer based on utility scores.
* **Cell 9 (`ask_with_reflection`):** Implements the "Self-Critique" loop. The model reviews its own generated answer to correct hallucinations, adding significant latency.
* **Cell 10 (`ask_full_selfrag`):** The "Judge" layer. It creates a feedback loop where the model evaluates multiple candidate answers to select the best one.

### **Evaluation (Baseline)**
* **Cell 11-12:** Runs a benchmark on test queries. Since there is **no router**, simple queries like "Hi" or math problems like "Calculate 2+2" are inefficiently processed via document search, establishing the high-latency baseline.

## Usage
Run these cells sequentially to establish the `llm` and `retriever` objects. This state is required before introducing the Router optimization in later cells.