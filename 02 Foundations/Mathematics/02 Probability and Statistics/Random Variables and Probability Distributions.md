---
type: concept
status: not-started
domain: mathematics
topic: probability-statistics
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Random Variables and Probability Distributions

## Tujuan

Memahami random variable, PMF, PDF, CDF, serta distribusi penting yang muncul dalam data, loss function, dan probabilistic modeling.

## Prasyarat

- [[Probability]]
- [[Statistics]]

## Intuisi

Eksperimen acak menghasilkan outcome. Random variable mengubah outcome tersebut menjadi angka, sedangkan probability distribution menjelaskan seberapa mungkin setiap nilai terjadi.

```text
Eksperimen acak → Outcome → Random variable → Nilai numerik
```

## Random Variable

Random variable $X$ adalah fungsi yang memetakan outcome $\omega$ ke bilangan:

$$
X:\Omega\rightarrow\mathbb{R}
$$

### Discrete Random Variable

Memiliki nilai yang dapat dihitung satu per satu, misalnya jumlah objek dalam gambar.

Probability Mass Function:

$$
p_X(x)=P(X=x)
$$

dan:

$$
\sum_xp_X(x)=1
$$

### Continuous Random Variable

Memiliki nilai pada interval kontinu, misalnya tinggi badan atau noise sensor.

Probability Density Function:

$$
P(a\le X\le b)=\int_a^b f_X(x)\,dx
$$

Nilai PDF bukan probabilitas pada satu titik. Untuk continuous variable, biasanya $P(X=x)=0$.

## Cumulative Distribution Function

CDF menyatakan peluang nilai tidak melebihi $x$:

$$
F_X(x)=P(X\le x)
$$

CDF berlaku untuk discrete maupun continuous variable.

## Distribusi Bernoulli

Untuk percobaan dengan dua hasil:

$$
X\sim\operatorname{Bernoulli}(p)
$$

$$
P(X=x)=p^x(1-p)^{1-x},\quad x\in\{0,1\}
$$

Dipakai pada binary classification.

## Distribusi Binomial

Jumlah keberhasilan dari $n$ percobaan Bernoulli independen:

$$
P(X=k)=
\binom{n}{k}p^k(1-p)^{n-k}
$$

## Distribusi Categorical

Satu outcome dari $K$ kategori dengan probabilitas $\boldsymbol{\pi}$.

Dipakai pada multi-class classification dan menjadi dasar cross-entropy.

## Distribusi Uniform

Semua nilai pada rentang memiliki density yang sama:

$$
X\sim U(a,b)
$$

Sering dipakai pada random initialization atau sampling sederhana.

## Distribusi Normal

$$
X\sim\mathcal{N}(\mu,\sigma^2)
$$

$$
f(x)=
\frac{1}{\sqrt{2\pi\sigma^2}}
\exp\left(
-\frac{(x-\mu)^2}{2\sigma^2}
\right)
$$

- $\mu$: mean atau pusat
- $\sigma^2$: variance atau penyebaran

Normal distribution muncul pada noise, asumsi model, initialization, dan Central Limit Theorem.

## Distribusi Multivariate Normal

Untuk vector $\mathbf{x}\in\mathbb{R}^d$:

$$
\mathbf{x}\sim\mathcal{N}(\boldsymbol{\mu},\boldsymbol{\Sigma})
$$

$\boldsymbol{\Sigma}$ adalah covariance matrix yang menyimpan varians tiap dimensi dan hubungan antar-dimensi.

## Sampling

Sampling berarti mengambil realization dari distribution:

```python
import numpy as np

rng = np.random.default_rng(42)
bernoulli = rng.binomial(n=1, p=0.7, size=1000)
normal = rng.normal(loc=0.0, scale=1.0, size=1000)
```

Gunakan random generator dan seed eksplisit agar eksperimen reproducible.

## Visualisasi

```python
import matplotlib.pyplot as plt
import numpy as np

rng = np.random.default_rng(42)
samples = rng.normal(0, 1, size=10_000)

plt.hist(samples, bins=40, density=True)
plt.xlabel("x")
plt.ylabel("density")
plt.show()
```

## Distribusi dalam Machine Learning

- Bernoulli → binary target
- Categorical → class probabilities
- Gaussian → regression noise dan latent variable
- Uniform → sampling dan initialization
- Multivariate Gaussian → feature distribution dan generative modeling

## Best Practice

- Bedakan parameter distribution dari statistik sample.
- Visualisasikan data sebelum mengasumsikan distribution.
- Catat seed dan generator yang digunakan.
- Jangan menganggap semua data normal tanpa pemeriksaan.

## Kesalahan Umum

- Menganggap PDF sebagai probabilitas langsung.
- Menyamakan sample histogram dengan distribution sebenarnya.
- Menganggap feature independen hanya karena correlation kecil.
- Menggunakan `scale` NumPy sebagai variance; `scale` adalah standard deviation.

## Ringkasan

Random variable memberi nilai numerik pada outcome acak. Distribution mendeskripsikan kemungkinan nilai tersebut melalui PMF, PDF, atau CDF.

## Hubungan Konsep

- [[Expectation Variance and Covariance]]
- [[Conditional Probability and Independence]]
- [[Bayes Theorem]]
- [[Machine Learning Evaluation Metrics]]
- [[Loss Function]]

## Checklist Pemahaman

- [ ] Bisa membedakan discrete dan continuous random variable
- [ ] Bisa menjelaskan PMF, PDF, dan CDF
- [ ] Tahu kapan Bernoulli, Categorical, dan Normal digunakan
- [ ] Paham parameter mean dan variance

## Latihan

1. Modelkan jumlah gambar rusak dari 20 gambar sebagai distribution yang sesuai.
2. Sampling 10.000 angka dari dua Normal distribution berbeda lalu bandingkan histogram.
3. Jelaskan kenapa $P(X=x)=0$ tidak berarti nilai $x$ mustahil muncul pada continuous variable.

