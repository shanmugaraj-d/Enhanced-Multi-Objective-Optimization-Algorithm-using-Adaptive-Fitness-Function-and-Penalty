# SGA-NSGA-III: Enhanced Multi-Objective Optimization with Adaptive Fitness & Penalty

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)
![Conference](https://img.shields.io/badge/ICCIDS_2026-IEEE-red?style=flat-square)

A surrogate-guided, FN-aware multi-objective evolutionary algorithm for robust medical diagnosis. Outperforms NSGA-II, NSGA-III, and MOEA/D across 5 DTLZ benchmarks and 11 medical datasets.

> **Published at:** IEEE Technically Sponsored ICCIDS 2026 — SSN College of Engineering, Chennai.

---

## Key Idea

Standard NSGA-III uses a static fitness function and fixed penalties that can't adapt to population dynamics. This project introduces:

- **Adaptive Fitness:** `F_i(x) = f_i(x) + θ(t, r, β) · P(x)` — penalty weight evolves with generation progress, feasible ratio, and false-negative sensitivity
- **FN-Aware Surrogate:** `p_eff = p_base · exp(−λ · FN_risk)` — surrogate usage is suppressed when false-negative risk is high; true model always has final authority
- **Dynamic Penalty:** `P(x) = Σ max(0, gⱼ(x)) + Σ |hₖ(x)|` — flexible constraint handling

**Result: 44.5% reduction in true model evaluations with no loss in accuracy.**

---

## Setup

```bash
git clone https://github.com/shanmugaraj-d/SGA-NSGA-III.git
cd SGA-NSGA-III
pip install -r requirements.txt
jupyter notebook SGA-NSGA-III.ipynb
```

---

## Datasets

Download and place in `datasets/` folder.

| # | Dataset | Size | Link |
|---|---|---|---|
| 1 | Cleveland Heart Disease | 303 × 13 | [UCI](https://archive.ics.uci.edu/dataset/45/heart+disease) |
| 2 | Statlog Heart | 270 × 13 | [UCI](https://archive.ics.uci.edu/dataset/145/statlog+heart) |
| 3 | SPECT Heart | 267 × 22 | [UCI](https://archive.ics.uci.edu/dataset/95/spect+heart) |
| 4 | SPECTF Heart | 267 × 44 | [UCI](https://archive.ics.uci.edu/dataset/96/spectf+heart) |
| 5 | EEG Eye State | 14,980 × 14 | [UCI](https://archive.ics.uci.edu/dataset/264/eeg+eye+state) |
| 6 | Breast Cancer (WDBC) | 569 × 30 | [UCI](https://archive.ics.uci.edu/dataset/17/breast+cancer+wisconsin+diagnostic) |
| 7 | Hepatitis | 155 × 18 | [UCI](https://archive.ics.uci.edu/dataset/46/hepatitis) |
| 8 | Parkinson's Disease | 197 × 22 | [UCI](https://archive.ics.uci.edu/dataset/174/parkinsons) |
| 9 | Pima Indian Diabetes | 768 × 8 | [Kaggle](https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database) |
| 10 | BUPA Liver Disorders | 345 × 6 | [UCI](https://archive.ics.uci.edu/dataset/60/liver+disorders) |
| 11 | Cardiovascular Disease | 70,000 × 12 | [Kaggle](https://www.kaggle.com/datasets/sulianova/cardiovascular-disease-dataset) |

DTLZ1–5 benchmarks are generated automatically via `pymoo`.

---

## Results Summary

**DTLZ Benchmarks (HV higher is better / IGD lower is better):**

| Problem | NSGA-III | MOEA/D | Adaptive | **Surrogate (Ours)** |
|---|---|---|---|---|
| DTLZ1 HV | 0.172 | 0.137 | 0.179 | **0.195** |
| DTLZ3 HV | 7.10 | 6.50 | 9.26 | **10.65** |
| DTLZ4 IGD | 0.10 | 0.72 | 0.09 | **0.08** |

**Medical Classification Accuracy (%):**

| Dataset | NSGA-III | Adaptive | **Surrogate (Ours)** |
|---|---|---|---|
| Statlog | 75.93 | 77.10 | **87.79** |
| SPECTF | 81.48 | 83.22 | **90.59** |
| Pima | 72.46 | 76.33 | **86.36** |

---

## Project Structure

```
SGA-NSGA-III/
├── SGA-NSGA-III.ipynb      # Full pipeline notebook
├── requirements.txt
├── .gitignore
├── README.md
├── datasets/               # Download datasets here (see table above)
│   └── .gitkeep
└── results/                # Auto-generated on run
    └── .gitkeep
```

---

## Hardware

| | Minimum | Recommended |
|---|---|---|
| CPU | Core i5 | Core i7 |
| RAM | 8 GB | 16 GB |
| GPU | — | NVIDIA CUDA |

---

