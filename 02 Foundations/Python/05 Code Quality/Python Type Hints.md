---
type: concept
status: not-started
domain: python
topic: code-quality
level: foundations
order: 11
created: 2026-07-28
---

# Python Type Hints

## Tujuan

- Mendokumentasikan bentuk data yang diharapkan.
- Membantu editor dan static type checker menemukan bug.
- Menulis interface yang jelas untuk pipeline AI.

## Intuisi

Type hints adalah kontrak komunikasi bagi manusia dan tools:

```python
def normalize(value: float, maximum: float) -> float:
    return value / maximum
```

Python tetap dynamically typed. Type hints umumnya tidak memaksa type saat runtime.

## Konsep Dasar

Type annotation menjadi metadata yang dapat dibaca:

- manusia
- editor
- static analyzer
- documentation generator
- runtime validation library jika sengaja digunakan

## Kenapa Dibutuhkan?

Project AI sering mempunyai contract kompleks: tensor, path, config, prediction, annotation, dan callback. Type hints menangkap mismatch lebih awal dan membuat refactor lebih aman.

## Cara Kerja Static Analysis

```text
Source + annotations
        ↓
Static type checker
        ↓
Possible mismatch report
```

Program tidak perlu dijalankan. Karena analysis tidak mengetahui seluruh runtime behavior, hasilnya membantu tetapi tidak membuktikan program benar.

## Built-in Generics

```python
def count_labels(labels: list[str]) -> dict[str, int]:
    counts: dict[str, int] = {}

    for label in labels:
        counts[label] = counts.get(label, 0) + 1

    return counts
```

## Optional Value

```python
def find_checkpoint(name: str) -> str | None:
    ...
```

`str | None` berarti function dapat menghasilkan string atau `None`.

## Type Alias

```python
from pathlib import Path

ImagePath = Path
ImageSize = tuple[int, int]
```

Alias memperjelas intent, tetapi tidak membuat type baru yang benar-benar berbeda.

## Callable

```python
from collections.abc import Callable

Transform = Callable[[float], float]
```

## Protocol

Protocol mendefinisikan behavior yang dibutuhkan tanpa inheritance langsung:

```python
from typing import Protocol


class Predictor(Protocol):
    def predict(self, image: object) -> list[float]:
        ...
```

Ini mendukung structural typing atau duck typing yang dapat diperiksa.

## Dataclass

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class TrainingConfig:
    batch_size: int
    learning_rate: float
    epochs: int
```

Dataclass cocok untuk object yang terutama menyimpan data.

## Generic Type

```python
from typing import TypeVar

T = TypeVar("T")


def first(items: list[T]) -> T:
    if not items:
        raise ValueError("items cannot be empty")
    return items[0]
```

Generic mempertahankan hubungan type input dan output.

## Tensor Shape

Type hint seperti `torch.Tensor` belum otomatis menjelaskan shape atau dtype:

```python
def predict(images: "torch.Tensor") -> "torch.Tensor":
    """Expected input shape: (N, C, H, W)."""
```

Shape contract tetap perlu dokumentasi, validation, atau tooling tambahan.

## Static Type Checker

Tools seperti mypy atau pyright menganalisis type tanpa menjalankan program.

Type checker tidak menggantikan unit test atau runtime validation.

## Implementasi: Prediction API

```python
from dataclasses import dataclass
from pathlib import Path


@dataclass(frozen=True)
class Prediction:
    label: str
    confidence: float


def predict_image(path: Path) -> list[Prediction]:
    ...
```

Interface menjelaskan bentuk data jauh lebih baik daripada `dict` tanpa schema.

## Studi Kasus: Tensor Contract

```python
def classify(images: "torch.Tensor") -> "torch.Tensor":
    """Classify a batch.

    Args:
        images: Float tensor shaped (N, C, H, W),
            normalized with the training convention.

    Returns:
        Logits shaped (N, num_classes).
    """
```

Annotation `Tensor` perlu dilengkapi semantic contract.

## Best Practice

- Type public function dan interface penting.
- Hindari `Any` jika type sebenarnya diketahui.
- Gunakan type yang spesifik tetapi tidak terlalu kaku.
- Dokumentasikan tensor shape, dtype, dan device jika relevan.
- Gunakan runtime validation untuk input eksternal.

## Kesalahan Umum

- Mengira type hints memvalidasi runtime secara otomatis.
- Menggunakan type terlalu umum.
- Menulis annotation kompleks yang lebih sulit dibaca daripada manfaatnya.
- Menganggap `Tensor` sudah menjelaskan seluruh contract.

## Debugging

- Jalankan type checker secara konsisten.
- Baca inferred type dari editor.
- Hindari langsung membungkam error dengan `Any` atau ignore.
- Periksa apakah library mempunyai type stubs.
- Jika input eksternal, tambahkan runtime validation.
- Jika type terlalu kompleks, sederhanakan API.

## Checklist Pemahaman

- [ ] Bisa menjelaskan type hint tidak otomatis memvalidasi runtime.
- [ ] Bisa menulis annotation collection dan optional value.
- [ ] Bisa memakai dataclass.
- [ ] Bisa menjelaskan Protocol dan Generic dasar.
- [ ] Bisa mendokumentasikan tensor contract.
- [ ] Bisa membaca laporan static checker.

## Ringkasan

- Type hints meningkatkan keterbacaan dan tooling.
- Python tetap dynamically typed.
- Type checker, test, dan runtime validation menyelesaikan masalah berbeda.

## Hubungan Konsep

- Prasyarat: [[Python Functions]], [[Object-Oriented Programming in Python]]
- Parent: [[Python MOC]]
- Lanjutan: [[Python Logging]]
- Terkait: [[Tensor]], [[Clean Code]], [[Testing]]

## Latihan

Tambahkan type hints ke function yang menerima daftar image path dan mengembalikan dictionary jumlah file per extension.

2. Buat dataclass `BoundingBox`.
3. Buat Protocol untuk object dengan method `predict`.
4. Jalankan static checker dan perbaiki satu mismatch sengaja.
