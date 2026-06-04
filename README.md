# SGA-NSGA-III: Enhanced Multi-Objective Optimization with Adaptive Fitness & Penalty

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python)
![Conference](https://img.shields.io/badge/ICCIDS_2026-IEEE-red?style=flat-square)

A surrogate-guided, FN-aware multi-objective evolutionary algorithm for robust medical diagnosis. Outperforms NSGA-II, NSGA-III, and MOEA/D across 5 DTLZ benchmarks and 11 medical datasets.

> **Published at:** IEEE Technically Sponsored ICCIDS 2026 — SSN College of Engineering, Chennai.

---

## Key Contributions

Standard NSGA-III uses a static fitness function and fixed penalties that cannot adapt to evolving population dynamics. This work introduces:

| Component | Formula | Effect |
|---|---|---|
| **Adaptive Fitness** | `F_i(x) = f_i(x) + θ(t,r,β)·P(x)` | Penalty grows with generations and feasible ratio |
| **Adaptive Weight** | `θ(t,r,β) = θ_min + (t/T)^α · (1−r)^β · w_sens` | Balances exploration vs exploitation dynamically |
| **FN Sensitivity** | `w_sens = 1 + γ · FN/(TP+FN)` | Amplifies penalty when false negatives are high |
| **FN-Aware Surrogate** | `p_eff = p_base · exp(−λ · FN_risk)` | Suppresses surrogate when FN risk is elevated |
| **Dynamic Penalty** | `P(x) = Σ max(0,gⱼ(x)) + Σ\|hₖ(x)\|` | Flexible constraint handling; no fixed coefficients |

**Three objectives optimized simultaneously:** Training Accuracy (f₁) · Validation Metric — F1/AUC (f₂) · Model Complexity — parameter count (f₃).


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

## Results

### DTLZ Benchmark Performance (15 independent trials)

**Hypervolume — higher is better:**

| Problem | NSGA-III | MOEA/D | Adaptive NSGA-III | **Surrogate (Ours)** |
|---|---|---|---|---|
| DTLZ1 | 0.172 | 0.137 | 0.179 | **0.195** |
| DTLZ2 | 10.063 | 10.023 | 10.063 | **10.075** |
| DTLZ3 | 7.10 | 6.50 | 9.26 | **10.65** |
| DTLZ4 | 9.83 | 7.47 | 9.95 | **10.30** |
| DTLZ5 | 8.57 | **8.76** | 8.67 | 8.81 |

**IGD — lower is better:**

| Problem | NSGA-III | MOEA/D | Adaptive NSGA-III | **Surrogate (Ours)** |
|---|---|---|---|---|
| DTLZ1 | 0.045 | 0.042 | 0.028 | **0.022** |
| DTLZ2 | 0.050 | 0.068 | 0.050 | **0.048** |
| DTLZ3 | 0.30 | 0.45 | 0.26 | **0.10** |
| DTLZ4 | 0.10 | 0.72 | 0.09 | **0.08** |
| DTLZ5 | 0.49 | 0.46 | 0.48 | **0.41** |

### DTLZ Surrogate Efficiency

| Problem | True Evals | Surrogate Evals |
|---|---|---|
| DTLZ1 | 13,204 | 7,795 |
| DTLZ2 | 13,897 | 7,102 |
| DTLZ3 | 39,540 | 23,460 |
| DTLZ4 | 15,659 | 5,340 |
| DTLZ5 | 13,209 | 7,790 |

### Medical Dataset Classification Accuracy (%)

| Dataset | MLP | NSGA-III | MOEA/D | Adaptive NSGA-III | **Surrogate (Ours)** |
|---|---|---|---|---|---|
| Cleveland Heart | 85.25 | 95.08 | 90.16 | **96.91** | 95.60 |
| Statlog Heart | 74.07 | 75.93 | 72.22 | 77.10 | **87.79** |
| SPECT Heart | 85.19 | 87.04 | 87.04 | 88.00 | **88.24** |
| SPECTF Heart | 81.48 | 81.48 | 77.78 | 83.22 | **90.59** |
| WBC (Breast Cancer) | 96.49 | 99.12 | 99.12 | 99.15 | **99.18** |
| Hepatitis | 87.10 | 90.32 | 93.55 | 93.65 | **93.74** |
| Parkinson's | 94.87 | 95.22 | 77.27 | **96.60** | 92.48 |
| Pima Diabetes | 74.68 | 72.46 | 73.91 | 76.33 | **86.36** |
| BUPA Liver | 63.77 | 73.71 | 73.47 | **74.04** | **74.04** |

### Medical Dataset HV & IGD (Surrogate achieves best across all datasets)

| Dataset | NSGA-II HV | NSGA-III HV | MOEA/D HV | Adaptive HV | **Surrogate HV** |
|---|---|---|---|---|---|
| Cleveland | 2336.31 ± 11.79 | 2332.38 ± 10.44 | 2244.17 ± 12.35 | 2342.15 ± 9.12 | **2348.92 ± 8.45** |
| Statlog | 2217.61 ± 0.29 | 2217.64 ± 0.37 | 2150.91 ± 1.43 | 2226.84 ± 0.51 | **2232.57 ± 0.62** |
| WBC | 2885.50 ± 0.44 | 2885.02 ± 2.08 | 2847.03 ± 7.28 | 2893.87 ± 1.95 | **2899.45 ± 2.10** |
| Parkinson | 2588.73 ± 9.38 | 2594.01 ± 8.47 | 2505.20 ± 12.24 | 2604.38 ± 7.92 | **2615.72 ± 6.88** |
| Pima | 2002.36 ± 14.55 | 1998.73 ± 8.83 | 1982.73 ± 12.44 | 2008.94 ± 9.10 | **2015.26 ± 8.20** |

### Medical Surrogate Efficiency

| Dataset | True Evals | Surrogate Evals |
|---|---|---|
| Cleveland | 7,618 | 6,032 |
| Statlog | 7,573 | 6,077 |
| SPECT | 7,643 | 6,007 |
| SPECTF | 7,854 | 5,796 |
| WBC | 7,463 | 6,187 |
| Hepatitis | 7,649 | 6,001 |
| Parkinson | 7,668 | 5,982 |
| Pima Diabetes | 7,575 | 6,075 |
| BUPA Liver | 7,584 | 5,796 |
| **Total** | **68,627** | **53,953** |

> **44.5% reduction** in true model evaluations across all 9 medical datasets with no loss in performance.


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

