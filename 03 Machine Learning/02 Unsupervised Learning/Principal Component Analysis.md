---
type: concept
status: not-started
domain: machine-learning
topic: dimensionality-reduction
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Principal Component Analysis

## Tujuan

Memahami dimensionality reduction linear yang mempertahankan variance sebanyak mungkin.

## Prasyarat

- [[Matrix]]
- [[Eigenvalue and Eigenvector]]
- [[Expectation Variance and Covariance]]
- [[Vector Geometry]]

## Intuisi

PCA memutar sistem koordinat ke arah variasi data terbesar, lalu mempertahankan beberapa arah terpenting.

```text
Data berdimensi d
    ↓ center
Covariance / SVD
    ↓
Principal directions
    ↓ project
Representasi k dimensi
```

## Centering

$$
\mathbf{X}_c=
\mathbf{X}-\boldsymbol{\mu}
$$

Tanpa centering, arah utama dapat dipengaruhi offset terhadap origin.

## Covariance View

$$
\boldsymbol{\Sigma}
=
\frac{1}{n-1}\mathbf{X}_c^\top\mathbf{X}_c
$$

Eigenvector covariance matrix menjadi principal directions; eigenvalue menyatakan variance pada arah tersebut.

## Projection

Jika $\mathbf{W}_k$ berisi $k$ principal components:

$$
\mathbf{Z}
=
\mathbf{X}_c\mathbf{W}_k
$$

## SVD View

$$
\mathbf{X}_c=
\mathbf{U}\boldsymbol{\Sigma}\mathbf{V}^\top
$$

Implementasi numerik biasanya memakai SVD karena lebih stabil daripada membentuk covariance matrix dan eigendecomposition secara manual.

## Explained Variance Ratio

$$
r_j=
\frac{\lambda_j}{\sum_i\lambda_i}
$$

Cumulative ratio membantu memilih jumlah component, tetapi threshold seperti 95% bukan aturan universal.

## Implementasi

```python
from sklearn.decomposition import PCA
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

pipeline = Pipeline([
    ("scale", StandardScaler()),
    ("pca", PCA(n_components=0.95, svd_solver="full")),
])

X_train_reduced = pipeline.fit_transform(X_train)
X_test_reduced = pipeline.transform(X_test)
```

PCA harus di-fit hanya pada training set.

## Scaling atau Tidak?

- Gunakan StandardScaler jika fitur memiliki satuan/skala berbeda.
- Jangan scale otomatis jika variance absolut memang memiliki makna domain penting.

## Kelebihan

- mengurangi dimensi dan noise
- mempercepat model tertentu
- membantu visualisasi
- components orthogonal

## Kekurangan

- linear
- component sulit diinterpretasi
- sensitif terhadap scaling dan outlier
- variance tinggi belum tentu informasi paling relevan terhadap target

## Kompleksitas

Full SVD dapat mahal pada matrix besar. Randomized atau incremental PCA dapat dipakai untuk data berdimensi besar.

## Studi Kasus CV

- eigenfaces
- kompresi feature
- visualisasi embedding
- preprocessing classical vision

## Best Practice

- Fit PCA dalam pipeline setelah preprocessing.
- Catat explained variance.
- Evaluasi downstream metric, bukan variance saja.
- Jangan menggunakan proyeksi 2D sebagai bukti cluster definitif.

## Kesalahan Umum

- Fit PCA sebelum train/test split.
- Lupa centering.
- Menyamakan PCA dengan feature selection.
- Menganggap principal component sebagai fitur asli.

## Ringkasan

PCA mencari basis orthogonal yang mengurutkan arah berdasarkan variance dan memproyeksikan data ke beberapa arah utama.

## Hubungan Konsep

- [[Eigenvalue and Eigenvector]]
- [[Expectation Variance and Covariance]]
- [[K-Means Clustering]]
- [[Data Splitting and Leakage]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan centering
- [ ] Paham hubungan covariance, eigenvalue, dan PCA
- [ ] Bisa membaca explained variance ratio
- [ ] Paham leakage saat fit PCA

## Latihan

1. Implementasikan PCA 2D ke 1D dengan NumPy.
2. Bandingkan PCA sebelum dan sesudah scaling.
3. Evaluasi classifier dengan dan tanpa PCA.

