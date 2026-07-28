---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: foundations
order: 4
created: 2026-07-28
---

# Tensor

## Tujuan

- Memahami penggunaan istilah tensor dalam matematika dan Deep Learning.
- Membedakan tensor matematis dari array multidimensi.
- Membaca shape image tensor dan batch tensor.

## Intuisi untuk AI

Dalam framework AI, tensor adalah struktur data multidimensi:

```text
Scalar       → shape ()
Vector       → shape (D,)
Matrix       → shape (H, W)
Image RGB    → shape (C, H, W) atau (H, W, C)
Batch image  → shape (N, C, H, W)
```

## Konsep Dasar

Dalam konteks framework, setiap axis mempunyai ukuran:

$$
X\in\mathbb{R}^{d_1\times d_2\times\cdots\times d_k}
$$

- $k$: jumlah axis atau `ndim`
- $d_i$: ukuran axis ke-$i$
- jumlah elemen: $\prod_{i=1}^{k}d_i$

Contoh batch image:

$$
X\in\mathbb{R}^{32\times3\times224\times224}
$$

Jumlah elemennya:

$$
32(3)(224)(224)=4{,}816{,}896
$$

Dengan `float32`, data mentah membutuhkan sekitar 19.3 MB.

## Kenapa Dibutuhkan?

Data AI mempunyai banyak struktur sekaligus:

- batch
- channel
- spatial dimension
- time
- sequence
- feature

Tensor mempertahankan struktur axis tersebut sehingga operasi dapat diterapkan secara vectorized.

## Cara Kerja di Framework AI

Tensor menyimpan:

- buffer angka
- shape
- stride atau cara melangkah di memori
- dtype
- device
- informasi autograd jika diperlukan

View, transpose, dan permute kadang hanya mengubah metadata tanpa langsung menyalin buffer. Akibatnya, tensor bisa menjadi non-contiguous.

## Tensor Matematis vs Array Multidimensi

Perbedaannya terletak pada **objek matematika** dan **cara objek tersebut direpresentasikan di komputer**.

### Array multidimensi

Array multidimensi adalah struktur data yang menyusun angka dalam beberapa axis. Fokusnya pada:

- `shape`
- index
- `dtype`
- layout memori
- operasi numerik

Array angka tidak otomatis menjadi tensor dalam definisi matematika yang ketat.

### Tensor matematis

Tensor matematis adalah objek abstrak yang merepresentasikan hubungan **multilinear**. Komponennya mengikuti aturan transformasi tertentu ketika basis atau sistem koordinat berubah.

> [!important]
> Komponen angka dapat berubah ketika basis berubah, tetapi objek matematis yang direpresentasikan tetap sama.

[[Vector]] adalah tensor orde 1. Sebuah vector geometris tetap menunjuk ke arah yang sama walaupun komponennya berubah setelah sistem koordinat diputar.

Tidak semua [[Matrix]] otomatis merupakan tensor matematis. Matrix dapat menjadi tabel data, representasi [[Linear Transformation]], atau komponen tensor orde 2.

### Kenapa PyTorch menyebutnya tensor?

PyTorch memakai istilah tensor sebagai abstraksi komputasi untuk array multidimensi yang mendukung:

- CPU dan GPU
- automatic differentiation
- computation graph
- broadcasting
- `dtype` dan `device`

```python
import torch

images = torch.randn(32, 3, 224, 224)
print(images.shape)
```

Shape `(32, 3, 224, 224)` biasanya berarti:

```text
(batch, channel, height, width)
```

> [!summary]
> Semua tensor matematis dapat direpresentasikan sebagai array setelah basis dipilih, tetapi tidak semua array multidimensi otomatis merupakan tensor matematis.

## Axis Convention

Format umum:

```text
NumPy/OpenCV image → (H, W, C)
PyTorch image      → (C, H, W)
PyTorch batch      → (N, C, H, W)
Video batch        → (N, T, C, H, W) atau convention lain
```

Convention bukan hukum matematika. Selalu baca dokumentasi model atau library.

## Operasi Shape

```python
import torch

images = torch.randn(8, 224, 224, 3)  # NHWC
images = images.permute(0, 3, 1, 2)   # NCHW

flattened = images.flatten(start_dim=1)
restored = flattened.reshape(8, 3, 224, 224)
```

