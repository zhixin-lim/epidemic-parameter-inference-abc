# Simulation-Based Inference for Epidemic Parameters on an Adaptive Network

**Module:** ST3247 — Simulation Methods | National University of Singapore  
**Topic:** Approximate Bayesian Computation (ABC) and Sequential Monte Carlo ABC (ABC-SMC) for inferring epidemic parameters on an adaptive contact network

---

## Overview

This project investigates whether more computationally efficient inference algorithms can overcome fundamental limitations in the information content of epidemic data. We model disease spread on an **adaptive network** — one where susceptible individuals rewire connections away from infected neighbours — and attempt to jointly infer two key parameters:

- **β** — infection rate along S–I edges
- **ρ** — rewiring rate (rate at which susceptible nodes sever connections to infected nodes)

We implement and compare two simulation-based inference approaches:
- **ABC (Approximate Bayesian Computation)** — rejection sampling with distance-based acceptance
- **SMC-ABC (Sequential Monte Carlo ABC)** — iterative particle refinement with adaptive tolerance schedules

---

## Key Finding

Despite SMC-ABC achieving tighter posterior tolerances and substantially better computational efficiency than vanilla ABC, **neither method resolves a persistent β–ρ correlation in the posterior**.

This is not a failure of the sampler — it is a structural **non-identifiability** in the epidemic mechanism itself:

> Both β and ρ act on the same transmission channel: S–I edges. β increases infection pressure along these edges; ρ removes them through rewiring. Epidemic dynamics are therefore governed by their **joint effect on effective infection pressure**, not by either parameter independently.

Because S–I edge dynamics are unobserved in typical epidemic data (we observe case counts, not network topology), different (β, ρ) combinations produce statistically indistinguishable trajectories — inducing a fundamental confounding that more efficient samplers cannot resolve.

---

## Implications

The results highlight a **key principle of simulation-based inference**:

> *Posterior quality is ultimately constrained by the identifiability of the data-generating process — not by the sophistication of the sampler.*

Progress requires **more informative data representations** — particularly summary statistics that capture the temporal evolution of network structure and the interaction between infection and rewiring dynamics (e.g., tracking S–I edge counts over time, not just aggregate case trajectories).

---

## 🚨 START HERE (FOR GRADING)

The **main file for this project is**:

👉 `Final Assignment.ipynb`

Please **run this notebook from top to bottom**.

It contains the complete and final workflow used in the report, including:

- data loading and preprocessing  
- summary statistics construction and selection  
- distance metric design  
- rejection ABC implementation  
- ABC-SMC implementation  
- final results and analysis  

All other notebooks in this repository are **exploratory or intermediate work** and are **not required for grading**.

---

## ⚙️ Environment Setup

This project uses standard Python scientific libraries.

### Requirements

- Python 3.10 or above  
- numpy  
- pandas  
- matplotlib  
- scipy  
- jupyter  

### Install dependencies

```bash
pip install numpy pandas matplotlib scipy jupyter
