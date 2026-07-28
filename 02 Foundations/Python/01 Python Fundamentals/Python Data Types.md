---
type: concept
status: not-started
domain: python
topic: fundamentals
level: foundations
order: 3
created: 2026-07-28
---

# Python Data Types

## Tujuan

- Mengenal built-in type utama Python.
- Membedakan mutable dan immutable object.
- Memilih struktur data yang sesuai.

## Intuisi

Type menentukan:

- nilai seperti apa yang direpresentasikan
- operasi yang valid
- apakah object dapat diubah
- bagaimana object dibandingkan
- apakah object dapat menjadi dictionary key atau set member

## Konsep Dasar

Python memakai object model: hampir semua nilai adalah object dengan type dan identity.

```python
value = 42

print(type(value))
print(isinstance(value, int))
```

## Kenapa Dibutuhkan?

Pemilihan type yang tepat meningkatkan:

- kejelasan data model
- validasi
- performa lookup
- keamanan mutation
- kualitas type hints
- interoperability dengan library

## Cara Kerja Protocol

Operasi Python memanggil protocol object:

```python
len(value)      # membutuhkan sized protocol
for item in x   # membutuhkan iterable protocol
x[key]          # membutuhkan subscription
```

Duck typing berfokus pada behavior yang tersedia, bukan hanya nama class.

## Scalar-like Types

```python
epochs = 10              # int
learning_rate = 0.001    # float
is_training = True       # bool
model_name = "resnet18"  # str
value = 2 + 3j           # complex
```

### Numeric Precision

`float` Python umumnya mengikuti double-precision binary floating-point:

```python
print(0.1 + 0.2)  # tidak persis 0.3
```

Gunakan tolerance:

```python
import math

assert math.isclose(0.1 + 0.2, 0.3)
```

Ini berbeda dari dtype NumPy atau PyTorch seperti `float32`.

## Collection Types

### List

Berurutan, mutable, dan boleh berisi duplicate:

```python
class_names = ["cat", "dog", "bird"]
class_names.append("horse")
```

### Tuple

Berurutan dan immutable:

```python
image_size = (224, 224)
```

Tuple cocok untuk data yang secara konsep tidak berubah, seperti shape atau koordinat tetap.

### Dictionary

Pasangan key–value:

```python
config = {
    "batch_size": 32,
    "learning_rate": 0.001,
}
```

### Set

Kumpulan elemen unik:

```python
extensions = {".jpg", ".jpeg", ".png"}
```

## `None`

`None` menyatakan tidak ada nilai:

```python
checkpoint_path = None

if checkpoint_path is None:
    print("Training from scratch")
```

## Mutable vs Immutable

Immutable:

- `int`
- `float`
- `bool`
- `str`
- `tuple`

Mutable:

- `list`
- `dict`
- `set`

Mutability penting saat object diteruskan ke function.

## Hashability

Dictionary key dan set member harus hashable. Immutable object sering hashable:

```python
coordinates = {(10, 20): "object-center"}
```

List tidak dapat menjadi key karena mutable.

## Type Conversion

```python
class_id = int("3")
threshold = float("0.5")
label = str(7)
```

Conversion dapat gagal dan menghasilkan exception.

## Slicing

```python
scores = [0.1, 0.8, 0.3, 0.9]

print(scores[1:3])
print(scores[::-1])
```

## Comprehension

```python
valid_scores = [score for score in scores if score >= 0.5]
```

Gunakan comprehension untuk transformasi sederhana. Untuk logic kompleks, gunakan loop biasa.

## Implementasi: Data Record

```python
from dataclasses import dataclass
from pathlib import Path


@dataclass(frozen=True)
class ImageRecord:
    path: Path
    label: str
    size: tuple[int, int]
```

Data yang memiliki schema jelas lebih baik daripada dictionary bebas untuk core domain object.

## Studi Kasus: Annotation Detection

```python
annotation = {
    "image_id": 10,
    "boxes": [[12.0, 8.0, 50.0, 40.0]],
    "labels": [3],
    "metadata": None,
}
```

Tentukan contract:

- format box
- type angka
- arti class ID
- apakah collection boleh kosong
- kapan `None` valid

Type saja tidak cukup; semantics tetap harus didokumentasikan.

## Best Practice

- Gunakan `list` untuk urutan mutable.
- Gunakan `tuple` untuk record kecil yang tetap atau gunakan dataclass jika field perlu nama.
- Gunakan `set` untuk uniqueness dan membership.
- Gunakan `dict` untuk mapping.
- Jangan mencampur banyak type tanpa contract jelas.
- Gunakan `isinstance()` jika runtime check memang dibutuhkan.

## Memilih Struktur Data

```text
Butuh urutan dan perubahan?       → list
Butuh urutan yang tetap?          → tuple
Butuh lookup berdasarkan key?     → dict
Butuh elemen unik?                → set
Tidak ada nilai?                  → None
```

## Kesalahan Umum

- Mengakses key dictionary yang belum ada tanpa fallback.
- Mengubah collection saat sedang diiterasi.
- Menggunakan list untuk membership test besar ketika set lebih tepat.
- Mengira tuple selalu membuat isi di dalamnya immutable.

## Debugging

```python
print(type(value))
print(repr(value))
print(isinstance(value, expected_type))
```

Untuk `KeyError`, periksa key yang tersedia:

```python
print(mapping.keys())
```

Untuk floating-point comparison, gunakan tolerance. Untuk mutation error, cek apakah object immutable atau shared.

## Ringkasan

- Type menentukan operasi yang tersedia pada object.
- Pemilihan collection memengaruhi kejelasan dan performa.
- Mutability perlu dipahami untuk mencegah side effect.

## Checklist Pemahaman

- [ ] Bisa memilih list, tuple, dict, atau set.
- [ ] Bisa menjelaskan mutable, immutable, dan hashable.
- [ ] Bisa menjelaskan floating-point approximation.
- [ ] Bisa menggunakan slicing dan comprehension.
- [ ] Bisa mendesain data record sederhana.
- [ ] Bisa mendiagnosis type dan key error.

## Hubungan Konsep

- Prasyarat: [[Python Variables]]
- Parent: [[Python MOC]]
- Lanjutan: [[Python Control Flow]], [[Python Functions]]
- Terkait: [[Python Type Hints]]

## Latihan

Pilih type yang tepat untuk class names, image shape, model config, dan kumpulan extension unik.

2. Kenapa tuple yang berisi list tidak sepenuhnya immutable?
3. Buat `ImageRecord` dan simpan tiga instance dalam list.
4. Bandingkan membership test list dan set pada data besar.
