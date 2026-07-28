---
type: concept
status: not-started
domain: python
topic: fundamentals
level: foundations
order: 4
created: 2026-07-28
---

# Python Control Flow

## Tujuan

- Mengontrol alur program dengan condition dan loop.
- Menggunakan iteration secara aman dan terbaca.
- Mengenal pattern yang umum dalam data pipeline.

## Intuisi

Control flow menentukan **kode mana** yang dijalankan dan **berapa kali**:

```text
Sequence  → langkah berurutan
Branch    → pilih jalur
Loop      → ulangi langkah
Exit      → hentikan atau kembalikan hasil
```

## Konsep Dasar

Python mengevaluasi expression boolean untuk memilih branch. Loop bekerja di atas iterable, bukan hanya collection.

## Kenapa Dibutuhkan?

Pipeline AI perlu:

- memvalidasi input
- melewati sample rusak
- mengulang batch dan epoch
- memilih behavior berdasarkan mode
- berhenti saat kondisi terpenuhi

## Cara Kerja Boolean

Operator:

```python
and
or
not
```

`and` dan `or` memakai short-circuit evaluation:

```python
if image is not None and image.size > 0:
    process(image)
```

Bagian kedua tidak dievaluasi jika bagian pertama false.

## Conditional

```python
confidence = 0.82

if confidence >= 0.9:
    label = "high"
elif confidence >= 0.5:
    label = "medium"
else:
    label = "low"
```

## Truthiness

Nilai seperti `None`, `False`, `0`, dan collection kosong dianggap false:

```python
predictions = []

if not predictions:
    print("No predictions")
```

Gunakan perbandingan eksplisit jika `0` dan `None` punya makna berbeda.

## `for` Loop

```python
image_paths = ["a.jpg", "b.jpg", "c.jpg"]

for image_path in image_paths:
    print(image_path)
```

Dengan index:

```python
for index, image_path in enumerate(image_paths):
    print(index, image_path)
```

Iterasi paralel:

```python
labels = ["cat", "dog", "bird"]

for image_path, label in zip(image_paths, labels):
    print(image_path, label)
```

`zip()` berhenti pada iterable terpendek. Jika panjang harus sama, validasi atau gunakan strict mode pada versi Python yang mendukungnya.

## Iterable dan Iterator

Iterable dapat menghasilkan iterator. Iterator menyimpan state traversal dan menghasilkan item satu per satu:

```python
iterator = iter([10, 20, 30])

print(next(iterator))
print(next(iterator))
```

Setelah habis, `next()` menghasilkan `StopIteration`. `for` loop menangani detail ini.

## `while` Loop

```python
epoch = 0

while epoch < 5:
    print(epoch)
    epoch += 1
```

Pastikan kondisi dapat berubah agar tidak terjadi infinite loop.

## `break`, `continue`, dan `else`

```python
for score in [0.1, 0.4, 0.95]:
    if score < 0:
        continue
    if score >= 0.9:
        print("Target found")
        break
```

## Pattern Matching

Pada Python modern:

```python
task = "classification"

match task:
    case "classification":
        print("Use class labels")
    case "detection":
        print("Use bounding boxes")
    case _:
        raise ValueError(f"Unknown task: {task}")
```

## Loop dalam Training

```python
for epoch in range(num_epochs):
    for images, targets in train_loader:
        predictions = model(images)
        loss = criterion(predictions, targets)
```

## Implementasi: Filter Prediction

```python
def select_predictions(
    labels: list[str],
    scores: list[float],
    threshold: float,
) -> list[tuple[str, float]]:
    selected: list[tuple[str, float]] = []

    for label, score in zip(labels, scores, strict=True):
        if not 0.0 <= score <= 1.0:
            raise ValueError(f"Invalid score: {score}")

        if score < threshold:
            continue

        selected.append((label, score))

    return selected
```

## Studi Kasus: Dataset Iteration

```text
For each sample
  ├── file hilang? → log lalu continue
  ├── label invalid? → fail fast
  ├── preprocessing
  └── yield sample
```

Tidak semua error harus dilewati. Bedakan corrupt sample yang sudah diputuskan boleh diskip dari bug schema yang seharusnya menghentikan pipeline.

## Kompleksitas

Nested loop sering mengalikan jumlah operasi:

```text
Loop n item              → O(n)
Loop n × loop m          → O(nm)
Membership list di loop  → bisa O(n²)
Membership set di loop   → rata-rata O(n)
```

## Best Practice

- Hindari nesting terlalu dalam.
- Pecah logic kompleks menjadi function.
- Gunakan early return untuk guard condition.
- Gunakan `enumerate()` dan `zip()` daripada index manual.
- Jangan memakai `while True` tanpa exit condition yang jelas.

## Kesalahan Umum

- Off-by-one pada `range()`.
- Mengubah list saat sedang diiterasi.
- Infinite loop.
- Menggunakan `if value` ketika harus membedakan `0` dan `None`.
- Menulis conditional berulang yang seharusnya menjadi mapping dictionary.

## Debugging

- Print atau log branch yang dipilih.
- Gunakan breakpoint dan inspect loop variable.
- Cek batas `range(start, stop, step)` karena `stop` tidak ikut.
- Cek panjang iterable sebelum `zip`.
- Jika loop lambat, ukur kompleksitas dan profiling.
- Jika generator hanya berjalan sekali, ingat iterator dapat habis.

## Checklist Pemahaman

- [ ] Bisa menjelaskan truthiness dan short-circuit.
- [ ] Bisa memakai `enumerate()` dan `zip()`.
- [ ] Bisa membedakan iterable dan iterator.
- [ ] Bisa menggunakan `break`, `continue`, dan early return.
- [ ] Bisa memperkirakan kompleksitas loop.
- [ ] Bisa menentukan kapan error harus diskip atau dihentikan.

## Ringkasan

- Conditional memilih jalur eksekusi.
- Loop mengulang operasi.
- Control flow yang dangkal dan eksplisit lebih mudah diuji.

## Hubungan Konsep

- Prasyarat: [[Python Data Types]]
- Parent: [[Python MOC]]
- Lanjutan: [[Python Functions]], [[Python Exception Handling]]

## Latihan

Buat loop yang hanya memproses file berekstensi `.jpg`, `.jpeg`, atau `.png`.

2. Buat generator yang menghasilkan path gambar valid satu per satu.
3. Demonstrasikan short-circuit yang mencegah `AttributeError`.
4. Refactor nested conditional menjadi guard clause.
