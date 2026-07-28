---
type: concept
status: not-started
domain: python
topic: environment
level: foundations
order: 9
created: 2026-07-28
---

# Python Virtual Environment

## Tujuan

- Memahami isolasi dependency per project.
- Membuat, mengaktifkan, dan menghapus virtual environment.
- Memastikan interpreter yang dipakai sesuai project.

## Intuisi

Virtual environment adalah ruang kerja Python terisolasi untuk satu project. Ia mencegah dependency project saling mengotori.

## Konsep Dasar

Environment berisi atau menunjuk:

- Python interpreter
- `site-packages`
- executable script package
- metadata environment

Aktivasi bukan syarat mutlak; ia hanya mengubah `PATH` agar command environment dipilih lebih dulu.

## Kenapa Dibutuhkan?

- dependency version berbeda
- eksperimen reproducible
- menghindari global installation
- editor dan test memakai interpreter yang sama
- mudah menghapus dan membuat ulang setup

## Masalah yang Diselesaikan

Project berbeda dapat membutuhkan versi package berbeda:

```text
Project A → package X versi 1
Project B → package X versi 2
```

Tanpa isolasi, dependency dapat bentrok.

## Cara Kerja

Virtual environment menyediakan:

- interpreter reference atau executable khusus environment
- folder package terisolasi
- command `python` dan `pip` yang mengarah ke environment tersebut

Environment tidak otomatis mengisolasi driver GPU atau seluruh dependency system.

## Implementasi Workflow

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install pillow
python -c "import PIL; print(PIL.__version__)"
```

Verifikasi setelah membuka terminal atau editor baru.

## Membuat Environment dengan `venv`

```bash
python -m venv .venv
```

Aktivasi macOS atau Linux:

```bash
source .venv/bin/activate
```

Aktivasi Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Keluar:

```bash
deactivate
```

## Verifikasi Interpreter

```bash
python -c "import sys; print(sys.executable)"
python -m pip --version
```

Selalu verifikasi jika package terasa “sudah di-install tetapi tidak ditemukan”.

## Conda Environment

Conda dapat mengelola Python serta sebagian non-Python dependency:

```bash
conda create --name cv-project python
conda activate cv-project
```

Pilih satu workflow utama per project agar dependency tidak membingungkan.

## Folder `.venv`

Jangan commit isi environment:

```gitignore
.venv/
```

Commit dependency specification, bukan folder environment.

## Reproducibility

Virtual environment membantu isolasi, tetapi reproducibility juga memerlukan:

- versi dependency
- versi Python
- system atau CUDA requirement
- seed dan config
- dokumentasi setup

## Studi Kasus: Dua Project

```text
food-classifier/.venv → dependency A
robot-tracker/.venv   → dependency B
```

Keduanya boleh memakai versi package berbeda tanpa konflik langsung. Driver GPU dan system library tetap berada di luar boundary `venv`.

## Environment vs Container

- Virtual environment mengisolasi Python package.
- Container mengisolasi filesystem dan sebagian system environment.
- Keduanya dapat dipakai bersama.

## Best Practice

- Satu environment per project.
- Nama `.venv` mudah dideteksi editor.
- Gunakan `python -m pip` agar pip mengikuti interpreter aktif.
- Rekam dependency dengan format yang sesuai.
- Recreate environment jika kondisinya sudah tidak jelas.

## Kesalahan Umum

- Install package ke global Python.
- `pip` dan `python` berasal dari environment berbeda.
- Commit `.venv`.
- Menganggap virtual environment menggantikan Docker.
- Mencampur Conda dan pip tanpa memahami urutan install.

## Debugging

```bash
which python
which pip
python -m pip --version
python -c "import sys; print(sys.executable)"
```

Jika editor berbeda dari terminal, pilih interpreter `.venv` di editor. Jika environment kacau, sering lebih aman recreate daripada memperbaiki manual.

## Checklist Pemahaman

- [ ] Bisa menjelaskan boundary virtual environment.
- [ ] Bisa membuat, mengaktifkan, dan menghapus environment.
- [ ] Bisa memeriksa interpreter aktif.
- [ ] Tahu `.venv` tidak di-commit.
- [ ] Bisa membedakan venv dan container.
- [ ] Bisa mendiagnosis pip/interpreter mismatch.

## Ringkasan

- Virtual environment mengisolasi Python dependency.
- Aktivasi mengubah command yang diprioritaskan shell.
- Environment harus dapat dibuat ulang dari dependency specification.

## Hubungan Konsep

- Prasyarat: [[Python Fundamentals]]
- Parent: [[Python MOC]]
- Lanjutan: [[Python Package Management]]
- Terkait: [[Docker]], [[Reproducibility]]

## Latihan

Buat `.venv`, aktifkan, lalu verifikasi path interpreter dan pip yang digunakan.

2. Install satu package lalu import.
3. Deactivate dan buktikan import berbeda jika package tidak global.
4. Hapus dan recreate environment.
