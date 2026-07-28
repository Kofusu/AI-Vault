---
type: concept
status: not-started
domain: machine-learning
topic: learning-paradigms
level: foundations
order: 3
created: 2026-07-28
---

# Unsupervised Learning

## Tujuan

- Memahami belajar structure tanpa target label eksplisit.
- Mengenal clustering dan dimensionality reduction.
- Mengevaluasi hasil tanpa mengarang makna cluster.

## Intuisi

Model hanya melihat input:

```text
x₁, x₂, ..., xₙ
      ↓
Structure, groups, representation
```

## Konsep Dasar

Tidak ada target $y$ yang diberikan untuk task utama. Algorithm mencari regularity berdasarkan objective dan inductive bias tertentu.

## Kenapa Dibutuhkan?

- label mahal
- exploratory analysis
- anomaly discovery
- representation compression
- visualization
- preprocessing

## Cara Kerja

### Clustering

Mengelompokkan sample berdasarkan similarity.

K-means objective:

$$
\min_{\mu_1,\ldots,\mu_K}
\sum_i
\min_k
\|x_i-\mu_k\|_2^2
$$

### Dimensionality Reduction

Mencari representation berdimensi lebih rendah yang mempertahankan property tertentu.

PCA mempertahankan arah variance linear terbesar.

## Similarity dan Scaling

Distance bergantung scale:

```text
age: 0–100
income: 0–1,000,000
```

Tanpa scaling, feature besar dapat mendominasi Euclidean distance.

## Implementasi

```python
from sklearn.cluster import KMeans
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

pipeline = make_pipeline(
    StandardScaler(),
    KMeans(n_clusters=3, random_state=42),
)

clusters = pipeline.fit_predict(X)
```

## Evaluasi

Internal metric:

- silhouette score
- inertia

External metric membutuhkan reference label dan harus dipakai hati-hati.

Cluster usefulness juga perlu domain validation.

## Studi Kasus: Image Embedding

```text
Images
 ↓ pretrained encoder
Embedding vectors
 ↓ clustering
Candidate visual groups
 ↓ human inspection
Interpretation
```

Cluster ID tidak otomatis berarti semantic class.

## Anomaly Detection

Model belajar pattern normal atau density, lalu memberi anomaly score. Evaluation membutuhkan anomaly definition dan threshold.

## Best Practice

- Scale feature.
- Uji beberapa seed.
- Visualisasikan cluster.
- Audit stability.
- Jangan memberi nama semantic tanpa evidence.
- Bandingkan dengan simple baseline.

## Kesalahan Umum

- Menganggap cluster adalah ground-truth class.
- Memilih $K$ dari plot lalu mengklaim kepastian.
- Tidak scaling.
- Menilai dari visual 2D saja.
- Data leakage pada learned embedding.

## Debugging

- Cek feature variance dan scale.
- Cek cluster size.
- Jalankan beberapa initialization.
- Inspect centroid dan sample terdekat.
- Bandingkan random assignment.

## Ringkasan

- Unsupervised learning mencari structure tanpa target eksplisit.
- Hasil bergantung objective, feature, metric, dan scaling.
- Interpretation membutuhkan domain validation.

## Checklist Pemahaman

- [ ] Bisa membedakan supervised dan unsupervised.
- [ ] Bisa menjelaskan K-means objective.
- [ ] Bisa menjelaskan pentingnya scaling.
- [ ] Bisa mengevaluasi cluster secara terbatas.
- [ ] Tidak menyamakan cluster dan class.

## Latihan

1. Cluster dataset embedding.
2. Bandingkan sebelum/sesudah scaling.
3. Inspect cluster size.
4. Tulis limitation interpretasi.

