---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: intermediate
order: 10
created: 2026-07-28
---

# Eigenvalue and Eigenvector

## Tujuan

- Memahami arah yang tidak berubah akibat transformasi.
- Mengenal persamaan eigenvalue.
- Menghubungkannya dengan PCA dan analisis transformasi.

## Intuisi

Sebagian besar vector berubah arah ketika dikalikan matrix. Eigenvector adalah arah khusus yang hanya diregangkan, diperkecil, atau dibalik.

$$
A\mathbf{v}=\lambda\mathbf{v}
$$

- $A$: matrix transformasi
- $\mathbf{v}$: eigenvector nonzero
- $\lambda$: eigenvalue atau faktor skala

## Konsep Dasar

Eigenvector harus nonzero. Jika $\mathbf{v}$ eigenvector, setiap kelipatan nonzero $c\mathbf{v}$ juga merupakan eigenvector untuk eigenvalue yang sama.

Eigenvalue dapat:

- positif: arah dipertahankan
- negatif: arah dibalik
- nol: arah diruntuhkan
- kompleks: dapat merepresentasikan rotasi dan scaling pada ruang yang sesuai

## Kenapa Dibutuhkan?

Eigen decomposition mengungkap arah alami sebuah transformasi. Ini membantu:

- dimensionality reduction
- analisis covariance
- spectral graph methods
- stability analysis
- memahami repeated transformations

## Cara Kerja

Persamaan:

$$
(A-\lambda I)\mathbf{v}=0
$$

memiliki solusi nonzero hanya jika $A-\lambda I$ singular:

$$
\det(A-\lambda I)=0
$$

Ini menghasilkan characteristic polynomial.

## Contoh

$$
A=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix},
\quad
\mathbf{v}=
\begin{bmatrix}
1\\
0
\end{bmatrix}
$$

$$
A\mathbf{v}
=
\begin{bmatrix}
2\\
0
\end{bmatrix}
=2\mathbf{v}
$$

Jadi $\mathbf{v}$ adalah eigenvector dengan eigenvalue $\lambda=2$.

## Cara Menemukan Eigenvalue

$$
\det(A-\lambda I)=0
$$

Setelah $\lambda$ ditemukan, cari $\mathbf{v}$ dari:

$$
(A-\lambda I)\mathbf{v}=0
$$

## Implementasi

```python
import numpy as np

A = np.array([[2.0, 0.0], [0.0, 3.0]])
eigenvalues, eigenvectors = np.linalg.eig(A)

print(eigenvalues)
print(eigenvectors)
```

Untuk symmetric matrix, `np.linalg.eigh` biasanya lebih tepat dan stabil.

### Verifikasi Hasil

```python
for index, eigenvalue in enumerate(eigenvalues):
    eigenvector = eigenvectors[:, index]
    assert np.allclose(
        A @ eigenvector,
        eigenvalue * eigenvector,
    )
```

NumPy menyimpan eigenvector sebagai **kolom**.

## Relevansi untuk AI dan CV

- PCA memakai eigenvector covariance matrix sebagai principal directions.
- Spectral clustering memakai eigenstructure graph.
- Analisis kestabilan sistem dinamis dan robotics.
- Memahami bagaimana transformasi memperkuat atau melemahkan arah tertentu.

## Studi Kasus: PCA

1. Center data.
2. Hitung covariance matrix.
3. Cari eigenvector dan eigenvalue.
4. Urutkan berdasarkan eigenvalue terbesar.
5. Project data ke principal directions.

```text
Data berdimensi tinggi
        ↓ covariance
Eigenvectors + eigenvalues
        ↓ pilih komponen utama
Representasi berdimensi rendah
```

Eigenvalue besar berarti arah tersebut menjelaskan variance lebih besar.

## Limitasi

- Matrix non-symmetric dapat mempunyai eigenvalue kompleks.
- Matrix tertentu tidak mempunyai cukup eigenvector independen untuk diagonalization.
- PCA linear dan sensitif terhadap scaling serta outlier.

## Kesalahan Umum

- Menganggap eigenvector unik; kelipatan nonzero tetap eigenvector.
- Lupa bahwa tidak semua matrix mempunyai eigenvalue real.
- Mengira setiap vector adalah eigenvector.

## Best Practice

- Gunakan `eigh` untuk real symmetric matrix.
- Center data sebelum PCA.
- Standardize feature jika satuannya berbeda secara tidak sebanding.
- Verifikasi $A\mathbf{v}\approx\lambda\mathbf{v}$.
- Jangan menafsirkan tanda eigenvector sebagai identitas unik; $\mathbf{v}$ dan $-\mathbf{v}$ setara.

## Debugging

Jika hasil eigenvector tampak “terbalik” antar-run, periksa sign ambiguity. Jika muncul komponen kompleks kecil, periksa symmetry dan numerical noise.

## Ringkasan

- Eigenvector mempertahankan arah setelah transformasi.
- Eigenvalue memberi faktor skalanya.
- Konsep ini penting untuk dimensionality reduction dan analisis sistem.

## Checklist Pemahaman

- [ ] Bisa menjelaskan persamaan $A\mathbf{v}=\lambda\mathbf{v}$.
- [ ] Bisa memverifikasi eigenpair.
- [ ] Bisa menjelaskan hubungan eigenvalue dan variance PCA.
- [ ] Mengerti sign ambiguity.
- [ ] Tahu kapan memakai `eigh`.

## Hubungan Konsep

- Prasyarat: [[Vector]], [[Matrix]], [[Matrix Multiplication]], [[Determinant]]
- Parent: [[Linear Algebra MOC]]
- Digunakan di: [[PCA]], [[Spectral Clustering]], [[Robotics]]

## Latihan

Untuk matrix diagonal $\operatorname{diag}(4,7)$, tentukan dua eigenvalue dan arah eigenvector dasarnya.

2. Verifikasi eigenpair tersebut dengan NumPy.
3. Kenapa covariance matrix cocok diproses dengan `eigh`?
