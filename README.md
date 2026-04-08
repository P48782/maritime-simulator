# When Shipping Networks Break
### An interactive global shipping disruption simulator

**Built by Priyanka Gupta · MS Industrial Engineering · UNC Charlotte**  
*Paper under review: Maritime Policy & Management*

---

## What this is

Around 80% of world trade moves by sea. When a major port or shipping corridor is disrupted — by a storm, a conflict, a cyberattack, or a strike — costs ripple across the entire global network.

This tool lets you explore exactly how much costs rise, which hubs are most vulnerable, and what the data says about resilience — across 18 real-world disruption scenarios.

**[Live demo → **

---

## Key findings

| Hub | 30% capacity loss | 50% capacity loss |
|---|---|---|
| Singapore | **+36.0% cost increase** | **+100.0% cost increase** |
| China | +6.4% cost increase | +17.7% cost increase |
| Netherlands | +0.8% cost increase | +2.2% cost increase |

- Singapore is the most critical node in the network — cost response is near-quadratic, meaning each additional percent of lost capacity hurts more than the last
- Flow retention is 100% across all scenarios — goods always find a route, the question is always **price, not access**
- Red Sea route cost shocks produce near-zero network cost change — the optimizer finds alternative routing at no extra cost, suggesting strong redundancy in long-haul corridors

---

## How it works

The model uses a **network simplex algorithm** (via NetworkX) to find the cost-minimizing routing for global cargo flows across a 20-country trade network.

### Phase 1 — Baseline optimization
- Loads bilateral shipping flow probabilities and UN transport cost data (2021–2023)
- Adjusts flows by port connectivity (UNCTAD PLSCI 2023) and median time in port
- Solves a minimum-cost flow problem to find the optimal routing
- Outputs: arc-level flows (TEU) and costs (USD)

### Phase 2 — Resilience stress testing
- Applies port capacity shocks (10%–50% reduction) at Singapore, China, and Netherlands
- Measures cost change % and flow retention % at each disruption level
- Detects nonlinear threshold effects using first-difference slope analysis
- Outputs: cost sensitivity curves per hub

---

## Data sources

| Dataset | Source |
|---|---|
| Transport costs 2021–2023 | UN Transport Cost Database |
| Port connectivity scores (PLSCI) | UNCTAD, 2023 |
| Shipping flow probabilities | Derived from bilateral trade matrices |
| Disruption scenarios | Literature review + expert assessment |

---

## Repository structure

```
maritime-simulator/
│
├── index.html                  # Interactive web simulator (the live tool)
├── baseline_optimization.py    # Phase 1: network optimization model
├── phase2_resilience.py        # Phase 2: stress testing & threshold detection
├── Database.xlsx               # Core dataset (countries, ports, flows, costs)
└── README.md
```

---

## Running the model locally

**Requirements:** Python 3.8+, pandas, numpy, networkx, matplotlib, openpyxl

```bash
pip install pandas numpy networkx matplotlib openpyxl

# Run Phase 1 — baseline optimization
python baseline_optimization.py

# Run Phase 2 — resilience stress testing
python phase2_resilience.py
```

**Phase 1 outputs** (saved to `outputs/`):
- `arc_flows.xlsx` — optimal flows per route (TEU and USD)
- `baseline_flow.pkl` — serialized flow solution

**Phase 2 outputs:**
- `Phase2_Resilience_Results.xlsx` — cost change % and flow retention % per scenario
- `Critical_Disruption_Thresholds.xlsx` — nonlinear threshold detection results
- `Figure 3.jpg`, `Figure 4.jpg`, `Figure 5.jpg` — publication-ready charts

---

## About the research

This project is part of a graduate research paper investigating how maritime shipping networks can be modeled and optimized under real-world disruption conditions.

The core question: **when a disruption hits, what is the minimum-cost re-routing — and at what point does the network fail to absorb the shock gracefully?**

The model reveals that network resilience is highly asymmetric. Some hubs (Singapore) are so central that partial disruption triggers disproportionate cost escalation. Others (Netherlands) are embedded in dense regional alternatives and show near-linear, manageable responses.

---

## Contact

**Priyanka Gupta**  
MS Industrial Engineering, UNC Charlotte  
[LinkedIn](https://www.linkedin.com/in/priyanka-gupta-10thfeb/) · pgupta19@charlotte.edu
