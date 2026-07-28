---
type: concept
status: not-started
domain: python
topic: modular-python
level: foundations
order: 5
created: 2026-07-28
---

# Python Functions

## Tujuan

- Memecah program menjadi unit yang bisa digunakan ulang dan diuji.
- Memahami parameter, return value, scope, dan default argument.
- Menghindari mutable default argument.

## Intuisi

Function menerima input, melakukan satu tanggung jawab, lalu menghasilkan output.

```text
Input → Function → Output
```

## Konsep Dasar

Function adalah callable object dengan:

- nama
- parameter
- body
- optional return value
- local scope
- optional closure state

## Kenapa Dibutuhkan?

Function mengurangi duplikasi, memberi nama pada operasi, memisahkan tanggung jawab, dan membuat logic dapat diuji.

## Cara Kerja Pemanggilan

```text
Caller
  ↓ bind arguments ke parameters
Execution frame baru
  ↓ jalankan body
Return value atau exception
  ↓
Caller melanjutkan
```

Setiap call mempunyai local namespace sendiri.

## Definisi Function

```python
def normalize_score(score: float, maximum: float) -> float:
    return score / maximum
```

## Parameter dan Argument

```python
def resize_shape(
    height: int,
    width: int,
    scale: float = 0.5,
) -> tuple[int, int]:
    return int(height * scale), int(width * scale)
```

- Parameter didefinisikan oleh function.
- Argument diberikan saat function dipanggil.
- Keyword argument meningkatkan keterbacaan.

```python
new_shape = resize_shape(height=1080, width=1920, scale=0.25)
```

Positional-only dan keyword-only parameter dapat memperjelas API:

```python
def resize(
    image,
    /,
    *,
    width: int,
    height: int,
):
    ...
```

## `*args` dan `**kwargs`

```python
def report(*metrics: float, **metadata: str) -> None:
    print(metrics, metadata)
```

Gunakan saat API memang membutuhkan jumlah argument fleksibel. Jangan memakainya untuk menyembunyikan signature yang seharusnya eksplisit.

## Return Value

Function tanpa `return` eksplisit menghasilkan `None`.

```python
def split_shape(shape: tuple[int, int, int]) -> tuple[int, int, int]:
    channels, height, width = shape
    return channels, height, width
```

## Scope

Variable lokal hanya tersedia di dalam function:

```python
def calculate_loss() -> float:
    loss = 0.42
    return loss
```

Hindari global state karena menyulitkan testing dan reproduksi.

## Mutable Default Argument

Buruk:

```python
def add_label(label: str, labels: list[str] = []) -> list[str]:
    labels.append(label)
    return labels
```

Benar:

```python
def add_label(
    label: str,
    labels: list[str] | None = None,
) -> list[str]:
    if labels is None:
        labels = []

    labels.append(label)
    return labels
```

Default argument dievaluasi satu kali saat function didefinisikan.

## Function sebagai Object

```python
def relu(value: float) -> float:
    return max(0.0, value)


activation = relu
print(activation(-2.0))
```

Konsep ini dipakai pada callback, transform pipeline, dan activation selection.

## Closure

Inner function dapat mengingat value dari enclosing scope:

```python
def make_threshold_filter(threshold: float):
    def keep(score: float) -> bool:
        return score >= threshold

    return keep


keep_high = make_threshold_filter(0.8)
```

Closure berguna untuk factory sederhana, tetapi hidden state berlebihan dapat membingungkan.

## Decorator

Decorator membungkus callable:

```python
from functools import wraps


def trace(function):
    @wraps(function)
    def wrapper(*args, **kwargs):
        print(f"Calling {function.__name__}")
        return function(*args, **kwargs)

    return wrapper
```

Framework memakai decorator untuk registration, caching, dan context behavior.

## Lambda

```python
sorted_scores = sorted(
    [("cat", 0.8), ("dog", 0.4)],
    key=lambda item: item[1],
    reverse=True,
)
```

Gunakan lambda hanya untuk expression pendek.

## Pure Function

Pure function:

- output ditentukan input
- tidak mengubah external state
- tidak memiliki side effect tersembunyi

Pure function lebih mudah diuji dan direproduksi.

## Implementasi: Preprocessing Pipeline

```python
from collections.abc import Callable, Iterable

Transform = Callable[[object], object]


def compose(
    transforms: Iterable[Transform],
) -> Transform:
    def apply(value: object) -> object:
        for transform in transforms:
            value = transform(value)
        return value

    return apply
```

## Studi Kasus: Dataset Split

Function split yang baik:

- menerima input dan seed eksplisit
- tidak membaca global state
- memvalidasi ratio
- mengembalikan hasil
- tidak mengubah input tanpa dokumentasi

Ini membuat split reproducible dan testable.

## Best Practice Tambahan

- Pertahankan abstraction level yang konsisten.
- Validasi di boundary.
- Gunakan keyword-only argument untuk option penting.
- Hindari boolean flag berlebihan; kadang dua function lebih jelas.
- Jangan return banyak nilai tanpa struktur yang jelas.

## Best Practice

- Satu function, satu tanggung jawab.
- Nama function berupa kata kerja yang jelas.
- Gunakan type hints dan docstring untuk public API.
- Hindari function terlalu panjang.
- Return data, jangan hanya mencetaknya.

## Kesalahan Umum

- Mutable default argument.
- Terlalu banyak parameter.
- Side effect tersembunyi.
- Mengandalkan global variable.
- Menangkap dan mengubah data tanpa return yang jelas.

## Debugging

- Gunakan traceback untuk melihat call stack.
- Print `function.__name__` dan signature jika callback salah.
- Uji function dengan input kecil.
- Cek apakah function mengubah mutable input.
- Jika closure memakai nilai loop yang salah, pahami late binding.

## Checklist Pemahaman

- [ ] Bisa membedakan parameter dan argument.
- [ ] Bisa menjelaskan call frame dan local scope.
- [ ] Bisa menghindari mutable default.
- [ ] Bisa membuat pure function.
- [ ] Bisa memakai callable sebagai argument.
- [ ] Bisa menjelaskan closure dan decorator dasar.

## Ringkasan

- Function membuat kode modular dan testable.
- Parameter adalah input; `return` adalah output.
- Scope dan mutability memengaruhi side effect.

## Hubungan Konsep

- Prasyarat: [[Python Control Flow]]
- Parent: [[Python MOC]]
- Lanjutan: [[Object-Oriented Programming in Python]], [[Python Type Hints]]
- Digunakan di: [[Training Pipeline]], [[Data Preprocessing]]

## Latihan

Buat function `is_valid_image()` yang menerima path dan mengembalikan `bool` berdasarkan extension.

2. Tambahkan keyword-only argument `allowed_extensions`.
3. Tulis unit test sederhana untuk function tersebut.
4. Buat closure pembuat confidence filter.
