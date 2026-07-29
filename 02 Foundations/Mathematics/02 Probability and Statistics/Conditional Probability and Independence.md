---
type: concept
status: not-started
domain: mathematics
topic: probability-statistics
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Conditional Probability and Independence

## Tujuan

Memahami bagaimana informasi baru mengubah probabilitas dan membedakan independence dari conditional independence.

## Prasyarat

- [[Probability]]
- [[Random Variables and Probability Distributions]]

## Conditional Probability

Probabilitas $A$ setelah mengetahui $B$:

$$
P(A\mid B)
=
\frac{P(A\cap B)}{P(B)}
$$

dengan $P(B)>0$.

Informasi $B$ mempersempit ruang outcome yang relevan.

## Product Rule

$$
P(A\cap B)
=
P(A\mid B)P(B)
$$

Untuk banyak variable, chain rule:

$$
P(x_1,\ldots,x_n)
=
\prod_{i=1}^{n}
P(x_i\mid x_1,\ldots,x_{i-1})
$$

## Law of Total Probability

Jika $B_1,\ldots,B_k$ membentuk partition:

$$
P(A)
=
\sum_iP(A\mid B_i)P(B_i)
$$

Ini menjadi denominator pada [[Bayes Theorem]].

## Independence

$A$ dan $B$ independent jika:

$$
P(A\cap B)=P(A)P(B)
$$

Ekuivalen, jika probabilitas terdefinisi:

$$
P(A\mid B)=P(A)
$$

Artinya mengetahui $B$ tidak mengubah keyakinan terhadap $A$.

## Conditional Independence

$X$ dan $Y$ conditionally independent given $Z$ jika:

$$
P(X,Y\mid Z)
=
P(X\mid Z)P(Y\mid Z)
$$

Ditulis:

$$
X\perp Y\mid Z
$$

Dua variable dapat dependent secara marginal tetapi independent setelah faktor penyebab bersama diketahui.

## Contoh Confounder

```text
Cuaca panas → Penjualan es krim
Cuaca panas → Orang berenang
```

Penjualan es krim dan orang berenang berkorelasi. Setelah temperature diketahui, hubungan langsung keduanya dapat menghilang.

## Naive Bayes

Naive Bayes mengasumsikan fitur conditionally independent given class:

$$
P(\mathbf{x}\mid y)
=
\prod_jP(x_j\mid y)
$$

Asumsinya sering tidak benar secara literal, tetapi model tetap dapat bekerja baik pada kasus tertentu.

## Implementasi Sederhana

```python
import numpy as np

# Contoh tabel probabilitas gabungan P(A, B)
joint = np.array([
    [0.42, 0.18],
    [0.28, 0.12],
])

p_b1 = joint[:, 1].sum()
p_a1_given_b1 = joint[1, 1] / p_b1
print(p_a1_given_b1)
```

## Studi Kasus CV

- Probability objek given image: $P(y\mid x)$
- Sensor fusion: informasi kamera diperbarui oleh LiDAR
- Medical imaging: peluang penyakit berubah setelah hasil scan diketahui
- Bayesian tracking: belief posisi diperbarui setiap frame

## Best Practice

- Nyatakan dengan jelas apa yang dikondisikan.
- Jangan menganggap feature independent tanpa alasan.
- Bedakan correlation, dependence, dan causation.
- Gunakan causal reasoning jika keputusan menyangkut intervensi.

## Kesalahan Umum

- Membalik $P(A\mid B)$ menjadi $P(B\mid A)$.
- Menganggap mutually exclusive sama dengan independent.
- Mengira conditional independence berarti marginal independence.
- Mengabaikan base rate.

## Ringkasan

Conditional probability memperbarui peluang berdasarkan informasi. Independence berarti informasi lain tidak memberi perubahan, sedangkan conditional independence berlaku setelah variable tertentu diketahui.

## Hubungan Konsep

- [[Bayes Theorem]]
- [[Probability]]
- [[Dataset and Data Quality]]
- [[Supervised Learning]]

## Checklist Pemahaman

- [ ] Bisa menghitung conditional probability dari tabel
- [ ] Bisa menjelaskan product rule
- [ ] Bisa membedakan independence dan mutually exclusive
- [ ] Bisa memberikan contoh conditional independence

## Latihan

1. Buat contingency table dua event dan hitung $P(A\mid B)$.
2. Buktikan dua lemparan koin fair independent.
3. Jelaskan kenapa gejala dan hasil tes medis tidak boleh dibalik probabilitasnya.

