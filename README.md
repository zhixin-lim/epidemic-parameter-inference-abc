# Epidemic Parameter Inference on Adaptive Networks

**Module:** ST3247 — Simulation Methods | National University of Singapore  
**Topic:** Approximate Bayesian Computation (ABC) and Sequential Monte Carlo ABC (ABC-SMC) for inferring epidemic parameters on an adaptive contact network

---

## Overview

This project investigates whether more computationally efficient inference algorithms can overcome fundamental limitations in the information content of epidemic data. We model disease spread on an **adaptive network** — one where susceptible individuals rewire connections away from infected neighbours — and attempt to jointly infer two key parameters:

- **β** — infection rate along S–I edges  
- **ρ** — rewiring rate (rate at which susceptible nodes sever connections to infected nodes)

We implement and compare two simulation-based inference approaches:

1. **Rejection ABC** — baseline: simulate, compute distance to observed data, accept/reject
2. **ABC-SMC** — Sequential Monte Carlo with adaptive tolerance schedule; iteratively refines particle populations toward the posterior

---

## Repository Structure

```
epidemic-parameter-inference-abc/
├── notebooks/
│   ├── 01_rejection_abc.ipynb       # Rejection ABC implementation
│   └── 02_abc_smc.ipynb             # ABC-SMC implementation
├── results/
│   ├── abc_A_infection_only.csv     # ABC results: β only (ρ fixed)
│   ├── abc_B_infection_rewiring.csv # ABC results: β and ρ joint inference
│   ├── abc_C_all_seven.csv          # ABC results: full 7-summary-statistic model
│   ├── abc_results_full.csv         # Full ABC posterior samples
│   ├── abc_results_confirm.csv      # Confirmation run (robustness check)
│   ├── abc_results_mahal.csv        # Results using Mahalanobis distance metric
│   └── pilot_results.csv            # Pilot runs for tolerance calibration
├── data/                            # Observed epidemic trajectory data
├── Final Assignment.ipynb           # Combined final analysis and conclusions ← start here
├── simulator.py                     # Adaptive network epidemic simulator
└── README.md
```

> **Start here:** `Final Assignment.ipynb` contains the complete analysis — posterior comparisons, β–ρ correlation diagnosis, and conclusions. The `notebooks/` folder contains the individual working notebooks split by method during the collaborative phase of the project.

---

## Experimental Design

The results CSVs reflect a deliberate progression of complexity:

| File | What was varied | Purpose |
|---|---|---|
| `abc_A_infection_only` | β only, ρ fixed | Baseline: can we recover β in isolation? |
| `abc_B_infection_rewiring` | β and ρ jointly | Core experiment: joint inference on the adaptive network |
| `abc_C_all_seven` | Full 7-statistic summary | Does richer summary information resolve the correlation? |
| `abc_results_mahal` | Mahalanobis distance | Does a better distance metric help? |
| `abc_results_confirm` | Repeat of best run | Robustness and reproducibility check |

---

## Key Finding

Despite ABC-SMC achieving tighter posterior tolerances and substantially better computational efficiency than vanilla ABC, **neither method resolves a persistent β–ρ correlation in the posterior**.

This is not a failure of the sampler — it is a structural **non-identifiability** in the epidemic mechanism itself:

> Both β and ρ act on the same transmission channel: S–I edges. β increases infection pressure along these edges; ρ removes them through rewiring. Epidemic dynamics are therefore governed by their **joint effect on effective infection pressure**, not by either parameter independently.

Because S–I edge dynamics are unobserved in typical epidemic data (we observe case counts, not network topology), different (β, ρ) combinations produce statistically indistinguishable trajectories — inducing a fundamental confounding that more efficient samplers cannot resolve.

---

## Conclusion

> *Posterior quality is ultimately constrained by the identifiability of the data-generating process — not by the sophistication of the sampler.*

Progress requires **more informative data representations** — particularly summary statistics that capture the temporal evolution of network structure and the interaction between infection and rewiring dynamics (e.g. tracking S–I edge counts over time, not just aggregate case trajectories).

---

## Methods Summary

| Component | Detail |
|---|---|
| Network model | Adaptive SIS on Erdős–Rényi graph |
| Inference methods | Rejection ABC · ABC-SMC (adaptive ε) |
| Summary statistics | Final epidemic size, peak prevalence, time-to-peak + 4 additional |
| Distance metrics | Weighted Euclidean · Mahalanobis |
| Language | Python (NumPy, Matplotlib) |
