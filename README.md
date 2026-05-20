# Inventory Simulation Optimization

## 1. Project Overview

This project implements a **simulation optimization model for inventory management**. The model is inspired by Chapter 13, *Simulation Optimization*, from Nicolas Vandeput's book *Inventory Optimization: Models and Simulations*.

The main objective is to find the **optimal safety stock level** for an `(R, S)` inventory policy under:

- stochastic demand;
- stochastic lead time;
- order cost;
- holding cost;
- backlog cost.

Instead of relying only on closed-form formulas, the project simulates different safety stock levels and selects the one that minimizes the average total cost.

---

## 2. Business Problem

A company needs to decide how much **safety stock** it should keep. If safety stock is too low, the company faces stockouts and backlog costs. If safety stock is too high, the company faces unnecessary inventory holding costs.

The business question is:

> What safety stock level minimizes total inventory cost while maintaining an acceptable service level?

---

## 3. Inventory Policy

The project uses an `(R, S)` policy:

- `R`: review period. The inventory position is reviewed every `R` periods.
- `S`: order-up-to level. At each review period, an order is placed to raise net inventory back to `S`.
- `Ss`: safety stock. This is the decision variable optimized in the project.

The order-up-to level is computed as:

```text
S = expected demand during risk period + safety stock
S = demand_mean × (R + lead_time_mean) + Ss
```

---

## 4. Model Inputs

### Demand distribution

Demand is modeled as a discrete PMF created from a normal distribution:

```text
mean demand = 1,378 units per period
standard deviation = 285 units per period
```

### Lead-time distribution

Lead time follows a discrete probability distribution:

| Lead time | Probability |
|---:|---:|
| 3 periods | 10% |
| 4 periods | 70% |
| 5 periods | 20% |

### Cost parameters

| Cost type | Value | Meaning |
|---|---:|---|
| Order cost | 1,000 | Fixed cost per replenishment order |
| Holding cost | 1.25 | Cost per unit held per period |
| Backlog cost | 25 | Cost per unit backlogged per period |

---

## 5. Optimization Logic

The model follows a simulation optimization process:

1. Create a demand PMF.
2. Create a lead-time PMF.
3. Estimate a smart-start safety stock using a service-level heuristic.
4. Simulate multiple safety stock levels.
5. Compute average total cost for each safety stock level.
6. Select the safety stock level with the lowest average cost.

Total cost is calculated as:

```text
Total cost = holding cost + backlog cost + order cost
```

---

## 6. Service Metrics

The project reports two service-level metrics:

| Metric | Meaning |
|---|---|
| Cycle Service Level | Probability that a replenishment cycle does not experience stockout |
| Fill Rate | Percentage of total demand fulfilled directly from available inventory |

---

## 7. Repository Structure

```text
inventory-simulation-optimization/
├── README.md
├── requirements.txt
├── .gitignore
├── src/
│   ├── simulation.py
│   └── run_simulation.py
├── notebooks/
│   └── inventory_simulation_optimization.ipynb
├── outputs/
│   ├── simulation_results.csv
│   └── best_result.csv
├── figures/
│   └── cost_vs_safety_stock.png
└── data/
    └── .gitkeep
```

---

## 8. How to Run

### Step 1: Clone the repository

```bash
git clone https://github.com/your-username/inventory-simulation-optimization.git
cd inventory-simulation-optimization
```

### Step 2: Install dependencies

```bash
pip install -r requirements.txt
```

### Step 3: Run the simulation

```bash
python src/run_simulation.py
```

The results will be saved to:

```text
outputs/simulation_results.csv
outputs/best_result.csv
figures/cost_vs_safety_stock.png
```

---

## 9. Key Results

The model identifies the safety stock level that minimizes the simulated average cost. The chart below illustrates the relationship between safety stock and average cost.

```text
figures/cost_vs_safety_stock.png
```

Typical interpretation:

- Low safety stock creates high backlog cost.
- High safety stock creates high holding cost.
- The optimal point is where the total average cost is minimized.

---

## 10. Key Learning Points

This project demonstrates:

- how to build a discrete demand PMF;
- how to model stochastic lead time;
- how to simulate an `(R, S)` inventory policy;
- how to calculate cycle service level and fill rate;
- how to optimize safety stock using simulation instead of only relying on formulas.

---

## 11. Future Improvements

Possible extensions:

- compare normal, gamma, and kernel-based PMF demand distributions;
- add lost-sales simulation instead of backlog-only simulation;
- test multiple review periods `R`;
- run Monte Carlo replications for stability analysis;
- build a Streamlit dashboard for interactive inventory optimization.

---

## 12. Reference

Vandeput, N. (2020). *Inventory Optimization: Models and Simulations*. De Gruyter.
