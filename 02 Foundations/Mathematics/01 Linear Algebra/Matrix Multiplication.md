---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: foundations
order: 5
created: 2026-07-28
---

# Matrix Multiplication

## Tujuan

- Memahami syarat shape untuk perkalian matrix.
- Menghitung satu contoh secara manual.
- Menghubungkannya dengan layer linear dan neural network.

## Intuisi

Matrix multiplication menggabungkan baris dari matrix pertama dengan kolom dari matrix kedua melalui dot product.

$$
A_{m\times n}B_{n\times p}=C_{m\times p}
$$

Dimensi bagian dalam harus sama.

## Konsep Dasar

Untuk:

$$
A\in\mathbb{R}^{m\times n},
\qquad
B\in\mathbb{R}^{n\times p}
$$

elemen output:

$$
c_{ij}
=
\sum_{k=1}^{n}a_{ik}b_{kj}
$$

Setiap $c_{ij}$ adalah dot product baris ke-$i$ dari $A$ dengan kolom ke-$j$ dari $B$.

## Kenapa Dibutuhkan?

Matrix multiplication memungkinkan satu transformasi diterapkan pada banyak vector dan beberapa transformasi digabung. Operasi ini menjadi inti:

- linear layer
- convolution setelah diubah ke bentuk matrix tertentu
- self-attention
- perubahan basis
- camera projection
- batch inference

## Cara Kerja Step-by-Step

```text
1. Ambil satu baris A
2. Ambil satu kolom B
3. Kalikan elemen berpasangan
4. Jumlahkan hasilnya
5. Simpan sebagai satu elemen C
6. Ulangi untuk semua baris dan kolom
```

## Contoh Manual

$$
A=
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix},
\quad
B=
\begin{bmatrix}
5&6\\
7&8
\end{bmatrix}
$$

Elemen pertama:

$$
c_{11}=1(5)+2(7)=19
$$

Hasil lengkap:

$$
AB=
\begin{bmatrix}
19&22\\
43&50
\end{bmatrix}
$$

## Sifat Penting

- Umumnya $AB\neq BA$.
- Bersifat asosiatif: $(AB)C=A(BC)$.
- Bersifat distributif: $A(B+C)=AB+AC$.

## Implementasi

```python
import numpy as np

A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

C = A @ B
print(C)
```

```python
import torch

A = torch.tensor([[1.0, 2.0], [3.0, 4.0]])
B = torch.tensor([[5.0, 6.0], [7.0, 8.0]])

print(torch.matmul(A, B))
print(A @ B)
```

`A * B` adalah element-wise multiplication, bukan matrix multiplication.

## Relevansi untuk AI

Layer linear:

$$
Y=XW+b
$$

- $X$: batch input
- $W$: bobot model
- $b$: bias
- $Y$: output

Operasi ini muncul di neural network, attention, projection, dan transformasi koordinat.

### Shape Tracing pada Batch

$$
X:(B,D),\quad W:(D,K)
\Rightarrow Y:(B,K)
$$

Contoh:

```text
X: (32, 512)
W: (512, 10)
Y: (32, 10)
```

Setiap dari 32 sample menghasilkan 10 output logit.

### Batched Matrix Multiplication

```python
queries = torch.randn(8, 12, 64)
keys = torch.randn(8, 12, 64)

scores = queries @ keys.transpose(-2, -1)
print(scores.shape)  # (8, 12, 12)
```

Batch dimension dipertahankan, sedangkan dua axis terakhir dikalikan sebagai matrix.

## Studi Kasus: Attention Score

$$
\operatorname{Attention}(Q,K,V)
=
\operatorname{softmax}
\left(
\frac{QK^T}{\sqrt{d_k}}
\right)V
$$

$QK^T$ menghitung kemiripan setiap query dengan setiap key. Scaling $\sqrt{d_k}$ membantu menjaga magnitude score.

## Kompleksitas

Algoritma klasik untuk $(m\times n)(n\times p)$ membutuhkan sekitar:

$$
O(mnp)
$$

## Kesalahan Umum

- Shape bagian dalam tidak sama.
- Menggunakan `*` sebagai pengganti `@`.
- Mengira urutan perkalian dapat ditukar.

## Best Practice

- Tulis shape sebelum menghitung.
- Gunakan `@` untuk matrix multiplication yang jelas.
- Bedakan `@`, `*`, dan `torch.mul`.
- Hindari transpose tanpa mengetahui axis.
- Profiling diperlukan sebelum mengoptimalkan operasi besar.

## Debugging

Error umum:

```text
mat1 and mat2 shapes cannot be multiplied
```

Periksa:

```python
print(A.shape, B.shape)
assert A.shape[-1] == B.shape[-2]
```

Jangan “memperbaiki” shape dengan reshape acak karena dapat merusak makna axis.

## Ringkasan

- Matrix multiplication terdiri dari banyak dot product.
- Shape output berasal dari dimensi luar.
- Ini adalah salah satu operasi inti Deep Learning.

## Checklist Pemahaman

- [ ] Bisa menjelaskan kenapa inner dimensions harus sama.
- [ ] Bisa menghitung elemen output manual.
- [ ] Bisa menentukan shape output.
- [ ] Bisa membedakan matrix dan element-wise multiplication.
- [ ] Bisa melacak shape $QK^T$.

## Hubungan Konsep

- Prasyarat: [[Vector]], [[Matrix]]
- Parent: [[Linear Algebra MOC]]
- Terkait: [[Transpose]]
- Digunakan di: [[Neural Network]], [[Attention Mechanism]], [[Linear Transformation]]

## Latihan

Apakah matrix shape `(4, 3)` dapat dikalikan dengan matrix shape `(2, 5)`? Jelaskan.

2. Tentukan shape hasil `(16, 128) @ (128, 64)`.
3. Hitung manual $\begin{bmatrix}1&2\end{bmatrix}\begin{bmatrix}3\\4\end{bmatrix}$.
