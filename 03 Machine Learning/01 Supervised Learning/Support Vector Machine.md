---
type: concept
status: not-started
domain: machine-learning
topic: supervised-learning
level: intermediate
created: 2026-07-29
updated: 2026-07-29
---

# Support Vector Machine

## Tujuan

Memahami maximum-margin classifier dan kernel trick.

## Intuisi

SVM mencari decision boundary yang memiliki margin terbesar terhadap sample terdekat. Sample penentu boundary disebut support vectors.

$$
f(\mathbf{x})=\mathbf{w}^\top\mathbf{x}+b
$$

## Hard Margin

Untuk data linearly separable:

$$
\min_{\mathbf{w},b}\frac{1}{2}\|\mathbf{w}\|_2^2
$$

dengan constraint:

$$
y_i(\mathbf{w}^\top\mathbf{x}_i+b)\ge1
$$

Margin berbanding terbalik dengan $\|\mathbf{w}\|$.

## Soft Margin

Slack variable mengizinkan violation:

$$
\min
\frac{1}{2}\|\mathbf{w}\|^2+
C\sum_i\xi_i
$$

- $C$ besar → penalti error kuat, margin cenderung sempit
- $C$ kecil → regularization kuat, margin lebih lebar

## Hinge Loss

$$
L=\max(0,1-yf(\mathbf{x}))
$$

Prediction benar tetapi terlalu dekat boundary masih dapat dikenai loss.

## Kernel Trick

Kernel menghitung inner product pada feature space tanpa membentuk mapping eksplisit:

$$
K(\mathbf{x},\mathbf{z})
=
\phi(\mathbf{x})^\top\phi(\mathbf{z})
$$

Kernel umum:

- linear
- polynomial
- RBF

## Implementasi

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC

model = Pipeline([
    ("scale", StandardScaler()),
    ("svm", SVC(
        kernel="rbf",
        C=1.0,
        gamma="scale",
        probability=False,
    )),
])

model.fit(X_train, y_train)
prediction = model.predict(X_test)
```

## Skala Data

Scaling penting karena margin dan RBF kernel menggunakan distance.

## Kompleksitas

Kernel SVM dapat membutuhkan memory $O(n^2)$ dan training antara $O(n^2)$ hingga $O(n^3)$ pada kasus buruk. Linear SVM lebih cocok untuk sample besar dan fitur sparse.

## Kelebihan

- efektif pada data kecil-menengah
- kuat pada dimensi tinggi
- kernel menangani boundary nonlinear

## Kekurangan

- kernel SVM sulit diskalakan
- tuning $C$ dan $\gamma$ sensitif
- probability bukan output native margin
- interpretasi sulit untuk kernel nonlinear

## Best Practice

- Scale fitur di dalam pipeline.
- Mulai dari linear kernel sebagai baseline.
- Tuning parameter dengan cross-validation.
- Hindari kernel SVM untuk dataset sangat besar tanpa alasan kuat.

## Kesalahan Umum

- Menganggap score margin sebagai probability.
- Tidak melakukan scaling.
- Grid search sangat besar tanpa baseline.
- Menggunakan test set untuk memilih kernel.

## Ringkasan

SVM memaksimalkan margin dan kernel trick memungkinkan decision boundary nonlinear, tetapi biaya kernel meningkat tajam terhadap jumlah sample.

## Hubungan Konsep

- [[Vector Geometry]]
- [[Convex Optimization]]
- [[Logistic Regression]]
- [[Cross-Validation and Model Selection]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan margin dan support vector
- [ ] Paham peran C
- [ ] Paham intuisi kernel trick
- [ ] Tahu keterbatasan skalabilitas

## Latihan

1. Bandingkan linear dan RBF SVM pada dataset `make_moons`.
2. Amati perubahan boundary saat C berubah.
3. Jelaskan kenapa scaling memengaruhi RBF kernel.

