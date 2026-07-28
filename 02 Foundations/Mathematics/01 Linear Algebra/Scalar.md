---
type: concept
status: not-started
domain: mathematics
topic: linear-algebra
level: foundations
order: 1
created: 2026-07-28
---

# Scalar

## Tujuan

- Memahami scalar sebagai satu nilai.
- Membedakan scalar dari vector berisi satu elemen.
- Mengenali penggunaan scalar dalam AI dan Computer Vision.

## Intuisi

Scalar adalah **satu nilai tunggal**.

```text
5
-3
0.001
255
```

Dalam AI, learning rate, loss, accuracy, dan satu intensitas pixel grayscale adalah scalar.

## Konsep Dasar

Scalar biasanya ditulis dengan huruf kecil:

$$
x = 5
$$

Scalar mempunyai orde atau jumlah axis nol. Karena itu, NumPy merepresentasikan shape scalar sebagai `()`.

Scalar dapat berasal dari beberapa himpunan bilangan:

- bilangan bulat: $x\in\mathbb{Z}$
- bilangan real: $x\in\mathbb{R}$
- bilangan kompleks: $x\in\mathbb{C}$
- boolean dalam konteks komputasi

## Kenapa Dibutuhkan?

Scalar menjadi unit paling kecil dari hampir semua komputasi AI:

- elemen sebuah [[Vector]], [[Matrix]], atau [[Tensor]]
- hyperparameter seperti learning rate
- satu prediction score
- satu nilai loss
- satu evaluation metric
- satu intensitas pixel

Walaupun model mengolah jutaan nilai, training biasanya membutuhkan satu scalar objective agar optimizer tahu apa yang harus diminimalkan.

## Cara Kerja dalam Struktur Data

```text
Scalar                         5
  ↓ susun beberapa nilai
Vector                  [1, 2, 3]
  ↓ susun beberapa vector
Matrix                  [[1, 2], [3, 4]]
  ↓ tambah axis
Tensor                  batch image
```

Scalar tidak mempunyai axis. Angka `5` berbeda dari collection `[5]`, walaupun keduanya mengandung nilai numerik yang sama.

## Scalar dalam Digital Image

Pada gambar grayscale:

$$
I =
\begin{bmatrix}
0 & 50 & 100 \\
120 & 180 & 200 \\
220 & 240 & 255
\end{bmatrix}
$$

Keseluruhan $I$ adalah [[Matrix]], sedangkan setiap elemen $I_{ij}$ adalah scalar.

## Scalar vs Vector

```python
import numpy as np

scalar = np.array(5)
vector = np.array([5])

print(scalar.ndim, scalar.shape)  # 0, ()
print(vector.ndim, vector.shape)  # 1, (1,)
```

> [!important]
> `5` adalah scalar, sedangkan `[5]` adalah [[Vector]] dengan satu elemen. Nilainya serupa, tetapi strukturnya berbeda.

## Broadcasting

Scalar dapat diterapkan ke seluruh elemen array:

```python
normalized_image = image.astype(np.float32) / 255.0
```

Scalar `255.0` membagi setiap pixel melalui broadcasting.

### Contoh Perhitungan Manual

Pixel grayscale $x=128$ dinormalisasi ke rentang $[0,1]$:

$$
x'=\frac{x}{255}
=\frac{128}{255}
\approx0.502
$$

Satu aturan scalar yang sama diterapkan ke setiap pixel gambar.

## Implementasi Python, NumPy, dan PyTorch

```python
# Python scalar
learning_rate = 0.001
epochs = 10
is_training = True
```

```python
import numpy as np

scalar = np.array(5.0)

print(scalar.ndim)   # 0
print(scalar.shape)  # ()
print(scalar.dtype)  # biasanya float64
```

```python
import torch

scalar = torch.tensor(5.0, requires_grad=True)
loss = scalar**2
loss.backward()

print(scalar.shape)  # torch.Size([])
print(scalar.grad)   # tensor(10.)
```

Tensor scalar PyTorch dapat berada dalam computation graph, mempunyai gradient, dan dipindahkan ke device.

## Scalar Loss dalam Training

Misalkan prediction $\hat{y}=0.8$ dan target $y=1$:

$$
L=(\hat{y}-y)^2
=(0.8-1)^2
=0.04
$$

```text
Banyak error per sample
        ↓ reduction: mean atau sum
Satu scalar loss
        ↓
Backpropagation
```

Tanpa reduction, framework dapat membutuhkan gradient awal eksplisit karena output bukan scalar.

## Contoh dalam AI

- `learning_rate = 0.001`
- `loss = 0.42`
- `accuracy = 0.94`
- `IoU = 0.78`
- `pixel_intensity = 128`

## Kesalahan Umum

- Menganggap scalar selalu bilangan bulat.
- Menganggap scalar `5` sama secara struktur dengan vector `[5]`.
- Tidak memperhatikan `dtype` saat normalisasi pixel.

## Studi Kasus Computer Vision

Saat membaca gambar `uint8`, nilai pixel berada pada $[0,255]$. Sebelum training, gambar sering diubah ke floating-point lalu dinormalisasi:

```python
image = image.astype(np.float32) / 255.0
```

Urutan ini penting. Konversi ke floating-point mencegah operasi lanjutan terjebak pada representasi integer yang terbatas.

## Best Practice

- Gunakan Python scalar untuk konfigurasi sederhana.
- Gunakan tensor scalar jika nilai harus ikut autograd atau berada di GPU.
- Catat satuan dan range sebuah scalar.
- Periksa `dtype` sebelum pembagian atau normalisasi.
- Jangan memakai `.item()` di tengah computation yang masih membutuhkan gradient.

## Debugging

Jika scalar unexpectedly mempunyai shape `(1,)`, cari operasi seperti:

```python
value = np.array([5.0])  # vector satu elemen
```

Bandingkan:

```python
assert np.array(5.0).shape == ()
assert np.array([5.0]).shape == (1,)
```

## Ringkasan

- Scalar adalah satu nilai dan mempunyai nol axis.
- Banyak scalar dapat disusun menjadi vector, matrix, atau tensor.
- Loss model biasanya direduksi menjadi satu scalar sebelum backpropagation.

## Checklist Pemahaman

- [ ] Bisa membedakan `5` dan `[5]`.
- [ ] Bisa menjelaskan kenapa scalar mempunyai `ndim = 0`.
- [ ] Bisa memberi contoh scalar dalam training dan CV.
- [ ] Bisa menjelaskan broadcasting scalar.
- [ ] Bisa menjelaskan kenapa loss biasanya scalar.

## Hubungan Konsep

- Parent: [[Linear Algebra MOC]]
- Lanjutan: [[Vector]], [[Matrix]], [[Tensor]]
- Digunakan di: [[Loss Function]], [[Gradient Descent]], [[Pixel]]

## Latihan

1. Apa perbedaan shape `np.array(7)` dan `np.array([7])`?
2. Dalam `image / 255.0`, apa peran `255.0`?
3. Apa risiko memanggil `.item()` sebelum `backward()`?
4. Buat tensor scalar PyTorch dan hitung gradient dari $L=x^3$ pada $x=2$.
