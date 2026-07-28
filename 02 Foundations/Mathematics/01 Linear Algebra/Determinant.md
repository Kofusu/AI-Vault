---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: intermediate
order: 7
created: 2026-07-28
---

# Determinant

## Tujuan

- Memahami determinant sebagai faktor perubahan luas atau volume.
- Menghitung determinant matrix $2\times2$.
- Menentukan apakah matrix invertible.

## Intuisi

Determinant memberi tahu bagaimana suatu transformasi linear mengubah luas atau volume.

- $|\det(A)|>1$: memperbesar.
- $0<|\det(A)|<1$: memperkecil.
- $\det(A)<0$: membalik orientasi.
- $\det(A)=0$: meruntuhkan dimensi dan kehilangan informasi.

## Konsep Dasar

Determinant adalah scalar yang hanya didefinisikan untuk matrix persegi:

$$
\det:\mathbb{R}^{n\times n}\rightarrow\mathbb{R}
$$

Nilainya menggambarkan signed volume scaling dari transformasi.

## Kenapa Dibutuhkan?

Determinant membantu menjawab:

- apakah transformasi kehilangan informasi?
- apakah matrix mempunyai inverse?
- apakah beberapa vector independen?
- bagaimana density berubah di bawah transformasi?
- apakah orientasi ruang terbalik?

## Cara Kerja Geometris

Untuk dua vector kolom $\mathbf{a}$ dan $\mathbf{b}$ dalam matrix:

$$
A=[\mathbf{a}\ \mathbf{b}]
$$

$|\det(A)|$ adalah luas parallelogram yang dibentuk keduanya. Jika luas nol, kedua vector segaris dan tidak independen.

## Rumus $2\times2$

$$
A=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
\quad\Rightarrow\quad
\det(A)=ad-bc
$$

Contoh:

$$
\det
\begin{bmatrix}
2&1\\
3&4
\end{bmatrix}
=2(4)-1(3)=5
$$

## Hubungan dengan Inverse

Matrix persegi mempunyai [[Inverse Matrix]] jika dan hanya jika:

$$
\det(A)\neq0
$$

## Sifat Penting

$$
\det(AB)=\det(A)\det(B)
$$

$$
\det(A^T)=\det(A)
$$

$$
\det(A^{-1})=\frac{1}{\det(A)}
$$

Menukar dua baris membalik tanda determinant.

## Implementasi

```python
import numpy as np

A = np.array([[2.0, 1.0], [3.0, 4.0]])
det = np.linalg.det(A)
print(det)
```

Untuk matrix besar atau perhitungan probabilitas, `slogdet` lebih stabil:

```python
sign, log_abs_det = np.linalg.slogdet(A)
```

Mengalikan banyak nilai eigen atau menghitung determinant langsung dapat overflow atau underflow.

## Relevansi untuk AI, Robotics, dan 3D

- Mengecek transformasi singular.
- Jacobian determinant muncul dalam change of variables dan generative models.
- Menilai perubahan volume akibat transformasi.

## Studi Kasus: Change of Variables

Pada normalizing flow, density berubah berdasarkan Jacobian:

$$
\log p_X(x)
=
\log p_Z(f(x))
+
\log\left|
\det\frac{\partial f}{\partial x}
\right|
$$

Jacobian determinant mengoreksi perubahan volume akibat transformasi.

## Kesalahan Umum

- Menghitung determinant untuk matrix non-square.
- Menganggap determinant adalah jumlah diagonal.
- Membandingkan hasil floating-point dengan nol secara persis.

## Best Practice

- Gunakan tolerance untuk mendeteksi singularity numerik.
- Jangan memakai determinant sebagai satu-satunya tes stabilitas.
- Gunakan `slogdet` untuk log-determinant.
- Interpretasikan tanda dan magnitude sesuai konteks.

## Debugging

Jika determinant sangat kecil:

```python
condition_number = np.linalg.cond(A)
rank = np.linalg.matrix_rank(A)
```

Matrix mungkin secara matematis invertible tetapi sangat tidak stabil secara numerik.

## Ringkasan

- Determinant hanya didefinisikan untuk matrix persegi.
- Nilai nol berarti transformasi kehilangan dimensi.
- Determinant nonzero berarti matrix invertible.

## Checklist Pemahaman

- [ ] Bisa menghitung determinant $2\times2$.
- [ ] Bisa menjelaskan makna geometris tanda dan magnitude.
- [ ] Bisa menjelaskan hubungan determinant, rank, dan inverse.
- [ ] Tahu kapan memakai `slogdet`.

## Hubungan Konsep

- Prasyarat: [[Matrix]]
- Parent: [[Linear Algebra MOC]]
- Lanjutan: [[Inverse Matrix]], [[Matrix Rank]], [[Eigenvalue and Eigenvector]]

## Latihan

Hitung determinant $\begin{bmatrix}1&2\\2&4\end{bmatrix}$ dan jelaskan maknanya.

2. Apa pengaruh menukar dua baris terhadap determinant?
3. Jika $\det(A)=2$ dan $\det(B)=3$, berapa $\det(AB)$?
