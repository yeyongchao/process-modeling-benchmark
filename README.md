# little m: An AI Agent for Industrial Process Optimization

This is the official open-source repository accompanying the paper **“little m: An AI Agent for Industrial Process Optimization.”** It provides the IPC-Bench evaluation benchmark, a reference implementation of the little m workflow, and the domain knowledge base used for retrieval.

## IPC-Bench

IPC-Bench is the multimodal benchmark introduced in the paper for evaluating industrial process optimization modeling. It contains **50 canonical, textbook-derived scenarios** from process control and chemical engineering optimization.

Each case combines a natural-language process description with any available process diagram and asks the system to formulate a structured mathematical optimization model. The benchmark therefore evaluates both semantic interpretation and grounding in process structure, rather than text-only equation generation.

### Dataset Structure

```
data/
├── 1.md – 50.md          # 50 problem descriptions with ground truth models
└── figures/               # Process flow diagrams and schematics (41 images)
    └── {id}_{source-case}_{figure-index}.png
```

### Case Format

Each markdown file (`{id}.md`) contains a structured optimization problem with:

- **Problem Description** — Narrative background describing the industrial process, operational goals, and physical context, with an embedded figure reference.
- **Variables** — Decision variables with mathematical symbols and physical descriptions.
- **Objective Function** — The optimization target (`min`/`max`) in LaTeX.
- **Constraints** — Physical, operational, and safety constraints in LaTeX, including system dynamics, bounds, and conservation laws.

## Implementation and Knowledge Base

The reference release consists of two files:

| File | Description |
|------|-------------|
| `Agent_EN.yml` | The [Dify](https://github.com/langgenius/dify) implementation of the paper's non-interactive workflow: information structuring, task planning, retrieval enhancement, knowledge retrieval, strategy determination, and mathematical modeling. |
| `industrial_optimization_knowledgebase.md` | The knowledge base used by the retrieval stage. It contains 45 general industrial pathways and 10 brewing-specific pathways organized around symptoms, optimization objectives, candidate algorithms, required information, mathematical patterns, and applicability limits. |

To reproduce the reference implementation in Dify, upload `industrial_optimization_knowledgebase.md` as a knowledge base, import `Agent_EN.yml`, configure the required model providers, and bind the uploaded knowledge base to the retrieval node with the same name. Knowledge-base IDs are specific to each Dify instance, so the binding must be configured after import.

## Citation

```bibtex
@inproceedings{ye2026little,
  title     = {{little} m: An AI Agent for Industrial Process Optimization},
  author    = {Ye, Yongchao and He, Xinyu and Boshoff, Dutliff and Kuo, Way and Li, Lishuai},
  booktitle = {Findings of the 2026 Conference on Empirical Methods in Natural Language Processing, EMNLP 2026},
  year      = {2026},
  address   = {Budapest, Hungary},
}
```
