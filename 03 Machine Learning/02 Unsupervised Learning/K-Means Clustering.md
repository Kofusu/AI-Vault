---
type: concept
status: not-started
domain: machine-learning
topic: unsupervised-learning
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# K-Means Clustering

## Tujuan

Memahami algoritma clustering berbasis centroid, objective function, asumsi, dan cara mengevaluasinya tanpa label.

## Prasyarat

- [[Unsupervised Learning]]
- [[Vector Geometry]]
- [[Expectation Variance and Covariance]]

## Intuisi

K-Means membagi data menjadi $K$ kelompok sehingga setiap point dekat dengan centroid kelompoknya.

## Objective Function

$$
J=
\sum_{i=1}^{n}
\|\mathbf{x}_i-\boldsymbol{\mu}_{c_i}\|_2^2
$$

- $c_i$: cluster assignment sample $i$
- $\boldsymbol{\mu}_{c_i}$: centroid cluster

## Cara Kerja

1. Pilih $K$ centroid awal.
2. Assignment: tetapkan setiap sample ke centroid terdekat.
3. Update: hitung mean sample pada setiap cluster.
4. Ulangi sampai assignment/centroid stabil atau batas iteration tercapai.

Setiap iteration tidak menaikkan objective, tetapi hasilnya bisa local optimum.

## Initialization

`k-means++` menyebarkan centroid awal agar convergence dan kualitas lebih stabil. Jalankan beberapa initialization karena hasil tetap bergantung seed.

## Implementasi

```python
from sklearn.cluster import KMeans
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

model = Pipeline([
    ("scale", StandardScaler()),
    ("cluster", KMeans(
        n_clusters=4,
        init="k-means++",
        n_init=20,
        random_state=42,
    )),
])

labels = model.fit_predict(X)
```

## Memilih K

- elbow method pada inertia
- silhouette score
- stability antar-run
- interpretasi dan kebutuhan domain

Silhouette bukan sumber kebenaran tunggal. Cluster terbaik secara metric belum tentu berguna.

## Asumsi Implisit

K-Means cocok untuk cluster yang:

- relatif spherical
- memiliki skala/variance serupa
- terpisah menurut Euclidean distance

Ia buruk untuk bentuk non-convex, density berbeda, outlier berat, atau fitur kategorikal.

## Kompleksitas

Kira-kira:

$$
O(nKdi)
$$

dengan $n$ sample, $K$ cluster, $d$ dimensi, dan $i$ iteration.

## Studi Kasus

- segmentasi warna sederhana
- customer grouping
- prototype/centroid embedding
- vector quantization

## Best Practice

- Scale fitur.
- Coba beberapa seed/initialization.
- Visualisasikan dengan PCA/UMAP tanpa menganggap proyeksi 2D sepenuhnya mewakili ruang asli.
- Audit apakah cluster stabil dan bermakna.

## Kesalahan Umum

- Menganggap cluster sebagai ground-truth class.
- Memilih K hanya dari elbow yang ambigu.
- Tidak menangani outlier.
- Memberi nomor cluster makna ordinal.

## Ringkasan

K-Means mengoptimalkan jarak kuadrat ke centroid. Algoritmanya cepat dan sederhana, tetapi memiliki asumsi geometris kuat.

## Hubungan Konsep

- [[Principal Component Analysis]]
- [[Vector Geometry]]
- [[Feature Engineering and Preprocessing]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan assignment dan update
- [ ] Paham objective K-Means
- [ ] Tahu asumsi bentuk cluster
- [ ] Bisa mengevaluasi stability

## Latihan

1. Implementasikan satu iteration K-Means manual.
2. Bandingkan hasil beberapa seed.
3. Uji K-Means pada data `make_moons` dan jelaskan kegagalannya.

