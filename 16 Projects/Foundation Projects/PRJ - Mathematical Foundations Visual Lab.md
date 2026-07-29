---
type: project
status: idea
domain:
  - mathematics
  - scientific-computing
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# PRJ - Mathematical Foundations Visual Lab

## Problem Statement

Rumus matematika mudah dihafal tanpa memahami perubahan geometris dan numeriknya. Project ini membuat visual lab interaktif berbasis NumPy dan Matplotlib.

## Tujuan

- Memvisualisasikan vector, matrix transformation, distribution, derivative, dan gradient descent.
- Menghubungkan perhitungan manual dengan implementasi.
- Membuat hasil yang reproducible.

## Scope

### Wajib

1. Vector addition, norm, dot product, cosine, projection
2. Matrix transformation pada grid 2D
3. Histogram sample Bernoulli dan Normal
4. Mean, variance, covariance visualization
5. Derivative numerical vs analytical
6. Gradient descent pada fungsi convex 2D

### Opsional

- eigenvector visualization
- Hessian curvature
- interactive slider

## Tech Stack

- Python
- NumPy
- Matplotlib
- pytest

## Struktur Project

```text
math-visual-lab/
├── README.md
├── pyproject.toml
├── src/math_visual_lab/
│   ├── vectors.py
│   ├── transforms.py
│   ├── probability.py
│   ├── calculus.py
│   └── optimization.py
├── tests/
└── reports/figures/
```

## Milestone

- [ ] M1 — operasi vector dan unit test
- [ ] M2 — matrix transformation
- [ ] M3 — probability/statistics visualization
- [ ] M4 — numerical derivative
- [ ] M5 — gradient-descent animation
- [ ] M6 — README dan interpretasi

## Acceptance Criteria

- Semua rumus penting memiliki contoh manual.
- Numerical derivative mendekati analytical derivative dalam tolerance.
- Gradient descent menampilkan trajectory dan loss per iteration.
- Seed sampling dicatat.
- Figure memiliki title, label, legend, dan satuan yang relevan.
- Test lulus pada environment bersih.

## Eksperimen

- Ubah learning rate dan amati divergence.
- Ubah covariance lalu lihat orientasi point cloud.
- Bandingkan L1 dan L2 geometry.
- Ubah matrix transformation dan interpretasikan determinant.

## Deliverable

- repository
- command untuk menghasilkan seluruh figure
- laporan singkat insight
- gallery minimal enam visualisasi

## Materi Terkait

- [[Vector Geometry]]
- [[Matrix]]
- [[Eigenvalue and Eigenvector]]
- [[Random Variables and Probability Distributions]]
- [[Expectation Variance and Covariance]]
- [[Derivative]]
- [[Gradient Descent]]

## Next Action

- [ ] Buat repository dan implementasikan modul `vectors.py`.

