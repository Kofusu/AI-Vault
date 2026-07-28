---
type: concept
status: not-started
domain: scientific-computing
topic: numerical-foundations
level: foundations
order: 1
created: 2026-07-28
---

# Scientific Computing Fundamentals

## Tujuan

- Memahami scientific computing dan numerical representation.
- Mengenal precision, error, reproducibility, serta vectorized workflow.
- Memilih tool berdasarkan bentuk data dan task.

## Intuisi

Scientific computing mengubah masalah ilmiah menjadi representasi numerik yang dapat dihitung komputer.

```text
Masalah nyata
    ↓ model dan asumsi
Data numerik
    ↓ algorithm
Hasil
    ↓ validation
Kesimpulan terbatas
```

## Konsep Dasar

Komponen utama:

- representasi data
- numerical algorithm
- error dan approximation
- experiment design
- visualization
- reproducibility

## Kenapa Dibutuhkan?

AI bukan hanya memanggil model. Engineer harus mampu:

- memahami bentuk data
- memeriksa kualitas dataset
- menghindari numerical bug
- memvisualisasikan hasil
- membangun baseline
- mereproduksi eksperimen

## Cara Kerja Floating-Point

Komputer menyimpan real number dengan precision terbatas:

```python
print(0.1 + 0.2 == 0.3)  # False
```

Gunakan tolerance:

```python
import math

assert math.isclose(0.1 + 0.2, 0.3)
```

Jenis error:

- rounding error
- truncation error
- overflow
- underflow
- cancellation

## Absolute dan Relative Error

$$
\text{absolute error}
=
|\hat{x}-x|
$$

$$
\text{relative error}
=
\frac{|\hat{x}-x|}{|x|}
$$

Relative error bermasalah jika $x$ mendekati nol; gunakan metric sesuai konteks.

## Dtype dan Precision

```text
uint8    → image encoded umum
int64    → integer besar
float32  → training DL umum
float64  → analysis presisi lebih tinggi
bool     → mask
```

Precision lebih tinggi memakai lebih banyak memory dan computation. Precision rendah dapat mempercepat training tetapi memerlukan stability strategy.

## Tool Map

```text
Array numerik      → NumPy
Data tabular       → Pandas
Plot               → Matplotlib
Image sederhana    → Pillow
CV dan video       → OpenCV
Classical ML       → Scikit-Learn
Tensor + GPU       → PyTorch
```

## Implementasi Environment Check

```python
import platform
import numpy as np

print("Python:", platform.python_version())
print("NumPy:", np.__version__)
print("float32 epsilon:", np.finfo(np.float32).eps)
print("float64 epsilon:", np.finfo(np.float64).eps)
```

## Reproducibility

Catat:

- code version
- dependency version
- random seed
- dataset version dan split
- config
- hardware
- output artifact

Seed meningkatkan repeatability, tetapi tidak menjamin bitwise determinism pada semua hardware dan operation.

## Studi Kasus: Mean Pixel

Buruk:

```python
mean = image.sum() / image.size
```

Jika `image` bertipe integer sempit, reduction atau operasi intermediate perlu diperiksa. Lebih eksplisit:

```python
mean = image.astype(np.float64).mean()
```

Selalu pahami input dtype dan output dtype.

## Best Practice

- Validasi shape, dtype, range, dan missing value.
- Gunakan tolerance untuk float.
- Pilih metric sesuai scale.
- Simpan seed dan version.
- Pisahkan exploration dari reusable pipeline.
- Uji hasil numerik dengan contoh kecil manual.

## Kesalahan Umum

- Menganggap float adalah bilangan real eksak.
- Menggunakan dtype tanpa memahami range.
- Membandingkan float dengan equality.
- Menarik kesimpulan dari plot tanpa audit data.
- Menganggap seed menjamin seluruh determinism.

## Debugging

```python
print(array.shape)
print(array.dtype)
print(array.min(), array.max())
print(np.isfinite(array).all())
```

Jika hasil berbeda:

- cek seed
- cek version
- cek urutan data
- cek parallelism
- cek hardware/backend

## Ringkasan

- Scientific computing adalah pemodelan, komputasi, dan validasi numerik.
- Floating-point bersifat approximate.
- Shape, dtype, range, dan reproducibility adalah contract penting.

## Checklist Pemahaman

- [ ] Bisa menjelaskan floating-point approximation.
- [ ] Bisa membedakan absolute dan relative error.
- [ ] Bisa memilih tool berdasarkan data.
- [ ] Bisa menjelaskan trade-off dtype.
- [ ] Bisa membuat environment report.
- [ ] Bisa menyebut batas seed.

## Latihan

1. Bandingkan epsilon `float32` dan `float64`.
2. Cari nilai maksimum `uint8`.
3. Buat environment report project.
4. Jelaskan kapan `float64` lebih tepat daripada `float32`.

## Referensi

- [NumPy User Guide](https://numpy.org/doc/stable/user/)

