---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: foundations
order: 3
created: 2026-07-28
---

# Matrix

## Tujuan

- Memahami matrix sebagai susunan angka dua dimensi.
- Mengenali shape, index, baris, dan kolom.
- Memahami matrix sebagai data dan sebagai transformasi linear.

## Intuisi

Matrix adalah grid angka:

$$
A =
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6
\end{bmatrix}
$$

Matrix tersebut memiliki 2 baris, 3 kolom, dan shape $(2,3)$.

## Konsep Dasar

Elemen baris ke-$i$ dan kolom ke-$j$ ditulis $a_{ij}$.

$$
A\in\mathbb{R}^{m\times n}
$$

- $m$: jumlah baris
- $n$: jumlah kolom
- $\mathbb{R}$: elemen berupa bilangan real

## Kenapa Dibutuhkan?

Matrix memungkinkan banyak vector diproses sekaligus dan merepresentasikan transformasi. Hampir setiap model AI menggunakan matrix untuk:

- batch data
- weight layer
- feature map grayscale
- transformasi koordinat
- confusion matrix
- covariance matrix

## Cara Kerja dan Membaca Matrix

$$
A=
\begin{bmatrix}
a_{11}&a_{12}\\
a_{21}&a_{22}\\
a_{31}&a_{32}
\end{bmatrix}
\in\mathbb{R}^{3\times2}
$$

$a_{31}$ berarti elemen baris 3, kolom 1. Dalam Python, indexing dimulai dari nol sehingga elemen tersebut diakses dengan `A[2, 0]`.

### Operasi Element-wise

Untuk matrix dengan shape sama:

$$
(A+B)_{ij}=a_{ij}+b_{ij}
$$

$$
(A\odot B)_{ij}=a_{ij}b_{ij}
$$

Simbol $\odot$ menyatakan Hadamard atau element-wise product, bukan [[Matrix Multiplication]].

## Dua Makna Penting

### Matrix sebagai data

Baris dapat mewakili sample dan kolom mewakili feature.

### Matrix sebagai transformasi

Matrix dapat memetakan vector input ke vector output:

$$
\mathbf{y}=A\mathbf{x}
$$

Ini adalah fondasi layer linear pada neural network.

### Contoh Transformasi Manual

Scale 2D:

$$
S=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix},
\quad
\mathbf{x}=
\begin{bmatrix}
4\\
5
\end{bmatrix}
$$

$$
S\mathbf{x}
=
\begin{bmatrix}
8\\
15
\end{bmatrix}
$$

Koordinat $x$ diperbesar 2 kali dan koordinat $y$ 3 kali.

## Matrix dalam Computer Vision

Gambar grayscale berukuran $H\times W$ dapat direpresentasikan sebagai matrix intensitas pixel.

Transformasi rotasi 2D:

$$
R(\theta)=
\begin{bmatrix}
\cos\theta & -\sin\theta \\
\sin\theta & \cos\theta
\end{bmatrix}
$$

## Implementasi

```python
import numpy as np

A = np.array([
    [1, 2, 3],
    [4, 5, 6],
])

print(A.shape)   # (2, 3)
print(A[0, 1])   # 2
print(A[:, 0])   # kolom pertama
```

```python
import torch

weights = torch.tensor(
    [[0.2, -0.1], [0.5, 0.3]],
    dtype=torch.float32,
)

print(weights.shape)
print(weights.dtype)
```

## Shape dan Broadcasting

Broadcasting dapat memperluas axis berukuran satu secara konseptual:

```python
batch = np.ones((4, 3))
bias = np.array([1.0, 2.0, 3.0])
output = batch + bias
```

Bias shape `(3,)` diterapkan pada setiap baris. Broadcasting nyaman tetapi dapat menyembunyikan bug shape jika asumsi tidak diperiksa.

## Studi Kasus: Batch Linear Layer

$$
X\in\mathbb{R}^{B\times D},
\quad
W\in\mathbb{R}^{D\times K},
\quad
b\in\mathbb{R}^{K}
$$

$$
Y=XW+b
\in\mathbb{R}^{B\times K}
$$

- $B$: batch size
- $D$: jumlah input feature
- $K$: jumlah output feature

## Kompleksitas dan Memori

Matrix $m\times n$ menyimpan $mn$ elemen. Dengan `float32`, kebutuhan data mentahnya sekitar:

$$
4mn\text{ bytes}
$$

Belum termasuk overhead framework dan gradient.

## Best Practice

- Tulis shape pada diagram dan komentar interface.
- Gunakan nama axis yang bermakna.
- Bedakan operasi element-wise dari matrix multiplication.
- Periksa layout `(H,W)` vs `(W,H)` pada library.
- Jangan menghitung inverse jika linear solver cukup.

## Kesalahan Umum

- Menukar urutan `(rows, columns)` dengan `(width, height)`.
- Menganggap semua matrix mempunyai inverse.
- Menggunakan `*` ketika yang dimaksud adalah [[Matrix Multiplication]].

## Debugging

Saat shape mismatch:

```python
print("X:", X.shape)
print("W:", W.shape)
assert X.shape[-1] == W.shape[0]
```

Saat hasil tampak terbalik, periksa apakah data membutuhkan [[Transpose]] atau hanya salah memahami convention axis.

## Ringkasan

- Matrix adalah susunan dua dimensi.
- Matrix dapat merepresentasikan dataset, gambar, parameter, atau transformasi.
- Shape harus kompatibel agar operasi matrix valid.

## Checklist Pemahaman

- [ ] Bisa membaca $A\in\mathbb{R}^{m\times n}$.
- [ ] Bisa mengakses elemen, baris, dan kolom.
- [ ] Bisa membedakan matrix sebagai data dan transformasi.
- [ ] Bisa membedakan Hadamard product dan matrix multiplication.
- [ ] Bisa melacak shape batch linear layer.

## Hubungan Konsep

- Prasyarat: [[Scalar]], [[Vector]]
- Parent: [[Linear Algebra MOC]]
- Operasi: [[Matrix Multiplication]], [[Transpose]], [[Determinant]], [[Inverse Matrix]], [[Matrix Rank]]
- Lanjutan: [[Tensor]], [[Eigenvalue and Eigenvector]]

## Latihan

Untuk matrix dengan shape $(4,3)$, berapa jumlah baris, kolom, dan elemennya?

2. Hitung $S\mathbf{x}$ pada contoh scale secara manual.
3. Berapa memori mentah matrix `(1000, 512)` dengan `float32`?
