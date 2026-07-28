---
type: concept
status: not-started
domain: scientific-computing
topic: numpy
level: foundations
order: 2
created: 2026-07-28
---

# NumPy Fundamentals

## Tujuan

- Memahami `ndarray`, shape, axis, dtype, stride, view, dan copy.
- Membuat, mengindeks, serta mereduksi array.
- Menghubungkan NumPy dengan image dan tensor.

## Intuisi

NumPy array adalah blok data homogen dengan metadata:

```text
Data buffer
├── shape
├── dtype
├── strides
└── ndim
```

## Konsep Dasar

```python
import numpy as np

array = np.array([
    [1, 2, 3],
    [4, 5, 6],
], dtype=np.float32)

print(array.shape)  # (2, 3)
print(array.ndim)   # 2
print(array.dtype)
print(array.size)   # 6
```

## Kenapa Dibutuhkan?

NumPy menyediakan:

- compact homogeneous storage
- vectorized operation
- broadcasting
- linear algebra
- random sampling
- interoperability dengan scientific Python

## Cara Kerja Memory

Array mempunyai shape dan stride. Stride menyatakan lompatan byte ketika index pada suatu axis bertambah.

```python
print(array.strides)
```

Transpose atau slice sering membuat **view** yang berbagi buffer.

## Membuat Array

```python
zeros = np.zeros((2, 3), dtype=np.float32)
ones = np.ones((2, 3))
sequence = np.arange(0, 10, 2)
grid = np.linspace(0.0, 1.0, num=5)
identity = np.eye(3)
```

## Indexing dan Slicing

```python
matrix = np.arange(12).reshape(3, 4)

value = matrix[1, 2]
row = matrix[1]
column = matrix[:, 2]
region = matrix[0:2, 1:3]
```

Basic slicing biasanya menghasilkan view. Advanced indexing dengan integer array atau boolean mask biasanya menghasilkan copy.

## Boolean Mask

```python
scores = np.array([0.1, 0.8, 0.3, 0.9])
selected = scores[scores >= 0.5]
```

Mask harus compatible dengan axis yang diindeks.

## Reshape dan Axis

```python
image = np.zeros((224, 224, 3), dtype=np.uint8)
flat = image.reshape(-1, 3)
restored = flat.reshape(224, 224, 3)
```

`-1` meminta NumPy menghitung ukuran axis. Jumlah elemen harus tetap sama.

## Reduction

```python
image_float = image.astype(np.float32)

global_mean = image_float.mean()
channel_mean = image_float.mean(axis=(0, 1))
```

- tanpa `axis`: reduce seluruh array
- `axis=0`: hilangkan axis 0
- tuple axis: reduce beberapa axis
- `keepdims=True`: pertahankan axis ukuran 1

## View vs Copy

```python
original = np.arange(6)
view = original[1:4]
view[0] = 99

print(original)
```

Explicit copy:

```python
independent = original[1:4].copy()
```

## Implementasi: Normalize Image

```python
def normalize_image(image: np.ndarray) -> np.ndarray:
    if image.dtype != np.uint8:
        raise TypeError("Expected uint8 image")

    return image.astype(np.float32) / 255.0
```

## NumPy dan PyTorch

```python
import torch

np_image = np.zeros((224, 224, 3), dtype=np.uint8)
tensor = torch.from_numpy(np_image)
```

Keduanya dapat berbagi memory. Gunakan `.copy()` atau `.clone()` jika membutuhkan ownership terpisah.

## Studi Kasus: Dataset Image Batch

```text
Batch NHWC: (N, H, W, C)
Batch NCHW: (N, C, H, W)
```

NumPy tidak mengetahui arti axis. Developer harus menjaga convention.

## Best Practice

- Tulis shape contract.
- Pilih dtype eksplisit.
- Gunakan `.copy()` ketika ownership harus terpisah.
- Hindari `object` dtype untuk data numerik.
- Gunakan `keepdims=True` jika hasil perlu dibroadcast kembali.
- Periksa contiguity ketika interoperability bermasalah.

## Kesalahan Umum

- Tertukar `(H,W,C)` dengan `(C,H,W)`.
- Mengubah view dan tidak sadar original ikut berubah.
- Integer overflow.
- `reshape` dipakai untuk mengganti urutan axis.
- Reduction pada axis yang salah.

## Debugging

```python
print("shape:", array.shape)
print("dtype:", array.dtype)
print("strides:", array.strides)
print("C contiguous:", array.flags.c_contiguous)
print("owns data:", array.flags.owndata)
```

## Ringkasan

- `ndarray` adalah data homogen plus metadata.
- Axis menentukan struktur operasi.
- Slice dapat berbagi memory.
- Dtype menentukan precision dan range.

## Checklist Pemahaman

- [ ] Bisa membaca shape, axis, dan dtype.
- [ ] Bisa memakai slicing dan boolean mask.
- [ ] Bisa menjelaskan view vs copy.
- [ ] Bisa melakukan reduction per-channel.
- [ ] Bisa membedakan reshape dan transpose.
- [ ] Bisa mengubah NumPy ke PyTorch dengan aman.

## Latihan

1. Buat array shape `(3,4)` dan ambil kolom kedua.
2. Hitung mean tiap kolom.
3. Demonstrasikan view dan copy.
4. Normalisasi image `uint8` menjadi `float32`.

## Referensi

- [NumPy User Guide](https://numpy.org/doc/stable/user/)

