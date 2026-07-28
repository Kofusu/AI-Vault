---
type: concept
status: not-started
domain: mathematics
topic: probability-statistics
level: foundations
order: 1
created: 2026-07-28
---

# Probability

## Tujuan

- Memahami probability sebagai ukuran ketidakpastian.
- Mengenal event, sample space, conditional probability, dan independence.
- Menghubungkannya dengan output model classification.

## Intuisi

Probability mengukur seberapa mungkin suatu kejadian terjadi:

$$
0\leq P(A)\leq1
$$

- $P(A)=0$: mustahil.
- $P(A)=1$: pasti.
- Nilai di antaranya menunjukkan tingkat keyakinan.

## Konsep Dasar

Probability model terdiri dari sample space $\Omega$, event yang merupakan subset dari $\Omega$, dan probability measure $P$.

Tiga aksioma dasar:

$$
P(A)\geq0,\qquad P(\Omega)=1
$$

Untuk event saling lepas $A_i$:

$$
P\left(\bigcup_i A_i\right)=\sum_iP(A_i)
$$

## Kenapa Dibutuhkan?

AI bekerja dengan data, sensor, dan label yang tidak sempurna. Probability memberi bahasa untuk prediction uncertainty, noise, distribusi data, sampling, generative model, dan risk dalam decision-making.

### Sample Space

Sample space $\Omega$ adalah kumpulan semua kemungkinan hasil.

Untuk lemparan dadu:

$$
\Omega=\{1,2,3,4,5,6\}
$$

### Event

Event adalah subset dari sample space. Misalnya hasil genap:

$$
A=\{2,4,6\}
$$

Jika semua hasil sama mungkin:

$$
P(A)=\frac{|A|}{|\Omega|}=\frac{3}{6}=0.5
$$

## Aturan Penting

Komplemen:

$$
P(A^c)=1-P(A)
$$

Union:

$$
P(A\cup B)=P(A)+P(B)-P(A\cap B)
$$

Conditional probability:

$$
P(A\mid B)=\frac{P(A\cap B)}{P(B)}
$$

## Independence

$A$ dan $B$ independen jika:

$$
P(A\cap B)=P(A)P(B)
$$

Independence berbeda dari mutual exclusivity. Dua event mutually exclusive tidak bisa terjadi bersama, sedangkan event independen tidak saling memengaruhi.

## Random Variable

Random variable memetakan hasil acak menjadi angka. Contohnya $X$ adalah jumlah wajah pada dua lemparan koin.

Expected value:

$$
\mathbb{E}[X]=\sum_x xP(X=x)
$$

Variance:

$$
\operatorname{Var}(X)
=
\mathbb{E}[(X-\mathbb{E}[X])^2]
$$

### Discrete dan Continuous Distribution

- Discrete random variable memakai probability mass function.
- Continuous random variable memakai probability density function.

$$
P(a\leq X\leq b)=\int_a^bp(x)\,dx
$$

Density dapat lebih dari 1; probability hasil integral harus berada pada $[0,1]$.

Distribution penting:

- Bernoulli untuk outcome biner
- Categorical untuk beberapa kelas
- Gaussian untuk model noise atau latent variable

## Cara Kerja Monte Carlo

Jika probability sulit dihitung:

1. sample banyak outcome
2. evaluasi event
3. ambil rata-rata indikator event
4. estimasi mendekati nilai sebenarnya saat sample bertambah

## Implementasi

```python
import numpy as np

rng = np.random.default_rng(42)
rolls = rng.integers(1, 7, size=100_000)

estimated_probability = np.mean(rolls % 2 == 0)
print(estimated_probability)
```

Hasil simulasi mendekati $0.5$.

## Relevansi untuk AI

- Softmax menghasilkan distribusi probabilitas kelas.
- Data augmentation dan dropout melibatkan randomness.
- Probabilistic models merepresentasikan uncertainty.
- [[Bayes Theorem]] dipakai untuk conditional inference.

> [!warning]
> Confidence model tidak otomatis berarti probabilitasnya terkalibrasi dengan baik.

## Studi Kasus: Classification

Softmax mengubah logits $z_i$ menjadi:

$$
p_i=\frac{e^{z_i}}{\sum_je^{z_j}}
$$

Output nonnegative dan berjumlah 1. Namun, confidence 0.9 belum tentu berarti model benar 90% pada seluruh prediction dengan confidence serupa. Calibration harus dievaluasi terpisah.

## Best Practice

- Definisikan event dan conditioning dengan jelas.
- Bedakan probability, density, dan arbitrary model score.
- Periksa asumsi independence.
- Gunakan log-probability untuk mencegah underflow.
- Evaluasi calibration jika probability dipakai untuk keputusan.

## Debugging dan Sanity Check

- Setiap probability harus berada pada $[0,1]$.
- Categorical distribution harus berjumlah sekitar 1.
- $P(A|B)$ membutuhkan $P(B)>0$.
- Frequency simulation harus mendekati probability saat sample besar.

## Kesalahan Umum

- Menyamakan probability dan certainty.
- Menganggap conditional probability simetris: $P(A|B)\neq P(B|A)$.
- Menyamakan mutually exclusive dengan independent.
- Menginterpretasikan confidence model tanpa calibration.

## Ringkasan

- Probability mengukur ketidakpastian.
- Conditional probability memasukkan informasi tambahan.
- Independence berarti satu event tidak mengubah probability event lain.

## Checklist Pemahaman

- [ ] Bisa membedakan sample space, event, dan probability.
- [ ] Bisa menghitung complement, union, dan conditional probability.
- [ ] Bisa membedakan independent dan mutually exclusive.
- [ ] Bisa menjelaskan expectation dan variance.
- [ ] Bisa membedakan PMF dan PDF.
- [ ] Bisa menjelaskan calibration.

## Hubungan Konsep

- Parent: [[Probability and Statistics MOC]]
- Lanjutan: [[Statistics]], [[Bayes Theorem]]
- Digunakan di: [[Softmax]], [[Classification]], [[Uncertainty]]

## Latihan

Jika $P(A)=0.6$, $P(B)=0.5$, dan keduanya independen, hitung $P(A\cap B)$.

2. Jika $P(A)=0.7$, berapa $P(A^c)$?
3. Simulasikan dua lemparan koin dan estimasi probability tepat satu head.
4. Kenapa probability titik tunggal pada continuous distribution biasanya nol?
