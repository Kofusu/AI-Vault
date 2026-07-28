---
type: concept
status: not-started
domain: python
topic: modular-python
level: foundations
order: 6
created: 2026-07-28
---

# Object-Oriented Programming in Python

## Tujuan

- Memahami class, object, attribute, dan method.
- Mengenal encapsulation, inheritance, composition, dan polymorphism.
- Memahami pola OOP pada PyTorch.

## Intuisi

Class adalah blueprint. Object adalah instance nyata yang dibuat dari blueprint tersebut.

```text
Class ImageClassifier
    ↓ instance
model = ImageClassifier(...)
```

## Konsep Dasar

- Class mendefinisikan structure dan behavior.
- Instance mempunyai identity serta state.
- Attribute menyimpan data.
- Method adalah function yang di-bind ke object.
- Interface menjelaskan behavior yang dapat digunakan caller.

## Kenapa Dibutuhkan?

OOP berguna ketika beberapa operasi berbagi state dan lifecycle, misalnya:

- model dengan parameter
- dataset dengan index dan transform
- tracker dengan state antar-frame
- client API dengan connection config

## Cara Kerja Method Binding

Saat:

```python
sample.describe()
```

Python mengikat `sample` sebagai argument `self` ke function class. Secara konseptual mirip:

```python
ImageSample.describe(sample)
```

## Class dan Object

```python
class ImageSample:
    def __init__(self, path: str, label: str) -> None:
        self.path = path
        self.label = label

    def describe(self) -> str:
        return f"{self.path}: {self.label}"


sample = ImageSample("cat.jpg", "cat")
print(sample.describe())
```

- `self` merujuk ke instance.
- `__init__` menginisialisasi state object.
- Attribute menyimpan state.
- Method mendefinisikan behavior.

## Encapsulation

Gabungkan state dan behavior yang memang saling terkait. Python memakai konvensi underscore untuk implementation detail:

```python
self._cache = {}
```

Ini bukan security boundary, melainkan sinyal bagi developer.

Property dapat menjaga interface:

```python
class BoundingBox:
    def __init__(self, width: float, height: float) -> None:
        if width < 0 or height < 0:
            raise ValueError("Size cannot be negative")

        self._width = width
        self._height = height

    @property
    def area(self) -> float:
        return self._width * self._height
```

## Inheritance

```python
class Dataset:
    def __len__(self) -> int:
        raise NotImplementedError


class ImageDataset(Dataset):
    def __init__(self, paths: list[str]) -> None:
        self.paths = paths

    def __len__(self) -> int:
        return len(self.paths)
```

Inheritance cocok untuk hubungan “is-a”, tetapi jangan dipaksakan.

## Composition

```python
class Pipeline:
    def __init__(self, model, preprocessor) -> None:
        self.model = model
        self.preprocessor = preprocessor
```

Composition sering lebih fleksibel: `Pipeline` **memiliki** model dan preprocessor.

> [!tip]
> Prefer composition ketika hubungan antarbagian mudah diganti atau diuji secara terpisah.

## Polymorphism

Beberapa object dapat menyediakan interface yang sama:

```python
def run_inference(model, image):
    return model.predict(image)
```

Function tidak perlu mengetahui class konkret selama object mendukung method yang dibutuhkan.

## OOP dalam PyTorch

```python
import torch
from torch import nn


class TinyClassifier(nn.Module):
    def __init__(self, input_features: int, num_classes: int) -> None:
        super().__init__()
        self.classifier = nn.Linear(input_features, num_classes)

    def forward(self, inputs: torch.Tensor) -> torch.Tensor:
        return self.classifier(inputs)
```

PyTorch memakai inheritance dari `nn.Module` untuk mendaftarkan parameter dan submodule.

## Implementasi: Custom Dataset

```python
from pathlib import Path
from torch.utils.data import Dataset


class ImagePathDataset(Dataset[Path]):
    def __init__(self, paths: list[Path]) -> None:
        self.paths = paths

    def __len__(self) -> int:
        return len(self.paths)

    def __getitem__(self, index: int) -> Path:
        return self.paths[index]
```

Contract `Dataset` memungkinkan `DataLoader` berinteraksi tanpa mengetahui implementation detail.

## Dunder Method

Method khusus seperti:

- `__repr__`
- `__len__`
- `__getitem__`
- `__iter__`
- `__enter__` dan `__exit__`

membuat object terintegrasi dengan Python protocol. Implementasikan hanya jika semantics-nya jelas.

## Studi Kasus: Vision Pipeline

```text
VisionPipeline
├── preprocessor
├── predictor
├── postprocessor
└── logger
```

Composition memungkinkan setiap komponen diganti saat testing. Pipeline tidak perlu mewarisi semua component.

## Design Principle

- High cohesion: satu class punya tanggung jawab yang berkaitan.
- Low coupling: dependency eksplisit dan mudah diganti.
- Prefer composition over deep inheritance.
- Depend on interface, bukan implementation detail.

## Best Practice

- Gunakan class jika state dan behavior memang menyatu.
- Gunakan dataclass untuk record data.
- Jaga constructor tetap ringan.
- Hindari side effect tersembunyi pada property.
- Buat dependency eksplisit melalui constructor.
- Tulis `__repr__` yang membantu diagnosis bila perlu.

## Kapan Tidak Perlu OOP?

Gunakan function biasa jika:

- logic sederhana
- tidak ada state
- composition object tidak memberi manfaat

OOP bukan tanda kode otomatis lebih baik.

## Kesalahan Umum

- Membuat class untuk function sederhana.
- Inheritance terlalu dalam.
- God class yang mengerjakan semuanya.
- State tersembunyi yang sulit dites.
- Lupa memanggil `super().__init__()` pada subclass PyTorch.

## Debugging

- `AttributeError`: cek initialization path dan typo attribute.
- Parameter PyTorch hilang: pastikan disimpan sebagai attribute `nn.Module` dan `super().__init__()` dipanggil.
- Shared state tak terduga: cek class attribute vs instance attribute.
- Infinite recursion pada property: gunakan backing attribute seperti `_value`.
- Sulit dites: pecah god class dan inject dependency.

## Checklist Pemahaman

- [ ] Bisa membedakan class dan instance.
- [ ] Bisa menjelaskan method binding.
- [ ] Bisa memilih composition atau inheritance.
- [ ] Bisa membuat dataclass dan custom dataset.
- [ ] Bisa menjelaskan protocol dari dunder method.
- [ ] Bisa mendiagnosis parameter PyTorch yang tidak terdaftar.

## Ringkasan

- Class menyatukan state dan behavior.
- Composition sering lebih fleksibel daripada inheritance.
- PyTorch model adalah object turunan `nn.Module`.
- Gunakan OOP hanya jika membantu desain.

## Hubungan Konsep

- Prasyarat: [[Python Functions]], [[Python Data Types]]
- Parent: [[Python MOC]]
- Lanjutan: [[Python Exception Handling]], [[Python Type Hints]]
- Digunakan di: [[PyTorch]], [[Dataset]], [[Neural Network]]

## Latihan

Buat class `ImageMetadata` dengan attribute path, width, height, serta method untuk menghitung aspect ratio.

2. Ubah menjadi frozen dataclass.
3. Buat `ImagePathDataset`.
4. Desain pipeline dengan preprocessor dan predictor yang dapat diganti.
