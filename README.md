# Process Modeling Benchmark

This repository hosts a specialized benchmark for evaluating AI agents in industrial process modeling contexts, featuring 12 diverse case studies derived from seminal chemical engineering textbooks. 
It serves as a testbed for assessing an agent's ability to bridge the gap between physical descriptions and mathematical formulations.

## File Structure

```
process-modeling-benchmark/
├── Agent_EN.yml          # Configuration file for the Dify implementation of the modeling agent "Little M"
├── Data/                 # Contains the 12 benchmark case studies (details below)
│   ├── survey_*.md       # Individual benchmark problems including Problem Description and Ground Truth Model
│   └── figs/             # System schematics and diagrams for the case studies
└── LICENSE
```

## Benchmark Cases

The `Data` folder contains 12 industrial optimization scenarios. Each `.md` file corresponds to a specific case study as detailed below:

| No. | Problem Description & Optimization Goal |
| :--- | :--- |
| 1 | Optimizes fuel and steam allocation across utilities to minimize operating costs. |
| 2 | Jointly optimizes column sizing and operating reflux ratio for minimum annualized cost. |
| 3 | Adjusts solvent flow rates to maximize separation efficiency and profit. |
| 4 | Calculates the specific reflux ratio setpoint that minimizes steam and cooling water usage. |
| 5 | Adjusts interstage pressures in a multistage compressor to minimize total energy consumption. |
| 6 | Optimizes cycle time and flow rate to maximize production rate or profit. |
| 7 | Maximizes furnace profit by adjusting feed rates and temperature severity constraints. |
| 8 | Adjusts operational setpoints (e.g., acid strength) to optimize product quality and value. |
| 9 | Determines the optimal temperature trajectory along the reactor for uniform film thickness. |
| 10 | Adjusts substrate feed rate to regulate cell mass and product formation. |
| 11 | Optimizes nitrate feed distribution across two reactor stages to maximize mononitrate yield. |
| 12 | Allocates fuel oil and waste gas across generators to minimize oil consumption. |
