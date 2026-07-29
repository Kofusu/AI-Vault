---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Vector Geometry

## Tujuan

Memahami dot product, vector norm, distance, cosine similarity, dan projection sebagai bahasa geometris yang dipakai di Machine Learning, Computer Vision, retrieval, dan robotics.

## Prasyarat

- [[Vector]]
- [[Scalar]]

## Intuisi

Vector bukan cuma daftar angka. Ia bisa menyatakan arah, posisi, fitur, embedding, atau perubahan. Vector geometry membantu menjawab:

- Seberapa panjang vector?
- Seberapa jauh dua data?
- Apakah dua vector mengarah ke arah serupa?
- Seberapa besar satu vector berada pada arah vector lain?

## Dot Product

Untuk dua vector $\mathbf{a},\mathbf{b}\in\mathbb{R}^n$:

$$
\mathbf{a}\cdot\mathbf{b}
=
\sum_{i=1}^{n}a_i b_i
$$

Secara geometris:

$$
\mathbf{a}\cdot\mathbf{b}
=
\|\mathbf{a}\|_2\|\mathbf{b}\|_2\cos\theta
$$

- hasil positif: sudut kurang dari $90^\circ$
- hasil nol: orthogonal
- hasil negatif: arah cenderung berlawanan

### Contoh Manual

$$
\mathbf{a}=[1,2],\quad \mathbf{b}=[3,4]
$$

$$
\mathbf{a}\cdot\mathbf{b}
=(1)(3)+(2)(4)=11
$$

## Vector Norm

Norm mengukur besar atau panjang vector.

### L1 Norm

$$
\|\mathbf{x}\|_1=\sum_i|x_i|
$$

L1 mendorong sparsity dan muncul pada Lasso regularization.

### L2 Norm

$$
\|\mathbf{x}\|_2=\sqrt{\sum_i x_i^2}
$$

L2 adalah jarak Euclidean dari origin dan muncul pada weight decay.

### General $p$-Norm

$$
\|\mathbf{x}\|_p=
\left(\sum_i|x_i|^p\right)^{1/p}
$$

## Distance

Jarak antara dua vector biasanya dihitung dari norm selisih:

$$
d(\mathbf{x},\mathbf{y})=\|\mathbf{x}-\mathbf{y}\|
$$

Euclidean distance:

$$
d_2(\mathbf{x},\mathbf{y})
=
\sqrt{\sum_i(x_i-y_i)^2}
$$

Manhattan distance:

$$
d_1(\mathbf{x},\mathbf{y})
=
\sum_i|x_i-y_i|
$$

> [!warning]
> Distance sensitif terhadap skala fitur. Tinggi dalam sentimeter dapat mendominasi fitur kecil jika data tidak distandardisasi.

## Cosine Similarity

Cosine similarity membandingkan arah tanpa terlalu dipengaruhi panjang:

$$
\operatorname{cos\_sim}(\mathbf{a},\mathbf{b})
=
\frac{\mathbf{a}\cdot\mathbf{b}}
{\|\mathbf{a}\|_2\|\mathbf{b}\|_2}
$$

Nilainya berada pada $[-1,1]$ untuk vector real non-zero.

Dipakai pada:

- image dan text retrieval
- CLIP embedding
- nearest-neighbor search
- clustering embedding

## Vector Projection

Projection $\mathbf{a}$ ke arah $\mathbf{b}$:

$$
\operatorname{proj}_{\mathbf{b}}(\mathbf{a})
=
\frac{\mathbf{a}\cdot\mathbf{b}}
{\mathbf{b}\cdot\mathbf{b}}\mathbf{b}
$$

Projection memisahkan komponen yang sejajar dan orthogonal:

$$
\mathbf{a}
=
\operatorname{proj}_{\mathbf{b}}(\mathbf{a})
+
\mathbf{a}_{\perp}
$$

## Implementasi NumPy

```python
import numpy as np

a = np.array([1.0, 2.0])
b = np.array([3.0, 4.0])

dot = np.dot(a, b)
l1 = np.linalg.norm(a, ord=1)
l2 = np.linalg.norm(a, ord=2)
distance = np.linalg.norm(a - b)
cosine = dot / (np.linalg.norm(a) * np.linalg.norm(b))
projection = (dot / np.dot(b, b)) * b

print(dot, l1, l2, distance, cosine, projection)
```

## Implementasi PyTorch

```python
import torch
import torch.nn.functional as F

a = torch.tensor([1.0, 2.0])
b = torch.tensor([3.0, 4.0])

dot = torch.dot(a, b)
distance = torch.linalg.vector_norm(a - b)
cosine = F.cosine_similarity(a, b, dim=0)
```

## Studi Kasus AI

### Image Retrieval

Model mengubah dua gambar menjadi embedding. Cosine similarity tinggi berarti representasinya dianggap mirip.

### k-Nearest Neighbors

Prediksi ditentukan oleh data training dengan distance paling kecil.

### Robotics

Projection dapat memisahkan kecepatan robot menjadi komponen sejajar dan tegak lurus terhadap jalur.

## Kompleksitas

Semua operasi dasar pada vector berdimensi $n$ membutuhkan waktu $O(n)$ dan memori tambahan $O(1)$ hingga $O(n)$, tergantung implementasi.

## Best Practice

- Standardisasi fitur sebelum memakai distance.
- Normalisasi embedding jika cosine similarity menjadi metric utama.
- Tambahkan epsilon untuk mencegah pembagian nol.
- Pilih metric berdasarkan makna data, bukan karena paling populer.

## Kesalahan Umum

- Menganggap dot product menghasilkan vector; hasilnya scalar.
- Menyamakan cosine similarity dengan Euclidean distance.
- Menghitung cosine pada zero vector.
- Menggunakan distance pada fitur kategorikal tanpa encoding yang sesuai.

## Ringkasan

Dot product mengukur interaksi arah, norm mengukur besar, distance membandingkan posisi, cosine similarity membandingkan arah, dan projection mengambil komponen pada arah tertentu.

## Hubungan Konsep

- [[Matrix Multiplication]]
- [[Feature Engineering and Preprocessing]]
- [[k-Nearest Neighbors]]
- [[Support Vector Machine]]
- [[Principal Component Analysis]]
- [[Embedding]]

## Checklist Pemahaman

- [ ] Bisa menghitung dot product manual
- [ ] Bisa membedakan L1 dan L2 norm
- [ ] Paham kenapa feature scaling memengaruhi distance
- [ ] Bisa menjelaskan cosine similarity
- [ ] Bisa menghitung projection sederhana

## Latihan

1. Hitung dot product, L2 norm, dan cosine similarity dari $[2,1]$ dan $[-1,3]$.
2. Apa yang terjadi pada cosine similarity jika satu vector dikalikan 10?
3. Bandingkan Euclidean dan Manhattan distance pada dua titik buatan lu.