- `reshape`: mengubah grouping elemen jika jumlah total sama
- `permute`: mengubah urutan axis
- `squeeze`: menghapus axis ukuran 1
- `unsqueeze`: menambah axis ukuran 1
- `flatten`: menggabungkan beberapa axis

> [!warning]
> `reshape` bukan pengganti `permute`. Keduanya mempunyai makna berbeda.

## Dimensi, Rank, dan Order

Dalam Deep Learning, “rank tensor” sering berarti jumlah axis (`ndim`). Dalam linear algebra, [[Matrix Rank]] berarti jumlah baris atau kolom yang independen. Keduanya berbeda.

```python
x = torch.randn(32, 3, 224, 224)
print(x.ndim)  # 4
```

## Implementasi NumPy dan PyTorch

```python
import numpy as np
import torch

np_image = np.zeros((224, 224, 3), dtype=np.uint8)
torch_image = torch.from_numpy(np_image).permute(2, 0, 1)

torch_image = torch_image.to(dtype=torch.float32) / 255.0

print(np_image.shape)     # HWC
print(torch_image.shape)  # CHW
```

NumPy dan PyTorch dapat berbagi memori pada kondisi tertentu. Perubahan in-place perlu dilakukan dengan sadar.

## Relevansi untuk CV

- Gambar RGB: 3D tensor.
- Batch gambar: 4D tensor.
- Video batch: sering berupa 5D tensor.
- Feature map CNN: tensor berisi activation.

Definisi formal menjadi lebih penting pada geometry, robotics, [[3D Computer Vision]], physics-informed ML, dan equivariant neural networks.

## Studi Kasus: CNN Feature Map

Input:

$$
X\in\mathbb{R}^{N\times3\times224\times224}
$$

Setelah convolution pertama:

$$
F\in\mathbb{R}^{N\times64\times112\times112}
$$

Channel berubah dari RGB menjadi 64 learned feature maps, sedangkan resolusi spatial dapat mengecil karena stride.

## Best Practice

- Dokumentasikan nama axis, bukan shape saja.
- Assert shape pada boundary penting.
- Gunakan `float32` sebagai default umum kecuali ada alasan lain.
- Periksa device sebelum operasi antar-tensor.
- Hindari in-place operation saat dapat mengganggu autograd.
- Gunakan `contiguous()` hanya jika operasi berikutnya membutuhkannya.

## Kesalahan Umum

- Menganggap “dimensi gambar” selalu sama dengan jumlah axis tensor.
- Tertukar antara format `NCHW` dan `NHWC`.
- Menyamakan tensor rank di PyTorch dengan matrix rank.
- Mengabaikan `dtype` dan `device`.

## Debugging

Checklist shape error:

```python
print("shape:", tensor.shape)
print("ndim:", tensor.ndim)
print("dtype:", tensor.dtype)
print("device:", tensor.device)
print("contiguous:", tensor.is_contiguous())
```

Jika model mengharapkan NCHW tetapi menerima NHWC, jangan memakai `reshape`; gunakan `permute`.

## Ringkasan

- Dalam engineering AI, tensor berarti array multidimensi dengan kemampuan komputasi tambahan.
- Dalam matematika, tensor mempunyai definisi koordinat-independen dan aturan transformasi.
- Shape memberi tahu ukuran setiap axis.

## Checklist Pemahaman

- [ ] Bisa membaca shape `(N,C,H,W)`.
- [ ] Bisa menghitung jumlah elemen dan estimasi memori.
- [ ] Bisa membedakan `reshape` dan `permute`.
- [ ] Bisa membedakan matrix rank dan tensor `ndim`.
- [ ] Bisa menjelaskan tensor matematis vs tensor framework.
- [ ] Bisa memeriksa shape, dtype, dan device.

## Hubungan Konsep

- Prasyarat: [[Scalar]], [[Vector]], [[Matrix]]
- Parent: [[Linear Algebra MOC]]
- Digunakan di: [[PyTorch]], [[CNN]], [[Computer Vision]], [[Deep Learning]]

## Latihan

1. Jelaskan arti shape `(16, 3, 224, 224)`.
2. Kenapa tidak semua array multidimensi otomatis merupakan tensor matematis?
3. Ubah array NumPy HWC menjadi tensor PyTorch NCHW.
4. Hitung jumlah elemen dan memori `float32` tensor `(8, 64, 56, 56)`.
