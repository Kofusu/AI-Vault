---
type: concept
status: not-started
domain: mathematics
topic: probability-statistics
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Expectation Variance and Covariance

## Tujuan

Memahami pusat distribution, penyebaran, dan hubungan linear antarf fitur sebagai dasar loss, normalization, PCA, dan analisis data.

## Prasyarat

- [[Random Variables and Probability Distributions]]
- [[Statistics]]

## Expectation

Expectation adalah rata-rata teoretis jangka panjang.

Untuk discrete variable:

$$
\mathbb{E}[X]=\sum_x x\,p_X(x)
$$

Untuk continuous variable:

$$
\mathbb{E}[X]=\int_{-\infty}^{\infty}x f_X(x)\,dx
$$

Lebih umum:

$$
\mathbb{E}[g(X)]
=
\sum_xg(x)p_X(x)
$$

atau integral untuk continuous variable.

### Contoh Manual

Untuk dadu fair:

$$
\mathbb{E}[X]
=
\frac{1+2+3+4+5+6}{6}
=3.5
$$

Expectation tidak harus menjadi outcome yang mungkin.

## Variance

Variance mengukur penyebaran terhadap mean:

$$
\operatorname{Var}(X)
=
\mathbb{E}[(X-\mu)^2]
$$

Bentuk ekuivalen:

$$
\operatorname{Var}(X)
=
\mathbb{E}[X^2]-\mathbb{E}[X]^2
$$

Standard deviation:

$$
\sigma=\sqrt{\operatorname{Var}(X)}
$$

Standard deviation memiliki satuan yang sama dengan data.

## Covariance

Covariance mengukur bagaimana dua variable berubah bersama:

$$
\operatorname{Cov}(X,Y)
=
\mathbb{E}[(X-\mu_X)(Y-\mu_Y)]
$$

- positif: cenderung naik bersama
- negatif: satu naik saat yang lain turun
- sekitar nol: tidak ada hubungan linear yang kuat

Covariance nol tidak selalu berarti independence.

## Correlation

Correlation menormalisasi covariance:

$$
\rho_{X,Y}
=
\frac{\operatorname{Cov}(X,Y)}
{\sigma_X\sigma_Y}
$$

Nilainya berada pada $[-1,1]$ jika standard deviation tidak nol.

## Covariance Matrix

Untuk vector acak $\mathbf{x}$:

$$
\boldsymbol{\Sigma}
=
\mathbb{E}
\left[
(\mathbf{x}-\boldsymbol{\mu})
(\mathbf{x}-\boldsymbol{\mu})^\top
\right]
$$

Diagonal berisi variance, elemen di luar diagonal berisi covariance.

## Population vs Sample

Sample variance yang umum:

$$
s^2=
\frac{1}{n-1}
\sum_{i=1}^n(x_i-\bar{x})^2
$$

Pembagi $n-1$ adalah Bessel correction untuk estimator variance yang unbiased.

## Implementasi NumPy

```python
import numpy as np

x = np.array([1.0, 2.0, 3.0, 4.0])
y = np.array([2.0, 4.0, 5.0, 8.0])

mean = np.mean(x)
population_variance = np.var(x, ddof=0)
sample_variance = np.var(x, ddof=1)
covariance_matrix = np.cov(x, y, ddof=1)
correlation_matrix = np.corrcoef(x, y)
```

## Hubungan dengan Loss

Mean Squared Error adalah expectation empiris dari squared error:

$$
\operatorname{MSE}
=
\frac{1}{n}\sum_{i=1}^{n}
(y_i-\hat{y}_i)^2
$$

## Hubungan dengan Standardization

$$
z=\frac{x-\mu}{\sigma}
$$

Hasilnya berpusat sekitar nol dan memiliki standard deviation sekitar satu pada data yang digunakan untuk menghitung statistik.

> [!warning]
> Mean dan standard deviation preprocessing hanya boleh dihitung dari training set untuk menghindari data leakage.

## Hubungan dengan PCA

PCA mencari arah dengan variance terbesar melalui eigenvector covariance matrix.

## Best Practice

- Pastikan pilihan `ddof` sesuai population atau sample.
- Gunakan correlation saat skala variable berbeda.
- Visualisasikan scatter plot; correlation hanya menangkap hubungan linear.
- Fit statistik preprocessing pada training data saja.

## Kesalahan Umum

- Menyamakan covariance dengan causation.
- Menyimpulkan independence dari covariance nol.
- Membandingkan covariance antarfitur dengan satuan berbeda.
- Mengabaikan outlier yang dapat mengubah mean dan variance drastis.

## Ringkasan

Expectation mengukur pusat teoretis, variance mengukur penyebaran, covariance mengukur perubahan bersama, dan correlation membuat hubungan tersebut bebas skala.

## Hubungan Konsep

- [[Feature Engineering and Preprocessing]]
- [[Principal Component Analysis]]
- [[Machine Learning Evaluation Metrics]]
- [[Random Variables and Probability Distributions]]

## Checklist Pemahaman

- [ ] Bisa menghitung expectation discrete sederhana
- [ ] Bisa membedakan variance dan standard deviation
- [ ] Bisa membaca covariance matrix
- [ ] Paham covariance nol tidak menjamin independence
- [ ] Paham risiko leakage pada standardization

## Latihan

1. Hitung mean, population variance, dan sample variance data `[1, 2, 3]`.
2. Buat dua variable dengan covariance nol tetapi tidak independent.
3. Jelaskan peran covariance matrix dalam PCA.

