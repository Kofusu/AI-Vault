---
type: concept
status: not-started
domain: python
topic: reliability
level: foundations
order: 7
created: 2026-07-28
---

# Python Exception Handling

## Tujuan

- Memahami exception dan traceback.
- Menangani error yang memang dapat dipulihkan.
- Membuat custom exception dan menjaga informasi penyebab error.

## Intuisi

Exception adalah sinyal bahwa program tidak dapat melanjutkan operasi secara normal.

```text
Operasi
  ├── berhasil → lanjut
  └── gagal    → raise exception
```

## Konsep Dasar

Exception adalah object yang merepresentasikan kegagalan. Saat di-raise, normal control flow berhenti dan interpreter mencari handler yang cocok sepanjang call stack.

Hierarchy umum:

```text
BaseException
├── SystemExit
├── KeyboardInterrupt
└── Exception
    ├── ValueError
    ├── TypeError
    ├── OSError
    │   └── FileNotFoundError
    └── RuntimeError
```

Biasanya application menangkap turunan `Exception`, bukan `BaseException`.

## Kenapa Dibutuhkan?

Sistem AI berinteraksi dengan file, network, GPU, dan data eksternal. Failure tidak dapat dihilangkan, tetapi harus:

- terdeteksi
- diberi context
- dipulihkan jika aman
- dihentikan jika state tidak dapat dipercaya
- tercatat untuk diagnosis

## Cara Kerja Exception Propagation

```text
Function C raises
      ↓ tidak ada handler
Function B
      ↓ tidak ada handler
Function A catches
      ↓
Recovery atau re-raise
```

Handler paling dekat yang cocok menerima exception.

## Membaca Traceback

Traceback menunjukkan:

- jenis exception
- pesan error
- urutan function call
- file dan line tempat error terjadi

Biasakan membaca dari bagian paling bawah untuk melihat exception utama, lalu telusuri call stack ke atas.

## `try` dan `except`

```python
from pathlib import Path


def read_label(path: Path) -> str:
    try:
        return path.read_text(encoding="utf-8")
    except FileNotFoundError:
        return "unknown"
```

Tangkap exception yang spesifik.

Buruk:

```python
try:
    process_image()
except Exception:
    pass
```

Kode tersebut menyembunyikan bug dan menghilangkan informasi penting.

## `else` dan `finally`

```python
try:
    file = open("metrics.txt", encoding="utf-8")
except FileNotFoundError:
    print("Metrics file does not exist")
else:
    print(file.read())
finally:
    if "file" in locals():
        file.close()
```

Untuk file, context manager lebih aman:

```python
with open("metrics.txt", encoding="utf-8") as file:
    metrics = file.read()
```

## `raise`

Validasi input:

```python
def validate_threshold(threshold: float) -> None:
    if not 0.0 <= threshold <= 1.0:
        raise ValueError("threshold must be between 0 and 1")
```

Gunakan exception type berdasarkan masalah:

- `ValueError`: nilai tidak valid
- `TypeError`: type atau interface salah
- `FileNotFoundError`: file tidak ada
- custom domain exception: failure dengan makna domain khusus

## Exception Chaining

```python
class DatasetLoadError(RuntimeError):
    """Raised when a dataset cannot be loaded."""


def load_dataset(path: Path):
    try:
        return path.read_text(encoding="utf-8")
    except OSError as error:
        raise DatasetLoadError(f"Failed to load {path}") from error
```

`from error` mempertahankan penyebab asli.

## LBYL vs EAFP

- **LBYL:** Look Before You Leap; cek kondisi sebelum operasi.
- **EAFP:** Easier to Ask Forgiveness than Permission; coba operasi dan tangani kegagalan.

Python sering memakai EAFP, terutama jika kondisi dapat berubah antara pengecekan dan operasi.

## Implementasi: Safe Image Loader

```python
from pathlib import Path
from PIL import Image, UnidentifiedImageError


class ImageLoadError(RuntimeError):
    pass


def load_rgb_image(path: Path) -> Image.Image:
    try:
        with Image.open(path) as image:
            return image.convert("RGB").copy()
    except FileNotFoundError:
        raise
    except UnidentifiedImageError as error:
        raise ImageLoadError(
            f"Unsupported or corrupted image: {path}"
        ) from error
```

File hilang dibiarkan sebagai `FileNotFoundError`; image rusak diberi domain context.

## Relevansi untuk AI/CV

- File gambar corrupted.
- Annotation hilang.
- Shape tensor tidak valid.
- GPU out of memory.
- Model checkpoint tidak cocok.
- Input API tidak valid.

Tidak semua error harus ditangani. Programming error sebaiknya terlihat dan diperbaiki.

## Studi Kasus: Failure Policy Dataset

Tentukan policy sebelum training:

```text
File temporer hilang        → fail
Image corrupted terdata     → log + skip jika policy mengizinkan
Annotation schema invalid   → fail
GPU OOM                     → fail atau retry terkontrol
Checkpoint incompatible     → fail dengan context
```

Diam-diam melewati sample dapat mengubah distribusi dataset.

## Best Practice

- Tangkap exception sesempit mungkin.
- Tulis pesan error yang menjelaskan context.
- Pertahankan root cause.
- Log sebelum fallback jika kegagalan penting.
- Jangan gunakan exception untuk control flow normal yang sering terjadi.

## Kesalahan Umum

- Bare `except:`.
- `except Exception: pass`.
- Menangkap error terlalu jauh dari sumbernya.
- Mengganti exception tanpa menyimpan penyebab.
- Melanjutkan training setelah data corruption tanpa laporan.

## Debugging

- Baca exception type dan message paling bawah.
- Telusuri traceback menuju call yang lu kontrol.
- Jangan menghapus traceback dengan `raise error`; gunakan bare `raise` saat re-raise.
- Tambahkan context dengan chaining `from`.
- Reproduksi dengan input terkecil.
- Gunakan debugger untuk inspect local variables sebelum frame hilang.

## Checklist Pemahaman

- [ ] Bisa membaca traceback.
- [ ] Bisa memilih exception type yang tepat.
- [ ] Bisa menjelaskan propagation.
- [ ] Bisa memakai `try/except/else/finally`.
- [ ] Bisa membuat custom exception dan chaining.
- [ ] Bisa merancang failure policy dataset.

## Ringkasan

- Exception memisahkan jalur sukses dan gagal.
- Tangkap hanya error yang dapat ditangani dengan masuk akal.
- Traceback adalah alat diagnosis, bukan sekadar pesan kegagalan.

## Hubungan Konsep

- Prasyarat: [[Python Control Flow]], [[Python Functions]]
- Parent: [[Python MOC]]
- Lanjutan: [[Python File Handling]], [[Python Logging]]
- Digunakan di: [[Dataset]], [[Training Pipeline]], [[Deployment MOC]]

## Latihan

Buat function pembaca annotation yang menangani file hilang tetapi tetap membiarkan JSON invalid menghasilkan error yang jelas.

2. Tambahkan custom `AnnotationLoadError` dengan root cause.
3. Tulis test untuk file hilang dan JSON rusak.
4. Jelaskan kapan recovery lebih berbahaya daripada fail fast.
