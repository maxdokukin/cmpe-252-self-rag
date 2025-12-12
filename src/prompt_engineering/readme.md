# Enhanced SELF-RAG Pipeline: Prompt Engineering Improvements

### Overview
This repository contains an enhanced implementation of the SELF-RAG (Self-Refine Retrieval-Augmented Generation) pipeline.
The goal of this work is to improve grounding, reduce hallucinations, and achieve more consistent output quality through advanced prompt engineering, including:

- Structured grounding templates
- Retrieval decision refinement
- Enhanced self-critique logic
- A grounding-first LLM judge

Pipeline-wide alignment between retrieval, generation, refinement, and evaluation

This project demonstrates how prompt engineering—without modifying model weights—can significantly improve RAG answer quality across diverse tasks.

### Architecture Summary

The enhanced SELF-RAG pipeline introduces improvements in four main areas:

1. Retrieval Controller
A reasoning-based classifier determines whether retrieval should occur, reducing irrelevant or harmful retrieval for conceptual questions.

2. Grounding Template
A strict answer-generation template enforces:

- Evidence Summary
- Final Answer
- Title citations
- “I don't know” fallback
- Short, deterministic structure

3. Refined Self-Critique

The critique prompt detects hallucinations, missing citations, and unsupported reasoning.
It corrects only the erroneous portions of the answer — improving stability.

4. Grounding-First Judge

The judge LLM evaluates answers on:

- Grounding
- Clarity
- Completeness
- Reasoning

A strict JSON schema produces reproducible, comparable scores across queries.

### How to run the notebook

#### Step 1 — Environment Setup
Run Cells 0–4 to:

- Verify GPU
- Install dependencies
- Clone SELF-RAG repository
- Load the base model + retriever

#### Step 2 — Prompt Engineering Components
Run Cells 5–12 sequentially:

Cell 5: Prompt helpers
Cell 9: Enhanced prompt engineering (core contribution)
Cell 10: Updated judge
Cell 11: Baseline vs enhanced evaluation
Cell 12: Score visualization

This will produce:

- A summary table of baseline vs enhanced answers
- A chart showing improved grounding scores
- CSV + JSON logs of all evaluation metadata