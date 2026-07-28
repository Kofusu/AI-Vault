---
type: concept
status: learning
domain: python
topic: fundamentals
level: foundations
order: 1
created: 2026-07-28
---

# Python Fundamentals

## Tujuan

- Memahami cara Python mengeksekusi program.
- Bisa menjalankan script dan memakai interactive interpreter.
- Mengenal expression, statement, indentation, module, dan import.

## Intuisi

Python adalah bahasa pemrograman general-purpose yang menekankan keterbacaan. Di AI, Python berfungsi sebagai “lem” yang menghubungkan library numerik, GPU framework, dataset, eksperimen, dan deployment.

```text
Kode Python
    ↓
Python interpreter
    ↓
Library AI/CV
    ↓
CPU atau GPU
```

## Konsep Dasar

Python adalah:

- high-level: banyak detail mesin diabstraksikan
- dynamically typed: type object ditentukan saat runtime
- garbage-collected: memory object dikelola runtime
- multi-paradigm: procedural, object-oriented, dan functional style
- interpreted dalam penggunaan sehari-hari, walaupun implementation seperti CPython mengompilasi source menjadi bytecode terlebih dahulu

## Cara Kerja Eksekusi

Pada CPython:

```text
Source code .py
      ↓ parsing
Abstract Syntax Tree
      ↓ compilation
Python bytecode
      ↓ Python Virtual Machine
Program berjalan
```

Ini berbeda dari virtual environment. Python Virtual Machine menjalankan bytecode; [[Python Virtual Environment]] mengisolasi dependency.

## Interpreter dan Runtime

Interpreter menangani:

- execution frame dan call stack
- namespace
- object model
- exception
- memory management
- import system

Operasi NumPy atau PyTorch sering berpindah ke compiled native code sehingga loop berat tidak selalu dijalankan oleh interpreter Python.

## Kenapa Python Dipakai di AI?

- Syntax relatif sederhana.
- Ekosistem besar: NumPy, PyTorch, OpenCV, Hugging Face.
- Cocok untuk eksperimen cepat.
- Bisa memanggil implementation berperforma tinggi dari C, C++, atau CUDA.

> [!note]
> Python sendiri tidak selalu cepat. Banyak library AI cepat karena operasi beratnya dijalankan oleh native code atau GPU.

## Menjalankan Python

Interactive interpreter:

```bash
python
```

Script:

```bash
python main.py
```

Contoh `main.py`:

```python
print("Hello, Computer Vision!")
```

Periksa interpreter:

```bash
python -c "import sys; print(sys.executable)"
python --version
```

## Implementasi Program Kecil

```python
from pathlib import Path


def count_images(directory: Path) -> int:
    extensions = {".jpg", ".jpeg", ".png"}
    return sum(
        path.suffix.lower() in extensions
        for path in directory.rglob("*")
    )


def main() -> None:
    dataset_root = Path("data")
    print(f"Images: {count_images(dataset_root)}")


if __name__ == "__main__":
    main()
```

## Expression dan Statement

Expression menghasilkan nilai:

```python
2 + 3
len("image")
```

Statement melakukan aksi:

```python
image_count = 10
print(image_count)
```

## Indentation

Python menggunakan indentation untuk membentuk code block:

```python
is_training = True

if is_training:
    print("Model is training")
```

Gunakan empat spasi per level indentation.

## Comment dan Docstring

```python
# Comment menjelaskan alasan atau konteks.

def load_image(path: str):
    """Load one image from disk."""
```

Comment yang baik menjelaskan **kenapa**, bukan mengulang apa yang sudah jelas dari kode.

## Module dan Import

Setiap file `.py` dapat menjadi module:

```python
from pathlib import Path
from math import sqrt
```

Entry point:

```python
def main() -> None:
    print("Start pipeline")


if __name__ == "__main__":
    main()
```

Pola ini mencegah kode utama langsung berjalan saat file di-import.

## Pythonic Code

Pythonic berarti memakai pola yang jelas dan idiomatis:

```python
image_names = ["cat.jpg", "dog.png"]

for image_name in image_names:
    print(image_name)
```

Utamakan kode yang mudah dibaca daripada kode pendek tetapi membingungkan.

## Script, Module, Package, dan Notebook

- **Script:** file yang dijalankan sebagai program.
- **Module:** file Python yang dapat di-import.
- **Package:** kumpulan module dalam namespace terstruktur.
- **Notebook:** dokumen interaktif dengan state antar-cell.

Notebook cocok untuk eksplorasi, tetapi execution order tersembunyi dapat merusak reproducibility. Logic penting sebaiknya dipindah ke module.

## Studi Kasus: Struktur Project AI

```text
project/
├── pyproject.toml
├── README.md
├── src/
│   └── vision_project/
│       ├── __init__.py
│       ├── data.py
│       ├── model.py
│       └── train.py
├── tests/
└── notebooks/
```

Notebook mengimpor logic dari `src`, bukan menjadi satu-satunya sumber kode.

## Performa dan GIL

CPython mempunyai Global Interpreter Lock pada execution thread Python biasa. Dampaknya terutama pada CPU-bound Python code. Library numerik dapat melepaskan GIL atau menjalankan operasi native/GPU.

Jangan melakukan premature optimization. Ukur dengan profiler, lalu vectorize atau pindahkan bottleneck ke library yang tepat.

## Best Practice

- Gunakan Python 3 versi modern.
- Pakai empat spasi, bukan campuran tab dan spasi.
- Ikuti PEP 8 secara wajar.
- Pisahkan konfigurasi dari logic.
- Gunakan `main()` untuk entry point script.
- Jangan menamai file sama dengan standard library, misalnya `random.py`.

## Kesalahan Umum

- Menjalankan script dengan interpreter dari environment yang salah.
- Salah indentation.
- File bernama sama dengan library yang di-import.
- Menaruh seluruh program dalam satu file besar.
- Mengandalkan notebook untuk semua kode produksi.

## Debugging

### `SyntaxError`

Periksa line yang ditunjuk dan line sebelumnya; kurung atau quote yang belum ditutup sering membuat lokasi error tampak bergeser.

### `ModuleNotFoundError`

Periksa:

```python
import sys

print(sys.executable)
print(sys.path)
```

Pastikan package terpasang pada interpreter aktif dan working directory sesuai.

### State Notebook Membingungkan

Restart kernel dan jalankan seluruh cell dari awal. Jika gagal, notebook belum reproducible.

## Ringkasan

- Python mengeksekusi kode melalui interpreter.
- Indentation adalah bagian dari syntax.
- Module membantu memecah program.
- Python populer di AI karena ekosistemnya, bukan karena semua operasi Python murni cepat.

## Checklist Pemahaman

- [ ] Bisa membedakan source, bytecode, dan runtime.
- [ ] Bisa menjalankan script dan memeriksa interpreter.
- [ ] Bisa menjelaskan indentation dan entry point.
- [ ] Bisa membedakan script, module, package, dan notebook.
- [ ] Bisa menjelaskan kenapa library AI tetap cepat.
- [ ] Bisa mendiagnosis import dari environment salah.

## Hubungan Konsep

- Parent: [[Python MOC]]
- Lanjutan: [[Python Variables]], [[Python Data Types]], [[Python Control Flow]]
- Digunakan di: [[Scientific Computing MOC]], [[Computer Vision MOC]]

## Latihan

Buat script dengan function `main()` yang mencetak nama project dan jumlah gambar.

2. Pecah script menjadi `main.py` dan `image_utils.py`.
3. Jalankan dari terminal dan import function-nya dari interpreter.
4. Buat notebook kecil yang memakai module tersebut tanpa menyalin function.
