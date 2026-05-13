# Code: "little m" Agent Implementation

## Overview

This directory contains the implementation configuration of **"little m"**, an AI agent for verifiable synthesis of industrial process optimization models.

## Files

```
data_and_code/
├── Agent_EN.yml       # Dify workflow configuration (agent implementation)
├── readme_code.md     # This file
```

## Agent Configuration (`Agent_EN.yml`)

The agent is implemented as a **Dify advanced-chat workflow** and can be imported directly into a Dify instance. The YAML file defines the complete agent architecture including node graph, edges, LLM configurations, knowledge retrieval settings, and system prompts.

### Requirements

- **Dify** (v0.4.0+)
- **Plugins** (installed via Dify marketplace):
  - `langgenius/gemini` — Google Gemini models for reasoning and classification
  - `langgenius/siliconflow` — SiliconFlow for the BGE reranker model
  - `langgenius/openai` (if using OpenAI embeddings)

### Required API Keys

| Provider | Model(s) | Purpose |
|----------|----------|---------|
| Google | `gemini-2.5-pro`, `gemini-2.5-flash`, `gemini-2.5-flash-lite` | Main reasoning, classification, fallback |
| OpenAI | `text-embedding-3-large` | Knowledge base embeddings |
| SiliconFlow | `BAAI/bge-reranker-v2-m3` | Retrieval reranking |

### Deployment

1. Import `Agent_EN.yml` into Dify as a new application.
2. Configure API credentials for the required model providers.
3. Create a knowledge base with domain-specific content (physical principles, control heuristics, optimization algorithms) and link it to the workflow's knowledge retrieval nodes.
4. Publish the application.

## Architecture

The workflow implements a multi-stage cognitive pipeline:

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

