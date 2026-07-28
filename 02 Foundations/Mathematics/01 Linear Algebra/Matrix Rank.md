---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: intermediate
order: 9
created: 2026-07-28
---

# Matrix Rank

## Tujuan

- Memahami rank sebagai jumlah arah informasi independen.
- Membedakan matrix rank dari jumlah axis tensor.
- Menghubungkan rank dengan redundancy dan invertibility.

## Intuisi

Rank memberi tahu berapa banyak baris atau kolom yang membawa informasi independen.

$$
A=
\begin{bmatrix}
1&2\\
2&4
\end{bmatrix}
$$

Baris kedua adalah dua kali baris pertama, sehingga hanya ada satu arah independen:

$$
\operatorname{rank}(A)=1
$$

## Konsep Dasar

Row rank dan column rank selalu sama. Rank dapat ditafsirkan sebagai:

- dimensi column space
- dimensi row space
- jumlah singular value nonzero
- jumlah arah informasi independen

## Kenapa Dibutuhkan?

Rank membantu mengetahui:

- apakah sistem mempunyai solusi unik
- apakah feature redundant
- apakah transformasi kehilangan dimensi
- apakah matrix persegi invertible
- seberapa jauh model dapat dikompresi dengan pendekatan low-rank

## Cara Kerja

Gaussian elimination menghitung jumlah pivot. SVD menghitung jumlah singular value yang lebih besar dari tolerance.

```text
Matrix
  ↓ eliminasi atau SVD
Independent directions
  ↓
Rank
```

## Batas Rank

Untuk $A\in\mathbb{R}^{m\times n}$:

$$
\operatorname{rank}(A)\leq\min(m,n)
$$

Matrix disebut full-rank jika rank mencapai nilai maksimum tersebut.

## Hubungan dengan Inverse

Matrix persegi $n\times n$ invertible jika:

$$
\operatorname{rank}(A)=n
$$

## Implementasi

```python
import numpy as np

A = np.array([[1.0, 2.0], [2.0, 4.0]])
print(np.linalg.matrix_rank(A))  # 1
```

```python
singular_values = np.linalg.svd(A, compute_uv=False)
print(singular_values)
```

Pada floating-point, “nonzero” ditentukan dengan tolerance, bukan equality persis.

## Matrix Rank vs Tensor Rank

> [!warning]
> `torch_tensor.ndim` kadang disebut tensor rank, yaitu jumlah axis. Ini berbeda dari matrix rank yang mengukur jumlah arah linear independen.

## Relevansi untuk AI

- Rank rendah dapat menunjukkan redundancy.
- Low-rank approximation dipakai untuk compression dan LoRA.
- Rank deficiency dapat membuat solusi sistem linear tidak unik.
- PCA mencari representasi berdimensi lebih rendah.

## Studi Kasus: Low-Rank Adaptation

LoRA merepresentasikan update weight besar sebagai hasil dua matrix kecil:

$$
\Delta W=BA
$$

Jika rank adaptasi $r$ kecil:

$$
A\in\mathbb{R}^{r\times d_{in}},
\quad
B\in\mathbb{R}^{d_{out}\times r}
$$

Jumlah parameter training jauh lebih kecil daripada mengubah seluruh $W$.

## Kesalahan Umum

- Menyamakan rank dengan jumlah baris.
- Menyamakan matrix rank dengan `ndim`.
- Mengabaikan toleransi numerik pada floating-point.

## Best Practice

- Gunakan SVD untuk diagnosis rank numerik.
- Laporkan tolerance jika rank penting bagi kesimpulan.
- Jangan menganggap low-rank selalu mempertahankan semua informasi penting.
- Bedakan exact rank dan effective rank.

## Debugging

Jika rank berubah antar-platform, inspect singular values di sekitar tolerance. Masalahnya mungkin bukan bug, tetapi keputusan numerik tentang nilai yang sangat kecil.

## Ringkasan

- Rank mengukur jumlah informasi linear independen.
- Full-rank penting untuk invertibility.
- Low-rank structure dapat dimanfaatkan untuk kompresi model.

## Checklist Pemahaman

- [ ] Bisa menjelaskan rank sebagai independent directions.
- [ ] Bisa menentukan rank matrix sederhana.
- [ ] Bisa menjelaskan full-rank.
- [ ] Bisa membedakan matrix rank dan tensor `ndim`.
- [ ] Bisa menjelaskan ide low-rank approximation.

## Hubungan Konsep

- Prasyarat: [[Matrix]], [[Determinant]]
- Parent: [[Linear Algebra MOC]]
- Terkait: [[Inverse Matrix]], [[Eigenvalue and Eigenvector]], [[Tensor]]
- Digunakan di: [[PCA]], [[LoRA]], [[Model Compression]]

## Latihan

Berapa rank matrix yang semua barisnya nol? Kenapa?

2. Tentukan rank $\begin{bmatrix}1&2\\2&4\\3&6\end{bmatrix}$.
3. Jelaskan bagaimana tolerance memengaruhi numerical rank.
