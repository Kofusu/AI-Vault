---
type: concept
status: not-started
domain: python
topic: fundamentals
level: foundations
order: 2
created: 2026-07-28
---

# Python Variables

## Tujuan

- Memahami variable sebagai nama yang mereferensikan object.
- Memahami assignment, naming, mutability, dan object identity.
- Menulis nama variable yang jelas untuk project AI.

## Intuisi

Variable bukan kotak yang menyimpan nilai secara fisik. Variable adalah **nama yang menunjuk ke object**.

```python
learning_rate = 0.001
```

```text
learning_rate ──referensi──> object float 0.001
```

## Konsep Dasar

Assignment mengikat nama pada object:

```python
x = 10
y = x
```

Nama `x` dan `y` awalnya mereferensikan object integer yang sama. Karena integer immutable, operasi:

```python
x = x + 1
```

mengikat `x` ke object baru; object yang direferensikan `y` tidak berubah.

## Kenapa Dibutuhkan?

Mental model name → object mencegah bug terkait:

- aliasing
- mutation tidak sengaja
- scope
- object identity
- copy
- state antar-function

## Cara Kerja Namespace

Namespace memetakan nama ke object. Python mencari nama dengan aturan LEGB:

```text
Local
  ↓ jika tidak ada
Enclosing
  ↓
Global
  ↓
Built-in
```

```python
threshold = 0.5  # global


def classify(score: float) -> bool:
    threshold = 0.7  # local
    return score >= threshold
```

Hindari global mutable state karena behavior function menjadi sulit diprediksi.

## Assignment

```python
epochs = 20
model_name = "resnet18"
is_training = True
```

Multiple assignment:

```python
height, width = 224, 224
```

## Dynamic Typing

Python menentukan type saat runtime:

```python
value = 10
value = "ten"
```

Ini valid, tetapi mengganti type tanpa alasan membuat kode sulit dipahami.

## Naming Convention

Gunakan `snake_case`:

```python
batch_size = 32
image_path = "data/cat.jpg"
validation_accuracy = 0.91
```

Constant secara konvensi memakai huruf besar:

```python
DEFAULT_IMAGE_SIZE = 224
```

Nama class memakai `PascalCase`:

```python
class ImageClassifier:
    pass
```

## Reference dan Mutability

```python
labels = ["cat", "dog"]
alias = labels
alias.append("bird")

print(labels)  # ['cat', 'dog', 'bird']
```

`labels` dan `alias` menunjuk list yang sama.

Copy dangkal:

```python
copied_labels = labels.copy()
```

Nested object masih dapat berbagi referensi:

```python
import copy

records = [{"labels": ["cat"]}]
shallow = records.copy()
deep = copy.deepcopy(records)
```

- shallow copy menyalin outer container
- deep copy mencoba menyalin object di dalamnya secara recursive

Deep copy tidak selalu tepat untuk tensor, file handle, model, atau object besar.

> [!warning]
> Assignment pada mutable object tidak otomatis membuat salinan.

## Identity vs Equality

```python
a = [1, 2]
b = [1, 2]

print(a == b)  # nilai sama
print(a is b)  # object berbeda
```

- `==` membandingkan nilai.
- `is` membandingkan identity.
- Gunakan `is None`, bukan `== None`.

## Unpacking

```python
shape = (3, 224, 224)
channels, height, width = shape
```

Unpacking sering dipakai saat membaca tensor shape.

Extended unpacking:

```python
batch_size, *feature_shape = (32, 3, 224, 224)
```

## Implementasi dan Introspection

```python
value = [1, 2, 3]
alias = value

print(type(value))
print(id(value))
print(id(alias))
print(value is alias)
```

`id()` berguna untuk belajar dan diagnosis identity, bukan sebagai identifier bisnis permanen.

## Studi Kasus: Config Training

Buruk:

```python
CONFIG = {"learning_rate": 0.001}


def train() -> None:
    CONFIG["learning_rate"] = 0.1
```

Lebih aman:

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class TrainingConfig:
    learning_rate: float
    batch_size: int
```

Immutable config mengurangi perubahan tersembunyi saat eksperimen.

## Best Practice

- Pilih nama berdasarkan makna, bukan type saja.
- Hindari nama satu huruf kecuali konteks matematis sangat lokal.
- Jangan menimpa built-in seperti `list`, `str`, atau `sum`.
- Gunakan constant untuk nilai konfigurasi tetap.

## Kesalahan Umum

- Mengira assignment membuat copy.
- Memakai `is` untuk membandingkan angka atau string.
- Mengubah type variable tanpa alasan.
- Menggunakan nama ambigu seperti `data`, `tmp`, atau `x` di scope besar.

## Debugging

Jika list berubah “sendiri”:

```python
print(id(original), id(alias))
print(original is alias)
```

Jika nama tidak ditemukan:

- baca `NameError`
- cek typo
- cek scope LEGB
- pastikan branch yang melakukan assignment sudah dijalankan

Jika muncul `UnboundLocalError`, assignment di dalam function membuat nama dianggap lokal kecuali dinyatakan lain. Biasanya desain lebih baik adalah meneruskan value melalui parameter dan return.

## Ringkasan

- Variable adalah nama yang mereferensikan object.
- Dynamic typing fleksibel tetapi tetap perlu disiplin.
- Mutability memengaruhi perilaku alias dan copy.

## Checklist Pemahaman

- [ ] Bisa menjelaskan variable sebagai name binding.
- [ ] Bisa membedakan equality dan identity.
- [ ] Bisa menjelaskan mutable dan immutable assignment.
- [ ] Bisa membedakan shallow dan deep copy.
- [ ] Bisa menjelaskan LEGB.
- [ ] Bisa mendiagnosis aliasing.

## Hubungan Konsep

- Prasyarat: [[Python Fundamentals]]
- Parent: [[Python MOC]]
- Lanjutan: [[Python Data Types]], [[Python Type Hints]]
- Terkait: [[Scalar]], [[Tensor]]

## Latihan

Prediksi output kode alias list di atas, lalu ubah agar `alias` tidak memengaruhi `labels`.

2. Eksperimen dengan nested list, shallow copy, dan deep copy.
3. Buat contoh variable global dan lokal dengan nama sama.
4. Refactor global config menjadi frozen dataclass.
