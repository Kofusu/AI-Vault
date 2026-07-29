---
type: concept
status: not-started
domain: machine-learning
topic: supervised-learning
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Linear Regression

## Tujuan

Memahami baseline regression yang memodelkan hubungan linear antara fitur dan target kontinu.

## Prasyarat

- [[Supervised Learning]]
- [[Vector]]
- [[Matrix Multiplication]]
- [[Gradient Descent]]

## Intuisi

Linear Regression mencari garis atau hyperplane yang paling dekat dengan data.

$$
\hat{y}=\mathbf{w}^\top\mathbf{x}+b
$$

- $\mathbf{x}$: feature vector
- $\mathbf{w}$: weight
- $b$: bias/intercept
- $\hat y$: prediction

## Masalah yang Diselesaikan

Target harus kontinu, misalnya harga, temperatur, latency, atau konsumsi energi. Model ini bukan pilihan tepat untuk label kategori.

## Objective Function

Mean Squared Error:

$$
J(\mathbf{w},b)=
\frac{1}{n}\sum_{i=1}^{n}
(y_i-\hat y_i)^2
$$

Squared error memberi penalti besar pada error besar dan bersifat differentiable.

## Solusi

### Normal Equation

Untuk desain matrix $\mathbf{X}$:

$$
\hat{\boldsymbol{\beta}}
=
(\mathbf{X}^\top\mathbf{X})^{-1}
\mathbf{X}^\top\mathbf{y}
$$

Dalam praktik gunakan pseudo-inverse atau solver numerik, bukan inverse eksplisit.

### Gradient Descent

Parameter diperbarui berulang:

$$
\mathbf{w}\leftarrow
\mathbf{w}-\eta\nabla_{\mathbf{w}}J
$$

## Regularization

Ridge:

$$
J_{\text{ridge}}=J+\lambda\|\mathbf{w}\|_2^2
$$

Lasso:

$$
J_{\text{lasso}}=J+\lambda\|\mathbf{w}\|_1
$$

Ridge mengecilkan weight; Lasso dapat membuat beberapa weight menjadi nol.

## Implementasi Scikit-Learn

```python
from sklearn.compose import TransformedTargetRegressor
from sklearn.impute import SimpleImputer
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_absolute_error, root_mean_squared_error
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

model = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scale", StandardScaler()),
    ("regressor", Ridge(alpha=1.0)),
])

model.fit(X_train, y_train)
prediction = model.predict(X_test)

print(root_mean_squared_error(y_test, prediction))
print(mean_absolute_error(y_test, prediction))
```

## Asumsi Penting

- hubungan cukup linear
- residual independen
- variance residual relatif konstan
- multicollinearity tidak ekstrem

Normalitas residual terutama penting untuk inference statistik, bukan syarat mutlak prediction.

## Evaluasi

- MAE mudah diinterpretasi dan lebih robust terhadap outlier
- RMSE memberi penalti lebih besar pada error besar
- $R^2$ membandingkan model dengan baseline mean

## Kompleksitas

Prediction memerlukan $O(nd)$ untuk $n$ sample dan $d$ fitur. Training bergantung solver; iterative solver cocok untuk data besar.

## Best Practice

- Mulai dengan model ini sebagai baseline.
- Plot residual terhadap prediction.
- Gunakan regularization saat fitur banyak atau berkorelasi.
- Fit preprocessing hanya pada training set.

## Kesalahan Umum

- Menggunakan $R^2$ saja.
- Menginterpretasikan correlation sebagai causation.
- Memakai inverse matrix eksplisit.
- Mengabaikan nonlinearity dan outlier.

## Ringkasan

Linear Regression adalah baseline cepat, interpretable, dan kuat ketika hubungan data mendekati linear.

## Hubungan Konsep

- [[Logistic Regression]]
- [[Feature Engineering and Preprocessing]]
- [[Overfitting and Underfitting]]
- [[Machine Learning Evaluation Metrics]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan weight dan bias
- [ ] Paham MSE dan regularization
- [ ] Bisa memilih MAE atau RMSE
- [ ] Bisa membaca residual plot

## Latihan

1. Implementasikan prediction linear dengan NumPy.
2. Bandingkan LinearRegression, Ridge, dan Lasso.
3. Buat data nonlinear dan analisis kegagalan model.

