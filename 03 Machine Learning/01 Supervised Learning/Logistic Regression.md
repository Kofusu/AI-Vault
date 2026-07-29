---
type: concept
status: not-started
domain: machine-learning
topic: supervised-learning
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Logistic Regression

## Tujuan

Memahami linear classifier probabilistik untuk binary dan multi-class classification.

## Intuisi

Model menghitung linear score lalu mengubahnya menjadi probabilitas.

$$
z=\mathbf{w}^\top\mathbf{x}+b
$$

$$
\sigma(z)=\frac{1}{1+e^{-z}}
$$

Untuk binary classification:

$$
P(y=1\mid\mathbf{x})=\sigma(z)
$$

## Decision Boundary

Dengan threshold $0.5$, boundary terjadi saat:

$$
\mathbf{w}^\top\mathbf{x}+b=0
$$

Boundary linear, meskipun output memakai fungsi nonlinear sigmoid.

## Loss Function

Binary cross-entropy:

$$
L=
-\frac{1}{n}\sum_i
\left[
y_i\log p_i+
(1-y_i)\log(1-p_i)
\right]
$$

Model dilatih dengan maximum likelihood atau ekuivalennya meminimalkan negative log-likelihood.

## Multi-Class

Multinomial Logistic Regression menggunakan softmax:

$$
P(y=k\mid\mathbf{x})
=
\frac{e^{z_k}}{\sum_j e^{z_j}}
$$

## Implementasi

```python
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import classification_report, roc_auc_score

model = Pipeline([
    ("scale", StandardScaler()),
    ("classifier", LogisticRegression(
        C=1.0,
        class_weight="balanced",
        max_iter=1000,
    )),
])

model.fit(X_train, y_train)
prediction = model.predict(X_test)
probability = model.predict_proba(X_test)[:, 1]

print(classification_report(y_test, prediction))
print(roc_auc_score(y_test, probability))
```

`C` adalah inverse regularization strength: nilai kecil berarti regularization lebih kuat.

## Threshold

Threshold bukan selalu $0.5$. Pilih berdasarkan biaya false positive dan false negative menggunakan validation set, bukan test set.

## Calibration

Probabilitas 0.8 idealnya benar sekitar 80% pada kelompok prediction serupa. Periksa calibration curve jika probabilitas dipakai untuk keputusan.

## Kelebihan

- training dan inference cepat
- interpretable
- probabilistic output
- baseline kuat untuk data berdimensi tinggi

## Kekurangan

- boundary linear tanpa feature transformation
- sensitif terhadap outlier dan skala
- coefficient bisa tidak stabil saat multicollinearity tinggi

## Kompleksitas

Prediction $O(nd)$; training iterative bergantung jumlah sample, fitur, class, dan solver.

## Best Practice

- Scale fitur numerik.
- Gunakan stratified split untuk classification.
- Evaluasi metric yang sesuai imbalance.
- Tuning `C` hanya pada validation/cross-validation.

## Kesalahan Umum

- Mengira ini regression karena namanya.
- Menilai data imbalanced hanya dengan accuracy.
- Tuning threshold pada test set.
- Menyamakan score dengan probabilitas terkalibrasi.

## Ringkasan

Logistic Regression adalah linear classifier yang memodelkan class probability menggunakan sigmoid atau softmax.

## Hubungan Konsep

- [[Linear Regression]]
- [[Machine Learning Evaluation Metrics]]
- [[Cross-Validation and Model Selection]]
- [[Conditional Probability and Independence]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan sigmoid dan decision boundary
- [ ] Paham cross-entropy
- [ ] Tahu fungsi threshold
- [ ] Bisa mengevaluasi data imbalanced

## Latihan

1. Hitung sigmoid untuk score $-2,0,2$.
2. Bandingkan threshold 0.3 dan 0.7.
3. Visualisasikan decision boundary pada data dua fitur.

