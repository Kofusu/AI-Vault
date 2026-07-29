---
type: project
status: idea
domain: machine-learning
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# PRJ - End-to-End ML Baseline

## Problem Statement

Model sering dibandingkan tanpa split, preprocessing, metric, dan baseline yang fair.

## Tujuan

Membangun classification pipeline reproducible dari validasi data sampai error analysis.

## Dataset

Gunakan dataset tabular kecil dari Scikit-Learn atau dataset publik berlisensi jelas. Catat version dan source.

## Model

Minimal:

- DummyClassifier
- [[Logistic Regression]]
- [[k-Nearest Neighbors]]
- [[Decision Tree]]
- [[Random Forest and Gradient Boosting]]

Opsional:

- [[Support Vector Machine]]
- [[Principal Component Analysis]]

## Pipeline

```text
problem definition
  ↓ data audit
split once
  ↓ preprocessing pipeline
dummy baseline
  ↓ model comparison with CV
model selection
  ↓ final test once
error analysis + model card
```

## Struktur Project

```text
ml-baseline/
├── README.md
├── pyproject.toml
├── configs/
├── src/ml_baseline/
│   ├── data.py
│   ├── train.py
│   ├── evaluate.py
│   └── report.py
├── tests/
└── reports/
```

## Milestone

- [ ] M1 — problem, metric, dan dataset card
- [ ] M2 — frozen split serta leakage audit
- [ ] M3 — dummy dan linear baseline
- [ ] M4 — model comparison dengan CV
- [ ] M5 — final test dan uncertainty
- [ ] M6 — error analysis serta reproducibility report

## Acceptance Criteria

- Split menggunakan strategi yang sesuai unit data.
- Preprocessing berada dalam Scikit-Learn Pipeline.
- Selection tidak melihat test set.
- Semua model dibandingkan pada fold sama.
- Mean dan variability CV dilaporkan.
- Test digunakan sekali setelah keputusan final.
- Config, seed, dependency, dan commit dicatat.
- Minimal satu regression test untuk bug pipeline.

## Error Analysis

Catat:

- false positive/false negative
- subgroup
- confidence
- kemungkinan label issue
- kemungkinan feature/data issue

## Deliverable

- repository installable
- config model
- evaluation report
- confusion matrix dan metric table
- limitation
- next experiment berdasarkan evidence

## Materi Terkait

- [[End-to-End Machine Learning Baseline]]
- [[Data Splitting and Leakage]]
- [[Feature Engineering and Preprocessing]]
- [[Cross-Validation and Model Selection]]
- [[Machine Learning Evaluation Metrics]]
- [[Software Engineering for AI MOC]]

## Next Action

- [ ] Pilih task, tetapkan primary metric, dan buat dataset note.

