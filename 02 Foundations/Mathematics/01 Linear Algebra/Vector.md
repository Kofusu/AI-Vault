---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: foundations
order: 2
created: 2026-07-28
---

# Vector

## Tujuan

- Memahami vector sebagai kumpulan komponen terurut.
- Mengenal operasi dasar, norm, dan dot product.
- Menghubungkan vector dengan feature dan embedding AI.

## Intuisi

Vector bisa dipahami sebagai **daftar angka yang memiliki urutan** atau sebagai objek yang mempunyai arah dan panjang.

$$
\mathbf{x} =
\begin{bmatrix}
x_1 \\
x_2 \\
x_3
\end{bmatrix}
$$

Contoh feature satu objek:

```text
[tinggi, lebar, berat] = [170, 60, 65]
```

## Konsep Dasar dan Notasi

Vector berdimensi $n$ ditulis:

$$
\mathbf{x}\in\mathbb{R}^n
$$

Komponennya:

$$
\mathbf{x}=
\begin{bmatrix}
x_1\\x_2\\\vdots\\x_n
\end{bmatrix}
$$

- $n$: jumlah komponen
- $x_i$: scalar pada posisi ke-$i$
- $\mathbb{R}^n$: ruang seluruh vector real berdimensi $n$

Row vector dan column vector berbeda orientasi:

$$
\mathbf{x}^T=[x_1,x_2,\ldots,x_n]
$$

## Kenapa Dibutuhkan?

Vector menyatukan banyak feature menjadi satu objek yang dapat dihitung. Dalam AI:

- satu sample tabular menjadi feature vector
- embedding menyimpan representasi semantik
- gradient menyimpan arah perubahan banyak parameter
- koordinat titik menyimpan posisi
- probability vector menyimpan distribusi kelas

## Cara Kerja Geometris

Vector dapat dilihat sebagai perpindahan dari origin menuju satu titik.

```text
y
|       • (3, 2)
|      /
|     /  v = [3, 2]
|____/____________ x
```

Penjumlahan vector menggabungkan perpindahan. Perkalian scalar mengubah panjang dan dapat membalik arah.

## Operasi Dasar

Untuk $\mathbf{a}=[a_1,a_2]$ dan $\mathbf{b}=[b_1,b_2]$:

$$
\mathbf{a}+\mathbf{b}
=
[a_1+b_1,\;a_2+b_2]
$$

Perkalian scalar:

$$
c\mathbf{a}=[ca_1,\;ca_2]
$$

## Magnitude atau Norm

Panjang Euclidean vector:

$$
\|\mathbf{x}\|_2 =
\sqrt{x_1^2+x_2^2+\cdots+x_n^2}
$$

Untuk $\mathbf{x}=[3,4]$:

$$
\|\mathbf{x}\|_2=\sqrt{3^2+4^2}=5
$$

## Dot Product

$$
\mathbf{a}\cdot\mathbf{b}
=
\sum_{i=1}^{n}a_i b_i
$$

Dot product mengukur keselarasan dua vector dan menjadi dasar cosine similarity serta [[Attention Mechanism]].

### Contoh Manual

$$
\mathbf{a}=[1,2,3],
\qquad
\mathbf{b}=[4,5,6]
$$

$$
\mathbf{a}\cdot\mathbf{b}
=1(4)+2(5)+3(6)
=32
$$

### Cosine Similarity

$$
\cos(\theta)
=
\frac{\mathbf{a}\cdot\mathbf{b}}
{\|\mathbf{a}\|_2\|\mathbf{b}\|_2}
$$

- mendekati $1$: arah serupa
- mendekati $0$: hampir orthogonal
- mendekati $-1$: arah berlawanan

### Normalization

Unit vector:

$$
\hat{\mathbf{x}}
=
\frac{\mathbf{x}}{\|\mathbf{x}\|_2}
$$

Tambahkan epsilon saat implementasi jika norm dapat nol.

## Implementasi

```python
import numpy as np

a = np.array([1.0, 2.0, 3.0])
b = np.array([4.0, 5.0, 6.0])

print(a + b)
print(a @ b)
print(np.linalg.norm(a))
```

```python
cosine_similarity = (a @ b) / (
    np.linalg.norm(a) * np.linalg.norm(b)
)
```

## Relevansi untuk AI dan CV

- Satu sample tabular dapat direpresentasikan sebagai feature vector.
- Embedding gambar atau teks adalah vector berdimensi tinggi.
- Keypoint 2D dapat ditulis sebagai $[x,y]$.
- Pixel RGB dapat ditulis sebagai $[R,G,B]$.

## Studi Kasus: Image Retrieval

Model mengubah query image dan gallery image menjadi embedding vector. Cosine similarity dipakai untuk mengurutkan gambar yang paling mirip.

```text
Query image → embedding q
Gallery     → embeddings g₁ ... gₙ
                    ↓
             cosine similarity
                    ↓
              ranking hasil
```

Similarity tinggi tidak otomatis berarti dua gambar identik; maknanya bergantung pada representasi yang dipelajari model.

## Kompleksitas

Untuk vector berdimensi $n$:

- penjumlahan: $O(n)$
- dot product: $O(n)$
- norm: $O(n)$
- memori: $O(n)$

## Best Practice

- Dokumentasikan arti dan urutan setiap komponen.
- Pastikan dua vector yang dibandingkan berada dalam feature space yang sama.
- Normalisasi embedding jika metric mengasumsikan unit norm.
- Gunakan epsilon untuk mencegah division by zero.
- Perhatikan dtype saat vector berdimensi besar.

## Kesalahan Umum

- Mengabaikan orientasi row vector dan column vector.
- Menyamakan dot product dengan perkalian elemen biasa.
- Membandingkan embedding tanpa normalisasi ketika cosine similarity dibutuhkan.

## Debugging

Jika dot product gagal:

```python
assert a.ndim == 1
assert b.ndim == 1
assert a.shape == b.shape
```

Jika cosine similarity menghasilkan `NaN`, periksa apakah salah satu norm bernilai nol.

## Ringkasan

- Vector adalah kumpulan komponen terurut.
- Norm mengukur panjang vector.
- Dot product mengukur hubungan arah dan dipakai luas di AI.

## Checklist Pemahaman

- [ ] Bisa membaca notasi $\mathbf{x}\in\mathbb{R}^n$.
- [ ] Bisa menghitung penjumlahan, norm, dan dot product manual.
- [ ] Bisa membedakan dot product dan element-wise multiplication.
- [ ] Bisa menjelaskan cosine similarity.
- [ ] Bisa memberi contoh vector dalam CV.

## Hubungan Konsep

- Prasyarat: [[Scalar]]
- Parent: [[Linear Algebra MOC]]
- Lanjutan: [[Matrix]], [[Matrix Multiplication]], [[Tensor]]
- Digunakan di: [[Embedding]], [[Attention Mechanism]], [[Computer Vision]]

## Latihan

1. Hitung norm vector $[6,8]$.
2. Hitung dot product $[1,2]\cdot[3,4]$.
3. Hitung unit vector dari $[3,4]$.
4. Apa hasil cosine similarity jika salah satu vector adalah zero vector?
