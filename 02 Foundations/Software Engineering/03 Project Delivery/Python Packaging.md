---
type: concept
status: not-started
domain: software-engineering
topic: packaging
level: foundation
created: 2026-07-29
updated: 2026-07-29
---

# Python Packaging

## Tujuan

Membuat source code AI dapat di-install sebagai package dengan dependency dan entry point yang jelas.

## Intuisi

Package menghilangkan ketergantungan pada manipulasi `sys.path` dan current directory. Import menjadi konsisten untuk script, test, dan notebook.

## `src` Layout

```text
project/
├── pyproject.toml
├── README.md
├── src/
│   └── vision_project/
│       ├── __init__.py
│       ├── data.py
│       └── train.py
└── tests/
```

## `pyproject.toml`

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "vision-project"
version = "0.1.0"
description = "Reproducible computer vision baseline"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
  "numpy>=2.0",
  "scikit-learn>=1.5",
]

[project.scripts]
vision-train = "vision_project.train:main"
```

Versi contoh harus disesuaikan dan diuji terhadap environment project.

## Editable Install

```bash
python -m pip install -e .
```

Perubahan source langsung tersedia tanpa install ulang.

## Dependency Groups

Pisahkan runtime dari development dependencies seperti test, lint, dan docs. Gunakan lock file/tool yang dipilih project untuk reproducibility.

## Versioning

Semantic versioning:

```text
MAJOR.MINOR.PATCH
```

- major: breaking change
- minor: feature kompatibel
- patch: bug fix kompatibel

Untuk project riset internal, versioning tetap berguna untuk menghubungkan artifact dan code.

## Import

Gunakan absolute import dari package:

```python
from vision_project.data import build_dataset
```

Hindari mengubah `sys.path` di notebook.

## Build

```bash
python -m build
```

Artifact umumnya berupa wheel dan source distribution.

## Best Practice

- Pakai `src` layout.
- Definisikan Python version.
- Pisahkan runtime dan development dependency.
- Test package setelah install pada environment bersih.
- Jangan memasukkan dataset/checkpoint ke wheel.

## Kesalahan Umum

- Nama import package berbeda tanpa dokumentasi.
- Dependency tidak dideklarasikan karena kebetulan tersedia global.
- Circular import.
- Import menjalankan proses berat sebagai side effect.
- Menggunakan wildcard import.

## Ringkasan

Python packaging membuat module dapat di-install, di-import, diuji, dan dijalankan secara konsisten pada berbagai environment.

## Hubungan Konsep

- [[Python Package Management]]
- [[Python Virtual Environment]]
- [[AI Project Structure]]
- [[Testing AI Code with Pytest]]

## Checklist Pemahaman

- [ ] Bisa menjelaskan `src` layout
- [ ] Bisa membaca `pyproject.toml`
- [ ] Bisa melakukan editable install
- [ ] Bisa membuat CLI entry point

## Latihan

1. Package-kan project kecil dengan `src` layout.
2. Tambahkan command CLI dari `[project.scripts]`.
3. Install pada virtual environment bersih dan jalankan test.

