# IPC-Bench: Industrial Process Control Benchmark

## Overview

IPC-Bench is a multimodal benchmark dataset for evaluating AI agents on industrial process optimization modeling. It contains **50 canonical optimization scenarios** sourced from seminal textbooks in process control and chemical engineering optimization.

Unlike existing benchmarks that rely on purely textual inputs, IPC-Bench requires reasoning over **multimodal inputs** (natural language descriptions paired with process flow diagrams) to formulate physically grounded mathematical models.

This repository also includes the implementation of **"little m"**, an AI agent for verifiable synthesis of industrial process optimization models.

## Dataset

```
data/
├── 1.md – 50.md          # 50 problem descriptions with ground truth models
└── figures/               # Process flow diagrams and schematics (41 images)
    └── {id}_{source-case}_{figure-index}.png
```

### Problem Format

Each markdown file (`{id}.md`) contains a structured optimization problem with:

- **Problem Description** — Narrative background describing the industrial process, operational goals, and physical context, with an embedded figure reference.
- **Variables** — Decision variables with mathematical symbols and physical descriptions.
- **Objective Function** — The optimization target (`min`/`max`) in LaTeX.
- **Constraints** — Physical, operational, and safety constraints in LaTeX, including system dynamics, bounds, and conservation laws.

## Agent and Knowledge Base

The repository includes two files supporting the **little m** implementation:

| File | Description |
|------|-------------|
| `Agent_EN.yml` | The Dify implementation of the non-interactive modeling workflow. It structures the input, plans the task, retrieves relevant knowledge, determines an optimization strategy, and produces the mathematical model. |
| `industrial_optimization_knowledgebase.md` | The English v3 knowledge base used by the retrieval stage. It contains 45 general industrial pathways and 10 brewing-specific pathways organized around symptoms, optimization objectives, candidate algorithms, required information, mathematical patterns, and applicability limits. |

To use the agent in Dify, upload `industrial_optimization_knowledgebase.md` as a knowledge base, import `Agent_EN.yml`, configure the required model providers, and bind the uploaded knowledge base to the retrieval node named `industrial_optimization_knowledgebase.md`. Knowledge-base IDs are specific to each Dify instance, so this binding must be configured after import.

## Citation


```bibtex
@inproceedings{ye2026ipcbench,
  title     = {{little m}: An AI Agent for Industrial Process Optimization},
  author    = {Ye, Yongchao and He, Xinyu and Boshoff, Dutliff and Kuo, Way and Li, Lishuai},
  booktitle = {Findings of the 2026 Conference on Empirical Methods in Natural Language Processing, EMNLP 2026},
  year      = {2026},
  address   = {Budapest, Hungary},
}
```
