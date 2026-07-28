---
type: concept
status: not-started
domain: scientific-computing
topic: numpy
level: foundations
order: 3
created: 2026-07-28
---

# NumPy Broadcasting and Vectorization

## Tujuan

- Memahami broadcasting rule dari trailing axis.
- Mengganti loop Python dengan vectorized operation.
- Mengenal ufunc, random generator, dan performance pitfall.

## Intuisi

Vectorization menyatakan operasi pada seluruh array. Broadcasting membuat shape berbeda seolah compatible tanpa selalu menyalin data.

## Konsep Dasar

Bandingkan shape dari kanan ke kiri. Dua ukuran compatible jika:

- sama, atau
- salah satunya 1

Contoh:

```text
(8, 224, 224, 3)
(               3)
------------------
(8, 224, 224, 3)
```

## Kenapa Dibutuhkan?

Loop Python mempunyai overhead per-iteration. NumPy menjalankan banyak operasi dalam compiled code dan memanfaatkan memory layout lebih baik.

## Cara Kerja Broadcasting

```python
image = np.ones((224, 224, 3), dtype=np.float32)
channel_scale = np.array([0.5, 1.0, 2.0])

scaled = image * channel_scale
```

`channel_scale` diperlakukan seolah shape `(1,1,3)`.

Contoh incompatible:

```text
(4, 3)
(4,)
```

Trailing dimension 3 dan 4 tidak compatible.

## Menambah Axis

```python
batch = np.zeros((8, 224, 224, 3))
mean = batch.mean(axis=(0, 1, 2), keepdims=True)
centered = batch - mean
```

`keepdims=True` menghasilkan shape `(1,1,1,3)`.

## Vectorization

Loop:

```python
result = []
for value in values:
    result.append(max(0.0, value))
```

Vectorized:

```python
result = np.maximum(values, 0.0)
```

## Universal Function

Ufunc bekerja element-wise:

```python
np.add
np.multiply
np.exp
np.log
np.sqrt
np.maximum
```

Ufunc mendukung broadcasting dan sering menyediakan parameter seperti `out` atau `where`.

## Implementasi: Standardization

$$
z=\frac{x-\mu}{\sigma}
$$

```python
def standardize_features(
    features: np.ndarray,
) -> np.ndarray:
    mean = features.mean(axis=0, keepdims=True)
    std = features.std(axis=0, keepdims=True)

    if np.any(std == 0):
        raise ValueError("Constant feature detected")

    return (features - mean) / std
```

Dalam ML, mean dan std harus dihitung dari training set saja.

## Matrix Operation

```python
inputs = np.random.default_rng(42).normal(
    size=(32, 128)
)
weights = np.random.default_rng(43).normal(
    size=(128, 10)
)

logits = inputs @ weights
```

## Random Generator

Gunakan explicit generator:

```python
rng = np.random.default_rng(42)
samples = rng.normal(size=(1000, 3))
```

Meneruskan generator secara eksplisit mengurangi hidden global state.

## Studi Kasus: Per-Channel Normalization

```python
images = np.asarray(images, dtype=np.float32) / 255.0

mean = images.mean(axis=(0, 1, 2), keepdims=True)
std = images.std(axis=(0, 1, 2), keepdims=True)
normalized = (images - mean) / np.maximum(std, 1e-6)
```

Jangan menghitung statistic dari validation atau test.

## Performance dan Memory

Vectorization tidak otomatis selalu hemat:

```python
distance = np.sqrt(((a[:, None, :] - b[None, :, :]) ** 2).sum(-1))
```

Intermediate broadcast dapat sangat besar. Gunakan batching, specialized routine, atau profiling.

## Best Practice

- Tulis shape di setiap operand.
- Gunakan trailing-axis rule.
- Gunakan `keepdims=True` untuk statistic.
- Profile runtime dan peak memory.
- Hindari temporary array besar.
- Gunakan explicit RNG.
- Uji hasil vectorized melawan loop kecil.

## Kesalahan Umum

- Broadcasting berhasil tetapi pada axis yang salah.
- Menambah axis sembarangan hingga kode “jalan”.
- Menganggap vectorization selalu lebih hemat.
- Menghitung preprocessing statistic dari seluruh dataset.
- Menggunakan global random state tersembunyi.

## Debugging

```python
print(left.shape, right.shape)
print(np.broadcast_shapes(left.shape, right.shape))
```

Bandingkan implementation vectorized dan loop:

```python
assert np.allclose(vectorized, reference)
```

## Ringkasan

- Broadcasting membandingkan trailing axes.
- Vectorization memindahkan loop ke compiled operations.
- Shape yang compatible belum tentu semantically benar.
- Memory intermediate harus dipantau.

## Checklist Pemahaman

- [ ] Bisa menentukan dua shape compatible.
- [ ] Bisa menambah axis dengan benar.
- [ ] Bisa menulis per-channel normalization.
- [ ] Bisa memakai ufunc.
- [ ] Bisa menggunakan explicit RNG.
- [ ] Bisa mengenali memory explosion.

## Latihan

1. Tentukan compatibility `(5,1,3)` dengan `(7,3)`.
2. Vectorize ReLU.
3. Standardize matrix per-column.
4. Bandingkan hasil loop dan vectorized.

## Referensi

- [NumPy Broadcasting](https://numpy.org/doc/stable/user/basics.broadcasting.html)
- [NumPy Quickstart](https://numpy.org/doc/stable/user/quickstart.html)

