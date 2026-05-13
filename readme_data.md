# IPC-Bench: Industrial Process Control Benchmark

## Overview

IPC-Bench is a multimodal benchmark dataset for evaluating AI agents on industrial process optimization modeling. It contains **50 canonical optimization scenarios** sourced from seminal textbooks in process control and chemical engineering optimization.

Unlike existing benchmarks that rely on purely textual inputs, IPC-Bench requires reasoning over **multimodal inputs** (natural language descriptions paired with process flow diagrams) to formulate physically grounded mathematical models.

## Dataset Structure

```
data/
├── 1.md – 50.md          # Problem descriptions with ground truth models
└── figures/               # Process flow diagrams and schematics (42 images)
    ├── 01_am_*.png        # Manufacturing problems
    ├── 02_am_*.png
    ├── 03_apec_*.png      # Advanced process engineering & control
    ├── ...
    └── 50_psa_*.png       # Process systems analysis
```

## Problem Format

Each markdown file (`{id}.md`) contains a structured optimization problem with the following sections:

- **Problem Description** — Narrative background describing the industrial process, operational goals, and physical context. May include an embedded figure reference (e.g., `![alt text](figures/XX_name.png)`).
- **Variables** — Decision variables with mathematical symbols and physical descriptions.
- **Objective Function** — The optimization target (`min`/`max`) in LaTeX.
- **Constraints** — Physical, operational, and safety constraints in LaTeX, including system dynamics, bounds, and conservation laws.

### Example

```markdown
### Problem: Optimal Tracking Control of a Mixing Tank

## Input
Consider a mixing tank system with two liquid inlet streams...

### 1. Variables
- $F_1(t)$: Hot feed flow rate
- $F_2(t)$: Cold feed flow rate

### 2. Objective Function
$$\min_{F_1, F_2} J = \int_0^{t_f} \left[ w_1 (F(t) - F_{set})^2 + w_2 (T(t) - T_{set})^2 \right] dt$$

### 3. Constraints
Mass Balance: $F(t) = F_1(t) + F_2(t)$
Energy Balance: ...
Bounds: $F_1^{min} \le F_1(t) \le F_1^{max}$
```
