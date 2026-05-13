# IPC-Bench: Industrial Process Control Benchmark

## Overview

IPC-Bench is a multimodal benchmark dataset for evaluating AI agents on industrial process optimization modeling. It contains **50 canonical optimization scenarios** sourced from seminal textbooks in process control and chemical engineering optimization.

Unlike existing benchmarks that rely on purely textual inputs, IPC-Bench requires reasoning over **multimodal inputs** (natural language descriptions paired with process flow diagrams) to formulate physically grounded mathematical models.

This repository also includes the implementation of **"little m"**, an AI agent for verifiable synthesis of industrial process optimization models.

## Dataset

```
data/
├── 1.md – 50.md          # 50 problem descriptions with ground truth models
└── figures/               # Process flow diagrams and schematics (42 images)
    ├── 01_am_*.png        # Manufacturing problems
    ├── 03_apec_*.png      # Advanced process engineering & control
    ├── 07_ced_*.png       # Chemical engineering design
    ├── 10_nonp_*.png      # Nonlinear programming
    ├── 21_optcp_*.png     # Optimal control
    ├── 26_pcpo_*.png      # Process control & optimization
    ├── 39_pd_*.png        # Process design
    └── 48_psa_*.png       # Process systems analysis
```

### Problem Format

Each markdown file (`{id}.md`) contains a structured optimization problem with:

- **Problem Description** — Narrative background describing the industrial process, operational goals, and physical context, with an embedded figure reference.
- **Variables** — Decision variables with mathematical symbols and physical descriptions.
- **Objective Function** — The optimization target (`min`/`max`) in LaTeX.
- **Constraints** — Physical, operational, and safety constraints in LaTeX, including system dynamics, bounds, and conservation laws.

## Agent Code

The agent is implemented as a **Dify advanced-chat workflow** in `Agent_EN.yml` and can be imported directly into a Dify instance.

### Requirements

- **Dify** (v0.4.0+)
- **Plugins** (installed via Dify marketplace):
  - `langgenius/gemini` — Google Gemini models
  - `langgenius/siliconflow` — SiliconFlow for BGE reranker
  - `langgenius/openai` — OpenAI embeddings

### Required API Keys

| Provider | Model(s) | Purpose |
|----------|----------|---------|
| Google | `gemini-2.5-pro`, `gemini-2.5-flash`, `gemini-2.5-flash-lite` | Reasoning, classification, fallback |
| OpenAI | `text-embedding-3-large` | Knowledge base embeddings |
| SiliconFlow | `BAAI/bge-reranker-v2-m3` | Retrieval reranking |

### Deployment

1. Import `Agent_EN.yml` into Dify as a new application.
2. Configure API credentials for the required model providers.
3. Create a knowledge base with domain-specific content (physical principles, control heuristics, optimization algorithms) and link it to the workflow's knowledge retrieval nodes.
4. Publish the application.

### Architecture

```
User Input
  → [Security Gate] Gemini 2.5 Flash (attack detection)
      → Attack blocked, or:
  → [Question Classifier] Gemini 2.5 Flash
      ├─ Optimization Exploration → RAG → Gemini 2.5 Pro
      ├─ Scene Concept Optimization → RAG → Gemini 2.5 Pro
      ├─ Specific Scene Optimization → RAG → Gemini 2.5 Pro
      │     Stage 1: Information Structuring (entity extraction, gap analysis)
      │     Stage 2: Strategy Design (variable classification, method selection)
      │     Stage 3: Mathematical Modeling (formulation + consistency checks)
      ├─ Method Consultation → RAG → Gemini 2.5 Pro
      ├─ Unclear Requirement → Gemini 2.5 Flash (clarification)
      └─ Out of Scope → Gemini 2.5 Flash Lite (fallback)
```
