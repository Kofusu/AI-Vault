---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: intermediate
order: 8
created: 2026-07-28
---

# Inverse Matrix

## Tujuan

- Memahami inverse sebagai pembatal transformasi.
- Menentukan kapan inverse ada.
- Mengetahui kenapa menyelesaikan sistem linear lebih baik daripada menghitung inverse langsung.

## Intuisi

Inverse matrix $A^{-1}$ membatalkan transformasi $A$:

$$
A^{-1}A=AA^{-1}=I
$$

$I$ adalah identity matrix.

## Konsep Dasar

Matrix $A^{-1}$ hanya ada jika $A$:

- persegi
- full-rank
- mempunyai determinant nonzero

Inverse mengembalikan output transformasi ke input:

$$
\mathbf{y}=A\mathbf{x}
\Rightarrow
\mathbf{x}=A^{-1}\mathbf{y}
$$

## Kenapa Dibutuhkan?

Inverse muncul ketika kita ingin:

- membatalkan transformasi koordinat
- menyelesaikan sistem linear
- berpindah dari camera frame ke world frame
- menurunkan formula statistik
- memahami identifiability sebuah transformasi

## Cara Kerja pada $2\times2$

Faktor $1/(ad-bc)$ menskalakan adjugate. Jika $ad-bc=0$, pembagian tidak mungkin karena transformasi sudah kehilangan satu arah informasi.

## Rumus $2\times2$

Untuk:

$$
A=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
$$

$$
A^{-1}
=
\frac{1}{ad-bc}
\begin{bmatrix}
d&-b\\
-c&a
\end{bmatrix}
$$

Inverse hanya ada jika [[Determinant]] tidak nol.

## Menyelesaikan Sistem Linear

Secara teori:

$$
A\mathbf{x}=\mathbf{b}
\Rightarrow
\mathbf{x}=A^{-1}\mathbf{b}
$$

Dalam komputasi, gunakan solver:

```python
import numpy as np

A = np.array([[2.0, 1.0], [1.0, 3.0]])
b = np.array([5.0, 6.0])

x = np.linalg.solve(A, b)
```

> [!tip]
> `np.linalg.solve(A, b)` biasanya lebih cepat dan lebih stabil daripada menghitung `np.linalg.inv(A) @ b`.

## Implementasi

```python
import numpy as np

A = np.array([[2.0, 1.0], [1.0, 3.0]])
inverse = np.linalg.inv(A)
identity_approx = A @ inverse

print(inverse)
print(identity_approx)
```

Gunakan `np.allclose(identity_approx, np.eye(2))`, bukan equality persis.

## Pseudoinverse

Matrix non-square atau rank-deficient tidak mempunyai inverse biasa. Moore–Penrose pseudoinverse memberi solusi least-squares tertentu:

$$
A^+
$$

```python
pseudo_inverse = np.linalg.pinv(A)
```

Pseudoinverse bukan berarti informasi yang hilang benar-benar dapat dipulihkan.

## Relevansi untuk CV dan Robotics

- Membalik transformasi koordinat.
- Mengubah titik dari camera frame kembali ke world frame.
- Menyelesaikan sistem persamaan pada calibration dan geometry.

## Studi Kasus: Transformasi Homogeneous

Pose rigid 3D:

$$
T=
\begin{bmatrix}
R&t\\
0&1
\end{bmatrix}
$$

Jika $R$ adalah rotation matrix:

$$
T^{-1}
=
\begin{bmatrix}
R^T&-R^Tt\\
0&1
\end{bmatrix}
$$

Karena rotation matrix orthogonal, $R^{-1}=R^T$.

## Kesalahan Umum

- Mencoba inverse matrix non-square.
- Menganggap setiap matrix persegi invertible.
- Menghitung inverse eksplisit ketika solver sudah cukup.
- Mengabaikan matrix yang hampir singular dan tidak stabil secara numerik.

## Best Practice

- Gunakan solver, QR, atau SVD sesuai masalah.
- Hindari inverse eksplisit dalam hot path.
- Periksa condition number.
- Gunakan pseudoinverse hanya dengan memahami asumsi least-squares.
- Uji hasil dengan residual $\|A\mathbf{x}-\mathbf{b}\|$.

## Debugging

```python
rank = np.linalg.matrix_rank(A)
condition_number = np.linalg.cond(A)
residual = np.linalg.norm(A @ x - b)
```

Condition number besar berarti error kecil pada input dapat diperbesar drastis.

## Ringkasan

- Inverse membatalkan transformasi.
- Inverse hanya ada untuk matrix persegi full-rank.
- Gunakan linear solver untuk menyelesaikan sistem.

## Checklist Pemahaman

- [ ] Bisa menjelaskan syarat inverse.
- [ ] Bisa menghitung inverse $2\times2$.
- [ ] Bisa menjelaskan kenapa solver lebih baik.
- [ ] Bisa membedakan inverse dan pseudoinverse.
- [ ] Bisa memeriksa residual solusi.

## Hubungan Konsep

- Prasyarat: [[Matrix]], [[Matrix Multiplication]], [[Determinant]]
- Parent: [[Linear Algebra MOC]]
- Terkait: [[Matrix Rank]], [[Transpose]]
- Digunakan di: [[Camera Calibration]], [[3D Computer Vision]], [[Robotics]]

## Latihan

Kenapa matrix dengan determinant nol tidak mempunyai inverse?

2. Buktikan secara numerik bahwa $AA^{-1}\approx I$.
3. Kapan `np.linalg.pinv()` lebih relevan daripada `inv()`?
