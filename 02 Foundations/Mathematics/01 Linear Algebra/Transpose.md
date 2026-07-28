---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: foundations
order: 6
created: 2026-07-28
---

# Transpose

## Tujuan

- Memahami pertukaran baris dan kolom.
- Mengenal sifat transpose.
- Memahami kegunaannya dalam operasi vector dan matrix.

## Intuisi

Transpose membalik orientasi matrix terhadap diagonal utamanya.

$$
A=
\begin{bmatrix}
1&2&3\\
4&5&6
\end{bmatrix}
\Rightarrow
A^T=
\begin{bmatrix}
1&4\\
2&5\\
3&6
\end{bmatrix}
$$

Jika $A$ memiliki shape $(m,n)$, maka $A^T$ memiliki shape $(n,m)$.

## Konsep Dasar

Secara index:

$$
(A^T)_{ij}=A_{ji}
$$

Elemen diagonal utama tetap berada pada posisi yang sama, sedangkan elemen di luar diagonal bertukar sisi.

## Kenapa Dibutuhkan?

Transpose dipakai untuk:

- menyelaraskan shape pada [[Matrix Multiplication]]
- mengubah row vector menjadi column vector
- membentuk Gram matrix
- menghitung attention score $QK^T$
- mendefinisikan symmetric matrix
- menurunkan banyak rumus gradient

## Cara Kerja

```text
Baris 1 A → Kolom 1 Aᵀ
Baris 2 A → Kolom 2 Aᵀ
...
```

## Sifat Penting

$$
(A^T)^T=A
$$

$$
(A+B)^T=A^T+B^T
$$

$$
(AB)^T=B^T A^T
$$

Perhatikan urutan perkalian ikut terbalik.

## Implementasi

```python
import numpy as np

A = np.array([[1, 2, 3], [4, 5, 6]])

print(A.T)
print(np.transpose(A))
```

Pada tensor berdimensi lebih tinggi, transpose perlu menyebut axis yang ditukar.

```python
import torch

x = torch.randn(8, 3, 224, 224)

# Tukar height dan width
y = x.transpose(2, 3)

# Susun NCHW menjadi NHWC
z = x.permute(0, 2, 3, 1)
```

`transpose()` PyTorch menukar dua axis. `permute()` dapat menyusun ulang banyak axis.

## Symmetric Matrix

Matrix symmetric memenuhi:

$$
A=A^T
$$

Covariance matrix dan Gram matrix sering symmetric. Sifat ini memungkinkan algoritma eigen yang lebih stabil.

## Studi Kasus: Attention

Jika:

$$
Q,K\in\mathbb{R}^{T\times d}
$$

maka:

$$
QK^T\in\mathbb{R}^{T\times T}
$$

Setiap elemen output membandingkan satu token dengan token lain.

## Relevansi untuk AI

- Mengubah row vector menjadi column vector.
- Menyusun operasi dot product dan matrix multiplication.
- Muncul pada attention: $QK^T$.
- Mengatur layout tensor, tetapi berbeda dari reshape.

## Kesalahan Umum

- Menganggap transpose sama dengan inverse.
- Mengira transpose mengubah urutan seluruh elemen seperti flatten.
- Tidak memperhatikan urutan pada $(AB)^T$.

## Best Practice

- Gunakan transpose hanya setelah memberi nama axis.
- Ingat bahwa `.T` pada tensor berdimensi lebih dari dua dapat mempunyai perilaku berbeda antar-library atau versi.
- Pada PyTorch multi-axis, pilih `transpose()` atau `permute()` secara eksplisit.
- Periksa contiguity sebelum memakai `view()`.

## Debugging

Jika output benar shape-nya tetapi salah makna, tulis mapping axis:

```text
Sebelum: N C H W
Sesudah: N H W C
```

Shape saja tidak menjamin urutan axis benar.

## Ringkasan

- Transpose menukar baris dan kolom.
- Shape berubah dari $(m,n)$ menjadi $(n,m)$.
- Transpose tidak sama dengan [[Inverse Matrix]].

## Checklist Pemahaman

- [ ] Bisa menghitung transpose manual.
- [ ] Bisa menjelaskan $(AB)^T=B^TA^T$.
- [ ] Bisa membedakan transpose, reshape, dan inverse.
- [ ] Bisa menggunakan transpose pada attention.
- [ ] Bisa menyusun NCHW menjadi NHWC.

## Hubungan Konsep

- Prasyarat: [[Matrix]]
- Parent: [[Linear Algebra MOC]]
- Terkait: [[Matrix Multiplication]], [[Inverse Matrix]]
- Digunakan di: [[Attention Mechanism]]

## Latihan

Hitung transpose dari $\begin{bmatrix}1&0\\2&3\\4&5\end{bmatrix}$.

2. Jika $Q$ shape `(32, 64)`, berapa shape $QQ^T$?
3. Tulis `permute()` untuk mengubah `(N,T,C,H,W)` menjadi `(N,C,T,H,W)`.
